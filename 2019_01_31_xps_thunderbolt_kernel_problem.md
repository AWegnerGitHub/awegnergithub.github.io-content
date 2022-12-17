Title: Fixing XPS 13 (Ubuntu) and Thunder Bolt 16 issue after BIOS update
Date: 2019-01-31 10:30
Tags: technical
Category: Technical Solutions
Slug: xps-tb16-bios-fix
Summary: Updating BIOS using a Dell provided update broke how my Thunder Bolt dock and XPS interacted. This is how I fixed it.
Status: published

[TOC]

## What happened?

In November of 2018, Dell released a [BIOS update][dell-bios] (version 2.10.0) for my XPS 13 9360 running Ubuntu 18.04.1. Among the enhancements
this updated added was:

> Enhanced the stability of Linux operating system

![BIOS Update available][update-available]

I've been putting off updating due to squeamishness involving touching the BIOS. If it goes poorly, it could make my day really
stressful.

It's currently -50 with the windchill outside. Schools are closed. Businesses are closed. Even the Post Office has said they aren't
delivering mail because it's so cold. This sounds like the perfect time to perform an update.

There was my mistake...

After installing the update and rebooting the machine, the XPS froze as the Ubuntu login screen was loading. There was only a
mouse cursor on an otherwise black screen. The mouse didn't respond to input from the touch pad or from a USB mouse. The keyboard
didn't appear to be responding either. The external monitors weren't receiving a signal and, finally, an attempt to SSH into the laptop failed.

## Troubleshooting

The laptop was entirely unresponsive. First step: "Did you reboot it?" Yes. I had to hold down the power button so it wasn't a clean reboot.
The exact same symptoms occurred: Black screen with only a mouse cursor right before the login page loads. Doesn't respond to any input.

Next, I unplugged everything. The laptop is plugged into a Thunder Bolt 16 docking station so that I can utilize two external
monitors. I also have Logitech headphones plugged into one of the USB ports and an external keyboard and mouse. Then I rebooted again by
holding down the power button.

This time, everything worked. The login screen popped up, the machine responded to input events and everything was fine. Victory?

Nope.

I started plugging stuff back in: Keyboard, then mouse, then headphones. No problems. Then I plugged in the Thunder Bolt docking station. A few
seconds later, the screen went black and stopped responding.

After a few reboots and a few tests of plugging in the dock, I realized that it was the dock causing the laptop issues. When it was plugged into
the wall adapter, it worked fine. The docking station was causing the problem. This isn't great, but at least the laptop works.

## Rolling back BIOS

At this point, it was time to roll back the BIOS. An update broke it, the original version should fix it...hopefully. The first step was
finding the old version - 2.9.0. Fortunately, Dell's support is helpful in this one single way. There is a page for old drivers and I quickly
found the [System BIOS version 2.9.0][old-bios].

To perform the roll back, there are only a few steps you need to do.

 - Get a USB drive that is formatted with FAT32
 - Copy the BIOS file to the drive and leave it plugged into the XPS
 - Make sure the XPS is plugged in (with the wall adapter, not the Thunder Bolt)
 - Restart the machine
 - At the Dell splash screen press [kb:F12] to open the One Time Boot Menu
 - Select "BIOS Flash Update"
 - Select the file you downloaded on the USB drive
 - Wait patiently as BIOS is flashed again

When the flashing was done, I was back to BIOS 2.9.0. Ubuntu restarted...and the same issue occurred. Plugging in the Thunder Bolt made the
laptop seize.

## Dell Support

With the BIOS reflash a failure, I turned to Dell support. I didn't have high expectations going in to this. Yet, somehow, I came out even
more disappointed. Through two phone calls, I was informed that

> Dell doesn't support Ubuntu

This is despite that fact that the laptop came with Ubuntu installed by Dell *and* the BIOS update was from Dell. Since my operating system
was not Windows, I couldn't get any support.

I turned to the Dell Community forums. After some private back and forth with a community moderator (and Dell Social Media Support employee), I
was given this:

> At this point, I do not think that the TB16 or XPS hardware are at fault here. I think that the BIOS update broke the laptop USB Type-C.
> Not physically, but in the operating system. So even though you backflashed, the issue remains.

I was encouraged to test the Thunder Bolt with another Dell laptop. It's fortunate that my wife has a Dell for her work. Without it, I
wouldn't have been able to get this message when attempting to flash the Thunder Bolt firmware I was pointed to:

> Error: Collecting Dock Information failed

I never heard back from Dell support after providing them with a screenshot of that error.

I was on my own.

## Inspiration

At first glance, the quote from the forum moderator above doesn't make much sense. How can the BIOS update break the USB Type-C port in the operating system?
How can it not be fixed by going to a previous version?

Then it hit me: There was more to this BIOS update. A day earlier I'd updated the kernel and hadn't rebooted yet and forgot about it.

    $ uname -r
    4.15.0-44-generic

Working with that, the phrase "Thunder Bolt" and Google, I stumbled across a post on [Stack Exchange][1] with the same issue. Of course.
Why didn't I start there? I had been chasing the wrong thing. BIOS wasn't the cause, it was just the reason for the system reboot.

## Fix

The fix involved going back to the previous kernel. I didn't follow the [Ask Ubuntu post][1] exactly. First, I edited by grub configuration to display the
grub menu. This is at `/etc/default/grub`. I changed the `GRUB_TIMEOUT` value to `-1` and uncommented `GRUB_HIDDEN_TIMEOUT`. After saving that config file
I ran `sudo update-grub`

Then I restarted.

When the grub menu appeared, I selected "Advanced Boot Options" and then selected the `4.15.0-43-generic` kernel and continued the boot.

Ubuntu loaded. It stayed responsive when I swapped from the wall adapter to the Thunder Bolt dock.

Victory!

## Final thoughts

With the XPS working again, I have decided to stay on BIOS 2.9.0 for now. I also haven't re-updated the kernel to the `-44` version. In fact, I purged that one
for the time being with this command:

    sudo apt-get purge -f linux-image-4.15.0-44-generic

This is a [known bug][bug-report] with 4.15.0-44 and a fix is being worked on. It also seems to impact more than just Dell products. It looks like
4.15.0-45 will fix the issue. We'll see.

Also, Dell support is less helpful than I thought it would be.


 [dell-bios]: https://www.dell.com/support/home/us/en/04/drivers/driversdetails?driverId=T7XJF&osCode=WT64A&productCode=xps-13-9360-laptop
 [update-available]: {attach}images/update-available.png
 [old-bios]: https://www.dell.com/support/home/us/en/19/drivers/driversdetails?driverId=RCKDK&osCode=WT64A&productCode=xps-13-9360-laptop
 [1]: https://askubuntu.com/a/1113954/183377
 [bug-report]: https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1813663
