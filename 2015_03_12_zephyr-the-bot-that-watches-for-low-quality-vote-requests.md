Title: Zephyr - The bot that watches for low quality vote requests
Date: 2015-3-12 23:34
Tags: Stack Exchange, automation, programming
Category: Programming Projects
Slug: zephyr-the-bot-that-watches-for-low-quality-vote-requests
Summary: Find out about the bot that watches Stack Exchange chat rooms for requests to close low quality content
Status: published
modified: May 8, 2015

[TOC]

## Introduction 

Stack Exchange receives thousands of questions per day across all of their sites. Not all of these are high quality
posts. Fortunately, users of the Stack Exchange network are given [tools][1] to help keep that low quality stuff to a 
minimum. One of these tools is the chat network that spans the Stack Exchange sites. 

In the chat rooms, a convention has arisen to tag a message as [kb:cv-pls] for questions that need to be closed for one reason 
or another. Over time, this evolved to include other tags such as:

 - [kb:del-pls] for a deletion request
 - [kb:spam] for notification that spam made it through the already [impressive][2] spam [filters][3]
 - [kb:reopen] for a reopen request
 - a few others to cover specific flag types (eg. Not an answer, Very Low Quality or Offensive)

## Introducing Zephyr

The problem with these is that the requests are only seen by users active in the specific room where it was posted. 
Other users across the network miss the request. **[Zephyr][4]** was built to resolve this problem. Zephyr monitors
several rooms where these types of requests are frequent. These requests all all posted into a single [chat room][5]. 
This provides users with a single room to monitor to see requests for multiple questions and sites across the network.

Here is an example of what Zephyr's chat activity looks like during a spam wave:

![Zephyr's chat activity during a spam wave][6]

### How it works

Zephyr utilizes the [ChatExchange][9] package to join and read the chat rooms. To do this, Zephyr required a dedicated
account. I decided to run Zephyr with a dedicated account to completely separate the bot that would sit and watch multiple chat
rooms 24/7 from my account. Zephyr maintains a small SQLite database of all the posts that it records. The idea behind this, 
is that eventually this data will be utilized to train other systems on unwanted content. This information is pulled via
the [API][10]. 

Zephyr watches the chat rooms for specific string [patterns][11]. If these patterns are matched, a message is posted if `should_post` 
is `True` for the matched pattern. 

Overall, a nice simple application. It performs some pattern matching and a couple API calls. 

### Other bots

In addition to watching user activity, Zephyr also watches two other quality bots that patrol Stack Exchange for low
quality content: [SmokeDetector][7] and [Phamhilator][8]. If either of these bots detect spam, Zephyr takes note of the information by
recording it to the database, but not reposting. Since both of those bots post their reports, it didn't make sense for Zephyr
to add a second (or third, if both of the others detected spam) message to the chat room. The information is recorded, though,
to help future training for other systems.

## Updates

*Updated May 8, 2015*

Over time Zephyr has been updated to include new rooms to monitor or new patterns to match. Those changes are small (and simple).
There are, however, a few larger changes that I'd like to note below.

### Commands

The other bots that Zephyr monitors respond to user input. Zephyr has very little that requires user interaction since all of it's
posts are generated *by* user input. However, there have been times where I, as the bot owner, would like to be able to issue
certain commands to it. My most common desire is to see a report of how many spam posts Zephyr has seen. Thus, Zephyr now responds
to the command `spamreport` from me. It then prints out a nice summary of information. This information has been utilized in 
SmokeDetector to watch for commonly spammed domains.

![Zephyr spam report for April 2015][12]

### Upgrade from SQLite to MariaDB

Zephyr was originally built against an SQLite database. This worked, but was getting slower as more data was being added. This slow down
was beginning to affect performance. I started seeing this error more and more frequently:

    Traceback (most recent call last):
      File "H:\python-virtualenvs\zephyr-se-voterequests\lib\site-packages\sqlalchemy\pool.py", line 255, in _close_connection
        self._dialect.do_close(connection)
      File "H:\python-virtualenvs\zephyr-se-voterequests\lib\site-packages\sqlalchemy\engine\default.py", line 418, in do_close
        dbapi_connection.close()
    ProgrammingError: SQLite objects created in a thread can only be used in that same thread.The object was created in thread id 4824 and this is thread id 4660
    
After spending a lot of time troubleshooting and not resolving it to my satisfaction, I decided to upgrade to a more robust database. I'd used
MySQL/MariaDB before and I happened to have another application utilizing MariaDB at the moment so that is the solution I picked. 
 
The first step was transferring data. I learned that there isn't a decent utility to do a straight migration. So, I took these steps to transfer the data:

 - Export table structures and data from SQLite
 - Convert the SQLite dump to MySQL format. Though both systems use SQL, there are slight differences in dialect. I utilized
 [this Python script][13] as a starting point. It got me most of the way there, but not completely.
 - Data clean up. Ugh. The dreaded part of the job for anyone who handles data. Fortunately, the script above did most of the work.
 I ended up fixing a couple stray back ticks that didn't convert properly, escaping a very extra quotation marks, and replacing
 a few "smart quotes" (of both the [left][14] and [right][15] variety). I wish data at the office job was this easy to clean...
 - Import into MariaDB

Since the transfer to MariaDB, I've noticed no performance degradation. The error about threads has been eliminated as well.

### Upgrade to utilize web sockets

Originally, Zephyr used the [`watch`][16] method when monitoring a room. This method would long poll the room. It turns out that this is 
pretty unreliable. I'd get multiple errors through out the week, ranging from `Connection Aborted` errors to random `404` messages. The 
solution has been to switch to [`watch_socket`][17]. The only time I've had problems since this switch is when the Stack Exchange 
web sockets go down. This saves a lot of restarts to get everything up and running again.


 [1]: http://blog.stackoverflow.com/2009/05/a-theory-of-moderation/
 [2]: http://meta.stackexchange.com/questions/228043/
 [3]: http://meta.stackexchange.com/a/237882/186281
 [4]: https://github.com/AWegnerGitHub/SE_Zephyr_VoteRequest_bot
 [5]: http://chat.meta.stackexchange.com/rooms/773/low-quality-posts-hq
 [6]: {attach}images/zephyr-spam-wave.png
 [7]: https://github.com/Charcoal-SE/SmokeDetector
 [8]: https://github.com/ArcticEcho/Phamhilator/wiki
 [9]: https://github.com/Manishearth/ChatExchange
 [10]: http://api.stackexchange.com/
 [11]: https://github.com/AWegnerGitHub/SE_Zephyr_VoteRequest_bot/blob/master/create_config_files.py
 [12]: {attach}images/zephyr-spam-report.png
 [13]: http://stackoverflow.com/a/1067365/189134
 [14]: http://www.fileformat.info/info/unicode/char/201c/index.htm
 [15]: http://www.fileformat.info/info/unicode/char/201d/index.htm
 [16]: https://github.com/Manishearth/ChatExchange/blob/master/chatexchange/rooms.py#L68
 [17]: https://github.com/Manishearth/ChatExchange/blob/master/chatexchange/rooms.py#L78