Title: Choosing an ORM library for a new project
Date: 2017-04-26 14:30
Tags: technical, programming
Category: Technical Solutions
Slug: choosing-orm-library
Summary: A discussion about how a team picked an ORM library for a new project.  
Status: published

[TOC]

## Project History

The [SmokeDetector][1] project is over three years old at this point. It's grown from a small python script to a 
decently sized application that integrates with another project. In that time, it's expanded what types of spam and
patterns it looks for, what chat rooms it posts to, what external services it integrates with and how permissions to
use the system are determined. 

A lot has changed under the hood. I was hoping to put a cool chart here showing code change over time, but some early
decisions with the project really throw off the chart. Using a [Ship of Theseus][2] analogy for code, you can see how 
much has changed. The basic idea is, if a ship leaves port and replaces every plank along it's journey, is it still the 
same ship when it returns? With code, the idea is to apply this to lines of code in an application.

[![SmokeDetector - Git of Theseus][3]][3]

### What happened in 2014?! 
 
In late 2014, the project attempted their first machine learning method of detecting spam. In this time period, a 
[commit][4] was added that added about 200,000 lines of code to the project. This was almost all training data for a 
Bayesian algorithm. It wasn't needed and probably shouldn't have been added to the main repository. Unfortunately, it 
stayed in the repository for over a year and was finally [removed][5] in late 2015. This is the cause of the weird graph 
above, and why almost everything added in 2014 looks like it's missing in later years.
 
### What has really changed?

After eliminating that bayesian directory from git history, you can get a much better idea of how much has changed. 

[![SmokeDetector - Git of Theseus - Filtered][6]][6]

Very little of the original code, written in 2014, remains untouched. The explosion in code after that is due to
new detection patterns, chat commands (and a rewrite), integration with MetaSmoke and the introduction of blacklists.
  
Even more dramatically, you can see how long a line of code is expected to survive in the code base.

[![SmokeDetector - Line Survival Rate][7]][7]

Within one year, the team is removing over 40% what's been committed to the repository. Looking at these commits, 
it was determined that a vast majority aren't even *code*. They are new items to blacklist or new patterns to detect. 

## Enter the database

All of this type of data can be stored in a database and managed outside of code. In early 2017, those discussions 
started taking place. Several team members come from a Ruby background and were familiar with it's [ORM][8] method of
accessing databases. They wanted something similar when a database was brought into SmokeDetector.

A bit of research was done and it was narrowed down to [peewee][9] and [SQLAlchemy][10]. 

### How to choose?

Fortunately for the SmokeDetector team, there weren't any strong opinions either way. The biggest reason for choosing
one over the other came down to a [comment made by the peewee author][11] on reddit. They state:

> [...] SQLAlchemy is the gold standard for ORM in the Python world. It has a very active community and a maintainer 
who is committed to excellence. If you're a glass-half-empty guy, to put it another way, you can't go wrong if you 
choose SQLAlchemy.

The weaknesses they list for using their own package is the smaller ecosystem, support and number of developers.

### Technical differences

That's a boring story though. Not to be deterred from such a glowing review from a competitor, I wanted to see what the
technical differences were between the two solutions.

To that end, I put together a small Python notebook showing the [differences between peewee and SQLAlchemy][12] in a 
handful of tests. These tests included inserting two settings in an SQLite database, retrieving one, inserting a large
list of users and then retrieving a subset of those users.

The results were...unremarkable. 

[![peewee vs SQLAlchemy results][13]][13]

The two libraries each took two tests (out of four) for being faster than the other. In both cases where SQLAlchemy was
faster, it was between two and six times faster. Where peewee was faster it was between a fraction faster and twice as
fast. 

The time scales are so small though, and SmokeDetector doesn't need to have thousands, hundreds or even tens of hits to
the database a second. A hundred extra milliseconds isn't going to cripple anything it handles.

Thus, the choice was made based on the recommendation of the author of the peewee library. SQLAlchemy has a larger
community and better support. 



 [1]: {filename}2017_02_19_can-a-machine-be-taught-to-flag-spam-automatically.md
 [2]: https://erikbern.com/2016/12/05/the-half-life-of-code.html
 [3]: {attach}images/smokey-git-theseus-all.png
 [4]: https://github.com/Charcoal-SE/SmokeDetector/commit/102aa9c64edafb7f5fef5ba16414f4cefad03d64
 [5]: https://github.com/Charcoal-SE/SmokeDetector/commit/68d49ccc0b4981a4ebe91d993f42643542e44d80
 [6]: {attach}images/smokey-git-theseus-filtered.png
 [7]: {attach}images/smokey-git-theseus-survival.png
 [8]: https://en.wikipedia.org/wiki/Object-relational_mapping
 [9]: http://docs.peewee-orm.com/en/latest/
 [10]: https://www.sqlalchemy.org/
 [11]: https://www.reddit.com/r/Python/comments/4tnqai/choosing_a_python_ormpeewee_vs_sqlalchemy/d5jyuug/
 [12]: https://gist.github.com/AWegnerGitHub/201dbaf09740f9ecd797c32ebfc15872
 [13]: {attach}images/peewee-vs-sqlalcheme-results.png