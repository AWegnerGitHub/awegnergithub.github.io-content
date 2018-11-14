Title: Setting up GitLab on the new server
Date: 2018-04-13 08:30
Tags: technical
Category: Technical Solutions
Slug: installing-gitlab
Summary: Let's set up some private repositories on GitLab
Status: published
Series: Recovering from data loss

[TOC]

## Introduction

Back when I ran Vipers, my fellow admins and I hosted a small set of code repositories -
SVN, Mercurial and Git - to host some of our custom code. We ran [RhodeCode][rhode] and
the fork, [Kallithea][kallithea], when RhodeCode close sourced some of it's code and
couldn't figure out if the license it used actually allowed themselves to do that. A private
repository was awesome for plugins, server configurations and personal projects.

When the community was shuttered, some of the [plugin code was migrated to GitHub][plugins] and it's
sat there untouched since. My personal projects were either migrated to GitHub or
simply stored outside of version control if it couldn't go in a public repository. That was
less than ideal, but it worked. With the new home server set up, I wanted to get source control set
back up for my non-public personal projects.

I rejected RhodeCode right away due to the experiences I had when they changed licenses. Turns out,
they had done it again in the meantime. I didn't want to deal with that. I attempted to install
Kallithea using their [instructions][1], but I kept running into Python syntax errors. It wasn't
worth the time and effort to figure out the problem.

So, I turned to [GitLab][2]. It'd definitely overkill for what I really need, but it works and
if I ever truly decide to get fancy, I have a lot of other tools I can use. The [core][3] functionality
is what I'll be using and is free. The three other versions cost some money and contain features that
would be useful for large team, not a single developer or very small team.

## Installation

### Dependencies 

Installing GitLab is pretty simple. There are a couple dependencies needed, but I already had both OpenSSH 
and Postfix installed, so I was able to skip the first step in the [official installation guide][4]. I installed
the Ubuntu Omnibus package.

    sudo apt-get install -y curl openssh-server ca-certificates postfix
	
### Getting the package

The GitLab repository needs to added and then installed. To add the repository, issue this command:

    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
	
To install the GitLab package, you need to provide an environment variable when you issue your
`apt-get install` command. This will be the URL where you want to access your GitLab installation.

    sudo EXTERNAL_URL="http://gitlab.example.com" apt-get install gitlab-ee
	
### Complete the installation

Once the install, above is complete, you need to log in to complete the process. In your browser,
navigate to the URL you provided above. Set/reset the password as prompted and then login. 

## Post-install Tweaks

### Using Apache instead of Nginx

The omnibus package comes with Nginx bundled. Unfortunately, I don't have any experience managing
an Nginx instance but do have experience with Apache. I want to use something that I know to make
my life easier. Fortunately, GitLab can handle this with a few [minor changes to the configuration][5]. 

In the `/etc/gitlab/gitlab.rb` file you'll need to make several settings changes. You also need Apache 
already installed and the `www-data` user (on Ubuntu) added to the `gitlab-www` group.

 * Find `nginx['enable']` and set it to `false`
 * In `web_server['external_users'], add `www-data` to the array. Note that this is an array and not a single string.
 * In `gitlab_rails['trusted_proxies'], add the IP address of the Apache web server. 
 * Change the gitlab workhorse settings to the following (default) values. 
 
These may already be in the configuration file. If so, you probably don't need to modify them.

	gitlab_workhorse['listen_network'] = "tcp"
	gitlab_workhorse['listen_addr'] = "127.0.0.1:8181"
	
Finally, run `sudo gitlab-ctl reconfigure` for the settings to take effect.

Now, you need to configure Apache's virtual host. GitLab provides [example virtual hosts][5]. Since I installed
the omnibus package and am using Apache 2.4, I selected the [`gitlab-omnibus-apache24.conf`][6] file. Adjust all
instances of `YOUR_SERVER_FQDN` to the fully qualified domain name of your server.

This will go in `/etc/apache2/sites-available/` and a symlink in `/etc/apache2/sites-enabled/` will point to this file.

    sudo touch /etc/apache2/sites-available/gitlab.conf
	sudo ln -s /etc/apache2/sites-available/gitlab.conf /etc/apache2/sites-enabled/gitlab.conf 

### Use SSL to access GitLab

