Title: Can a machine be taught to flag comments automatically
Date: 2015-1-02 8:47
Tags: Stack Exchange, machine learning, automation, programming
Category: Programming Projects
Slug: can-a-machine-be-taught-to-flag-comments-automatically
Summary: Description of how I automatically flag comments on Stack Overflow
Status: published
modified: January 9, 2016

[TOC]

## Introduction

This post was originally [published][a] by [me][b] on Meta Stack Overflow on December 14, 2014. I've republished it here
so that I can easily update information related to recent developments. If you have questions or comments, I highly
encourage you to visit the [question][a] on Meta Stack Overflow and post there.

---

TL;DR: Yes it can.

---

## Background

On June 27, 2014 Skynet awoke. It looked at Stack Overflow and thought "Why are all these people being so chatty and talking about obsolete things? I should nuke them all!" Fortunately, Skynet was a baby and only had access to my 100 comment flags a day.

Prior to this activation date, the system was fed with 10,000 "Good Comments", "Obsolete" comments and "Too Chatty" comments. These comments were taken from the [Stack Exchange Data Explorer][1]. The "Obsolete" and "Too Chatty" comment types had to meet the following criteria:

 - Total comment length of less than 100 characters
 - Comment has a 0 score
 - Had variations of the following phrases:

Phrases

    '%mark%answer%'
    '%mark%accept%'
    '%accept%answer%'
    '%lease%accept%'
    '%mark%answer%'
    '%thank%you%'
    '%thx%you%'
    '%.....'
    '+1%'
    '-1%'


"Good Comments" were assumed, initially, to be anything that didn't fall into the above criteria

This provided a base of 30,000 comments that were roughly categorized into 3 distinct groups. Manually scanning the classifications took several weeks, and through this some of the groupings were changed to reflect a more appropriate classification. Not all comments less than 100 characters starting with "Thank you" are "too chatty", just as not all comments over 100 characters are good comments. I reclassified these comments as if I had encountered them on Stack Overflow.

My next step was to train a classifier. I had initially assumed that I'd start with a Naive Bayes to get a baseline and then work to something more complicated from there. Perhaps, extract text features, user information, etc. and build a fancy classifier. My initial tests showed that the Naive Bayes was accurate 80-90% of the time with test data.

I combined the classifier's certainty of classification with an acceptable threshold of when I'd allow a flag to be issued in my name. Tuning these threshold took a few weeks but eventually I determined the following thresholds were appropriate for my use:

	Type			| Threshold		| Flagging Enabled
	--------------------------------------------------
    too chatty		| 0.9997		| True
	obsolete		| 0.99			| True
	good comment	| 0.9999		| False

When a comment is classified, if it exceeds the threshold for one of the above, it is recorded into my database for future retraining. If flagging is enabled, the API is [utilized][2] to issue an [appropriate][3] flag. Obviously, I don't want to flag good comments, but I do want to record them so that I can reuse the data in a later training step.

---

## Results

What have the results of this experiment been? From my point of view, I'd venture that it's been successful. I have automatically flagged over 17,000 comments. As of December 17, 2014, the process has been running for 173 days. My comment flagging stats are currently:

	26885	comments flagged
	26714	deemed helpful
	171	    declined

Started at (approximately):

	9885	comments flagged
	9847	deemed helpful
	38	    declined

This gives me an overall accuracy of 99.36%. Down from 99.61% when no automated process was involved.

---

There are pictures that help tell this story too. In this first one, we see that the rolling 10 day average for the number of declined flags has stayed below two flags a day. In October, there was a two week period where the rolling average was 0 and nearly a month long period where the system did not make any mistakes.

![Flags per day with rolling 10 day average][4]

Since November, the number of mistakes has climbed slightly. The biggest number of mistakes it has made was the opening day of Winter Bash 2014. Purely speculation, but I believe this was the moderators being protective of content and not wanting people to farm the [Resolution hat][5]. Of course, I don't know this. Another theory I have about this uptick since November is the adjustment to day light saving time. My process starts 10 minutes after UTC. It is possible that this earlier hour has caused my flags to be processed by a different moderator, or a moderator that is more awake/less hungry/in a different mood than previously at this point in the daily rotation cycle or because they [lost their keys][6] that day.

---

![Total flagged vs Total Declined][7]

Except for 3 days, since June 27th, the process has flagged 100 comments a day. In this chart, you can see the number of declined comment flags along the bottom.

---

![Number of comments saved per day][8]

Finally, this chart shows the number of comments that the system wanted to act on (and a rolling 5 day average). When the system was brought online, it was acting on 700-800 comments a day (saving to my local database). Many of these were being classified as "Good Comments". You can see the day that I adjusted the threshold for "Good Comments" to be acted upon (saved). The drop in the number of comments the system saved is dramatic. Instead of saving 700-800 comments daily, the system now averages about 150 comments to save. Since I don't flag "Good Comments", I feel this is the appropriate action to take.

