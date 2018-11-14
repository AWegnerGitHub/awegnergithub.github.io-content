Title: My experiences releasing a package to PyPI
Date: 2016-3-15 12:26
Tags: technical
Category: Technical Solutions
Slug: my-experiences-releasing-a-package-to-pypi
Summary: I released StackAPI to PyPI. This post talks about my experiences.
Status: published

[TOC]

## Motivation

In some of my [other projects][1], I've needed to make extensive use of the Stack Exchange API. I built a small library - StackAPI - to assist in this task and released it on Python's [PyPI repository][2]. This post is going to cover some of the technical decisions and issues I ran into while going through this process. This was my first project being released to PyPI.

My goals when releasing this were:

 - Clean up my own code so that it is usable by others
 - Improve the documentation and host the documentation on [Read the Docs][3]
 - Automatically release it to PyPI, if it passes basic tests

Those goals sound simple. In the future, they probably will be, but for this first release it wasn't as simple as I was hoping.

## Project Layout

Before this project, I'd written modules and libraries that were used by myself (for personal projects) or as part of a larger application (for work). In both cases, though, I had a directory structure that looked something like this:

    /project_root
	    /mymodule
		    __init__.py
			mymodule.py
		__init__.py

This was close to the end goal, but lacked some files in the `project_root` that were needed for a proper install via `pip`. The important file that was missing was `setup.py`. I needed this file to ensure that everything would install with a simple `pip install stackapi`	    

### setup.py

My [`setup.py`][4] is pretty basic and available on GitHub. There are a couple important things though.

 - `version`: This is needed, but I didn't want to have to constantly remember to update this when pushing a version to PyPI. This was one of my first criteria when starting the project. I wanted to automate as much as I could, and versioning was at the top of that list. It's small, but easy to forget and keep syncronized across all the files in the project. I decided to utilize [bumpversion][5] and [Fabric][6] to manage this specific field (both here and elsewhere in the project).
 - `install_requires`: StackAPI is built in the fantasitc [Requests][7] library. To ensure this was installed when StackAPI was install, it was needed in this field.
 - `tests_require`: The testsuite I build utilizes the `mock` library. I don't want that to be installed if the developer isn't running the tests, so it is added to this field.
 - `test_suite`: I wanted developers to be able to run `python setup.py test` to execute the test suite. To do so, I had to point to the where the tests were being executed from.
 
The rest of `setup.py` seemed to be fairly standard when compared to other Python libraries.

### Bumpversion and Fabric

As I mentioned, I wanted to automate any versioning that was required. To do so, I used the [bumpversion][5] library and wrote a small [Fabric][8] script to handle it automatically. `bumpversion` uses a [config file][9], to determine what it is going to do. I configured it to automatically create a commit and a new git tag for each version. I then pointed to a couple files where the current version is listed. When `bumpversion` is executed, it will change the version in each of those files to the new version. It will then create a single commit to the git repository with a commit message similar to 

> Bump version: 0.1.6 → 0.1.7

That is nice and clean. It tags the commit for me, which is useful later, when I want to push the change to PyPI.

To make running `bumpversion` a bit easier, I utilized a Fabric routine I found [online][14] and adjusted it for my purposes. When I run `fab release`, all of the `bumpversion` 'stuff' occurs. Then I just have to push the commit (and new tag) to GitHub. 

### Final Project Layout

The final project layout I settled on was this:

    /stackapi
        /docs
		    ...
    	/stackapi
		    __init__.py
			stackapi.py
		/tests
		.gitignore
		.travis.yml
	    fabfile.py
		setup.cfg
		setup.py


## Read the Docs

### Configure the project

In the final project layout, you can see there is a `docs` directory. One of my goals was to make this library usable and understandable by other developers. A good part of that means having decent documentation. I spent way more time than I expected cleaning the documentation in the code and creating documentation with examples. Most of that time was spent learning the sphinx documentation style and ReStructuredText, which Read the Docs utilizes.

The first step in this process was installing and setting up the initial configuration for the documentation:

    pip install sphinx sphinx-autobuild
	
Then, I created the `docs` directory and switched to it and ran:

    sphinx-quickstart
	
This starts a short, interactive, wizard. Fill out the questions. At the end of this, it creates a [`conf.py`][10] file in the `docs` directory. The rest of the [documentation][11] is ReStructuredText files.

To see how the documentation looks, from the `docs` directory, run:

    make html
	
This creates a `_build` directory. If you open `_build/html/index.html`, the documentation can be browsed locally. I do not commit this directory to git, though. It is ignored in `.gitignore`, as a user can regenerate it at will.

### Configure Read the Docs

Once I was satisfied with how the documentation looked, I had to configure Read the Docs to read my GitHub repository. To repeat those steps:

 - Sign up (or log in) at [Read the Docs][12] (part of this will be associating the account to a GitHub account)
 - Visit your [dashboard][13] and click "Import a project"
 - Fill out the form, but in my case the defaults were all appropriate. Do note that URLs are case sensitive.
 - Click "Create". This is the first version of your documentation.
 - To keep the code updating as you update GitHub, log into GitHub and go to the repository's "Settings" page.
 - Click "Webhooks & Services"
 - Click "Add Service"
 - Select "ReadTheDocs" and add the service
 
