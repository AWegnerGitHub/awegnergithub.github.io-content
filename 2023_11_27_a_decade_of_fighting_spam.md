Title: A Decade of Fighting Spam
Date: 2023-11-27 15:45
Tags: Stack Exchange, moderation
Category: Side Activities
Slug: decade-fighting-spam-charcoal
Summary: A run down of what a decade of spam fighting looks like on the Stack Exchange network.
Status: published

[TOC]

## Introduction

Charcoal is nearing a decade of existance. In January of 2024, the Stack Exchange community will have been fighting the good fight of keeping spam off the platform. I've written about a [machine being able to flag spam][1] in the past. I've also posted [the original][2] and it's follow up on being able to [spam flag even better][3] on Stack Exchange itself.

Recently, I was asked to talk a bit about a hobby of mine. I put together this presentation. 

![A Decade of Fighting Spam by the Stack Exchange community][slide1]

## What is Stack Exchange?

To set a bit of context for those need it. Stack Exchange is a network of over 180 sites covering almost any topic you can think of. It's a question and answer network. The slide you are seeing here are just a handful of the sites of more interesting logos - but you can see they cover a range of topics from [professional work place questions][4], to the [intricacies of the English Language][5], to [Data Science][6] and [gaming][7].

![A handful of Stack Exchange site logos - Workplace, English Language, Data Science and Arquade included][slide2]

But by far the largest and most popular is [Stack Overflow][8]. With 24 million questions covering any programming language or framework you have used. It consistently ranks in the top 500 most visited sites on the internet - depending on what service is doing the measuring. Basically, it gets a lot of eyeballs looking at it daily.

Which means it's a target for spam.

![The largest Stack Exchange site is Stack Overflow][slide3]

## What is spam?

The network and the community within it settled on a fairly standard definition of spam: 

> A post exists only to promote a product or service and doesn't disclose author's affiliation. 

The images here show what the site looks like when the community systems aren't operational. This is the front page of two sites and if you look closely at the time stamps, you'll see that these posts occurred within about 10 minutes.

If users - new or experienced - come to the site and see this, they start to turn away. 

Back in 2013/2014 this was common. Spam posts would stick around for hours and a group of users decided they could help out across the network by flagging these posts more quickly.

![Two examples of spam filled front pages on Stack Exchange sites][slide4]

## What is flagging?

The final bit of context that is needed is: flagging. It's exactly what you'd think it is. The goal of a flag to bring attention to the post by forcing it in the community review queues. This gets more people to look at it. Stack Exchange is built around community moderation. There is very little that elected "Diamond Moderators" need to handle that the community can't handle.

If enough people flag a post as spam, it's automatically deleted. The community and company decided that getting 6 people to agree a post is spam is an appropriate number.

Once a post is removed as spam, the post is locked, deleted and the author has 100 reputation points removed. These are the visible actions. The reputation hit is to prevent - or slow - a spammer from getting more privileges within the network. 

Behind the scenes, a spam post also triggers company checks against future posts matching similar information to the user. These aren't publicly disclosed. But, the company is fairly conservative in terms of blocking users. 

![Flagging brings attention to posts for others in the community to see, act upon and eventually remove spam][slide5]

## What is Charcoal?

The community hates spam. It's a bad user experience at best to have a page filled with spammy posts. It also makes the site, and community at large, look rather seedy. This isn't great for a community and a company that has built its reputation on accuracy and trust.

Charcoal was created to watch for spam across the 180+ sites. Actually, when we started it was less than half of that, but over the past decade the network has grown and the anti-spam systems have grown with it.

The community has a two phase process to dealing with spam. First is alerting the spam fighting community of potential spam. Users can go cast their flags across the network and deal with it. Second, for the truly egregious spam, the system can utilize the community's flags and automatically cast those flags.

How's this work?

![Characoal is the community organization that runs two systems to deal with spam. The first alerts users about potential spam and the second casts flags against detected spam.][slide6]

## SmokeDetector and Metasmoke

There are two systems behind this community effort - SmokeDetector and Metasmoke.

SmokeDetector, affectionatly named "Smokey", is designed to be the early warning system. It quickly provides a yes/no decision on whether a post is spam and alerts users for manual action. It passes off the more intense confidence checks and automatic flags to MetaSmoke.

![Two systems create the anti-spam system. SmokeDetector for a quick spam/not-spam decision and Metasmoke to handle confidence checks and automatic flags][slide7]

