Title: How I set up this site with GitHub Pages and CloudFlare
Date: 2015-7-9 11:26
Tags: Meta, technical
Category: Side Activities
Slug: how-i-set-up-this-site-with-github-pages-and-cloudflare
Summary: This post provides a brief description of how I set up the web site to utilize GitHub Pages and CloudFlare and eliminated my self hosting
Status: published

[TOC]

## Introduction

In a [previous post][1], I described why I moved from Wordpress to Pelican for my blog. This one goes a step further and describes how I eliminated the 
need for the dedicated server I'd been utilizing as a part of [Team Vipers][s]. By eliminating that server, I reduced my costs to zero but kept control 
over the DNS of my domain (thanks to [CloudFlare][2]) and had an easier method of updating the site using [GitHub Pages][3].

## GitHub Pages Setup

To utilize GitHub Pages, I needed to create a new [repository][4] that followed the format `GitHubUsername.github.io`. This repository would house the 
content that is this site. I also set up a second [repository][5] which contains the source for the blog. This repository includes the templates, plugins
and markdown version of the pages. The first repository was set up as submodule.

    git submodule add https://github.com/AWegnerGitHub/awegnergithub.github.io.git output

I ignored the `output` directory in `.gitignore` on the source repository. Finally, I had to adjust `publishconf.py` slightly to  

    DELETE_OUTPUT_DIRECTORY = False
	
Without this, I was constantly destroying the output repository and had to reinitialize it. This prevents that from occuring. 

Now, a new post consists of writing up the [Markdown page][6], generating the page with the command below (or the [batch script][7]) and then committing and
pushing the changes to the submodule to GitHub.

    # Generates HTML files without debugging information
    pelican content --output output --settings publishconf.py
	
The new content is available immediately.

### Custom Domain 

You may notice that the URL for this site isn't `awegnergithub.github.io`, but instead `andrewwegner.com`. To accomplish this, I added a directory to the `content`
named `extra`. In this directory is a single file named [`CNAME`][8] (no extension). In the file is my domain name. 

Next, I had to modify [`pelicanconf.py`][9] to add the `extra/CNAME` to the static path and then on generation move the `CNAME` file from this subdirectory to the root.
I could have put it in the root of `content` by default, but Pelican provides a way to do this and it keeps `content` clean. **One very important note**, the `EXTRA_PATH_METADATA` is
operating system sensitive. Since I am generating the content on a Windows machine, I had to use a backslash instead of the forward slash the documentation shows. I found this
after posing a [question][10] on Stack Overflow on why it wasn't working as the documentation suggested.

The two important fields to add or edit are:

    ...
    STATIC_PATHS = ['images', 'extra/CNAME']
	...
	EXTRA_PATH_METADATA = {'extra\CNAME': {'path': 'CNAME'},}
	
## Cloudflare Setup

The final thing I needed in order to get rid of my server was control over DNS. I could revert back to GoDaddy, but after a little research found that CloudFlare's additional CDN and 
security was a "good thing" (because, you know, I'm such a highly traffic'd blog these days). Step one was signing up to CloudFlare. This was a 3-5 minute thing. 

Once signed up and signed in, I went to set up DNS. This was as simple as adding my domain name and waiting for CloudFlare to import my existing DNS records. With this, I kept by Google Apps
email intact (which is what I was most concerned with). Next, I went and removed the `A` records. I replaced these with `CNAME` records pointing to my GitHub Pages URL. I also added a `www` CNAME 
pointing to the same location. Since I have Pelican configured to strip it with the setting below, it doesn't matter other than people expect to enter `www dot domain dot com` in their URL bar.

    SITEURL = 'http://andrewwegner.com'
	
Last, I had to point by name servers to CloudFlare instead of my dedicated server. They provide a list of registratars to choose from. Select your registrar and follow the instructions. My biggest
issue here was remembering my GoDaddy password. After I made it into my account, the steps to change name servers were very simple. Once those are saved, you wait for the changes to propagate and
enjoy your new GitHub Pages / CloudFlare web page for free. 



 [1]: {filename}2015_05_03_why-i-moved-from-wordpress-to-pelican.md
 [2]: https://www.cloudflare.com/
 [3]: https://pages.github.com/
 [4]: https://github.com/AWegnerGitHub/awegnergithub.github.io
 [5]: https://github.com/AWegnerGitHub/awegnergithub.github.io-source
 [6]: https://raw.githubusercontent.com/AWegnerGitHub/awegnergithub.github.io-source/master/content/2015_07_09_how-i-set-up-this-site-with-github-pages-and-cloudflare.md
 [7]: https://github.com/AWegnerGitHub/awegnergithub.github.io-source/blob/master/generate_content_production.bat
 [8]: https://github.com/AWegnerGitHub/awegnergithub.github.io-source/blob/master/content/extra/CNAME
 [9]: https://github.com/AWegnerGitHub/awegnergithub.github.io-source/blob/master/pelicanconf.py
 [10]: http://stackoverflow.com/a/30512242/189134
 [s]: {filename}2015_01_08_thanks-for-all-the-fish.md