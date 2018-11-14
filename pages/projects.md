Title: Portfolio
Date: 2016-05-01 00:00
Summary: Projects 
Status: draft

[TOC]

In addition to the projects I've done for my own personal projects, I've built things professionally as well. 
Unfortunately, most of that can not be released as open source software. Instead, I've provided brief overviews
of several projects below.

## Personal Projects

### [StackAPI][1]

StackAPI is a Python wrapper for the [Stack Exchange API][2]. I built this because of the many other projects I've 
done that interacted with the API. I needed a way to easily gather multiple pages of API data and write back to the 
API using their tokens. I released [StackAPI on PyPI][3]

### [Stack Exchange Comment Flagger][4]

I wrote extensively on this project in my post on whether [a machine can be taught to flag comments][5]. The code, but not
all the underlying data, is available on GitHub. This includes both the automated flagging script, the code to train the
machine based on comments, and the Flask based dashboard. 

### [IRVING][6]

Integrated Review and Visibility Interface Notification Gadget (IRVING) is a Django based dashboard that can query multiple
systems and present users with feedback. I built this initially, in my free time, to replace an Access based dashboard utilized
at work. It was slow, clunky and required that users have Microsoft Access installed. It is built to handle querying Oracle,
Microsoft SQL Server, MySQL and SQLite. 

### [Zephyr - Vote Request Bot][7]

Zephyr monitors various chat rooms on the Stack Exchange network for close vote requests and consolidates all of them
to a single chat room. This makes monitoring user feedback on low quality posts simple, as users only need to watch
a single room. 

## Professional Projects




 [1]: https://github.com/AWegnerGitHub/stackapi
 [2]: http://api.stackexchange.com/
 [3]: https://pypi.python.org/pypi/StackAPI
 [4]: https://github.com/AWegnerGitHub/StackOverflow-Auto-CommentFlag
 [5]: |filename|2015_01_02_can-a-machine-be-taught-to-flag-comments-automatically.md
 [6]: https://github.com/AWegnerGitHub/IRVING
 [7]: https://github.com/AWegnerGitHub/SE_Zephyr_VoteRequest_bot
 [8]: https://github.com/AWegnerGitHub/Stack-Exchange-Scripts
 
 