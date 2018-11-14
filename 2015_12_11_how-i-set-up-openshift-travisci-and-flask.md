Title: How I built a Flask application that integrates with Travis CI and OpenShift
Date: 2015-12-11 09:15
Tags: technical
Category: Technical Solutions
Slug: how-i-set-up-openshift-travisci-and-flask
Summary: A walkthrough on how I set up a Flask application on OpenShift and used TravisCI to deploy it
modified: Dec 12, 2015
Status: published

[TOC]

## Motivation

Since I [shut down][1] Vipers early this year, I've been itching to do *something* web related. Web technologies aren't my best technical skill, but I like trying out new things and learning something in the process. I use Python at work. I like Python a lot. With Christmas and New Years coming up, I want to have a project during my down time. My goal is to get a [Flask][2] application built and then deployed to [OpenShift][3]. Part of this deployment is to utilize [TravisCI][4]. I'm planning on using [pytest][14] and [hypothesis][15] for my test suite. Finally, I want to use my own (sub)domain, instead of the provided `rhcloud` one.

Of these three technologies, I've used only Flask before. The [comment flagging bot][5] I built has a dashboard built in Flask. I've never used OpenShift or TravisCI. I selected OpenShift because it has a couple [features][6] I want that Heroku doesn't. The biggest one, according to the previous link, was that OpenShift has support for MySQL and Heroku doesn't (surprisingly). I want to use TravisCI and automated testing, because one of my goals for next year at work is to introduce automated tested to our development. (I work with Engineers, not coders...that's my excuse and it's a bad excuse, so I'm going to try and fix it.) To get ready for that goal, I want to test out a system that does continuous integration/automated testing. Both OpenShift and Travis CI provide me with free services. Hypothesis and py.test provide me with a way to generate comprehensive test conditions. 

## OpenShift set up

Signing up for OpenShift was easy. Fill out the form, provide an email address - though they don't like email addresses with `+` signs, which is disappointing - and then click the link they email back to you. 

Next, the `rhc` OpenShift client tools are needed. This is a Ruby package. I have no experience with Ruby, so I needed to install Ruby as well. I ran into a problem almost immediately. The [page][7] for installing these tools says:

> OpenShift rhc can be run on any operating system with Ruby 1.8.7 or higher assuming you have the requisite user permissions to install programs. Instructions for specific operating systems are provided below. 

Based on that, I figured I'd install the latest version of [Ruby][8]. At the time I tested this, that was 2.2.3. Unfortunately, when I ran the command to install the `rhc` tools, I received an error. After a bit of Googling, I found that it doesn't like 2.2x. So, I installed 2.1.7 instead. Next, I ran:

    gem install rhc
	
This installs several gems and took a few minutes to complete. Next,

    rhc setup
	
This started the OpenShift setup wizard. It consisted of filling out the few prompts and letting it generate an SSH key and then connecting to my account. Remember the Namespace you select. Again, this took a few minutes.

## Flask setup

The next step was to set up my first "Gear". This would be the Flask application. I'll work on the database next. First, I just want Python and Flask to function properly. Fortunately, this is very easy, as OpenShift has a Flask template.

    rhc app create testapp python-2.7 --from-code=https://github.com/openshift-quickstart/flask-base.git
	
A few notes:

 - I am utilizing Python 2.7, because that is the recommendation from the Flask team.
 - `testapp` can be any alphanumeric string. This is the name that will appear in the Web Console. A specific note, `_` is not alphanumeric. I'm getting the feeling that OpenShift doesn't like "special" characters.
 - The `--from-code` parameter will download that repository and use it as the base of your application. 
 
At this point, Flask can be run locally using:

    python app.py
	
The application can be pushed back to OpenShift at this point and there should be a functional page on your OpenShift domain. In your command line, from the directory of your project:

    git add --all
	git commit -m "Adding Flask application"
	git push
	
This will take a moment. At the end, you should see these lines in your command prompt:

	remote: Git Post-Receive Result: success
	remote: Activation status: success
	remote: Deployment completed with status: success 
	
If all three are a success, then you should be able to visit your URL. Your URL is a combination of your selected Namespace and the application name you created.

    http://<namespace>-<testapp>.rhcloud.com/
	
This should show a "Welcome to Flask on OpenShift" page. If you append `/test` to your URL, you'll get a message that says "It's Alive!"

If it doesn't, you can check your error logs by running:

    rhc tail -a <testapp>

## MySQL setup

Next, I set up my application to utilize MySQL. It's the database I have the most experience with, so I decided to keep that aspect of this project simple for myself. The first step was to add a MySQL 5.5 cartridge to my test gear (OpenShift terminology). I did this in the OpenShift web console. The UI provided me with the option to install various databases and one of those was MySQL. Clicking the link caused a few second delay as it was set up, and then I was presented with login credentials to my database. Step one...done.

The next step is installing the correct Python modules to utilize MySQL. I selected [PyMySQL][9] (again, experience) and [SQLAlchemy][10]. I added these to both `requirements.txt` and `setup.py`. The idea behind doing it in both places is to make life easy for myself in the future. Additionally, the quick tutorials I've looked at for TravisCI encourage the usage of `requirements.txt`, while it seems OpenShift uses the `setup.py`. I'll fix that eventually, but getting it set up initially, this will be fastest.

Add these to `requirements.txt`

    sqlalchemy==1.0.9
    pymysql==0.6.7
	Flask-SQLAlchemy==2.1
	
Add this to the `install_requires` list in `setup.py`

    'sqlalchemy==1.0.9','pymysql==0.6.7','Flask-SQLAlchemy==2.1'
	
The nice thing about OpenShift is that the credentials to the database are placed in [environment variables][11], so I don't need to embed the passwords, connections strings, or anything potentially sensitive in my code. For MySQL these are available as:

	Variable Name				|	Purpose
	------------------------------------------------
	OPENSHIFT_MYSQL_DB_HOST		|	The host name or IP address used to connect to the database.
	OPENSHIFT_MYSQL_DB_PORT		|	The port the database server is listening on.
	OPENSHIFT_MYSQL_DB_USERNAME	|	The database administrative user name.
	OPENSHIFT_MYSQL_DB_PASSWORD	|	The database administrative user’s password.
	OPENSHIFT_MYSQL_DB_SOCKET	|	An AF socket for connecting to the database (for non-scaled apps only).
	OPENSHIFT_MYSQL_DB_URL		|	Database connection URL.
	
### Utilizing the database

Setting up SQLAlchemy and MySQL is fairly easy. I tested this with a simple table and ensured that it appeared in the database as expected.

	class User(db.Model):
		__tablename__ = 'users'
		id = db.Column('user_id', db.Integer, primary_key=True)
		name = db.Column(db.String(60))	
		
A few adjustments were made to the import statements of the Flask application:

	from flask_sqlalchemy import SQLAlchemy
	
A couple variables were created and loaded:

	app.config.from_pyfile('flaskapp.cfg')
	db = SQLAlchemy(app)
	
Finally, the `flaskapp.cfg` file was modified to include these two lines:

	SQLALCHEMY_DATABASE_URI = os.environ['OPENSHIFT_MYSQL_DB_URL'] + os.environ['OPENSHIFT_APP_NAME]
	SQLALCHEMY_ECHO = False
	
### Remote MySQL Access

I like to use [MySQL Workbench][12] while building and testing to watch what's happening in the database. To use that with OpenShift, I had to jump through a few small hoops. 

 - Open MySQL Workbench and create a new connection
 - Give the connection a name
 - In "Connection Method", select "Standard TCP/IP over SSH"
 - The SSH Hostname is the full name of your OpenShift gear where MySQL is installed. It should look like `namespace-appname.rhcloud.com`
 - The SSH Username is the gear's Unique Identifier. This can be found by looking at the `OPENSHIFT_GEAR_UUID` environment variable. It can also be found in the web console, but looking at the "remote access" section. It shows a connection string. You need the username portion. This is the part that appears before the `@` in the `ssh longuniquestring@namespace-appname.rhcloud.com` command. 
 - Set the SSH key file. On Windows this is in `\Users\<username>\.ssh\id_rsa` by default
 - Set MySQL Hostname equal to the value in `OPENSHIFT_MYSQL_DB_HOST`
 - Set Username equal to the value in `OPENSHIFT_MYSQL_DB_USERNAME` (this was also provided to you when you installed MySQL)
 - See Password equal to the value in `OPENSHIFT_MYSQL_DB_PASSWORD` (again, this was provided to you when MySQL was installed)
 

## TravisCI setup

I want to play with automated testing. The idea behind this is to get a jump start on a goal for next year at work and to learn something new. I'd like to utilize Travis CI to perform the tests and if they pass, deploy to OpenShift. If the tests fail, I don't want to push a broken build to OpenShift. That's the goal...we'll see how it turns out. But, the first step is getting Travis CI and OpenShift talking to one another.

Travis CI integrates with GitHub, so what I'm going to do in reality is push to GitHub and let Travis CI pick up the changes. From there, it will perform it's tests. If the tests pass, it will push the commit to OpenShift.

On GitHub, create a new repository for your source. This is where you will be pushing your code for Travis CI to pick up.

From your OpenShift directory (which is already a git repository):

    git remote rename origin openshift
	git remote add origin https://github.com/<USER>/<repositoryname>.git
	git push -u origin master
	
This resets the origin to point to your new GitHub repository and sets up a new remote. Then it pushes the changes to GitHub.

Log into your [Travis CI profile page][13]. Make sure you are logged into GitHub first, as this will create the profile automatically. Press the "Sync now" button at the top of the page to pull a list of all of your repositories. Once that is done, find the repository you just set up, and enable integration with that repository.

Next you need to set up Travis CI and the `.travis.yml` file. 

    gem install travis
	
This will install a Ruby script that assists in this process. If you get an error when running this, you need to create an empty `.travis.yml` file first and then run the command again.

    travis setup openshift
	
Fill out the prompts. Defaults should be fine in most cases, but do pay attention to "OpenShift application name". If your GitHub repository is named differently than your OpenShift application name, the default for this prompt will be incorrect.

One last note, for a quick test you can change the `script` section to 

    script: true
	
This forces the tests to pass. Once you've written tests, you can do something like:

    script:
	    - py.test

This will run your test scripts, utilizing `py.test`
		
Deploy these changes to GitHub:

	git add .travis.yml
	git commit -m "Deploying Travis"
	git push origin master
			
This will commit and push the changes to GitHub. A few seconds later, if you are watching Travis CI, you'll see it notices the new commit and starts running tests. If you tests complete with a status code of `0` (successful), it will deploy the changes to OpenShift. If the tests fail (any other status code), it will not deploy to OpenShift.

## py.test setup

Setting up the testing frame work involves a few Python modules. These need to be added to both `requirements.txt` and `setup.py`.

    pytest>=2.8.0
	hypothesis==1.16.0
	pytest-runner==2.6.2

The next step is setting up some quick integration with `setup.py`, so that users can run `python setup.py test` and execute your tests.

In `setup.py`, add (or edit) the `setup_requires` list to include `pytest-runner`. Add (or edit) the `tests_require` list to include `pytest`. I also added the following to my `setup.cfg`

	[aliases]
	test=pytest

I modified my `setup_requires` list a bit, so that it's conditional. Since this would install the `pytest-runner` on every call to `setup.py`, even when the module wouldn't be called, I wanted the runner to only be required when `pytest` is utilized.

    import sys
	needs_pytest = {'pytest', 'test', 'ptr'}.intersection(sys.argv)
	pytest_runner = ['pytest-runner'] if needs_pytest else []

	setup(
		setup_requires=[
			#... Other requirements here
		] + pytest_runner,
	)
	
I wanted to test that my tests were working correctly. I created a `tests` directory, which is where I plan on storing all of my test cases. `pytest` will find any files that start or end with `test` and execute them. I created a very simple `test_tests.py` file with the following simple test (taken from the [Hypothesis Quickstart][16])

	@given(st.integers(), st.integers())
	def test_ints_are_commutative(x, y):
		assert x + y == y + x
		
Finally, Travis CI needs to be told what to do. Modify the `script` key to include `py.test`

	script:
	   - py.test

After a successful run through Travis, you'll see something like this:

    tests/test_tests.py .
	=========================== 1 passed in 0.26 seconds ===========================
	The command "py.test" exited with 0.
	
## Custom Domain

I have my own domain name. I want to utilize OpenShift with one of those domains, instead of the default one provided. Since I've using the free tier, that will rule out using the SSL certificate that is wildcarded to the whole `rhcloud.com` domain. I can live with this. If I need SSL on my domain, I'll upgrade.

To set up OpenShift to use your domain, log into the web console. Go to the gear you are configuring. At the top, where the full domain is displayed, is the option to "Change". Select that option. Input the full domain (and subdomain) you want to utilize and click "Save". After a few seconds, you'll get a notification that the alias was created.

The next step is to configure the DNS records. I [utilize][17] CloudFlare for my domains, so the instructs will be specific to that, but should apply to any DNS system. Login to your management system and go to the area where you can specify DNS records.

For my test, I set up a subdomain of one of my domains as the alias I wanted to use. In your DNS system, set up a CNAME that points to the original hostname on OpenShift. The CNAME should be the subdomain you told OpenShift about. Save the record. 

CloudFlare recognized this immediately and redirected me to my Flask application. Hooray!
	
## Conclusion

With this, the set up is complete. You have a Flask application, connected to MySQL, that is integrated with a CI system which automatically deploys to OpenShift when all tests pass and uses CloudFlare (because I already was doing so), to provide a CDN. 

On to building something!


 [1]: {filename}2015_01_08_thanks-for-all-the-fish.md
 [2]: http://flask.pocoo.org/
 [3]: https://www.openshift.com/
 [4]: https://travis-ci.org/
 [5]: {filename}2015_01_02_can-a-machine-be-taught-to-flag-comments-automatically.md
 [6]: http://www.paasify.it/compare/heroku-vs-openshift%20online
 [7]: https://developers.openshift.com/en/managing-client-tools.html
 [8]: http://rubyinstaller.org/downloads/
 [9]: http://www.pymysql.org/
 [10]: http://www.sqlalchemy.org/
 [11]: https://developers.openshift.com/en/managing-environment-variables.html#database-variables
 [12]: https://www.mysql.com/products/workbench/
 [13]: https://travis-ci.org/profile
 [14]: http://pytest.org/latest/
 [15]: https://hypothesis.readthedocs.org/en/latest/
 [16]: https://hypothesis.readthedocs.org/en/latest/quickstart.html#writing-tests
 [17]: {filename}2015_07_09_how-i-set-up-this-site-with-github-pages-and-cloudflare.md