Every post on the network goes through the process below. A user clicks the submit button and Stack Exchange does their few checks - remember these are black boxed - and if the post makes it through these it gets published to a real time web socket.

SmokeDetector does a quick "Is this spam?" check. If it is, it's posted to chat rooms around the network - to the network wide Charcoal room and usually to a site specific room if the room is utilized enough. Users than go and investigate and if they agree that it's spam, cast a flag. After 6 of these are cast, the post is removed. Hooray! Another victory again spam.

When Smokey posts that spam is found, it also sends a message to MetaSmoke. This system is checking how confident we are that this is spam. If there is high confidence, it will start utilizing community member flagging privileges to cast spam flags on the post as well. If there isn't high confidence, no automatic flags will be cast. 

The goal is to remove the spammy posts as quickly as possible - and by utilizing automatic flags the number of people that have to go do this manually is reduced. Due to larger community and company discussions and outcomes, the system will not cast all 6 flags except in very very rare circumstances. Someone has to agree with the machines here.

![A process flow diagram of the Charcoal systems][slide8]

## How to detect spam

What's spam detection look like? Over the last decade we've tried things like classification schemes, machine learning algorithms, and a handful of AI attempts. But, by far the most reliable has been...

Regular expressions.

(Take a deep breath fellow engineers)

Each post - goes through thousands of regular expressions. Each expression is weighted based on how likely matching that particular expression means the post is spam. The higher weights are posted into the chatroom kicking off this entire process.

![Regular Expressions run the world - or at least anti-spam systems on Stack Exchange][slide9]

The community has built watchlists and blacklists over the decade to help find these posts. 

Watchlists are experimental checks. Spam evolves over time. It's actually pretty interesting to watch a dedicated spammer craft their posts to get it to last on the network more than a few minutes. These watchlists are designed to allow the team to test regular expressions without fear of automatically flagging something during testing.

Blacklists are finalized regular expressions that catch spam with a high number of true positives and very low false positives. These the weight spam checkers.

Like Stack Exchange itself, the spam fighting community has built tooling that allows work to be done without a high level user to be around. Users can watch for a new regular expression.

Users that aren't trusted just yet, will have their request created as a pull request in GitHub that needs to be approved. Trusted users will get their watchlist automatically added to the system. The same holds true for blacklisted items.

![Watchlists - Experimental regex detections and Blacklists - Tried and tested regex detection are added by users to fight spam][slide10]

But, watchlists and blacklists are only half the problem. The other half is validating that these are accurate. As posts are detected as spam, users provide a signal back to the system on whether a post is a `tp` - True Positive - Spam 

Or a false positive - `fp` - not spam. These feedback to the watchlists and will prevent elevating watchlists that are inaccurate to a full blacklist.

Sometimes, a post has features that the system doesn't detect as spam. In those cases, the community can manually report the post. This triggers the alerts through out the chatrooms so that others can flag it and get it removed. It also allows the community to find potential patterns to watch for in the future.

Users that aren't trusted yet get pull requests created for their patterns. All of this can be handled and approved within the chatrooms. A lot of this system is built on top of, and keeps most users within, the Stack Exchange ecosystem.

![Community Feedback on the detection reasons is critical to ensuring the system has reliable capabilities][slide11]

I mentioned the weighted reasons on a detected post. When these are posted in chat, the reasons are also posted as well as the weight of the post. The one on the slide below is particularly bad. Generally anything over about ~225-250 is spam with higher numbers becoming more and more certain. 

These weights shift over time and as a regular expression is utilized more. This keeps the system flexible.

For this particular post, the system determined it was spam and cast three automatic flags from our users. Each user that grants permissions for the system to utilize their flags - because they are responsible for the usage of the flags - can set their threshold for when to allow their name to be used. 

The 4th flag here came in via a user script the community built, but was not automatically cast. The remaining two flags would have come from the users of the site or from someone that saw the Smoke Detector alert and manually flagged it. Metasmoke doesn't have a record of that because it didn't go through Metasmoke.

![Automatic flags are cast when a post exceeds the threshold set by our users. The system can cast multiple flags from multiple users.][slide12]

## By the numbers

Let's look at some numbers.

SmokeDetector has been running since January of 2014. We didn't start recording stats until about 18 months later though, so the dates in the graph start in August 2015. Initially, the system didn't have watch lists, which is why you see the blue and orange lines are pretty close together.

