Title: Homing projectiles are awesome!
Date: 2012-6-30 8:02
Tags: team vipers, programming
Category: Vipers
Slug: homing-projectiles-are-awesome!
Summary: Pyro is an under utilized class on the Crit server. This post explains how I've fixed that.
Status: published
modified: May 20, 2015

[TOC]

## Stupid soldier spam

The appeal of the crit server is fast game play, overpowered shots, and nearly instant death if you aren't paying attention.
The down side is the soldier spam. Lots of it. It's not unusual to have a team of soldiers spamming rockets. This is part
of the reason we stuck a class limit on Soldiers. 

Pyro is a common way to counter a soldier firing at long range. The problem with pyro is that it has limited long range weapons 
in return. Unless you can sneak up on an enemy (not easy with spam and some of the maps), the pyro is stuck taking pot shots
with either the Flare gun or the shotgun. 

Two weeks ago, I added a plugin to the server that made Pyro much more effective at helping the team without needing to advance 
to the front line constantly.

## Reflectiles

Reflected Projectiles - Reflectiles, if you will - becoming homing projectiles when a Pyro air blasts them away. These
newly tracking projectiles will track an opposing team member and hunt them down. If the player being tracked dies before
the projectile hits them, the projectile will select a new target. 

 > Well, that seems unfair. How do you defend against a homing projectile as a soldier?
 
It is called **Team** Fortress 2. You have team mates. Utilize them. That homing projectile can be reflected again by a Pyro
on your team. Each time a projectile is reflected it gets just a bit faster. 

## Source Code
 
*Updated May 20, 2015 with link to GitHub instead of the old SVN, my apologies for missing that link when migrating to this blog
It is important to note that this version hasn't been updated in a LONG time but was still functioning when Vipers shut
down. If it doesn't work, the first thing to try is updating SourceMod's gamedata. This was the fix every other time
it didn't work*

The source code is released on Github. 

The repository is: [https://github.com/AWegnerGitHub/Vipers-Server-Plugins][1]


 [1]: https://github.com/AWegnerGitHub/Vipers-Server-Plugins
 [s]: {filename}2015_01_08_thanks-for-all-the-fish.md