At this point, each time you push a change to the repository, a new set of documents will be built. I then added the Read the Docs badge to my `README.rst` for a simple link to the detailed documentation.

	.. image:: https://readthedocs.org/projects/stackapi/badge/?version=latest
	:target: http://stackapi.readthedocs.org/en/latest/?badge=latest
	:alt: Documentation Status

### Force rebuild of docs

Toward the very end of this project, Read the Docs had a minor hiccup and failed on building my documentation. I didn't want to force a build by making a fake commit. Instead, Read the Docs provides the information needed to force a rebuild. It requires a very simple `POST` to the Post Commit Hook they provide. In my case, this was as simple as running this command (provided from the Dashboard):

    curl -X POST https://readthedocs.org/build/stackapi

## PyPI

Nearing the end of the journey, it was time to see what exactly PyPI required. The first step was setting up an account on both the Test and Production instances of PyPI.

 - PiPY Test: http://testpypi.python.org/pypi?%3Aaction=register_form
 - PyPI Live: https://pypi.python.org/pypi?%3Aaction=register_form
 
Having one on both was important while testing. It meant that I didn't have to send broken versions to the live PyPI server, and I could adjust ReStructuredText formatting issues without requiring another release to PyPI. Each time a version is pushed to PyPI it **must** have a new version number. By using the test instance, I could use as many of these fake versions as needed to fix things. Hooray for test environments!

Before we perform this step automatically, we need to test that the PyPI accounts work. By following portions of a ["First Time with PyPI"][15] tutorial, I focused by steps down to these:

 - Create a `.pypirc` file in your home directory - not your project directory. This won't be required once Travis CI is set up and configured, so having the passwords in this, temporarily, wasn't an issue because I eventually deleted the file.
 
 The file looks like this:
 
    [distutils]
	index-servers =
	  pypi
	  pypitest
  
    [pypi]
    repository=https://pypi.python.org/pypi
	username=your_username
	password=your_password
  
	[pypitest]
	repository=https://testpypi.python.org/pypi
	username=your_username
	password=your_password
	
 - Register the package on PyPI Test: `python setup.py register -r pypitest`
 - Register the package on PyPI Live: `python setup.py register -r pypi`
 - Upload the project to Test: `python setup.py register -r pypitest`
 - Upload the project to Live, *if* you're ready for your first release. Remember, once a version is released to PyPI, it can't be used again (or overwritten): `python setup.py register -r pypi`
 
If all of the above passed to your satisfaction, you can remove the `.pypirc` file and move on to configuring Travis CI.	
	
## Travis

The last step in this process will be using Travis CI to perform some basic tests and, if this was a new release, push the changes to PyPI. The Travis config file is available on [GitHub][15]. 

My goal is to support 'modern' Python with this library. I've configured Travis to test against multiple versions of Python, ranging from 2.7 to 3.5. StackAPI is installed using `python setup.py -q install`. Then the test suite is run. 

The important bits are in the `deploy` section. 

    on:
      tags: true
      branch: master
	
If there is a new git tag pushed to GitHub, and the tests pass, Travis CI will push the code to PyPI. Since `bumpversion` makes a new git tag with each new version, this works perfectly. 

This does require that my password be included in the yml file. To keep this secure, I utilized the [Travis Command Line Client][16] (`gem install travis`). In my local directory, I then ran:

    travis encrypt --add deploy.password
	
This added the password to the YML file. 

## Conclusion

This was the first time I've released something to PyPI. It took a lot more set up than I expected it would take. However, now that I've gone through the process, gotten used to the ReStructuredText format that Sphinx and Read the Docs require, and set up PyPI for one project, I think it'll be fairly simple to do in the future. Most of the work is getting the other services to talk with GitHub and practicing good developer habits (documentation...).


## All StackAPI Links

All of these links are to the various places that StackAPI lives on the internet

 - **GitHub**: https://github.com/AWegnerGitHub/stackapi
 - **Read the Docs**: http://stackapi.readthedocs.org/en/latest/
 - **TravisCI**: https://travis-ci.org/AWegnerGitHub/stackapi
 - **PyPI**: https://pypi.python.org/pypi/StackAPI







 [1]: http://andrewwegner.com/can-a-machine-be-taught-to-flag-comments-automatically.html
 [2]: https://pypi.python.org/pypi/StackAPI
 [3]: https://readthedocs.org/
 [4]: https://github.com/AWegnerGitHub/stackapi/blob/master/setup.py
 [5]: https://pypi.python.org/pypi/bumpversion
 [6]: http://www.fabfile.org/
 [7]: http://docs.python-requests.org/en/master/
 [8]: https://github.com/AWegnerGitHub/stackapi/blob/master/fabfile.py
 [9]: https://github.com/AWegnerGitHub/stackapi/blob/master/setup.cfg
 [10]: https://github.com/AWegnerGitHub/stackapi/blob/master/docs/conf.py
 [11]: https://github.com/AWegnerGitHub/stackapi/tree/master/docs
 [12]: https://readthedocs.org/
 [13]: https://readthedocs.org/dashboard/
 [14]: https://gist.github.com/jbarratt/85c91d7b904462702892
 [15]: https://github.com/AWegnerGitHub/stackapi/blob/master/.travis.yml
 [16]: https://blog.travis-ci.com/2013-01-14-new-client/