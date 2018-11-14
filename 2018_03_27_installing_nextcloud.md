Title: Installing NextCloud
Date: 2018-03-27 23:30
Tags: technical
Category: Technical Solutions
Slug: installing-nextcloud
Summary: The ZFS pool is set up. It's time to use all that storage space and install NextCloud.
Status: published
Series: Recovering from data loss

[TOC]

## Introduction

In the last post, I described how I [set up ZFS on the new server][1]. With a newly configured operating system and tons of space, it's time to start using it. One of the goals
I [mentioned][2] when I set up this server was the ability to: 

> Back up data from all devices in the house automatically. As camera phones have gotten better, we've found that we carry our bulky digital camera less and less. The problem
 with the phone camera is that we need to get the pictures to the computer. I don't want to hunt down a data cable or email the pictures to myself. I'm also not a fan of 
 posting everything to social media. I want my phone to send the pictures to a backup location automatically.
 
I'm going to accomplish that by hosting an instance of [NextCloud][3] on this new server. Fortunately, the install process is pretty simple for this one. NextCloud provides 
[installation instructions][4]. When I installed it in mid-February 2018, it was on version 12.x. As of this post, in late March 2018, it's on version 13.x. I'll cover install
and upgrade processes in this post.

## Installation

### Prerequisites 

For NextCloud you'll need either MySQL or MariaDB. I host it via Apache2, so we'll have that installed too. NextCloud is written in PHP, meaning we need that too.

    sudo apt-get install apache2 mariadb-server php7.0 libapache2-mod-php7.0 php7.0-mbstring php7.0-curl php7.0-zip php7.0-gd php7.0-mysql php7.0-mcrypt php7.0-bcmath php7.0-xml php7.0-json php7.0-tidy
	
Enable the Apache2 rewrite module and restart the web server.

    sudo a2enmod rewrite
	sudo service apache2 restart
	
### Set up the database

You'll need to create a database for NextCloud. Log into your database using credentials that can create new users and databases. `root` will work.

    mysql -uroot -p
	
Next, execute a couple SQL statements to create a database and create a user that can access the database. Make sure you use a secure password.

    CREATE DATABASE nextcloud;
	GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost' IDENTIFIED BY 'YOURSECUREPASSWORDHERE';
	FLUSH PRIVILEGES;
	\q
	
### Download NextCloud

As I mentioned above, I initially installed version 12 of NextCloud. The latest version can be found on the [NextCloud install page][4]. The URL from that page should be
used instead of the version 12 link in the following code block. The code block below will be putting NextCloud in the default location Ubuntu has Apache look. You can modify
that as needed. If you do so, the virtual host will need to be modified slightly.

    sudo cd /tmp && wget wget https://download.nextcloud.com/server/releases/nextcloud-12.0.2.zip
	sudo unzip nextcloud-12.0.2.zip
	sudo mv nextcloud/ /var/www/html
	
We need to adjust ownership of the files so that Apache can read them. The default user and group, in this case is `www-data`. If you have configured your server to use a 
different user or group, adjust this command accordingly.

    sudo chown www-data:www-data -R /var/www/html/nextcloud
	
### Create the Virtual Host