The example virtual host provided by GitLab uses HTTP only. I want to set up my instance to use HTTPS. I'll be 
doing this with [Let's Encrypt][7], like I did when I set up NextCloud in the previous post. I cover the exact 
[steps for Let's Encrypt][ssl] in another post. The keys referenced in the virtual host configuration file below created 
by that process. 

The first change to make is to redirect the HTTP version of your domain to HTTPS. The goal is that all traffic to
GitLab will go over SSL. Adjust the `ServerName` variable as appropriate.

	<VirtualHost *:80>
	  ServerName gitlab.example.com
	  ServerSignature Off

	  RewriteEngine on
	  RewriteCond %{HTTPS} !=on
	  RewriteRule .* https://%{SERVER_NAME}%{REQUEST_URI} [NE,R,L]
	</VirtualHost>

Then, everything in the [sample][6] virtual host file can be put in the `<VirtualHost *:443>` block.

At the top of this block, we need to reference the Let's Encrypt keys:

	SSLProtocol all -SSLv2
	SSLHonorCipherOrder on
	SSLCipherSuite "ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS"
	Header add Strict-Transport-Security: "max-age=15768000;includeSubdomains"
	SSLCompression Off
	SSLCertificateFile /path/to/dehydrated/certs/gitlab.example.com/cert.pem
	SSLCertificateKeyFile /path/to/dehydrated/certs/gitlab.example.com/privkey.pem
	SSLCertificateChainFile /path/to/dehydrated/certs/gitlab.example.com/chain.pem
	
Save and restart Apache. You should be automatically redirected over HTTPS when you visit your GitLab URL.

### Allow spaces in repository names

One of the only problems I ran into with GitLab is that, by default, repositories with spaces in them can't be viewed
in the web browser. It throws a `400 Bad Request` when trying to view the directory. There is a [bug report][9] 
regarding this problem. The developers are working on updating the samples in a way that is guaranteed to work through
the whole system. 

For me, though, the first comment which suggests a minor `RewireRule` change works great. In the virtual host, fine the line

    RewriteRule .* http://127.0.0.1:8181%{REQUEST_URI} [P,QSA,NE]
	
and remove the `NE` so that it reads

    RewriteRule .* http://127.0.0.1:8181%{REQUEST_URI} [P,QSA]

Restart Apache and you can navigate to the directory with a space.

### Setting up SMTP

GitLab can send out emails and requires the ability to do so when resetting a password, at minimum. I don't want this
email to be marked as spam, so I used one of the free providers from [here][10] and set up an account. After editing the 
`/etc/gitlab/gitlab.rb` file to match the provider I selected, I ran `gitlab-ctl reconfigure`. Now any emails GitLab
sends out goes through the trusted email provider instead of coming directly from my residential IP address. This means 
my mail provider trusts it. I also send out less than 5 emails a month currently, so I am *well* below the tier where I
lose my "free" status.

## Conclusion

At this point, GitLab is set up over SSL on my server. I can log in and start setting up repositories. Migrating and importing 
the code bases I didn't want to put on a public GitHub account was very satisfying. Maybe I'll look into some of the 
more advanced features GitLab offers in the near future, but for the time being I'm happy with what I have and the 
knowledge that I can expand what I do with GitLab.


 [rhode]: https://rhodecode.com/
 [kallithea]: https://kallithea-scm.org/
 [plugins]: https://github.com/AWegnerGitHub/Vipers-Server-Plugins
 [1]: http://kallithea.readthedocs.io/en/stable/installation.html
 [2]: https://about.gitlab.com/
 [3]: https://about.gitlab.com/pricing/self-hosted/feature-comparison/
 [4]: https://about.gitlab.com/installation/#ubuntu
 [5]: https://gitlab.com/gitlab-org/gitlab-recipes/tree/master/web-server/apache
 [6]: https://gitlab.com/gitlab-org/gitlab-recipes/blob/master/web-server/apache/gitlab-omnibus-apache24.conf
 [7]: https://letsencrypt.org/
 [8]: {filename}2018_03_27_installing_nextcloud.md
 [9]: https://gitlab.com/gitlab-org/gitlab-ce/issues/32585
 [10]: https://docs.gitlab.com/omnibus/settings/smtp.html#smtp-settings
 [ssl]: {filename}2018_04_25_setup_cloudflare_letsencrypt.md