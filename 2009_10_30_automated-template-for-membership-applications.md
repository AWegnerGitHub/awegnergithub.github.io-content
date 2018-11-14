Title: Automated template for membership applications
Date: 2009-10-30 22:30
Tags: team vipers, automation, community, programming
Category: Vipers
Slug: automated-template-for-membership-applications
Summary: How Team Vipers improved user applications and admin partipation in the process
Status: published

[TOC]

## Introduction

Not to long ago, applications for new membership in Team Vipers consisted of someone wandering onto the site, posting a 
few words and an administrator saying they were accepted as a member. This worked when we were a small community. We 
aren't small any more. We have 5 servers and hundreds of players a day. Each server has their own sub-community. There 
are players joining the forums than entire groups of people have never met because they play exclusively on one game server. 

Admins were also inconsistent in how (or if) they voted. Some admins didn't realize they could have a say, thinking it 
was a privilege granted to only the senior administrators. We've built a system to resolve many of these problems and to 
make the administration side easier.

## What's new?

The new system presents all users with an application template. They fill out the form and the system handles the rest. 
The New Users and Applications subforum has been modified so that no one, except the bot, can create topics. They 
topics will only be created when the form is submitted. When a user applies to join Vipers, we will automatically 
include relevant information about the user as we know them:

 - HLStats information: This helps us get a sense of where they play and how often they play. It will help admins identify 
 users they *should* recognize based on the servers they frequent (because we all know certains servers are better than 
 others ;) *cough* Vanilla Nest vs Crits *cough* )
 - Ban information: This will check if the user has any recorded bans in [Sourcebans][1]. It's important to know if the 
 user has been banned previously. 
 - Known aliases: Pulling information from our chat logs and Valve's profile page, we can build a list of known aliases. 
 This helps identify users that frequently change names but have been around a while. 
 
There may be other features we add in the future as well. 

*Updated January 2012* I've removed HLStats from the servers and removed it from new application information. We have 
added a check of a user's [Steam Reputation][2] instead.

After a user applies, they are put into a two week hold. During this week it is expected they will stick around the forums 
and learn about the community they just applied to. Even better would be that they had done this before applying. While 
this two week hold is in place, the administration team will be able to cast they votes in a separate sub-forum. They can 
hold administration specific discussions - usually details that are important for admins to know, but don't *need* to be 
public. Once voting is complete, if they become a member, the system automatically grants appropriate forum and server 
related permissions.

### Voting rules

 - Three admin "Yes" votes and zero "No" votes grants the user membership immediately after the two week window has expired
 - One or two "No" votes places a message on the user's application that the administrators are still considering the application
 - Three or more "No" votes rejects the application

## Possible Responses

### Accepted by admin team

If you reach a total of three or more admin "Yes" votes, and do not get three or more admin "No" votes, you will be accepted 
as a member of Vipers and automatically have your forum access modified. You will receive a message similar to this:

![Application Accepted][3]

### Denied by admin team

If you reach three or more "No" votes your application will be denied. This will occur even if you receive more "Yes" votes
than "No" votes. Vipers is not a majority rule community. The decision has been made that if three admins do not feel comfortable
accepting your application, you will not be granted membership. You will receive a message similar to this:

![Application Denied by Admins][4]

### Denied due to lack of votes

To be accepted, your application requires a minimum of three "Yes" votes. If this can not be reached (and you also can't reach
 three "No" votes), your application will be rejected due to lack of votes from the admin team. This means that the admin team
 does not feel strongly either way about your application. Post on the forums. Play in the servers. Get to know our players
 and the community at large and then try again in a month. You will receive a message similar to this:

![Application Denied with not enough votes][5]

### Denied because of age

Repeat after me: "Age does not equal maturity." However, it has a very strong correlation. Over time we have learned that younger
 players tend to bring a lower maturity level that most of the community does not care for. As such, we've decided to set a minimium
 age requirement of 16. If a user indicates they are less than that, the system will reject their application with a message similar to
 this:
 
![Application Denied because of age][6]


 
*Updated January 2010* This process has been in place for a few months now. It has gone very well. We've reduced the clutter 
in the applications forum. We've also seen the number of "forgotten" applications drop dramatically. 


## Original Announcement

The original announcement is posted here for future reference.

> Zephyr is an automated robot designed to improve our new member application process. The current process, which involves copying a template to a new thread and the potential for applications to become misplaced, is cumbersome and inefficient. The goal of Zephyr is to remove as much of the manual process as possible.
>
> Now, our new applicants will fill out an actual application online. The same information will be requested, but it will be in a more reliable format and will not require an applicant copy and paste anything between threads. This new application will be posted in the same forum as you're used to, and members will be freely able to comment and discuss the applicant via that thread. It will also display which date admin voting will be open, which is two full weeks after the original application post. This will hopefully cut down on any confusion related to the delay between application and voting.
>
> As another note, from now on Zephyr will be the only user capable of creating new threads in the New Member Application forum. As previously stated, members will be able to post comments on existing threads, but the only new threads will come from the application process. This will keep the forum cleaner and help prevent applications from becoming lost or forgotten about.
>
> Once again, Zephyr is an automated robot. It is not programmed to respond to comments or questions. Doing so will not get your question answered. As always, if you have questions or comments about the application process, Zephyr, or anything else, you're welcome to send them to any admin. We'll be more than happy to help.
>
> You may all bow to your Robotic Overlord now.



 [1]: http://www.sourcebans.net/
 [2]: http://steamrep.com/api.php
 [3]: {attach}images/application-accepted.png
 [4]: {attach}images/application-denied.png
 [5]: {attach}images/application-denied-not-enough-votes.png
 [6]: {attach}images/application-denied-underage.png