Around mid 2018/early 2019 we introduced watchlists. This was done because we started seeing persistent spammers. These were spammers that noticed their posts were being deleted quickly and worked to find ways to change the message to stick around longer. 

The chatrooms are open and based on some messages we have removed, it's obvious the room is watched by the bored spammers. The watchlists reduced the true positives. But because we didn't ever separate the data between blacklists and watchlists the lines began to separate.

In early 2017, autoflagging was introduced. With autoflagging the system can reduce the time on site for nearly half of the true positives.

You'll notice a major spike in the summer of 2022 and a dip in the summer of 2023. The spike was for a massive spam wave. This was the work of a spammer that had access to a lot of geographically distributed systems - which bypassed Stack Exchange's built in protections - and was a persistent spammer or team of spammers that watched the public chatrooms for changes the spam fighting community made to detect their posts. This went on for 2-3 weeks with thousands of posts being made, adjusted, and deleted. Ultimately, the spammer was blocked at the Stack Exchange level based on heuristics the Charcoal team presented.

This past summer, in 2023, the dip you see is because Stack Exchange experienced a crisis of confidence from the community at large. [Moderation work stopped for the months of June and July in protest of the company's policies][9] toward generative AI on the platform. Charcoal participated in that. While not fully resolved, some of the worst policies were reworked with input from the larger community and work resumed. 

![A graph showing total detections, true positives, and automatic flags over time.][slide13]

The goal of the Charcoal project is to remove spam quickly from the site. Flags that are cast by the system are tracked and we can clearly see that more automatic flags mean the post is active for less time.

When there is no system cast flags, an average spam post lives for 21 minutes on the site. If the system casts all 6 - which is only utilized during a spam wave like in the summer of 2022 and with company permission - a post lives for 16 seconds. During day to day operations, the system is configured to cast 3 automatic flags. This was determined by a lot of conversations with individual sites around the network and what they felt comfortable with.

![Timing of how long a post is visable based on the number of automatic flags. At the default 3 flags, a post will be visible for less than 5 minutes][slide14]

SmokeDetector has over 103 thousand commits to its repostory over the last 10 years with 90 different code contributors. In the slide below, the top two graphs show that it's rulesets are updated daily - except for this past summer.

Over the course of a 24 hour period, flags are automatically cast from nearly 420 different users around the network.

Finally, the entire goal: over 450,000 spam posts have been identified and deleted by the system and the community in the last decade.

![Daily ruleset updates, 100k+ code commits, 90 contributors, 420 users with automatic flags daily all results in over 450,000 spam posts removed in a decade][slide15]

## Conclusion

You now have an idea of how one of the largest sites on the internet handles spam. I do want to point out that StackExchange operates very differently from sites like reddit or YouTube or Facebook which spent a lot of company time building their anti-spam systems. Stack Exchange built basic protections themselves and then saw the technical community members step up and take on the challenge. 



 [1]: {filename}2017_02_19_can-a-machine-be-taught-to-flag-spam-automatically.md
 [2]: https://meta.stackexchange.com/q/291301/186281
 [3]: https://meta.stackexchange.com/q/307585/186281
 [4]: https://workplace.stackexchange.com/
 [5]: https://english.stackexchange.com/
 [6]: https://datascience.stackexchange.com/
 [7]: https://gaming.stackexchange.com/
 [8]: https://stackoverflow.com/
 [9]: https://andrewwegner.com/category/stack-exchange-strike.html
 [slide1]: {attach}images/charcoal-10years/slide1.png
 [slide2]: {attach}images/charcoal-10years/slide2.png
 [slide3]: {attach}images/charcoal-10years/slide3.png
 [slide4]: {attach}images/charcoal-10years/slide4.png
 [slide5]: {attach}images/charcoal-10years/slide5.png
 [slide6]: {attach}images/charcoal-10years/slide6.png
 [slide7]: {attach}images/charcoal-10years/slide7.png
 [slide8]: {attach}images/charcoal-10years/slide8.png
 [slide9]: {attach}images/charcoal-10years/slide9.png
 [slide10]: {attach}images/charcoal-10years/slide10.png
 [slide11]: {attach}images/charcoal-10years/slide11.png
 [slide12]: {attach}images/charcoal-10years/slide12.png
 [slide13]: {attach}images/charcoal-10years/slide13.png
 [slide14]: {attach}images/charcoal-10years/slide14.png
 [slide15]: {attach}images/charcoal-10years/slide15.png