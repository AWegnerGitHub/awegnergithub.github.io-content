Title: Updating PHP from 7.1 to 7.3 on Ubuntu 18.04
Date: 2019-07-26 10:00
Tags: technical
Category: Technical Solutions
Slug: updating-php-ubuntu-1804
Summary: A brief walkthrough on how to upgrade PHP to 7.3 (and make sure NextCloud still works)
Status: published

[TOC]

## Introduction

The [new server][1] has been up and running for about a year and a half now. It's still
working really well. The thing that I'm happiest with is [my NextCloud install][2]. Having
my pictures automatically backed up from the phones is a huge time saver. I no
longer need to worry about whether or not I grabbed a set of pictures off the
phone or which phone has which picture. It's all in one place in NextCloud. This
makes it easy to find what I'm looking for (and easy to backup).

NextCloud runs on PHP, which means I need to have PHP installed on the server
for it to work. This isn't a huge problem, but the last time I really used PHP,
it was during the transition between PHP 4 and PHP 5. So...a while ago. I set up PHP
(and Apache) to host NextCloud and really just forgot about it.

During a recent update of packages on the server - because I do like to keep
everything updated. I noticed this line during the `apt-get` scroll:

    php7.1 module already enabled, not enabling PHP 7.3

Time to figure out how to use that newly install/upgraded PHP 7.3

## What is actually running?

The first thing I did was check which version of PHP was being used in the terminal:

    $ php -v
    PHP 7.3.7-2+ubuntu18.04.1+deb.sury.org+1 (cli) (built: Jul 25 2019 11:44:59) ( NTS )
    Copyright (c) 1997-2018 The PHP Group
    Zend Engine v3.3.7, Copyright (c) 1998-2018 Zend Technologies
        with Zend OPcache v7.3.7-2+ubuntu18.04.1+deb.sury.org+1, Copyright (c) 1999-2018, by Zend Technologies

That's promising. The default version here is PHP 7.3.7.

But, throwing a quick `phpinfo();` together and looking at what it running via
Apache, shows something different:

    PHP Version 7.1.30-1+ubuntu18.04.1+deb.sury.org+1

Ok. Now I know which module is out of date. It's the run that is configured to
be used with Apache.

## Verifying what's installed

I assumed I had PHP 7.3 installed, but I wanted to confirm before I just started
disabling and enabling Apache modules.

To confirm I had PHP 7.3 available, I ran this:

    $ ls /etc/apache2/mods-available/php*
    /etc/apache2/mods-available/php7.1.conf
    /etc/apache2/mods-available/php7.1.load
    /etc/apache2/mods-available/php7.2.conf
    /etc/apache2/mods-available/php7.2.load
    /etc/apache2/mods-available/php7.3.conf
    /etc/apache2/mods-available/php7.3.load

And a quick check to see what's enabled:

    $ ls /etc/apache2/mods-enabled/php*
    /etc/apache2/mods-enabled/php7.1.conf
    /etc/apache2/mods-enabled/php7.1.load

Excellent. I have PHP 7.3 available, and PHP 7.1 is enabled. This is exactly what
I'm seeing.

## Updating Apache PHP Module

With PHP 7.3 already installed, I just need to disable PHP 7.1 and enable PHP 7.3.

    $ a2dismod php7.1
    $ a2enmod php7.3

Then restart Apache to use the new module.

    $ service apache2 restart

Finally, validate the correct module is enabled:

    $ ls /etc/apache2/mods-enabled/php*
    /etc/apache2/mods-enabled/php7.3.conf
    /etc/apache2/mods-enabled/php7.3.load

Another check of the `phpinfo();` page too:

    PHP Version 7.3.7-2+ubuntu18.04.1+deb.sury.org+1

This matches what `php -v` output.

We're done! Right?

## Checking NextCloud

With PHP updated, it was time to make sure the one PHP application I run still
worked. I visited my [NextCloud URL I set up CloudFlare][3]. There I was greeted
with a blank page. Oddly, I couldn't find any errors in my server logs.

Using the `a2dismod` and `a2enmod` commands from above, I downgraded back to
PHP 7.1. NextCloud worked. I upgraded to PHP 7.2 and it worked. Going back to PHP
7.3, and I was back to a blank page.

Even without server logs, this indicated that either NextCloud doesn't support
PHP 7.3 or I was missing modules. A check of the [system requirements for NextCloud][4]
shows that PHP 7.3 is supported. That just means I'm missing some modules.

The documentation also includes a [list of all needed modules][5] and a nice
[shell script][6] for easy installation. Looking through that, I found the `apt`
[packages I needed][7].

    $ apt-get install php7.3-fpm \
    php7.3-intl \
    php7.3-ldap \
    php7.3-imap \
    php7.3-gd \
    php7.3-pgsql \
    php7.3-curl \
    php7.3-xml \
    php7.3-zip \
    php7.3-mbstring \
    php7.3-soap \
    php7.3-smbclient \
    php7.3-json \
    php7.3-gmp \
    php7.3-bz2

A minute or so later, with those modules now installed, I restarted Apache again.

    $ service apache2 restart

Then I went to my NextCloud URL. The page loaded as expected and my phone
sync'd the one picture I took as a test to ensure it worked.

Overall, this was a really simple process. The biggest issue I ran into was missing
a module or two that NextCloud required. Simply installing everything it needed
worked perfectly.


 [1]: {filename}2018_02_12_a_new_server_for_the_house.md
 [2]: {filename}2018_03_27_installing_nextcloud.md
 [3]: {filename}2018_04_25_setup_cloudflare_letsencrypt.md
 [4]: https://docs.nextcloud.com/server/15/admin_manual/installation/system_requirements.html
 [5]: https://docs.nextcloud.com/server/15/admin_manual/installation/source_installation.html#prerequisites-for-manual-installation
 [6]: https://github.com/nextcloud/vm/blob/master/nextcloud_install_production.sh/
 [7]: https://github.com/nextcloud/vm/blob/c469b3045c7405261a0d9f20fec7ef5f0f508efe/nextcloud_install_production.sh#L256-L272
