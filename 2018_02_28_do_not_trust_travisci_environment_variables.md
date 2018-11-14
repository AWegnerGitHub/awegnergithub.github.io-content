Title: Travis CI doesn't keep your environment variable secure
Date: 2018-04-02 12:30
Tags: technical
Category: Technical Solutions
Slug: travisci-insecure-environment-variables
Summary: Travis CI does not keep your environment variables secure if you transfer a repository.   
Status: published

[TOC]

## Introduction

On December 27, 2017 I reported a security issue directly to the security team as their [CONTRIBUTING.md][1] recommends. I received an automated response that a human would
follow up with me soon. It was their end of year, two week vacation (which is awesome!). I sent the same email again on January 26, 2018 and received a response back from AJ Bowen, a Build Infrastructure Engineer at Travis CI on January 29, 2018. They'd created an internal issue to track the behavior and would follow up within two weeks. 

I followed up with AJ on February 28, 2018 and didn't receive a response. We're now over three months since my initial report. I believe it's time to make this more public so 
that others know to be careful with their Travis CI managed environment variables.

## The Issue

[Travis CI ][2] is an application that allows you to automatically test and deploy applications after a commit is pushed to GitHub. I've used this ability to run unit tests, 
[automatically deploy updates to PyPI][3], and more recently when testing deployment to AWS using the Serverless framework. It's that last one that led me to this issue.

Part of deploying to AWS requires that you have credentials to deploy. I didn't want to put my AWS deploy credentials in GitHub, even if they are [encrypted][4]. Instead, 
I decided to set my variables in the [Travis CI Settings][5]. I went forward with my testing, watched the deploys happen as expected and eventually needed to transfer my
repository to a third party. 

I used GitHub to transfer the repository to the new owner. We tested a build and watched it deploy. The Travis CI console showed a successful deploy. The problem is, 
it deployed to *my* AWS account using *my* AWS credentials. These "secure" environment variables had been transfered to a third party and were no longer in my control.

## Reproduction - Short Version

Reproducing the issue is trivial. The short version is this:

 1. On one GitHub account, create a repository with a `.travis.yml` file 
 2. On the Travis CI account associated with step 1, set up an environment variable and elect *not* to show the value in the build log
 3. Transfer the GitHub repository to another account.
 
At this point, the environment variables defined in step 2 are accessible by the new owner from step 3. 

## Reproduction - Long Version 

The detailed steps taken to reproduce this issue show that Travis CI is simply looking for the environment variable values and scrubbing those from the build logs. Once transfered, an edit can be introduced to show these variables with minimal work. 

### Create a repository

Create a new repository and add something. For this test, I created a simple Python Hello World file, and named it `hello.py`.

    print("Hello World")
	
Commit this change to your new repository.

### Enable Travis CI integration

Go to [Travis CI][2] and log in with the GitHub account associated with the above step. Sync your account. Then enable integration by changing the repository switch.

![Enable Integration][6]

### Create a .travis.yml file

With the integration now in place, set up a basic build script by adding a `.travis.yml` file to the repository. 

	language: python
	python:
	  - "3.5"
	script: 
	  - python hello.py

This script will set up a build task and run your `hello.py` file, using Python 3.5. You will see that "Hello World!" is printed in the build console.

![First Build][7]

### Add Environment Variables

Now that our build script is working, we can work on "deployment". Deployment to AWS (or other cloud services) requires that you provide credentials. I am not a fan of 
including credentials in my repository, even if they are encrypted. Opting for an environment variable should be more secure, as the credentials are never in your repository
in the first place. **It is important to note that you are still giving your credentials to Travis CI in this case.**

To set up environment variables, click on "More Options" and "Settings" within the Travis CI application.

![Travis Settings][8]

Now scroll down to "Environment Variables". Add the name of the variable and the value. Be sure to leave the default value of "Off" selected. You don't want to display this 
value in the build log. Finally click "Add".

![Adding a variable][9]

I've added a second variable for further testing.

![Adding a second variable][10]

Notice that variable values are hidden from view after clicking "Add".

![Variable values hidden][11]

### Check values during build

With the variables saved, restart your build. 

![Restart Build][12]

When the build has completed, check the build log. Even though we aren't using these values yet, we can see the environment variables exist and are "Secure".

!["Secure" Variables][13]

### Accessing the values

