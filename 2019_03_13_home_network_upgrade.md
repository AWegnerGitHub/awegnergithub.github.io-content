Title: Upgrading the home network to use Ubiquiti products
Date: 2019-03-13 11:30
Tags: technical
Category: Technical Solutions
Slug: ubiquiti-home-network
Summary: My home network received a major upgrade. This post covers the new hardware and impressions a month after the upgrade.
Status: published

[TOC]

## Routers past

I am not known for being gentle with routers at my house. I have pushed many consumer
grade routers to their limits and ended up replacing them for newer models or
competing models. My most recent model was an [ASUS RT-AC66U][asus]. I bought this
in 2015. It is one of the longer surviving routers in the house.

Since I started [working from home though][1], I've been noticing more problems.
These include randomly dropping all wireless connections, randomly losing connection
to the cable modem and frequently needing reboots to fix these problems. The issue
that finally made me start looking for another replacement was when the entire
router would reboot when a new wireless device connected to the network.

This was especially obnoxious when family members would come home and their phones
would reconnect to the home network. On more than one occasion a work call was
dropped, a college course interrupted or a review session with coworkers disconnected.

I've also found myself adding smart home "things" to my house in the past few years.
I'd told myself that I didn't need smart plugs, smart lights, voice assistants,
and the like, but I received a free voice assistant at some point. I set it up and
the family got hooked. One voice assistant became two then more. I attached
smart plugs to Christmas lights "as an experiment" and found it worked way
better than I expected. Christmas ended and the plugs were re-purposed. I brought
in smart lights and out fitted my office with those. I have more, but haven't yet
expanded to other rooms. It's coming though.

In short, I have more devices than ever connected to the network and the wireless
router I had didn't seem up to the task of handling all of it.

It was time for an upgrade. I didn't want just another router that I'd have to
replace in a few years though. I wanted something high quality and that could
handle anything I through at it.

## Enter Ubiquti

I don't remember what brought [Ubiquiti][ubiquiti] to my attention years ago, but I
was immediately impressed with the hardware, the reviews and the price point. It
is not a home router, but it's also not a five figure price.

I did some research I what I'd need in the house. I needed to be able to support
several wired devices, even more wireless ones (computers, phones, tablets, and
home automation devices). I wanted to have a guest network like the previous ASUS,
so that I could easily give visitors access to the internet without giving them
access to everything on the network. I also wanted to segregate all of those
smart devices to their own network.

I am trained as an information security professional. I understand the risks that
all of these devices can add. It's a lot of little mini-computers on the network.
I don't want an outlet plug compromising my house. I also don't have top secret
data, so a little risk for the sake of convenience is ok to me. If I could
put these devices on a network that is seperate from both the main network and
the guest network, that'd be good for my purposes.

## Ubiquiti Devices

After a lot of research on the [devices Ubiquiti offers][2], I picked out the
ones that would be used. I'd be using the UniFi branded products instead of the
Edgemax branded products. From everything I was able to find, the hardware between
the two brands is how they are managed.

I am *not* a network engineer. I work with some great ones at work and they
speak another technical language entirely. As useful as it'd be to know more
in that area for work, that's not a goal I wanted to pursue for a home project.
I went with UniFi because of it's easier management with the UniFi controller.

I went with the following devices

 - [UniFi Security Gateway][uf-sg]
 - [UniFi 24 port switch][uf-sw24]
 - [UniFi 24 port switch with PoE][uf-sw24poe]
 - [UniFi AP AC Pro][uf-apacpro]
 - [UniFi Cloud Key (gen 1)][uf-ck]

### Security Gateway

The [security gateway][uf-sg] is the router the handles the network. It's much
smaller than previous routers I've had. The purpose of this product is to provide
routing, not be the device everything in the network plugs into. That's the switches.

The cable modem plugs into WAN1 and the switch plugs into LAN1. That's it. Nice and simple.

### 24 port switch

The [24 port switch][uf-sw24] is the first switch I bought for this project. It was a mistake. The mistake
was mine though. The Access points are powered via power over ethernet. This switch doesn't
have that. The best I can come up with for how this mistake was made is that I had both the
PoE and non-PoE versions open when I went to order and put the wrong one in my cart.

Whoops.

That said, it ended up being a happy mistake. I have more wired devices than I realized. Previously,
I had a router and a couple small switches scattered around the house to allow multiple devices
to be plugged in. If I'd just ordered the single 24 Port PoE switch, I'd have used well over half
of the ports immediately between the existing devices and the new access points.

