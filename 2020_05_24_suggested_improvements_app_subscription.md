Title: Rise Gardens App/Subscription Improvement suggestions
Date: 2020-05-24 23:45
Tags: review, garden, hydroponics
Category: Review
Slug: rise-garden-suggestions-software
Summary: Several suggestion for the Rise Gardens app, their subscription service and product as a whole.
Status: published
Series: Rise Gardens Review


[TOC]

## Introduction

The [garden has been opened][unboxgarden]. The [garden has been assembled][assemblegarden]. The [nurseries were
opened][unboxnurseries]. The [first plants have sprouted][planting]. I've had my first lecture harvest (more on
this next time). While waiting for that harvest, I offered suggestion on how to [improve the hardware][physicalsuggestions].

Today, after about a month of growing, monitoring and watching the sprout go from seed to plate, I've been
using the application almost daily. I have thoughts on how to make it even better. I have thoughts on how to make
the subscription service better. I'll share those below.

## Mobile Application

One of the selling points of the [Rise Gardens][risegardens] is the mobile application. It's built to
help you maintain your garden. For the most part, it works well. But there are some warts that need to
be ironed out of it.

I am using the Android application.

### Loading Screen

Launching the application presents you with the Rise Gardens splash screen.

![Rise Garden App splash screen][loading]

Learn to love this splash screen. On a good day, it will show for a few seconds. On a bad day, the application will
hang forever. There is a simple work around, force close the application and relaunch it, but that's rather annoying.
I estimate that I encounter this problem every three to four times I launch the application.

### Login Screen

Once I've made it past the loading screen, I find that every four to five days my log in information is forgotten.

![Rise Garden App login][login]

So, every few days I have to log back in. It's also important to point out here that this log in is not
the same as the login you use on their website. My login on the website contains a `+`. The application
rejects that form of email address. Since I usually use `+servicename` to keep my login information easier
to remember, this is rather annoying.

I can stay logged into Slack, Hangouts, Gmail, NextCloud, and countless other applications on my phone
indefinitely. I should be able to do that same here too.

### Plant Information

One of the nice things about the application, in addition to it's ability to tell me how many and what kind
of nutrients to add, is suggestions on how when to harvest your plants. Unfortunately, I don't think it's
very accurate.

This accuracy is something I'm aware of and am keeping in mind as I add nutrients too. If I can't trust one
area, can I really trust the other? So far, with four different types of lettuce harvested, I don't see a
reason not to trust it, but that thought is still in my head.

Why is it inaccurate?

![Red Oak Harvest Day Suggestion][redoakapp]

On harvest day, this is the information presented by the application. There are a few things to
notice in this screen shot. First, "0 days to harvest", yet still showing the "Mid-growth" data.
Second, this didn't change to "harvest" until a full day later. Thinking about it now, I suspect
the datetime of when I added the plant to a nursery was recorded, not just the day and it's going
by "hours since planted", to display the stage information.

![Red Oak on Harvest Day][redoak]

This is the red oak lettuce on harvest day. This...is not ready. I will fully admit that I am not
an experienced gardener, but this is a tiny little plant. I waited almost two full weeks after this
picture was taken to actually harvest the plant. It had grown much larger by then.

I found the same type of problem with all of my lettuces. They all reached "Harvest day", but
were not ready to be harvested. The other plants I have in the garden at the moment are
longer growing and aren't to the flowering stages yet, so I'll watch for those stages in the
application versus reality.

## Subscription Service

One of the things I really like about Rise Gardens so far is the monthly subscription service,
which sends plants that I've selected and nutrients to grow them. My shipping date for May
just missed their newest plants, but I'm looking forward to getting a few of those in June.

That said, I do have a few suggestions for how to make the subscription service amazing.

### Multipacks

One of the things that I was sent during the shipping delays was variety packs of seeds.
I covered these in my post where I [opened the nurseries][unboxnurseries]. The subscription
service doesn't allow this type of package. Instead, it ships four pods of an identical
plant.

I'd like to try mint, or cat nip or a couple herbs that I haven't had or use very infrequently,
but I don't need four pods of these plants. Instead, if I could create my own
variety packs I'd be more willing to try some newer plants. This is how I got the
red oak lettuce and the kale that is in the garden now. I wouldn't have gotten four
pods of either of those, since that's just not what I need (or want, in the case of kale). But a
single pod...sure. Might as well, and the kids can try something new.

### Plan out multiple months

Plants are shipped out once a month. About ten days before that, I receive an email saying
that my shipment is coming up. Five days before shipping I have the ability to go pick the
plants I want and they are immediately locked in.

I'd love if this flow could go out multiple months. Instead of worrying about making sure
I hit that five day window to make selections, let me pick the plants I want for the next three
months or so and allow me to change those until a few days before shipping. Then I know what I'm going
to be getting and if I decide that I don't like something or want to try something else in a
few months I can still go change it.

## Wishlist

I have two items that I really think would make Rise Gardens fantastic and both are software!

### Access to old water readings

I know the application sending my water readings somewhere. I have to log in, and the
application doesn't work when the internet is down (tried...doesn't load). Give us the ability
to pull those old readings via an API call so that I can plot them. I love data and I love
visualizing date. More information via an API call would be even better. Things like:

 - How long the lights were on
 - Water level
 - Suggested nutrients to add
 - Suggested pH balance to add

Maybe if enough people can see and play with their old data we can even make some suggestions
on what would work better. As it stands though, we don't have access to it. We put in water readings
and that's it. It's gone.

I'd really love the ability to pull that old data back.

### Allow better light control

Right now light control is pretty limited. You pick a time for them to turn on and a number
of hours for them to stay on. That's it. (The hour thing is why I thought about when the
plant status shifted from "Mid-growth" to "Harvest" as well)

![Light control][lights]

I'd like the ability to be able to turn these lights on and off through out the day. As the sun
crosses the sky, there are hours where the plants are in direct sunlight. I could turn the lights
down to 50% intensity during those hours or off all together.

There are two open ports on the controller. A piece of hardware that can detect light level
could be used to control on/off or light intensity level. Obviously you couldn't put there right next
to a plant, because the LEDs would affect the sensor, but put it on a post and it could tell if you if
the sun was shining on the garden.

This type of thing could also be used in different seasons to determine when to turn the lights on
or off automatically. I can use natural light for part of the day, but when the days get shorter, I need the
lights to stay on longer.

## Summary

I'm nearly done with this series. I am only planning one last post - a final summary of everything
so far and a quick summary of first harvests (I think it'll only be lettuce at that point).

Here I've covered the things that have frustrated me (application loading/login), things that
can be improved (plant information) and offered a few suggestions to make the software and subscription better.

Overall, I think everything I've mentioned is do able with some development time. I am looking forward to
how Rise Gardens continues to improve the product.


 [risegardens]: https://risegardens.com/
 [unboxgardens]: {filename}2020_04_22_rise_garden_unbox.md
 [unboxnurseries]: {filename}2020_04_24_nursery_unbox.md
 [assemblegarden]: {filename}2020_04_26_assembling_garden.md
 [loading]: {attach}images/garden/6_app_suggestions/riseapp-loading.png
 [login]: {attach}images/garden/6_app_suggestions/riseapp-login.png
 [redoakapp]: {attach}images/garden/6_app_suggestions/redoak-harvest-day-app.png
 [redoak]: {attach}images/garden/6_app_suggestions/redoak-harvest-day.jpg
 [lights]: {attach}images/garden/6_app_suggestions/light-schedule.png
