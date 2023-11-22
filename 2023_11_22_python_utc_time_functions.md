Title: Python Gotcha: Timezone Naive Functions like utcnow() and utcfromtimestamp()
Date: 2023-11-23 10:15
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-timezone-naive-functions
Summary: utcnow() and utcfromtimestamp() don't know about timezones and that causes problems. Let's talk about those, how to fix the problem and their recent deprecation in Python 3.12
Status: published
Series: Programming Gotchas

[TOC]

## Introduction

There are two common functions in that are [naive datetime objects][1]. The behind these types of objects is that timezones don't matter. Perhaps you always assume that datetimes are in a set timezone. Or, you are aware of timezones and want to make things easy on yourself so always assume datetimes are set to UTC.

This is your mistake. 

The `utcnow()` and `utctimestamp()` functions are naive, but other `datetime` functions are not.

## The Gotcha

    >>> from datetime import datetime
    >>> dt = datetime.utcfromtimestamp(0)
    >>> dt
    datetime.datetime(1970, 1, 1, 0, 0)
    >>> dt.timestamp()
    21600.0

What happened here? 

This was run on a computer in the Central Standard Time time zone. First, I created an object set to the UNIX epoch - Jan. 1, 1970. Since I utilized `utcfromtimestamp()` to do this, there is no timezone data attached to this object. When I use the standard `timestamp()` function, Python sees there is no timestamp to utilize, so it uses the system configured timezone. Hence the 21,600 second offset from the epoch.

This didn't always happen though. Sometime [between Python 2 and Python 3 this behavior changed][2]. This assumption that "no timezone means local timezone" introduced this gotcha. In my opinion isn't a rather dangerous one too.

## Solution

Utilize explicit time stamps everywhere. From [The Zen of Python - PEP 20][3]:

> Explicit is better than implicit. 

    >>> import datetime
    >>> dt = datetime.datetime.fromtimestamp(0, datetime.UTC)
    >>> dt
    datetime.datetime(1970, 1, 1, 0, 0, tzinfo=datetime.timezone.utc)
    >>> dt.timestamp()
    0.0

Notice that by adding `datetime.UTC` to my `fromtimestamp()` call, I get a datetime object with `tzinfo`. This makes the conversion from the datetime object back into ta timestamp safe. 

This is the way that Python is moving. In an effort to correct these unsafe assumptions that were introduced early in Python 3, the naive datetime functions have been deprecated. If you were to run my first block of code in Python 3.12, you receive a deprecation warning. 

    >>> dt = datetime.utcfromtimestamp(0)
    <stdin>:1: DeprecationWarning: datetime.datetime.utcfromtimestamp() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.fromtimestamp(timestamp, datetime.UTC).

Notice that the message is telling you to add the timezone information and to utilize `fromtimestamp()`. In the coming year - years? - both the commonly seen `utcnow()` amd `utcfromtimestamp()` will be removed.

In my opinion, as someone that has fought this bug in the past, it's a welcome change. Timezone assumptions are difficult to troubleshoot. Assumptions about underlying behavior make it a challenge. Running the same script in different timezones, like you would with a geographicly diverse team, makes it even harder. It's especially annoying if one of your coworkers is utilizing UTC as their time zone so that the script behaves properly for them but no others.

I have code bases that will need to make this change, but in the end, they will be easier to maintain with the removal of the `utc*` functions.



 [1]: https://docs.python.org/3/library/datetime.html#aware-and-naive-objects
 [2]: https://github.com/python/cpython/issues/81669
 [3]: https://peps.python.org/pep-0020/