Title: Stack Exchange Strike - Community and AI Talk Reaction
Date: 2023-07-07 12:00
Tags: Stack Exchange, moderation
Category: Stack Exchange Strike
Slug: stackexchange-moderator-strike-overflowai-reaction
Summary: Stack Exchange moderators are striking. This is my reaction to Stack Exchange's products and updates announced at the WeAreDevelopers conference. 
Status: published
Series: Stack Exchange Moderation Strike 2023

[TOC]

## Introduction

I really was trying not to post anything about the strike again until it was resolved. There was a long period of no action in July so my 
assumption was that it was being worked on behind the scenes with the company and the designated representatives. Turns out, that it was just
quiet for a couple weeks with no movement. One important development was the release (finally) of the [private policy on GPT Generators][1]. This
is significantly different from the one that was released publicly and was part of the cause of the current moderator strike. The community's 
response to this policy was as expected - suprise at how bad the policy was even with moderators saying it was bad.

Today, Prashanth Chandrasekar - CEO of Stack Overflow - presented the next vision for Stack Exchange. The presentation is available on 
[YouTube][presentation]. It starts around the 9 minute mark.

## Reaction

### Welcoming

> You will continue to be the focus. 100%. We are here to serve you. To fight on your behalf. To make sure you are recognized for the work you are doing and that you are able to responsible harness the power of AI for your needs to be the best devolopers you can be. All of this is centered around the collective community and knowledge. That is irreplaceable.

This is a good start. It rings hollow to me right now though. These last two months in particular have shown that the community is easily tossed aside. 
2019, and it's literal legal settlement with a community member for tossing them aside, shows that current time isn't a one time thing. We'll see what the rest of this talk covers, but right now this feels like pandering to the audience.

### Guiding Principles

> Find new ways to give technologists more time to create amazing things.

Good. I'm all about efficiency for myself and my teams. If we can make developers' and engineers' lives easier, that's a win for me.

> Accuracy is fundamental. That comes from attributed, peer-reviewed sources that provide transparency.

Spoiler - I received a sneak peak of a portion of this presentation two days before the official one in Berlin. The information that will be shown 
later in the presentation will cover some of this. It doesn't address all of it though. I'm pleased with the attribution aspect of this. 
Stack Overflow content is [licensed under CC-by-SA][license]. Attribution is required and I approve of any effort to improve that. 

I'm concerned about accuracy. Their own test of the [GenAI powered formatting assistant][formattingassistant] last month showed this is a problem. 
Current GenAI - Chat GPT, Bard, etc - are well known for making information up instead of saying "I don't know". 

> The technology field should be accessible to all, including beginners to advanced users.

Weird ending phrase, but I agree. Technology fields are not limited to engineers, software developers and people with the desire to learn how to 
program. Technological improvements should come from any one who wants to make life easier. AI can and will help with that.

> Humans should always be included in the application of any new technology.

Again, agreed. My concern here is whether or not Stack Exchange has the capability to do that. Technically, they've had a human involved, but it's 
only been from a single perspective - that over the company. They have ignored the other side of this: the community. The people that provided the 
data for their business to thrive.

### Overflow AI

_insert applause_

This has been in development for "the past three months". It also covers 6 items being talked about today and 6 additional items. That time frame is 
concerning. Three months is not a lot of time to build a lot of these larger items. 

Search: Summarized verion of multiple answers with citations from where the answers came from. It also allows you to continue the conversation with the 
results - including code. It also allows the user to post the question to Stack Overflow if the generative AI portion gets stuck.

My concerns lie with how this impacts the community. Hand waving away that the system is able to synthesize an answer and properly attribute it, what
happens next? Stack Overflow has a reputation - deserved or not is up to the reader - for being harsh on new users that don't "try". If a new user 
comes and gets an answer without posting the question that's wonderful for the user. They move on and complete what they need to complete. For the 
community, though, we have a problem. 

I find it unlikely that a new user is going to post a question to Stack Overflow when they received an answer. Why would they? They already got the 
answer they need. If this repeats constantly, the knowledge base that Stack Overflow represents becomes significantly less useful. Fewer questions
mean fewer answers. With fewer answers, the platform becomes more dependant on the trained AI model which is receiving less data to feed it. The system
spirals.

Does this only matter to prevent duplicates though? If the system is only summarizing other topics, wouldn't that mean the question being asked is a 
duplicate? I don't think that's the case. I think with the "continue discussion" portion of this, Stack Overflow will be losing out on better written
and described problems. Problems with more detail that the community could benefit from seeing and answering.

The Visual Studio plugin looks interesting. I don't think it's adding much that other plugins can't do already, other than providing a first party 
integration to Stack Overflow. The same is true for the Slack / Stack Overflow integration. 

The Enterprise knowledge injestion is an interesting product. While we don't use Stack Overflow for teams at my current company, this would be 
a great way to start using it. The ability to load initial data into a new system is important. I'd love to see this in practice and how well it does 
at building initial Q&A pairs from the data it injests. 

I haven't looked into Stack Overflow for Teams for my profession work. A couple comments that were made do raise a few concerns about how Stack Exchange
is using company data. Before making a recommendation to utilize it for my company, I'd need to investigate that data from my company is not made 
available outside of my organization.

Finally, discussions. Stack Exchange is adding a forum to their collectives product. The community has spent over a decade to not be a forum. This change
is not great in my opinion. Forums, social media, are a different beast than Stack Overflow. 

## Final Thoughts

I am concerned about the direction Stack Exchange is taking and the employee resources they are using to go that route. Engagement, page views, and 
traffic has been a huge underlying concern at Stack Exchange for a while. It was particularly noticed when ChatGPT was released and with the comments 
about driving users away. However, this downward trend has been ongoing for years.

I don't think GenAI is going to solve this problem. I think it will improve new user experience, but the way this has been presented it will be to the
detriment of the larger community. Fewer questions will exist to answer and that will slowly cause users to disengage from the site. 

This is why the opening of the talk feels like lip service to the community to me. These changes are designed to put a barrier between users of the 
community. Even the discussions product has the barrier. Discussions will only be available in Collectives, not to the general community. 



 [presentation]: https://www.youtube.com/watch?v=g5F5t205pYA
 [license]: https://stackoverflow.com/help/licensing
 [formattingassistant]: {filename}2023_06_19_stackexchange_strike_update3.md
 [1]: https://meta.stackexchange.com/q/391626/186281