Title: Updating the design of the site
Date: 2022-11-17 10:00
Tags: technical, Pelican, meta
Category: Side Activities
Slug: relaunch-personal-site
Summary: I've updated this site with a new theme and some under the hood improvements. This article covers those updates and what I hope to accomplish.

[TOC]

## Welcome to the update!

I have a new theme for the site, and I think it looks great! This is a heavily modified 
[Flex](https://github.com/alexandrevicenzi/Flex) theme. I talk about the changes I made to the theme below, as well as my goals 
for the site going forward. A new theme isn't going be responsible for hitting all of those goals, but I think it will help. The
site was pretty bland before. It did what it needed to do, but it didn't look the best doing it. My skillset is _not_ front end
development, but I am rather proud of the new theme and the changes I put in place.

If you're curious about what the site used to look like, jump down to the [Where the site was](#where-the-site-was) section.

The default version of the Flex theme can be seen on it's [demo site][flex-demo], to give you an idea of the changes I've
made to my instance.

### Look & Feel

The logo is perhaps the most subtle change. It's designed, intentionally, to not be a circle (to common on other sites )or a square (to boring).
It's an off shape blob. Take a look at it. It very slowly shifts too. You can see it along the bottom of the shirt most clearly. It's designed
to move only a little and pretty slowly, so that it's not distracting. It's just active enough to catch your eye if you are looking but not 
designed to pull focus from the article you are reading.

The default flex theme is very...neat. There are nice crisp lines. It's very clinical. I changed this just a little with splash of a curved
red border between the side bar and the main article. It's not a major change, but it adds a little personality. Combined with the 
blob/animated head shot, it makes it feel unique among the flex theme'd blogs I looked at to evaluate if I liked the theme.


A favicon is such a little thing, but I didn't have one with the old theme. Having one now adds just a little bit more of a 
professional touch and it was so easy to do. The color of the favicon, the side bar, and the responsive bars from the tag and 
category pages all match.

### ToC

The table on contents on the side bar is an addition I made, because I dislike when the table of contents scrolls off the screen. It's the same 
reason I froze the table of contents to the side bar on the previous iteration of the site. When I'm reading, I like to know how far I am in 
and article and be able to easily jump to another section. Since I am also a reader of my site (especially on the technical posts), I try
to make it easy for myself too.

### Tags/Categories

The tags and category pages on my old site where bad. They were a giant list of meta-data and a link to a post. It was messy. I didn't like
it, but I also didn't use it so I spent very little time trying to make it work better. With a new theme, I wanted to make the pages more 
useful, at least to me. I found an excellent post by [John Paton][paton] on how to make a responsive bar chart for these pages.

Take look at my [tags][tags] or [categories][categories] pages. To me, that looks wonderful. I can get a quick look at how many articles 
are in each section and clicking on one brings me to a page specifically for articles in that slice of meta-data. 

### SEO

Before you run away...this is a good thing, and I've been doing it for a while. All of my [reviews][reviews], have been sporting Schema.org
Review microdata for years. My blog posts have been sporting Article microdata. This was all hard coded into my template. It was beyond time to
update to utilize JSON+LD and provide even more context to the topics I write about. 

Initially, I selected Flex because it claims to support Rich Snippets. My results on that are fuzzy, at best. The snippets provided are not full
rich snippets. However, they provided a great base. I've updated my instance to support JSON+LD for Person (click view source on my 
[About Me][about] to see it), Article, Review and Breadcrumbs (see the navigation bar at the top of this page). Search engines use all 
of that to get a better idea of what's on the site. I have a few more I want to implement too, but this gets me beyond parity with my old theme. 

I've also made sure that canonical links are included in my articles. I have a couple that Google sees as duplicates due to UTM tags I stuck on
links a while ago. Having cononical links in place should resolve some annoying errors and warnings I see in my search console.

## Goals of the update

I have a few goals with this newly updated site. A couple relate directly to the theme, and a few are general site goals. 

* Updated Tool set - I was running an old version of Pelican and missed out on some new features. By updating, I'm hoping that I 
can remain closer to the current release. I like to get the newest features, and that alone feels better.
* A useful tags and categories page. Like I mentioned above, they used to be useless. Now, there is more than just a list of 1-2 
work phrases sitting on my site across two pages. If I start another series - I believe I have two right now total, I may add a 
similar series page.
* Site face lift. As you can see below, the old version was pretty bland. This update adds some color, makes a few things more accessible,
and generally feels less like reading a sheet of paper on a screen. It's not the flashiest thing on the internet, but you can see 
by my social media links to the left, that I'm not that type of person anyway.
* Better SEO. Obviously a template doesn't immediately solve this problem, but it will help. Plus some other plans to add more content (gasp!)
and it should help. Right now, though, "Andrew Wegner" returns this page 9th on Google. It's behind someone with a different last name spelling,
someone with a different first name, and a few other "Andrew Wegners". While search for a job, I couldn't depend on my site being the one that 
was clicked. 
* Lastly, a vanity goal, is to get a Knowledge Graph when searching for my name. It'll take a while before I can get there, but some of the 
SEO work I'm putting in now will, hopefully, pay off. 

## Where the site was

One last trip down memory lane, as the template that has served as the base of the site since the intial [migration from Wordpress in 2015][migrate] is retired. The old template was using [Elegant][elegant]. I am the [#7 contributor to the theme][1] at the time of publishing 
this. That's not saying a whole lot, with my brief spurt of activity in 2019, but it was a contribution that I hope others using the theme 
are able to utilize. 

I was stuck on the Pelican 3.0 branch. I really don't remember why. I remember attempting to upgrade to Pelican 4.0 and everything broke. At the 
time I was unable to spend a significant time to fix it, so I rolled back and stuck with the latest version that ran. 

Going through this revamp, I upgraded to the latest Pelican, the newest Markdown and go all the new fancy things. We'll see if they actually 
improve my experience. From a user stand point, not much should change other than a visual overhual. It's a static site and is designed to be
pretty non-interactive. On my side though, I'm hoping things are a little less brittle. 

On to the last pictures of the old template. 

The Old Home Page:

![AndrewWegner.com Old "Elegant Theme" homepage design][old-home]

A few other screenshots of the old site sit here:

 - [The Archives][old-archives]
 - [Review article][old-review]
 - [Standard article][old-article]
 - [Tags page][old-tags]
 - [Category page][old-category]


 [old-home]: {attach}images/andrewwegner-elegant-homepage.png
 [old-archives]: {attach}images/andrewwegner-elegant-archives.png
 [old-review]: {attach}images/andrewwegner-elegant-review.png
 [old-article]: {attach}images/andrewwegner-elegant-article.png
 [old-tags]: {attach}images/andrewwegner-elegant-tags.png
 [old-category]: {attach}images/andrewwegner-elegant-categories.png
 [flex-demo]: https://flex.alxd.me/
 [paton]: https://johnpaton.net/posts/responsive-bar-chart/
 [tags]: /tags.html
 [categories]: /categories.html
 [reviews]: {tag}review
 [about]: about
 [migrate]: {filename}2015_05_03_why-i-moved-from-wordpress-to-pelican.md
 [elegant]: https://elegant.oncrashreboot.com/
 [1]: https://github.com/Pelican-Elegant/elegant/graphs/contributors