---

### Flagged but declined

As shown above, I've had comments flags declined. Some of these obviously should have been and required a retraining or threshold adjustment on my part. Others, in my opinion, should have been removed as noise. Below is a small sampling of both types of comments.

Recent comments that I feel are noise:

 - [yes thank you so much for you help it works sorry for the late reply](http://stackoverflow.com/questions/27420526/i-want-to-play-from-frame-2-and-then-stop-at-frame-3/27425983#comment43388489_27425983)
 - [Wow it works. Thank you very much!](http://stackoverflow.com/questions/27476522/how-to-call-a-function-by-a-pointer/27476639#comment43387801_27476639)
 - [wow that works!Thanks so much for your advice!](http://stackoverflow.com/questions/27284958/why-thread-id-creates-not-in-order/27285031#comment43038003_27285031)
 - [Ok, the works great, thank you so much!](http://stackoverflow.com/questions/27375504/remove-legends-for-each-point-and-keep-only-those-which-are-outliers-for-ggplot/27380631#comment43387125_27380631)
 - [Thank you very much for your explanation, you rock dude !!!](http://stackoverflow.com/questions/14907518/modal-view-controllers-how-to-display-and-dismiss/14910469#comment43386201_14910469)

Here are some comments that were incorrectly flagged:

 - [@Spina: yes. Check my answer. You can simply point MONGO_URL to an invalid URL.](http://stackoverflow.com/questions/18545905/meteor-without-mongo#comment42850716_18545905)
 - [Sorry, my error. I was: "position", not "display". Check it: jsfiddle.net/hvfku99c](http://stackoverflow.com/questions/27007685/how-can-i-position-divs-at-the-bottom-of-container-div-and-inline/27007772#comment42544238_27007772)
 - [I believe UI.registerHelper is, being deprecated. Please check my updated answer.](http://stackoverflow.com/questions/26745185/multiple-spacebar-conditional-operators/26745790#comment42078870_26745790)

Other comments are flagged but then edited prior to a moderator seeing the comment. The edit adds information to the post, thus the declination is justified:

 - [Yes, I have indexes. Let me show my schema](http://stackoverflow.com/questions/27406267/neo4j-very-slowly-using-shortestpath#comment43271781_27406267) was edited to the much more useful: `Yes, I have indexes for UUID and Permission. In fact rlationship is a variable length here (e)-[rp:Has_Pocket|Has_Document*0..]->d`
 - [Here is the question i had posted first using FIleStorage issue](http://stackoverflow.com/questions/26535662/how-to-read-files-in-sequence-from-a-directory-in-opencv/26536198#comment41709286_26536198) was edited to include the link to the referenced post.

It's also worth noting that despite getting flags declined, some comments do eventually disappear. This is due to either flags raised by other community members putting the comment back in front of a moderator or by simply accumulating enough community flags for the system to act automatically. In either case, the desired result of removing noise has been accomplished.

 - [Oh, derr. good point. Edited.](http://stackoverflow.com/questions/27006363/node-js-parse-filename-from-url/27006555#comment42544432_27006555)
 - [You're right! Hopefully you see my point anyways.](http://stackoverflow.com/questions/27073761/redefining-the-hitbox-of-objects/27073838#comment42659999_27073838)

---

## Lessons and Observations

- Replication to other sites would depend on site culture

As a (fairly) non-subjective site, Stack Overflow made a good test case for this. On a site like [Community Building][9], [Pets][10], [Parenting][11] or other site that accepts subjective answers, "too chatty" would be much harder to classify.

- [+/-1 has been discouraged][12]

The observation I made on my own that comments with this type of content were distracting has been noticed by others as well. This was actually a very nice validation of my own process and some of the [results][13] posted on that thread show many such comments continue to be noise. Of course, this change did also force users to modify their content and may have added new patterns that can be utilized in future training.

- Ability to [automatically check flags][14] would be great so that automated runs could be paused if it goes crazy

The process of checking that my flagging history remains accurate is time consuming. The status of a flag can't be acquired via the API. I've submitted a feature request for this information to be added to the API. With this information, flagging can be paused or stopped if X number of flags are declined.

- Stack Overflow's volume of comments is a crutch.

Due to the [high volume of comments][15] and limited number of comment flags my account has available, I can afford to be picky on which comments I want to act on. The classifier itself is about 85% accurate in determining the type of comment. However, I artificially increase my accuracy by only acting on comments that have a very high classifier certainty by forcing this certainty level to meet or surpass my threshold values from above. Smaller sites, with a lower volume, don't have the benefit of having enough comments to be this picky. It is on these sites that a more feature based classifier would be important.

- The human element is still unpredictable.

My classifier was trained utilizing my idea of how comments should be flagged. Prior to automating this, I was not 100% accurate. Additionally, moderators are not 100% accurate in their processing of flags. [Users][16] [disagree][17] on how these rules should be implemented, but are willing to [assist][18] in keeping the site clean. With more than 175K comments a week, every little bit helps.

---

## Discussion

As my title states, my original question was whether or not I can teach a machine how to flag comments as I would. The answer to that is yes. The next question is whether this type of system would be helpful in cleaning up comments across Stack Overflow. My system works only on new comments created around each new UTC. Once my 100 flags are hit (or the API tells me to stop), it shuts down for the day. Having something automated go through historical comments or that can run all day would be beneficial.

Finally, now that I've admitted that I've been automatically flagging comments, can I continue to do so?

---

## Update

*This section has been updated multiple times since the original post. Most recently, it was updated May 3, 2015*

As I mentioned in the introduction, this was originally published in December 2014. How is the system behaving now? It is performing very well.

### Process Changes

In January 2015, [another user][19] was using a basic query to look for invalid comments. This caused a high number of moderator flags, many of which were declined. My process was caught in this mass decline. This resulted in 49 declined flags for a single day.
This is, by far, the largest number of declined flags the process has generated in a day. It did, however, prompt a process change after consultation with the Stack Overflow moderators.

The process will no longer flag comments newer than 48 hours old. This provides users with a two day window to see a comment before the system will flag it. This single change has provided a huge improvement in terms of flag acceptance.

### May 2015 (11 Months)

After nearly a year of running, these are my flagging statistics:

	39938	comments flagged
	39659	deemed helpful
	279	    declined

This provides a helpful rate of 99.3%. This is down *just* slightly from 99.36% in December. I attribute a large part of the dip to the issue mentioned above.

---

![Flags per day with rolling 10 day average][20]

Here is an updated chart showing the rolling 10 day average for number of declined flags. I've had several stretches of multi-week time frames with no declined flags.

This is a busy chart, so I've narrowed it down to show just the last 90 days. From here you can see that in the past 90 days there have been only 10 declined flags.

![Flags per day with rolling 10 day average - 90 day window][21]

### Sept 2015 (15 Months)

It has been almost 15 months since the process started. In that time, the model has gotten more accurate. Since the last update in May, I've had only 3 declined comment flags:

    52351	comments flagged
    52069	deemed helpful
    282	    declined

This provides a helpful rate of 99.46%. Here is an updated chart showing the rolling 10 day average for number of declined flags. The 90 day window is not even worth showing. It has three days where a single flag was declined.

![Flags per day with rolling 10 day average - 15 Months of data training][22]

### Summary of 2015

I processed comments 359 days out of the year. I missed three in January due to stopping it after a mass decline of flags (more later), I can't account for a missed day in July and August. I don't recall stopping it, but I missed July 3rd and August 19. I also missed December 28th due to a power issue. I flagged 35,960 comments. Of that, 111 were declined.

By month, this is the break down of rejected flags.

![2015 Flag Summary][23]

The blip at the end of November is due to new moderators being elected and adjusting to what other moderators consider "good" versus "bad" comments. I didn't see the spike in the April election which is interesting, but after a couple days in November it's back to normal. The January spike I mentioned above. 

Interesting note: The longest stretch in the year with no declined flags was from August 13th through November 24th.


  [1]: http://data.stackexchange.com/
  [2]: http://api.stackexchange.com/docs/comment-flag-options
  [3]: http://api.stackexchange.com/docs/create-comment-flag
  [4]: {attach}images/flags_per_day_rolling_average.png
  [5]: http://winterbash2014.stackexchange.com/resolution
  [6]: http://meta.stackexchange.com/a/215397/186281
  [7]: {attach}images/total_flags_vs_total_declined.png
  [8]: {attach}images/comments_saved_per_day.png
  [9]: http://communitybuilding.stackexchange.com/
  [10]: http://pets.stackexchange.com/
  [11]: http://parenting.stackexchange.com/
  [12]: http://meta.stackoverflow.com/q/277314/189134
  [13]: https://stackedit.io/viewer#!provider=gist&gistId=af9d8186690cb658aafe&filename=commentblacklistresults.md
  [14]: http://meta.stackexchange.com/q/245416/186281
  [15]: http://data.stackexchange.com/stackoverflow/query/200435#graph
  [16]: http://meta.stackoverflow.com/q/278813/189134
  [17]: http://meta.stackoverflow.com/q/280426/189134
  [18]: http://meta.stackoverflow.com/q/278927/189134
  [a]: http://meta.stackoverflow.com/q/280546/189134
  [b]: http://meta.stackoverflow.com/users/189134/andy?tab=profile
  [19]: http://meta.stackoverflow.com/q/283030/189134
  [20]: {attach}images/latest_flags_per_day_rolling_average.png
  [21]: {attach}images/latest_flags_per_day_rolling_average_90day_window.png
  [22]: {attach}images/declined_per_day_15_months.png
  [23]: {attach}images/2015-flag-summary.png