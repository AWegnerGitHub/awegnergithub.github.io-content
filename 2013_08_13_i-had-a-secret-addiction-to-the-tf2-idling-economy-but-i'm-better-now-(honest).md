Title: I had a secret addiction to the TF2 idling economy but I'm better now (honest)
Date: 2013-8-13 0:01
Tags: automation, programming, gaming
Category: Side Activities
Slug: i-had-a-secret-addiction-to-the-tf2-idling-economy-but-i'm-better-now-(honest)
Summary: Hi everyone, I'm Andy, and I used to idle. 
Status: published

[TOC]

## Introduction

On July 11, 2013, Valve released a [patch][1] to Team Fortress 2 to limit idling. Idling is the process of launching
Team Fortress and then letting it run for a few hours and collecting item drops. It used to be simple to do by utilizing
the `-textmode` [game parameter][2]. This would "launch" the game without graphics. From there, you just let it run and 
come back in a few hours with 8-10 new items (your weekly limit). 

The idea behind this was to then trade or craft these into metal. The metal could be used to trade for keys, hats, etc. and
all you to be "rich". The quotes are there because I do realize that being rich in a virtual game does not make one rich in
the real world. But, since the [Mann-Conomy Update][3] introduced this massive meta game to Team Fortress, I've tried to stay
out of it. 

I failed. I didn't just idle one account. I idled 17 accounts. That is between 136 and 170 items a week. That's between 68 to 85
scrap metal a week. That's between 7.55 and 9.44 refined metal a week. That is between 3 and 4 keys a week (depending on 
who I trade with). Keys are the backbone of the economy. Get enough of those and you can trade for anything else. 

I honestly don't know what my goal was. I wasn't interested in the fancy hats (ooooh...shiny pixels). I didn't open crates
with the keys. I did utilize the keys to get [Tour of Duty][4] tickets for me and some friends. That's honorable, right?

## How did this happen?

This all started with [the bot I built for Vipers to handle the raffles][5]. I realized that I could automate much more
than trading with players. I could automate trading between bots. I could automate *crafting* - which is relatively time
consuming, especially when you have 136 items to go through. I could automate trading with established scrap bank bots. These
bots will take any two weapon drops and give you one scrap, even if the two items can't be crafted together. The idea is
that they get enough weapons that eventually they'll be able to craft it down to metal.

### The set up

To idle 17 accounts I needed to figure out what my computer could handle. I had to figure out how to run multiple instances
of Steam and Team Fortress at a time. Enter an application I'd utilized before: [Sandboxie][6]. This application provides 
isolated sandboxes for applications to run in. Normally Steam won't run more than once on a machine. But, if you launch
it via Sandboxie, the host OS (and other Sandboxie environments) can't see that Steam is running in another instance. 

A bit of experiementation showed that I could handle 6 accounts idling at a time and have the computer remain just barely 
usable. I created new accounts and split them into which day of the week they'd be run. I had three days out of the week 
designated for idling. 
 
A batch script was built to launch the Sandboxie environments of the day that was to idle. Then it'd launch Team Fortress
in each of those environments. Then I'd go to bed. When I woke up the next morning, I'd shut down the environments. I'd 
repeat this for two more nights. At the end of the third night, I had all the items I could get for the week.

Accounts could not be free accounts. This meant that I needed to either buy an item for each account, or trade for an 
["Upgrade to Premium Gift"](https://wiki.teamfortress.com/wiki/Upgrade_to_Premium_Gift). 
 
### Getting items to a single account

The next step in the process was to convert all the items to metal. Normally, this would take place by either logging into
each account and crafting there and then trading everything to a master account, or trading everything first then crafting. 
In either case, 17 accounts is a lot to handle and trading/crafting is rather boring.

My solution was to modify the raffle bot. I'd designate one account as a master account. This would be the account that
received items. All others would dump items to it. I'd log into the account that was receiving items and initiate a trade
with each other bot in turn. I'd issue a command `add all` and that bot would dump it's inventory into trade. Making it
through all the bots would take 5 minutes. Previously it would take me an hour or more to log into each account and
manually add items to the trade windows and then confirm the trade on both sides. *yawn*
 
### Crafting

The next step was crafting items to metal. The rule was that any two weapons utilized by the same class could be
crafted to scrap metal. That sounds simple enough.

After lots of trial and error, I'd built a set of commands that would select compatable weapons and craft them together. This
particular aspect wasn't documented by Steam (or the revese engineered SteamKit2 library I was utilizing). Crafting would 
take about 15 seconds to get all compatible items to scrap and then combining 9 scrap to get a refined metal. This saved 
inventory space.

With the simple command `craftall`, all those new weapons would be crafted into metal and then combined into refined metal. This 
would be done in less than a minute. Previously, I'd have to initiate each crafting session, add all items per craft, wait for the
craft to complete and then repeat. This was another 15-30 minutes saved.

### Left overs

After crafting, there would still be left over weapons. There were weapons that had run out of same class items to craft
with. The solution was to trade these away in groups of two to [ScrapBank.me][7] and get scrap metal back. This could be
further combined to refined metal again wasting space. 

### Metal to Keys

At this point, human intervention was required again. I had to convert my metal to keys. I did this via trading. The easy
way to do this was to find someone running a keybot, that would take metal for a key. Many exist, but all of them overcharge.
The other option was to look for a trader that was getting rid of a bulk set of keys and then trade them. In either case,
I'd end the week with 3-4 keys. I'd trade these for Tour of Duty tickets and go play a game with friends.

The actual work required on my part was starting the idling for the night, shutting down idling in the morning, starting
the automated trade, crafting and trade left overs processes and finally trading metal for keys. What used to take me hours 
to do each week, I could not do in less than an hour (the bulk of which ended up being trading metal for keys).

## What changed?

Valve released a patch to limit how effective idling was. Their "Mann-Conomy" was seeing rapid inflation. The price of 
keys (which they sold for physical money) in metal was rapidly increasing. From the time I started this to the time I ended
it, it jumped from 2-3 refined per key to 9-10 per key, with no sign of stopping. Casual players were complaining they couldn't
get enough items to trade for this stuff. The patch also required that you click to confirm each new received item.

## I'm ok. Honest.

I tried for a few days to find an effective work around. I didn't. When I wasn't able to find one, I transfered all items 
to the master account and went through the process of converting to keys. With that, I purchased the final set of 
Tour of Duty tickets. The idle bots were shut down. 

It's been a month now. The bots are still down. I'm still around. Looking back on this, I'm happy Valve broke this. It wasn't
doing anything for me other than providing me with something to do: "Need to idle tonight", "Need to craft and trade today", etc. 
Now I have that time back.
 
I am pleased with the technical challeges I over came to get this done though. Crafting was the biggest, but I think I'm
most proud of the automated transfer of items to the master. Since the bots had to communicate via Steam and not via
a local application, working out how I was going to do that took some time.

---

That, my friends, was my addiction to the meta game of a hat similator in a first person shooter. I'm over it now and 
looking forward to some other project.



 [1]: http://www.teamfortress.com/post.php?id=11105
 [2]: https://developer.valvesoftware.com/wiki/Command_Line_Options#Command-line_parameters_2
 [3]: https://wiki.teamfortress.com/wiki/Mann-Conomy_Update
 [4]: https://wiki.teamfortress.com/wiki/Tour_of_Duty_Ticket
 [5]: {filename}2013_02_25_give-some-refined-win-some-prizes.md
 [6]: http://www.sandboxie.com/
 [7]: http://scrapbank.me/