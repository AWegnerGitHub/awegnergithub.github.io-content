Title: Installing InvoiceNinja on Ubuntu 18.04
Date: 2018-07-30 13:00
Tags: technical
Category: Technical Solutions
Slug: invoiceninja-ubuntu-1804
Summary: A brief walkthrough on how to install InvoiceNinja on Ubuntu 18.04
Status: published

[TOC]

## Introduction

It's been a several years since I really did a lot of freelance work. Mostly, I did it in the evening hours
a few nights a week for some extra money. Over the last few years, that kind of work has
slowed down for me. Mostly because I enjoy relaxing in the evenings and spending time off
of my computer now. It's weird to type that, but "hobby coding" really hasn't had as much
appeal to me since I started my [current job][job] a few years ago.

I love my job. I love working from home 100% of the time and I love being home
when the kids get off the bus, during summer breaks, and when the weather sucks. The
one thing this job has done though, is remove my desire to sit in my office after
everyone else has gone to bed and just code.

But, that's ok. I've found other hobbies and am really excited about some up coming
things I'm trying out. If they work out, I'll be able to combine my coding skills
with other hobbies. I'm sure I'll write about it at some point.

I say all of that, because, even though I don't actively take on freelance work any
longer, I still have clients that I did work for that will reach out once and a while
for help, consultations or a small project. When that happens, I need a way to bill them
for my time.

Previously, my invoices would have been whipped up in Word and converted to a PDF and sent
off to the client. It worked for years. I was able to keep track of everything,
properly report it (don't want to mess with the tax man) and I kept chugging along. The
problem now, is that if I'm doing work for a client, I want to do the work and be done. I don't
want to spent time making a presentable invoice in Word.

So, I went hunting for some software. I found [InvoiceNinja][ninja]. It's amazing.

 - Hosted or self hosted version
 - Create simple invoices
 - Create full quotes
 - API (which I will probably never use, but...it's there)
 - Recurring billing
 - Zapier integration

I have this habit of self hosting my own software. See my posts on [NextCloud][nextcloud]
and [GitLab][gitlab] for other examples.

This rest of this post is going to walk through how I installed InvoiceNinja.

## Getting Started

I'll be self hosting [InvoiceNinja][ninja1]. You can find that on their `.org` site
or on [GitHub][ninjagithub].

In this, I'll be installing InvoiceNinja, setting up Apache to host it, and showing how
to update it in the future.

    $ wget https://download.invoiceninja.com/ninja-v4.5.13.zip
    $ unzip ninja-v4.5.13.zip

This downloads and unzips InvoiceNinja 4.5.14 to the current directory. Next,
let's move it to it's final install location and set up appropriate owner and
permissions.

    $ sudo mv ninja /var/www/html
    $ cd /var/www/html
    $ sudo chown -R www-data:www-data ninja/
    $ sudo chmod -R 755 ninja/storage

## Database setup

InvoiceNinja runs on MySQL (or MariaDB). We need to create the database, but
the installer will do the rest of the work for us. Log into MySQL with a user
that can create new databases, users and permissions.

    $ mysql -u root -p

Now we'll run three commands. The first is to create the new database, the second
is to create the user. The third is to set appropriate permissions for the
new user and new password.

!!! attention
    Change the default password from `ninja` to something secure

Our three commands:

    CREATE DATABASE ninja;
    CREATE USER 'ninja'@'localhost' IDENTIFIED BY 'ninja';
    GRANT ALL PRIVILEGES ON ninja.* TO 'ninja'@'localhost';

Exit MySQL.

## Configure Apache

Now we're going to set up a new virtualhost in Apache to serve InvoiceNinja.

Create an entry in sites-available.

    nano /etc/apache2/sites-available/invoiceninja.conf

My entry looks like this:

    <VirtualHost *:80>
         DocumentRoot /var/www/html/ninja/public
         ServerName invoice.example.com
         Redirect permanent / https://invoice.example.com/

         <Directory /var/www/html/ninja/public>
            Options +FollowSymlinks
            AllowOverride All
            Require all granted
         </Directory>
    </VirtualHost>

    <VirtualHost *:443>
        ServerName invoice.example.com
        DocumentRoot /var/www/html/ninja/public
        SSLEngine on
        SSLCertificateFile /path/to/dehydrated/certs/invoice.example.com/cert.pem
        SSLCertificateKeyFile /path/to/dehydrated/certs/invoice.example.com/privkey.pem
        SSLCertificateChainFile /path/to/dehydrated/certs/invoice.example.com/chain.pem
        <IfModule mod_headers.c>
            Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains"
        </IfModule>

        <Directory /var/www/html/ninja/>
            Options FollowSymLinks
            AllowOverride All
            Order allow,deny
            allow from all
        </Directory>
    </VirtualHost>

This sets up a direct from HTTP to HTTPS. Then it points to the SSL certificates I've
created for this subdomain. I previously wrote about [how I set up SSL][ssl]. I followed the
same steps, using the new subdomain.

With SSL and the subdomain set up in CloudFlare, it's time to enable the site:

    $ sudo a2ensite invoiceninja.conf
    $ sudo systemctl reload apache2

## Completing the install

Navigate to the new subdomain, and fill out the form. Installation will be
completed in a minute or so.

## Updating

At some point, InvoiceNinja will come out with an update, and I'll want to update
to get the newest features and security patches.

To start with, we want to download the newest version:

    $ wget https://download.invoiceninja.com/ninja-v4.5.14.zip
    $ unzip ninja-v4.5.14.zip

Next, `rsync` the files to the installed directory.

    $ sudo rsync -tr ninja/ /var/www/html/ninja/

This messes up permissions, so reset those.

    $ cd /var/www/html
    $ sudo chown -R www-data:www-data ninja/
    $ sudo chmod -R 755 ninja/storage

Finally, hit the `/update` URL to complete the process

    wget https://invoice.example.com/update



 [job]: {filename}2017_11_28_how_i_found_an_awesome_remote_job.md
 [ninja]: https://www.invoiceninja.com/
 [ninja1]: https://www.invoiceninja.org/
 [nextcloud]: {filename}2018_03_27_installing_nextcloud.md
 [gitlab]: {filename}2018_04_12_setting_up_gitlab.md
 [ninjagithub]: https://github.com/invoiceninja/invoiceninja
 [ssl]: {filename}2018_04_25_setup_cloudflare_letsencrypt.md
