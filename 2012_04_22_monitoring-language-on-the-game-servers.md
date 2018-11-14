Title: Monitoring Language on the game servers
Date: 2012-4-22 12:13
Tags: team vipers, automation, programming
Category: Vipers
Slug: monitoring-language-on-the-game-servers
Summary: Team Vipers is proud of it's friendly atmosphere. This post describes how I automated a large part of the process
Status: published
modified: January 8, 2015

[TOC]

## Introduction

My admin tool of choice for the TF2 servers is [HLSW][1]. It's decent at allowing me to manage a server without ever 
needing to log to the server. My biggest complaint about it is that I can only watch the game chat of one server at a time. 
Sometimes, it's helpful to see an ongoing conversation to resolve minor problems before they become big ones. For example, 
claims of "hacking" usually turn out to be completely baseless. But, if multiple users (and more importantly, multiple 
*trusted* users) suddenly start mentioning a hacker, I can step in and resolve the problem without entering the server. 
HLSW is good for this. A hacker is confined to one server.

The biggest problem is when there are reports of lag across all of the game servers. Vipers has a dedicated machine that 
runs 5 game servers. If all five suddenly report lag, there is a problem somewhere. With HLSW, though, I can't see all of 
the servers at once. Thus, I've built a tool...

## Chat Monitor

All chat that occurs on the servers is logged. I've used this to resolve complaints of unfair bans and reports of hackers. 
I built in a hook to these logs from the application template to quickly pull known aliases of users. It's been invaluable 
in solving problems of "what happened" on the servers.

![Multiserver Chat monitor][2]

I've expanded it's usefulness. Now I can load a single page and see all chat activity occurring on all active game servers 
on a single screen. It provides, at a glance, a quick way to see if there are problems on the servers. It also allows me 
to step back from picking which server I think will be "bad" and monitor that. Now I can monitor all of them at once. 

## Inappropriate words

As a community, we've chosen to set a higher standard for our players. As such, we have a restriction on a total of 3 
words and a few derivatives of those words. This system is in place because the community stepped forward and wanted to 
clean up the experience on the servers a bit. The problem with these higher standards is that we don't have admins on the 
game servers (or watch chat logs) 24 hours a day. Thus, while admins sleep, a troll can wander through and spew garbage. 
Unless a user reports this behavior, we will never be aware of it.

I've built a system to handle this automatically. The system will monitor chat logs across all servers. If a user hits the 
threshold for banning, they will be removed from the server and banned for a day. The logic to the system is this:

### Automated removal logic

 - Inappropriate terms are configured with a "weight". This "weight" will be used to calculate whether or not a user 
 surpasses a threshold set for being banned.
 - System monitors chat logs for configured terms.
 - If a term is found, the offending message is saved. The term weight is added to the user's current threshold value. 
 If this is the user's first time saying one of these terms, they start at 0 and this weight is added.
 - If user exceeds the threshold, the system issues a ban to Sourcebans. The user is then kicked from the game server. 
 The ban length will be 1 day.
 - The system keeps messages for a total of 5 minutes. If a message is older than that, the system forgets it.
 
Currently, the three inappropriate terms all have a threshold of `1`. This means they saying the words results in a ban. 
Homophobic, racist remarks aren't welcome on the Viper servers. We can't prevent it, but we can deal with offences swiftly. 
The 5 minute window is added because the community requested that excessive swearing also be limited. We don't want to 
outright ban it, but they don't want a swear filled rant to occur after every match. 

Thus, I built in the 5 minute window and the thresholds. The system is configured to catch common swear words, but the words 
have a low weight. It'd take repeated spamming of the words in a 5 minute window to reach the threshold and be removed from 
the server.

### Updated removal logic

*Updated May 17, 2012*

The automated system has been active for almost a month. I'm finding that the system has been removing the same set of 
players every other day. They aren't learning. This is despite the message they are shown when removed from the server. 
I've made a change to the logic in how long a ban will last. It provides a 4 strike system:

 - First offence: Weight of term(s) said times 1. This means, for most cases, they are issued a single day ban.
 - Second offence: Weight of term(s) said times 3. This means, for most cases, they are issued a three day ban.
 - Third offence: Weight of terms said times 21. This means, for most cases, they are issued a three week ban.
 - Forth offence: Permanent removal from the game servers.
 
The community has been very enthusiastic about how quickly users of inappropriate terms are removed. I've seen a few minor 
complaints about the permanent removal of users on the forth offence. I've told the community that *if* a user protests the 
ban and *if* they can show they've learned our rules, I will provide one additional chance after the user has waited a minimum 
of a month from from last time they were banned. If they return to their previous activities, they will be re-banned and 
they will not be able to return in the future. 

## Update at shutdown
*Updated January 20, 2015*

In January 2015, Team Vipers [shut down][s]. With that shutdown, all chat monitoring also shut down. The system was active 
from April 23, 2012 to January 4, 2015, for a total of 987 days. In that time, 4457 bans were issued for inappropriate 
language. That is over 4 users a day being removed from our player base because they couldn't maintain a respectful 
attitude. I consider that a success. I believe Viper community members did too.


 [1]: http://www.hlsw.org/
 [2]: {attach}images/vipers-chat-monitor.png
 [s]: {filename}2015_01_08_thanks-for-all-the-fish.md