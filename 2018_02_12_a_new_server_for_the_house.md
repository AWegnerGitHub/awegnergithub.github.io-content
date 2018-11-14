Title: ...and then there was a backup server
Date: 2018-02-12 15:15
Tags: technical
Category: Side Activities
Slug: new-house-server
Summary: Technical discussion about the new backup server
Status: published
Series: Recovering from data loss

[TOC]

## Introduction

In my [last post][1], I covered the events that lead to my data loss scare. Faulty, untested, backups will bite you every time. The question 
is just, "when will it happen?". By mid-to-late January (three months later), I'd gotten everything back from [SERT Data Recovery](http://sertdatarecovery.com) and
was happy that everything was recovered. It was time to finally build that huge NAS.

## Server Goals

A NAS for a home backup solution could be something as simple as a prebuilt device with a couple hard drives. I'm a bit of a geek and have a lot of digital data, not to 
mention family pictures, years worth of programs I've written, and a digital music and movie collection. I'm a digital pack rat, but a well organized digital pack rat. 
I also wanted to get more out of this server than "plug the device into the router". I ran game servers for [Vipers][2] for five years. I did a little bit of server 
work at Caterpillar. I've toyed with server virtual machines off and on for testing various packages and software over the years. Through all of this, though, I've 
never had a server in the house that I could use to run some of my scripts on. Those always ran on my desktop because it was always on. This server was going to change that. 

I had several goals when building this thing:

 - Have more storage space than I needed for several years. I didn't want to rebuild this in 18 months because I was bad at planning. Years ago when I built my computer
 for college, I stuck two 120 gigabyte hard drives in the machine and thought I'd never fill that. When I came home that first summer, I already had to upgrade hard drives 
 because I was low on space. 
 - Run a server version of Linux. I don't want to buy a license for a Microsoft server product and my Microsoft Academic Licenses expired a while ago. During my time with Vipers, I used a Red Hat variant of Linux. At Caterpillar we used Ubuntu. 
 - Utilize [ZFS][3] for protection against data corruption. This combined with my more recent usage of Ubuntu lead me to decide on using Ubuntu Server for the operating system. 
 At the time of this post, I'll be using the 16.04 LTS version. I'll continue to upgrade to future LTS versions.
 - Back up data from all devices in the house automatically. As camera phones have gotten better, we've found that we carry our bulky digital camera less and less. The problem
 with the phone camera is that we need to get the pictures to the computer. I don't want to hunt down a data cable or email the pictures to myself. I'm also not a fan of 
 posting everything to social media. I want my phone to send the pictures to a backup location automatically.
 - Host my personal git repositories and personal projects.
 - Be able to stream music and movies to other devices on the network.
 
## Hardware

Now that I've decided my goals, it was time to pick out hardware. The biggest decision was to determine how much storage space I'd be getting. The idea was that hard drives
would be the majority of the cost of this machine. In the end, I went with the following hardware:

 - Rosewell 4U server chasis. It's rack mountable for the future when I can convince myself that a server rack in the basement is a thing I want to spend money on and haul around.
 - Supermicro MBD-X11SSM-F-O Micro AT server motherboard (LGA 1151)
 - Intel Xeon E3-1230 V5 3.4 Ghz processor
 - 2x Supermicro certified MEM-DR416L-SL01-EU21 16 GB DDR4-2133 ECC server memory. Take careful note of that model. I originally ordered MEM-DR416L-SL01-ER21 (notice 
 the single "R" to "U" character difference). The motherboard did not like the ER21 at all. 
 - EVGA 650 power supply (I've been really happy with EVGA power supplies on my last 4 machine builds).
 - 1x Western Digital Blue 1 terabyte SSD (for the operating system and other applications)
 - 7x Western Digital Red 4 terabyte hard drives (for all the data)
 - Enough SATA cables for all 8 drives
 
I'll be running ZFS in RAIDZ2 (dual parity). This means with 7 drives, two will be effectively parity drives. I'll have a total of 20 terabytes, minus formatting, for data. After formatting this comes down to a little over 16 terabytes of usable space. Considering that the rest of the household has a combined 5 terabytes, if I use up every available bit, I'm hoping that 16 will last me a while.

### That RAM...

I went with the Supermicro board based on a recommendation from a friend. Supermicro's site is really good. It has [tested compatible hardware lists][4] and, it turns out, 
a knowledgeable person behind their online store's chat feature. The problem that I ran into when building this machine is that the compatible RAM was really hard to find. 
I didn't realize that and ordered the mother board in my first batch of components. When I finally went to look for RAM, I failed to notice a single character difference between
the EU21 version that I needed and the ER21 version that I ordered first. 

I assembled the machine, plugged everything in, and turned on the new server. Then it beeped at me. A lot. After some troubleshooting, re-seating the RAM and *finally*
 realizing that I ordered the wrong stuff, I exchanged what I ordered with what I needed. The EU21 RAM worked perfectly. 
 
## What's next

The hardware is assembled. Ubuntu 16.04 Server has been installed. The next step is configuring the server to be the backup solution for the entire house and meeting my other 
goals. I'll have a few more posts in this series on how I accomplished those goals. Stay tuned!



 [1]: {filename}2018_01_27_backup_your_data.md
 [2]: {filename}2015_01_08_thanks-for-all-the-fish.md
 [3]: https://en.wikipedia.org/wiki/ZFS
 [4]: https://www.supermicro.com/products/motherboard/Xeon/C236_C232/X11SSM-F.cfm