I'll be exposing this to the internet and I'll be accessing it via the internet. That means I really don't want to send data unencrypted to or from NextCloud. I'll be setting
up the standard port 80 web server traffic to redirect to the secure port of 443. I cover [generating SSL certificates][ssl] in another post. I use [Let's Encrypt][5]. The keys 
referenced in the virtual host configuration file below created by that process.

Create a new virtual host.

    sudo touch /etc/apache2/sites-available/nextcloud.conf
	sudo ln -s /etc/apache2/sites-available/nextcloud.conf /etc/apache2/sites-enabled/nextcloud.conf 
	
Now you need to edit this newly created file 

	sudo nano /etc/apache2/sites-available/nextcloud.conf 
	
Paste the following:

	<VirtualHost *:80>
		ServerAdmin YOUR@EMAILADDRESS
		DocumentRoot /var/www/html/nextcloud/
		ServerName nas.example.com
		Redirect permanent / https://nas.example.com/

		<Directory /var/www/html/nextcloud/>
			Options FollowSymLinks
			AllowOverride All
			Order allow,deny
			allow from all
		</Directory>

		ErrorLog /var/log/apache2/nas.example.com-error_log
		CustomLog /var/log/apache2/nas.example.com-access_log common
	</VirtualHost>

	<VirtualHost *:443>
		ServerName nas.example.com
		DocumentRoot /var/www/html/nextcloud/
		RewriteCond %{THE_REQUEST} ^.*/index\.php
		RewriteRule ^(.*)index.php$ /$1 [R=301,L]
		SSLEngine on
		SSLCertificateFile /path/to/dehydrated/certs/nas.example.com/cert.pem
		SSLCertificateKeyFile /path/to/dehydrated/certs/nas.example.com/privkey.pem
		SSLCertificateChainFile /path/to/dehydrated/certs/nas.example.com/chain.pem
		<IfModule mod_headers.c>
			Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains"
		</IfModule>

		<Directory /var/www/html/nextcloud/>
			Options FollowSymLinks
			AllowOverride All
			Order allow,deny
			allow from all
		</Directory>

		ErrorLog /var/log/apache2/nas.example.com-error_log
		CustomLog /var/log/apache2/nas.example.com-access_log common
	 </VirtualHost>
	 
There are two separate virtual host configurations being created here. The first one, on port 80, is setting up the permanent redirect to the HTTPS site. 

In the secure virtual host configuration, we're setting a small rewrite rule to provide nicer URLs and configuring the SSL certificates to use. The `DocumentRoot` variables
should match the path you installed NextCloud into in the previous step.

### Application Configuration

There are a few settings that you need to change in the NextCloud configuration. Do this by editing `/var/www/html/nextcloud/config/config.php`. If this file doesn't exist, 
you need to copy `/var/www/html/nextcloud/config/config.sample.php` to `/var/www/html/nextcloud/config/config.php`.

The important settings to check are:

    - `datadirectory`: In my case, this was pointed at a dataset I created when I [set up my ZFS pool][1]
	- `overwrite.cli.url`: Changed to point to the HTTPS version of the URL I want to use

### Complete the installation

Restart Apache and the navigate to the domain you've set up for your NextCloud installation. I am assuming that you know how to set up a DNS record for the server name
you specified in your virtual host configuration.

Once you've reached the domain in your web browser, follow the instructions on screen. You'll need the database username and password you created above. You'll also create an
administration user. 

### Upgrading

After some time, NextCloud will update. You should apply these updates, as they'll include new features and security patches. Log into NextCloud using your administration user.
Click on the Gear icon in the upper right and pick "Settings". On the left hand side, select "Basic settings". Half way down the page you'll see the version you are currently
running and whether or not there is an update available. If there is, you can begin the update from here.

NextCloud does not support skipping versions when updating. This means if you are on version 12, you can upgrade to version 13. You can not, however, upgrade directly from 12 to 14. 

## Syncing data

NextCloud provides client applications that allow you to automatically sync data to your install. There are clients for both computers and mobile devices. My use case only
requires the mobile clients right now, but that may change in the future. From the [install page][4], you can find the clients for Android, iOS and Windows devices. Select
the appropriate installer on your device.

Once the mobile client is installed, you need to provide the URL to your installation and a username and password that can access your information. I've enabled automatic
uploads of new pictures from my devices only when I'm on a wireless connection (no sense wasting mobile data). This, however, is why I wanted the SSL certificates. The client
doesn't let me whitelist uploading from specific networks. I'd prefer I don't send my pictures unencrypted.

## Results

I've been using NextCloud for almost three months so far. I love it. Previously, I'd have to find a data cable and remember to manually backup my pictures once and a while. Now,
it "just happens". If I take a picture at home, it's backed up within seconds. If I take a bunch of pictures while I'm out of the house, my pictures are backed up within 
minutes of me getting home. 
	

	
 [1]: {filename}2018_02_15_setting_up_zfs_on_ubuntu.md
 [2]: {filename}2018_02_12_a_new_server_for_the_house.md
 [3]: https://nextcloud.com/
 [4]: https://nextcloud.com/install/
 [5]: https://letsencrypt.org/
 [ssl]: {filename}2018_04_25_setup_cloudflare_letsencrypt.md