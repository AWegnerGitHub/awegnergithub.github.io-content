Title: Run Sanic on Heroku
Date: 2022-05-19 08:00
Tags: technical
Category: Technical Solutions
Slug: run-sanic-on-heroku
Summary: A quick guide on how to launch a small Sanic application on Heroku
Status: published

[TOC]

## Introduction

Recently I needed to set up a very simple proof of concept (POC) to demonstrate to myself and eventually my team that an idea would work. A POC isn't production ready, it doesn't need to be robust and in my case wasn't even going to be written using the same framework we use in production. I just needed to see if the integration would work as I thought it would. If it did, I'd work with my team to build a production ready version. If it didn't work, no problem. I expected this POC to take about two hours of my time, including initial research around the integration itself.

My background is utilizing Python. I've been using it for nearly 15 years now. In my previous role I utilized [Sanic][sanic] to build our API. I've also used Django and Flask in the path, but it had been five or more years so I was rusty in those. Plus, I have a fancy new book written by Adam Hopkins, the core developer of Sanic and an amazing former colleague, called [Python Web Development with Sanic][book] that I wanted to work through. I've been meaning to get to this, and haven't had a lot of time with my recent job change. This small POC won't get far, but I've found that if I start following a long tutorial or course or book, I'll dedicate more time so that I can finish it.

By the way, highly recommend the book if you are utilizing Sanic.

Onward!

## Problem

The problem I encounted during this POC wasn't the integration, like I expected. Instead, the problem I ran into was figuring out where I could deploy this POC at no cost. I considered [ngrok][ngrok], but that meant leaving something running on my machine until my team had a chance to play with the POC. Instead, I turned to [Heroku][heroku]. The next problem I ran into was determining how to deploy a Sanic application to Heroku. Google returned 5 year old articles, forum posts that don't have answers ([denvercoder9][1], is that you?), and Flask tutorials.

## Toy Sanic App

Before I get to the solution, here is a toy Sanic app that simply prints `Ok!` when you hit the root endpoint. Save this in a file called `server.py`


    from sanic import Sanic
    from sanic.response import text
    import os

    app = Sanic(__name__)

    @app.get("/")
    async def root_path(request):
        return text("Ok!")

    app.run(host="0.0.0.0", port=os.environ["PORT"], debug=True, access_log=False)


The one important thing to notice is that `port` parameter in `app.run()`. Heroku supplies the port it is listening on on an environment variable, and this is needed for Sanic.

## The quick solution

In Adam's book, in the "Organizing a Project" chapter, he shows that running an application uses a command like:

    sanic src.server:app -p 7777 --debug --workers=2

Simplifying that command a bit and setting up the Heroku `Procfile` (capitization matters and no extension) I have a single line in my `Procfile` that reads:

    web: sanic server:app

Creating a `requirements.txt` file with Sanic as an item was also required. If you're using a virtualenv, you can simply `pip freeze > requirements.txt` to get everything in your environment into the file.

Thus, my directory looks like this:

    ./poc
        Procfile
        requirements.txt
        server.py

Push this to Heroku and the application fires right up.

## More details

I did not have a personal Heroku account or the CLI installed on my machine before starting. That was easily resolved by creating an account at [Heroku][heroku]. For the `heroku-cli`, I followed the [documentation][2] to get that set up. Namely:

    curl https://cli-assets.heroku.com/install.sh | sh

**Note**: This does require `sudo`.

Once that's done, I verified the installation:

    $ heroku --version
    heroku/7.60.2 linux-x64 node-v14.19.0

Set up the new application in your Heroku account by logging in. Then click `New` then `Create new app`.

![Create new heroku app - New / Create new app][newapp]

Enter a unique application name.

Then add Python to the build path. Do this by going to `Settings` -> `Add buildpack` -> Selecting Python and clicking save.

![Add Python buildpack - Settings / Add buildpack / Python][buildpack]

The next step is setting up the two files I mentioned earlier - `Procfile` and `requirements.txt`. Since the `Procfile` is a single line for this simple proof of concept, that can been done in a single line from the shell.

    $ echo "web: sanic server:app" > Procfile

Assuming you've been using a virtualenv - because you *should* be using a virtualenv - the `requirements.txt` file is just as easy.

    $ pip freeze > requirements.txt

Everything is setup and ready to be loaded to Heroku. Next, login in your terminal. I used the browser based method, but you can do this entirely in the command line if you wish. My command was:

    heroku login

Last, I set up a heroku git remote endpoint so that I could push

    heroku git:remote -a NAME_OF_YOUR_HEROKU_APP
    git add .
    git commit -m "Deployment commit"
    git push heroku master

Change `NAME_OF_YOUR_HEROKU_APP` to match the name you input in the UI when setting it up.

Wait a minute or so, and then navigate to `NAME_OF_YOUR_HEROKU_APP.herokuapp.com` and see `Ok!`

## Finishing up

With the steps above, I got a very simple Sanic proof of concept running on Heroku in a matter of minutes. This was very helpful as I was able to easily show my team that the work we're thinking of doing can be accomplished. All told, my proof of concept took two hours of time. The next step will be building a production version.


 [sanic]: https://sanic.dev/en/
 [book]: https://www.packtpub.com/product/python-web-development-with-sanic/9781801814416
 [ngrok]: https://ngrok.com/
 [heroku]: https://www.heroku.com/
 [1]: https://xkcd.com/979/
 [2]: https://devcenter.heroku.com/articles/heroku-cli#standalone-installation-with-a-tarball
 [newapp]: {attach}images/sanic-heroku-new-app.png
 [buildpack]: {attach}images/sanic-heroku-buildpack.png