My solution was to keep this switch, and only plug in the wired and non-PoE devices into
this switch. Anything that needed to be powered via PoE would go into the PoE switch.

### 24 port switch PoE

The [24 port PoE device][uf-sw24poe] is slightly deeper than the non-PoE version. It also has
case fans. I am using this device to power the access points I purchased. It was originally
going to power the cloud key too.

### Access Points

I bought a 5 pack of [AC Pro access points][uf-apacpro]. As I mentioned, these are
powered via power over ethernet. That means I needed to run wires to locations around
the house. That's an exercise I don't want to repeat.

I've put up three of the five access points initially to see how much coverage
they provide to the house. So far, I am pleased with how they are performing. I'm
not sure where the last two will go yet, but I'll find a spot eventually.

These access points are handling the three networks without any issues. The
trust home devices are on one network, the smart home devices are on another, and
a third guest network is used occasionally with no issues.

### Cloud Key (Gen 1)

This is the device I wish I'd researched a little more before purchasing. I see
it's purpose, but I don't need it. I can run the UniFi controller on my Ubuntu
server. It took a little bit of work to get it installed because of port
conflicts with GitLab and poor error messages, but once that was figured out, it
works just fine.

My other complaint about the first generation cloud key is that it just hangs from
the switch. The second generation is mountable (and performs more tasks for other)
Ubiquiti products. I purchased the first generation though and don't think I'll
ever use it.

## Review

The set up of all devices is very simple. Plug it in, set up your management
account on the UniFi controller, adopt the devices and you are set up. A little
configuration is needed to set up wireless networks, but that didn't take a lot
of work either.

From the controller, updating the firmware of all devices is simple. If there
is an update, the controller lets you know and you click "update". Five minutes later,
the device reboots with the new firmware and everything works. I was able to update
everything within thirty minutes.

Be aware of which devices are plugged into what. You don't want to reboot an upstream
device before the down stream ones are finished. For that reason, I updated the
access points first, then the switches, then the security gateway.

Since everything was set up, I haven't been disconnected from the cable modem once.
I haven't had the entire wireless network shutdown when a family member came home.
I haven't had any spots without coverage in the house.

In fact, I have a brand new first world problem. A couple of the smart plugs I
have can only be configured on the 2.4Ghz band. The three access points I have cover
the entire house in a 5Ghz signal. To configure these plugs, I need to step out
into the yard to get outside of the 5Ghz range.

The amount of details available in the UniFi controller is amazing. I can see
signal strength of all wireless network devices. I can see nearby wireless networks
that I couldn't see with the previous router. I can see traffic patterns (I watch
a lot of video services). I even have the intrusion prevention system enabled
because my internet speed isn't fast enough to be impacted by the load this puts
on the security gateway. There is one persistent IP in Europe that likes to scan
me. Hi Netherlands! I see you!

## What's next?

So far, everything I've thrown at this new set up has handled it like it was nothing.
That makes sense. These are business class products and are designed to handle way
more than I should be able to throw at it as a home user. I like a challenge.

What's next though? I'd like to get even more information from the devices. A true
monitoring solution for the entire home network or at the least the network equipment
and the server. I've been investigating the [TICK stack][tick] for gathering metrics.
I'll see if I can set something like that up in a way that I like.

I'd also like to expand wireless coverage out to the shed. I'm not sure if I can
do that with the access points though. I don't want to run an ethernet cord out
there (and bury it). That's really far down on my wish list though.

 [asus]: https://www.newegg.com/Product/Product.aspx?Item=N82E16833320115
 [ubiquti]: https://www.ui.com/
 [1]: {filename}2017_11_28_how_i_found_an_awesome_remote_job.md
 [2]: https://www.ui.com/products/#default
 [uf-ck]: https://www.ui.com/unifi/unifi-cloud-key/
 [uf-sg]: https://www.ui.com/unifi-routing/usg/
 [uf-sw24]: https://www.ui.com/unifi-switching/unifi-switch-2448/
 [uf-sw24poe]: https://www.ui.com/unifi-switching/unifi-switch-poe/
 [uf-apacpro]: https://www.ui.com/unifi/unifi-ap-ac-pro/
 [tick]: https://www.influxdata.com/time-series-platform/
