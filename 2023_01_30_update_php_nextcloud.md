Title: Updated PHP and NextCloud
Date: 2023-01-30 9:30
Tags: technical
Category: Technical Solutions
Slug: update-php-and-nextcloud
Summary: Nearly five years ago I installed NextCloud. This article talks about the challenges I faced updating to a new version of PHP for a modern version of NextCloud.
Status: published

[TOC]

## Introduction

Almost five years ago, after a [personal data disaster][1], I [set up a new server][2] and [set up NextCloud on it][3]. Then later in the year
I updated Ubuntu to 18.04 from 16.04. Since initial install, I had gone from version 12 to version 23 on PHP 7.3. I documented the steps I took 
to [upgrade from PHP7.1 to PHP7.3 for NextCloud][php] in another article. With [NextCloud version 24][4], PHP7.3 was no longer supported. 

That meant it was time to update PHP again, and I had hoped to follow a similar process as before. It was pretty similar, as I tend to keep the 
server updated.

## Disabling PHP 7.3

The first step was to ensure I had at least PHP 7.4 installed. Since I'd be moving to [NextCloud 25][5], I wanted to ensure that I had at least PHP 8.0,
as PHP 7.4 had been deprecated.

```
$ ll /etc/apache2/mods-available/php*
/etc/apache2/mods-available/php7.1.conf
/etc/apache2/mods-available/php7.1.load
/etc/apache2/mods-available/php7.2.conf
/etc/apache2/mods-available/php7.2.load
/etc/apache2/mods-available/php7.3.conf
/etc/apache2/mods-available/php7.3.load
/etc/apache2/mods-available/php7.4.conf
/etc/apache2/mods-available/php7.4.load
/etc/apache2/mods-available/php8.0.conf
/etc/apache2/mods-available/php8.0.load
/etc/apache2/mods-available/php8.2.conf
/etc/apache2/mods-available/php8.2.load
```

The interesting thing here is that I don't have PHP 8.1 installed. Not 100% sure why, but right now that doesn't bother me. Since the documentation
doesn't mention PHP 8.2 support, I'll be using PHP 8.0 support, and cleaning up the old versions later.

Disable Apache's PHP 7.3 module and enable the 8.0 one, then restart Apache:

```
sudo a2dismod php7.3
sudo a2enmod php8.0
sudo service apache2 restart
```

## Adjusting PHP 8.0's memory limit

NextCloud recommends a 512M PHP memory limit. I forgot to do this initially and got stuck in the web updater. Fortunately, this is an easy fix.

Find your `php.ini` and adjust the `memory_limit` to be `512M`. Restart Apache once more.

My `php.ini` was located here: `/etc/php/8.0/apache2/php.ini`

## Fixing the stuck updater

Since I started the update with a bad memory limit, the update wasn't able to complete. NextCloud failed the Verification step (Step 5) and 
attempting to restart it resulted in `Step 5 is currently in process. Please reload this page later`. 

The solution to this, is to remove the `.step` file from your `data` directory. 

```
rm /nextcloud/data/updater-XXXXXXX/.step
```

The `XXXXXXX` appears to be randomly generated, so you will need to see what yours is named. Once that file was removed, I was able to complete
the updates through the web updater.

## Fix missing indexes

The last step in this process - which is really part of all NextCloud updates - was to get indexes in place. This is easily accomplished by running
the `occ` utility provide by NextCloud.

From your NextCloud directory:

```
sudo -u www-data php8.0 ./occ db:add-missing-indices
```

I'm running this as the correct user. If you don't, it will warn you and fail to run.

## Conclusion

This update was pretty similar to the one I did several years ago. The important thing that I missed was setting up the `memory_limit`. A small 
problem to resolve. I think between this upgrade and the [previous upgrade][php], the future upgrade to PHP 8.2 when it's required will be even easier.

After 5 years of using NextCloud to save family data, I continue to love it's simplicity and ability to "just work". Less technical family members 
don't need to do anything complicated to have their data backed up to a location we control. This update will ensure it continues to work with the 
newest versions.







 [1]: {filename}2018_01_27_backup_your_data.md
 [2]: {filename}2018_02_12_a_new_server_for_the_house.md
 [3]: {filename}2018_03_27_installing_nextcloud.md
 [php]: {filename}2019_07_26_updating_php_ubuntu_1804.md
 [4]: https://docs.nextcloud.com/server/24/admin_manual/installation/system_requirements.html
 [5]: https://docs.nextcloud.com/server/25/admin_manual/installation/system_requirements.html