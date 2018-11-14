Title: Well, there goes my data...
Date: 2018-01-27 00:04
Tags: technical
Category: Side Activities
Slug: backup-your-data
Summary: Learn from my mistake and test your backups
Status: published
Series: Recovering from data loss

[TOC]

## Introduction

October 2017 started nicely. The weather was still good. Fall hadn't really arrived yet, though I was still raking some leaves. Halloween was approaching and 
soccer season had just ended so I had time of weekends to do a few more projects. Then in mid-October one of my hard drives died. It was sudden. There were
no SMART warnings. There wasn't any weird sounds. 

In fact, the only indication that there was a problem initially was that one of my running applications couldn't access a log file. I didn't think any of it. 
I took a few minutes to install a Windows update that had been nagging me for a day or so and rebooted the machine. A reboot on my desktop normally takes twenty-ish
seconds. 

As the reboot stretched into the fifth minute, I realized there was a problem. However, I was initially getting ready to blame that Windows update. 

## A Data Disaster

Finally, the reboot finished and I logged into the machine. Immediately, I received alerts that files couldn't be accessed and programs couldn't start. I tried to launch
Firefox and was told it couldn't be found. "Well, this is bad," I thought. Turns out that was an understatement. After a short investigation, I found that one of my hard drives wasn't being detected by Windows. 

On my desktop, I have three hard drives. The first is, a decently sided solid state machine that hosts the operating system. The second is a large solid state drive that 
holds my game installs. The third is a large spinning hard drive (Seagate), which is where I install software and hold my data. The missing drive was the large one that holds
my data.

Panic set in. However, I had just backed stuff up to my trusty external hard drive. "At most I'll lose a week's worth of stuff. And, I still have the pictures on phones and
camera SD cards," I said, trying to comfort myself. I also new I had at least two brand new and unused drives identical to the one that had just had a problem. Those were the 
result of cheap hard drives when I build my desktop and the promise that one day I'd build a true backup solution for the house. Yeah, hadn't done that yet.

After messing with the missing drive by unplugging it and rebooting (bringing back normal boot times), swapping it to another SATA port ("hey, maybe it's the motherboard"), 
trying to access it in a Linux LiveCD and plugging it into an external drive, I concluded that it was dead. I pulled it from the desktop, resisted smashing it to pieces in
frustration and plugged one of it's unused siblings in. 

Windows booted right up. I had an empty hard drive. I'd be spending a day reinstalling software, but I could handle that. I plugged in my backup external drive and started 
copying data back into place. I breathed a sign of relief when all the family pictures were restored. I lost a weekend, but everything looked good. I went to bed Sunday night
happy and planning on finally building that backup solution. "That was almost a disaster!"

## The Problem Appears

On Monday morning, I saw the family off to school and work and then settled in for my day. I went to pull up a personal project that I'd work on for an hour or so before the
work day began. Well, it turns out that one of the things I stored on that drive that hosts all my data was my personal projects. It also turns out that I never actually
backed up my personal projects. 

More than decade worth of personal programming projects, horrible attempts at graphic work (seriously...graphics aren't my thing), notes for games that I'd love to design if
I ever get the time were gone. I dug through my most immediate backup. I found old drives - because "you never know when you'll need it" - and dug through those. Nothing. I 
have no idea how I missed this rather important directory in all my various manual and semi-automated backup routines over the years.

I'd either have to suck it up and lose that history or get serious about data recovery.

## Data Recovery Attempt

Back in college, I was manager of the university's help desk. One of the tasks we performed was data recovery for students. Usually this was on flash drives that had "suddenly 
deleted a paper", but occasionally we'd have to work on a hard drive too. Our recovery efforts were limited to a quick run of a handful of applications that attempted to 
recovery deleted files. In the world of stressed students, this was all that was usually needed. If you could recover that term paper, thesis paper or dissertation, you were
a hero. 

That's what I tried. I hoped it'd work. Windows still couldn't see the back drive. A Linux LiveCD could see that a drive existed, but couldn't find any data. That was still 
progress. I ran software for almost two days as it tried to find anything for me to recovery.

Nothing. Nothing at all. It looked like some how erased everything from the drive. I was back to my choices - lose the history or find a professional. 

I went with a professional. I couldn't lose that much of my history. I still those projects for both personal and professional work. 

## SERT Data Recovery

I went with [SERT Data Recovery](http://sertdatarecovery.com). The technician over the phone explained their process. They asked questions that made me confident in their
ability to at least diagnose the problem, and most importantly were thousands of dollars cheaper than a competitor I spoke with. They explained their pricing structure (they
provide a quote and it won't be a giant range), and pricing for replacement parts and allowed me to either send in an empty drive or pay for one that my data would be returned 
to me on. They answered my questions.

They also didn't scold me for doing the single most damaging thing to a drive. I had done it repeatedly. I had turned the bad drive back on. I had turned it on and let it spin 
for days as I attempted to recover data. 

I shipped them the drive. After the initial diagnosis came back - one of the platter heads had stopped working - I paid for the replacement part and waited. Eventually the 
data was recovered with a 100% recovery rate. I was ecstatic. My data was shipped back to me and I quickly transfered my projects to the new drive.

My only complaint in the entire process was that the recovery process took almost seven weeks. I shipped the drive at the end of October and received the data back the week 
before Christmas. However, all of my data was recovered, so it's a pretty minor complaint. 

## What's next?

It's been a little over a month since I've transfered my data to a the new drive in my desktop. I've manually run my back up process two or three times. I'm pretty much where
I was at the beginning of October. Crossing my fingers that when I get on the computer in the morning, all my data will be there. That's not acceptable any more. 

I'm finally building the backup solution. It'll be a huge (for a family) NAS. The goal is to be able to back up from Windows, Ubuntu and a couple Android phones automatically. 

The next entry will cover the device itself and my choices for certain hardware. I'll follow that up with an article on the software set up to get the entire thing working.