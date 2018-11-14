Title: A new, more fair, RTD
Date: 2011-2-04 20:01
Tags: team vipers, programming
Category: Vipers
Slug: a-new-more-fair-rtd
Summary: A description of how the Roll The Dice plugin has been updated 
Status: published
modified: May 20, 2015

[TOC]

## The old RTD

Viper's own LinuxLover, aka [pheadxdll][1] on the [SourceMod forums][2], wrote the [original version][3] of the Roll the Dice
plugin. It has provided countless hours of fun for players. After all, who doesn't love getting Toxic while standing near an enemy 
spawn and hearing the rage as they die immediately. It's usually worth the instant death that follows when the effect
wears off ten seconds later.

There were problems though. The biggest was that the chances of getting a Good vs Bad roll were not equal. Instead, you 
had a roughly equal chance of getting any roll. There were 14 possible rolls. You had a 1 out of 14 chance of getting a specific
effect. However, 9 of those effects were negative. Thus, you had a much higher chance of getting a negative effect vs a 
positive one. The other major problem was that it was very difficult to add new effects. Finally, with the released mod
not being updated, and LinuxLover departing Vipers to handle his own community on the Randomizer server, it was next to
impossible to get changes made.

*NOTE*: LinuxLover released a new version of RTD (the 0.4 branch) sometime after we had forked the version we had. The new
version on SourceMod contains many of the same features we have. It does not, however, contain all of them.

## The new RTD

The logic for how rolls are determined has been changed. There is now a list of Good Effects and a list of Bad Effects. When
someone rolls, the first thing that is done is determine whether or not we are going to have a Good or Bad effect. That is a 
50/50 chance. Then it randomly selects one of the effects that are active and appropriate for the player's current class
that falls under the winning category. This should even out the Good vs Bad complaints.
 
Another change that I've added is that we can now more easily add effects. Some new effects have been added:

 - Powerplay: When Uber isn't enough...you need Kritz and Uber. 
 - Freeze Bullets: You've been shot! You should run away, but you can't. You're frozen in place for the next ten seconds, if 
 you're lucky.
 - Fire Bullets: A bullet wound isn't enough. You need to be on fire too.
 - No crits: Haha! You are on an all crits server and you just lost your crits. Go sit at the little kid table.
 - Valve Rockets: We've included something that may be slightly overkill. You tell me:
   - +9900% damage
   - +9000% clip size
   - +75% firing speed
   - +500 health on kill
   - 10 seconds of crits on kill
   - +200% speed
   - +60% reload time
   
   Because sometimes overkill is needed. 
   
   We've been testing performance of this over the last few months. Today is the day that everyone can get it without
   an admin being around.
   
   For those that haven't seen it, here is one of our first tests back in November
   
   https://www.youtube.com/watch?v=OzHNr1Bz5QQ
   
 - The Headless Horseless Headmann: That's right, you can be the Halloween nightmare.
 
 *Update:*
 
  - Homing Projectiles: Is that soldier aimbotting?! Nope. His rockets are just following you where ever you go. Boom! Oh, 
 sniper arrows and pyro flares are probably something to avoid too.

Finally, I added in the ability to log rolls to a database so that we can find fancy stats and ensure it's rolling fairly. I'll
update this post in a few months with some of our gathered stats.

## Source Code

*Updated May 20, 2015 with link to GitHub instead of the old SVN, my apologies for missing that link when migrating to this blog
It is important to note that this version hasn't been updated in a LONG time and all effects may not work any more. Specifically,
homing probably doesn't work because the Sidewinder extension had been broken by a Valve update in mid-2014. Other effects
should continue to work*

The source code is released on Github. It requires several common modules which are all available on the SourceMod forums.

The repository is: [https://github.com/AWegnerGitHub/Vipers-Server-Plugins][7]

## Statistics

*This section was updated in January 2015 after the shutdown*

### Good vs Bad

I don't have exact stats on Good vs Bad rolls before the rewrite, but we started logging all rolls with the rewrite. This is the 
split of Good vs Bad. I am very happy with an almost exactly 50/50 split over nearly a million RTDs.

![RTD Split][4]

### Class with the most RTDs

Solider was the most popular class on the crit server. The images below show number of times our soldiers got certain rolls. Note that God Mode is 
low because the community decided having both God Mode and Powerplay was redundant. They decided to keep Power play. God Mode was
disabled about a year after this version was released. Homing is low because it wasn't implemented until about 18 months after
the initial release.

![RTD Soldier][5]

### Class with the least RTDs

Medics rolled RTD the least number of times. This means sense for two reasons. First, they were not a common class on the crit server. When 
nearly everything is one hit, one kill, it's very hard to build uber. Second, when you *do* try to build Uber, ruining it with a 50/50
shot at getting a bad roll isn't good for the team. The low God Mode and Homing results are the same here as they were for the soldier.
 
![RTD Medic][6] 


   


 [1]: https://forums.alliedmods.net/member.php?u=38829
 [2]: https://forums.alliedmods.net/index.php
 [3]: https://forums.alliedmods.net/showthread.php?p=666222
 [4]: {attach}images/rtd-split.png
 [5]: {attach}images/rtd-soldier.png
 [6]: {attach}images/rtd-medic.png
 [7]: https://github.com/AWegnerGitHub/Vipers-Server-Plugins