These values are not actually secure. Travis CI is filtering for the values of these environment variables and if the specific string is found, it is scrubbed from
the log. We can see this with a small change to `hello.py`.

	import os

	print("Hello World!")
	aws_id = os.environ['AWS_ACCESS_KEY_ID']
	aws_secret = os.environ['AWS_SECRET_ACCESS_KEY']
	print("AWS KEY ID: |{}{}|".format(aws_id[:len(aws_id)//2], aws_id[len(aws_id)//2:]))
	print("AWS SECRET KEY: |{} {}|".format(aws_secret[:len(aws_secret)//2], aws_secret[len(aws_secret)//2:]))

In this example, we are splitting the values of the environment variables in half. For the `AWS_ACCESS_KEY_ID` value, you smash these two together. This will match the 
environment variable value, and will not be shown because the pattern still matches the value:

    |{}{}|
	
For `AWS_SECRET_ACCESS_KEY`, we split the two halves with a space and print it out. This will be shown, because the extra space no longer matches the exact value of the 
environment variable.

    |{} {}|
	
Commit and push the change to GitHub. In Travis CI, we see the following in the build log:

![Accessing the values][14]

As expected, the first pattern is hidden because it matches the environment variable. The second pattern is shown, because the space in the middle means the pattern no longer 
matches.

### Transfer the repository

Now that we've shown the variables are accessible, it's time to transfer the repository to a new owner. In GitHub, this can be accomplished by going to the repository settings
and going down to the red "danger area". Once you've entered the name of the current repository and the name of the new owner, we wait for the new owner to accept the transfer.

![Transfer the repository][15]

### Build with the new owner

Make a change and commit it to the new repository. I simply modified the "Hello World" line:

    print("Hello World from new owner!")
	
A new build will kick off. You can see that the repository has transfered to the new owner in the build log. You can also see the environment variables were transfered to the 
new owner. Other than the new owner being listed, the build log shows the same output as before

![Everything has transfered][16]

### Variables in the UI

You can also see these variables have transfered by going back to "More Options", then "Settings" in Travis CI. The values are still hidden behind the input password field. 

![Variables in UI][17]

## Impact of bug

The example above shows two problems. The bigger problem, in my opinion, is that environment variables are transfered to a new owner. The secondary problem is that "secure"
variables are really just obfuscated. Accessing them is trivial. With this demonstration, we added in a step to show that the variables can be seen by the original
owner. However, it is just as likely that the new owner could introduce such a change after the repository is transfered.

This bug requires the owner of the repository to perform the "dangerous" GitHub action of transferring a repository. That means it's impact is limited. However, it's just as
likely that the original owner has forgotten that environment variables were set up in Travis CI, an entirely separate system. 

When a GitHub repository is transfered to a new owner, the environment variables in Travis CI should not travel with. This is especially true for the "secure" variables. I'd 
rather that a build breaks after the transfer due to the lack of appropriate variables being set up than having my cloud credentials be sent to a third party. 

## Mitigation

Mitigation of this bug, until Travis CI stops transferring environment variables to new repository owners, requires the original owner to remove the variables prior to 
transferring the repository. One of the steps that the owner should take is to log into Travis CI and ensure all secure variables have been removed from the Travis CI 
environment. This will break the builds, but it will also ensure that private variables aren't leaked unintentionally to a third party.

## Repository

The repository for testing is available on [GitHub][18].

This issue has been reported in the following places:

 - [Travis CI Github Issues][19]
 - [Hacker News][20]
 - [Charcoal HQ][21]



 [1]: https://github.com/travis-ci/travis-ci/blob/master/CONTRIBUTING.md
 [2]: https://travis-ci.org/
 [3]: {filename}2016_03_23_my-experiences-releasing-a-package-to-pypi.md
 [4]: https://docs.travis-ci.com/user/environment-variables/#Encrypting-environment-variables
 [5]: https://docs.travis-ci.com/user/environment-variables/#Defining-Variables-in-Repository-Settings
 [6]: {attach}images/1-travis-enable-repository.png
 [7]: {attach}images/2-travis-first-build.png
 [8]: {attach}images/3-add-variable-1.png
 [9]: {attach}images/4-add-variable-2.png
 [10]: {attach}images/5-add-variable-3.png
 [11]: {attach}images/6-variables-added.png
 [12]: {attach}images/7-restart-build.png
 [13]: {attach}images/8-variables-secure.png
 [14]: {attach}images/9-variables-filtered-by-match.png
 [15]: {attach}images/10-transfer-repository.png
 [16]: {attach}images/11-build-after-transfer.png
 [17]: {attach}images/12-variables-transfered.png
 [18]: https://github.com/AWegnerGitHub/TravisIssue
 [19]: https://github.com/travis-ci/travis-ci/issues/9430
 [20]: https://news.ycombinator.com/item?id=16737099
 [21]: https://chat.stackexchange.com/transcript/message/43757867#43757867