Title: Why I moved from Wordpress to Pelican
Date: 2015-5-03 22:41
Tags: Meta, Pelican
Category: Side Activities
Slug: why-i-moved-from-wordpress-to-pelican
Summary: A brief summary of why I dropped Wordpress and moved to Pelican
Status: published

[TOC]

## Introduction

For years, I maintained a Wordpress blog covering various things I've done or created. Most of these revolved around
things I created to make administering Team Vipers easier for me and for the rest of the admin team. It was my way
of documenting what I'd done (in case I ever needed to do it again) and providing a way to update the Team Vipers community
about new plugins or applications that would be deployed to the community. 
    
## My Issues with Wordpress

The problem I had with Wordpress was that it was just to bulky for the simple posts I was making. I needed a database, a full web
server (or a hosting provider), and either time to hunt for the "perfect" plugin(s) or PHP knowledge to do it myself. Early in my
development career, I used PHP a lot. That was part of the reason I chose Wordpress. 

 > Oh! I know that language. If I ever need something, I can just whip it up myself. 
 >
 > *-Me, before the real world ambushed me and beat me with a stick*

### Spam
  
I spent time setting up Wordpress. I picked out a theme, plugins, and started saving future me with documentation. Then life happened. For whatever
reason, I stopped updating Wordpress. My blog sat out there for weeks or months unvisited by anyone. Then, one morning, my phone vibrated
and told me that I had a new comment on my site. Woo! 

Except it was spam. Boo!

I marked it spam and moved on with my day. Later that morning, I glanced at my phone again. 32 emails. I am just not that popular. Something
was wrong. Turns out, a spam bot found me. I sighed and then removed all the comments and checked the box indicating that users had to
be registered to post. That solved my problem for a few months.

Then the bots got smarter. They started registering. They started posting legitimate looking messages, except for that associated URL their name
would link to in the comments. They pulled keywords out of the post and formulated a somewhat passable English question using those words. The spam
prevention plugins I installed would slow the tide for a few weeks. The bots would adapt and then I'd be awash with spam posts again. Eventually,
the solution was to completely disable comments. I'd spent way to much time dealing with spam on a blog that received very little legitimate traffic.

Since I don't utilize the comments, Pelican provides a nice simple page that I can post my thoughts and not worry about getting hit by a spam bot. It also
provides plugins so that I *can* include comments should I ever choose to do so in the future. For the time being, though, I have a nice simple page 
with no comments. That's exactly what I was looking for.

### Security

If you watch any technology web sites, you'll notice that there are vulnerabilities found in Wordpress frequently. These require patches,
which requires me to do something. It may be as simple as logging in and clicking a button to update, but it is still something I need to 
remember to do for a relatively minor site. When I'd log in to clear the spam backlog, I'd frequently also install updates for 10-20 plugins, themes or
Wordpress itself. It was mostly painless, but I didn't like the idea of the site sitting there vulnerable for weeks at a time because I didn't
visit and login.

The dynamic nature of Wordpress and the underlying database exposed a fairly sizable target for a web page so small. Pelican generates static
HTML pages. I don't have to worry about SQL injections, unauthorized logins, or anything else. I host a basic set of HTML, CSS and JavaScript files. 
That's it.

## PHP vs Python

As I mentioned before, I used to use PHP frequently. It was my go to language. I picked Wordpress with the idea that I'd be able to hack together features I needed.
The reality, it turns out, was that I wasn't actually interested in doing that. Instead, I picked out plugins that were close enough to the
exact functionality I wanted. 

I transferred to a job where I used Python. Instead of having a language I used on the side (PHP) and a job where I was a glorified project manager, without the 
actual title of "Project Manager", I now had a job where I used a language (Python) for 8 hours a day. My usage of PHP plummeted. I found I could get what I wanted
done in my side projects faster and easier with Python. At work I used Python to build tools for engineering problems. At home, I started using it for every day
tasks. 

Soon, I realized I hadn't used PHP for several versions of the language. My knowledge of the language was outdated. The biggest reason I'd chosen Wordpress was no longer
relevant, because I couldn't write anything complicated in PHP without glancing at documentation to do even simple things. It's sad that I lost the intimate knowledge of
a language, but I feel that I've been more productive with Python anyway.

Pelican is written in Python. Even more importantly though, it generates HTML files which are hosted. I don't need to run a Python environment on a server. I just
need to host HTML files. 

## Markdown

Finally, I've fought with Wordpress's text editor countless times. This happened most often when attempting to add code blocks. It was a pain to do. It was a pain
to fix when the blocks broke. Pelican supports Markdown. Markdown is supported by large organizations like GitHub, reddit and Stack Exchange. I use all three of those.
I know how to utilize Markdown to create code blocks, headers, insert images, create bulleted lists. All without needing to fight how the text editor is going to actually
save the data.