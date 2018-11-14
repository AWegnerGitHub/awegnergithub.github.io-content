Title: Set up Dynamic CloudFlare IP with Let's Encrypt
Date: 2018-04-25 09:30
Tags: technical
Category: Technical Solutions
Slug: setup-lets-encrypt
Summary: Time to make the server accessible from the internet and secure it with an SSL certificate
Status: published
Series: Recovering from data loss

[TOC]

## Introduction

In the two previous articles, I installed [NextCloud][1] and [GitLab][2]. These are running on the server, inside my local network, with 
no firewall rules set up to allow it to be accessible from the internet. That's great if I plan on sitting at home all the time and never
accessing anything from the outside. However, I do plan on that. That means I need to make this server accessible from the internet. On top
of that, I want to secure the connection to the server with SSL, so that I'm not uploading pictures or code in a way that everyone can read.

## Setting up CloudFlare

This new server sits in my house, which sits on a residential ISP network. Obviously, this isn't going to have 24x7 uptime, but that's fine
with me. One thing that I will need, is a way to access this server regardless of the IP address my ISP has given me. This can (and does) change
frequently enough that it'd be annoying to keep track of my current IP manually. 

My solution: set up a DNS entry. In the two previous articles, I set up the Apache virtual hosts with subdomains:

    ServerName nas.example.com
	
and 

    ServerName gitlab.example.com
	
It's time to utilize those. Then I will only need to visit those URLs and Apache will handle routing to the correct application.

I use CloudFlare to handle DNS for this blog. I described the process to [set up CloudFlare][3] a few years ago and never 
looked at it again. "It just works." Hooray! 

For this, we're going to add two new A entries to reflect the subdomains I want to use. I'll point it at my IP address initially too. 

### Automating the IP adddress updates

The initial set up of the A entry/IP address takes a minute. The trick is automating that process every time your IP address changes. I 
am doing that with a small Python script called [`cloudflare-ddns`][4]. Clone this to the server.

    git clone https://github.com/ethaligan/cloudflare-ddns.git
	
Next, we need to set up zone information. This is the configuration file that will be used to update your A records. Copy example.com.yml to the
name of your domain. For example:

    cd zones
	cp example.com.yml andrewwegner.com.yml

Now we need to edit the newly copied file to contain appropriate zone information, CloudFlare API information and your domain.

	%YAML 1.1
	# Your Cloudflare email address
	cf_email: 'your_cloudflare_email_address'

	# Your Cloudflare API key
	# https://support.cloudflare.com/hc/en-us/articles/200167836-Where-do-I-find-my-Cloudflare-API-key
	cf_api_key: YOUR_CLOUDFLARE_API

	# Cloudflare zone name
	# If you're updating 'ddns.example.com' set this to 'example.com'
	cf_zone: example.com

	# List of records
	# If you're updating 'example.com' record, set its name to '@'.
	# Only write the subdomain ('ddns' for 'ddns.example.com')
	cf_records:
		- 'nas':
			type: A
			log: ERROR
		- 'gitlab':
			type: A
			log: ERROR

	# This is the method used to discover the server's IP address
	# The faster one is 'dig' but it may not be available on your system
	# Available methods: 'http' or 'dig'
	cf_resolving_method: 'dig'

In this case, I am updating two subdomains (`nas` and `gitlab`) that are part of the `example.com` domain. Those should be changed to reflect your set up.

Last, we need to schedule this to run on a regular basis so that CloudFlare always points to the correct IP address. I did this with a crontab entry:

    */30 * * * * python3 /path/to/cloudflare-ddns.py -z example.com
	
Again, change `example.com` to your domain, and it will use the appropriate YML file. With this entry, my DNS entries are updated every 30 minutes. That 
is frequently enough for my needs.

## Let's Encrypt (SSL)

With the subdomains set up and working, it's time to install some SSL certificates. In previous articles, I had entries in my Apache virtual hosts that pointed to
SSL certificates. This is where we'll set those up. 

Let's Encrypt certificates are valid for 90 days. Renewing certificates, though, can be easily automated. Since I need my certificates to work through CloudFlare,
because it provides my DNS services, I use a hook in Let's Encrypt's ACME client [`dehydrated`][5] to handle everything.

	cd ~
	git clone https://github.com/lukas2511/dehydrated
	cd dehydrated
	mkdir hooks
	git clone https://github.com/kappataumu/letsencrypt-cloudflare-hook hooks/cloudflare
	pip install -r hooks/cloudflare/requirements.txt
	
This downloads deydrated and then downloads the CloudFlare hook that is needed. It installs the required libraries too. 

The last bit of configuration that is needed is setting up a `config` file in the `dehydrated` directory.

    nano `dehydrated/config`
	
Add the following three lines

	export CF_EMAIL=YOUR_CLOUDFLARE_EMAILADDRESS
	export CF_KEY=YOUR_CLOUDFLARE_API
	export CF_DEBUG=true

Substitute your CloudFlare login email and API key as appropriate. The `CF_DEBUG` line can be set to `false` if you don't wish debugging information to be printed to `logs/`. 

Register with Let's Encrypt and accept their terms of service:

    ./dehydrated --register --accept-terms
	
Finally, you're ready to generate/install the SSL certificates needed. One note: I needed to adjust the shebang line in hooks/cloudflare/hook.py to be `python3`. 

Run the following commands to generate the certificates. These will end up in `dehydrated/certs` with the full URL of each certificate. 

    ./dehydrated -c -d nas.example.com -t dns-01 -k 'hooks/cloudflare/hook.py'
    ./dehydrated -c -d gitlab.example.com -t dns-01 -k 'hooks/cloudflare/hook.py'
	
The path to these files are what will go in your Apache Virtual Host files:

	SSLCertificateFile /path/to/dehydrated/certs/nas.example.com/cert.pem
	SSLCertificateKeyFile /path/to/dehydrated/certs/nas.example.com/privkey.pem
	SSLCertificateChainFile /path/to/dehydrated/certs/nas.example.com/chain.pem
	
I set up a crontab entry for each of my subdomains to attempt to renew the certificate once a week. Dehydrated will not attempt to renew a certificate if it's not going to 
expire in less than 30 days, so we aren't making unneeded calls to Let's Encrypt. 

	0 1 6 * * /path/to/dehydrated/dehydrated -c -d nas.example.com -t dns-01 -k '/path/to/dehydrated/hooks/cloudflare/hook.py'
	10 1 6 * * /path/to/dehydrated/dehydrated -c -d gitlab.example.com -t dns-01 -k '/path/to/dehydrated/hooks/cloudflare/hook.py'

## Conclusion

With this final step, I have a home server that I can access from anywhere. It allows me to backup pictures automatically, holds my private repositories and is protected
by SSL. The SSL certificates renew automatically.


 [1]: {filename}2018_03_27_installing_nextcloud.md
 [2]: {filename}2018_04_12_setting_up_gitlab.md
 [3]: {filename}2015_07_09_how-i-set-up-this-site-with-github-pages-and-cloudflare.md
 [4]: https://github.com/Ethaligan/cloudflare-ddns
 [5]: https://github.com/lukas2511/dehydrated