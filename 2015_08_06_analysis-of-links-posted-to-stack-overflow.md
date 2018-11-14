Title: Analysis of links posted to Stack Overflow
Date: 2015-8-6 7:35
Tags: Stack Exchange, programming
Category: Side Activities
Slug: analysis-of-links-posted-to-stack-overflow
Summary: Approximately 10% of 1.5M randomly selected unique links in the Stack Overflow March 2015 data dump are unavailable. This is an analysis of how that was determined and a request for discussion on how to improve it
Status: published
modified: August 7, 2015
Series: Stack Overflow Link Analysis

[TOC]

This post was [published][a] by [me][b] on Meta Stack Overflow on August 6th, 2015. I've republished it here
so that I can easily update information related to recent developments. If you have questions or comments, I highly
encourage you to visit the [question][a] on Meta Stack Overflow and post there. 

TL;DR: Approximately 10% of 1.5M randomly selected unique links in the March 2015 [data dump][1] are unavailable. To be more precise, that is approximately 150K dead links.

---

## Motivation 

I've been running into more and more links that are dead on Stack Overflow and it's bothering me. In some cases, I've spent the time hunting down a replacement, in others I've notified the owner of the post that a link is dead, and (more shamefully), in others I've simply ignored it and left just a [down vote][2]. Obviously that's not good.

Before making sweeping generalizations that there are dead links everywhere, though, I wanted to make sure I wasn't just finding bad posts because I was wandering through the review queues. Utilizing the March 2015 data dump, I randomly selected about 25% of the posts (both questions and answers) and then parsed out the links. This works out to 5.6M posts out of 21.7M total.

Of these 5.6M posts, 2.3M contained links and 1.5M of these were unique links. I sent each unique URL a [`HEAD`][3] request, with a user agent mimicking Firefox<sup>1</sup>. I then retested everything that didn't return a successful response a week later. Finally, anything that failed from *that* batch, I resent a final test a week later. If a site was down in all three tests, I considered it down for this test.

--- 

## Results<sup>2</sup>

### By status code
Good news/Bad News: A majority of the links returned a valid response, but there are still roughly 10% that failed.

[![PIE CHART IMAGE][status_image]][status_image]
*(This image is showing the top status codes returned)*

The three largest slices of the pie are the status 200s (site working!), status 404 (page not found, but server responded saying the page isn't found) and Connection Errors. Connection errors are sites that had no proper server response. The request to access the page timed out. I was generous in the time out and allowed a request to live for 20 seconds before failing a link with this status. The `4xx` and `5xx` errors are status codes that fall in the 400 and 500 range of HTTP responses. These are client and server error ranges, thus counted as a failure. `2xx` errors (of which was are in the low triple) are pages that responded with a success message in the 200 range, but it wasn't a `200` code. Finally, there were just over a hundred sites that hit a redirect loop that didn't seem to end. These are the `3xx` errors. I failed a site with this range if it redirected more than 30 times. There are a negligible number of sites that returned status codes in the 600 and [700][10] range<sup>4</sup>

### By most common
There are, expectedly, many URLs that failed that appeared frequently in the sample set. Below is a list of the top 50<sup>3</sup> URLs that are in posts most often, but failed three times over the course of three weeks.

	http://docs.jquery.com/Plugins/validation
	http://www.eclipse.org/eclipselink/moxy.php
	http://jackson.codehaus.org/
	http://xstream.codehaus.org/
	http://opencv.willowgarage.com/wiki/
	http://developer.android.com/resources/articles/painless-threading.html
	http://valums.com/ajax-upload/
	http://sqlite.phxsoftware.com/
	http://qt.nokia.com/
	http://www.oracle.com/technetwork/java/codeconv-138413.html
	http://download.java.net/jdk8/docs/api/java/time/package-summary.html
	http://docs.oracle.com/javase/1.4.2/docs/api/java/text/SimpleDateFormat.html
	http://watin.sourceforge.net/
	http://leandrovieira.com/projects/jquery/lightbox/
	https://graph.facebook.com/
	https://ccrma.stanford.edu/courses/422/projects/WaveFormat/
	http://www.postsharp.org/
	http://www.erichynds.com/jquery/jquery-ui-multiselect-widget/
	http://ha.ckers.org/xss.html
	http://jetty.codehaus.org/jetty/
	http://cpp-next.com/archive/2009/08/want-speed-pass-by-value/
	http://codespeak.net/lxml/
	http://www.hpl.hp.com/personal/Hans_Boehm/gc/
	http://jquery.com/demo/thickbox/
	http://book.git-scm.com/5_submodules.html
	http://monotouch.net/
	http://developer.android.com/resources/articles/timed-ui-updates.html
	http://jquery.bassistance.de/validate/demo/
	http://codeigniter.com/user_guide/database/active_record.html
	http://www.phantomjs.org/
	http://watin.org/
	http://www.db4o.com/
	http://qt.nokia.com/products/
	http://referencesource.microsoft.com/netframework.aspx
	https://github.com/facebook/php-sdk/
	http://java.decompiler.free.fr/
	http://pivotal.github.com/jasmine/
	http://api.jquery.com/category/plugins/templates/
	http://code.google.com/closure/library
	http://www.w3schools.com/tags/ref_entities.asp
	http://xstream.codehaus.org/tutorial.html
	https://github.com/facebook/php-sdk
	http://download.java.net/maven/1/jstl/jars/jstl-1.2.jar
	https://developers.facebook.com/docs/offline-access-deprecation/
	http://www.parashift.com/c++-faq-lite/pointers-to-members.html
	https://developers.facebook.com/docs/mobile/ios/build/
	http://downloads.php.net/pierre/
	http://fluentnhibernate.org/
	http://net.tutsplus.com/tutorials/javascript-ajax/5-ways-to-make-ajax-calls-with-jquery/
	http://dev.iceburg.net/jquery/jqModal/


### By post score

Count of posts by score (top 10)  (Covers 94% of all broken links):

    | Score | Percentage of Total Broken |
	|-------|----------------------------|
	| 0     | 36.4087%                   |
	| 1     | 25.1674%                   |
	| 2     | 13.4089%                   |
	| 3     | 7.2806%                    | 
	| 4     | 4.2971%                    |
	| 5     | 2.7065%                    |
	| 6     | 1.8068%                    |
	| 7     | 1.2854%                    |
	| -1    | 1.1935%                    |
    | 8	    | 0.9415%                    |
	
### By number of views

*Note, this is number of views at the time the data dump was created, not as of today*

Count of posts by number of views (top 10):

    | Views        | Total Views |
	|--------------|-------------|
	| (0, 200]     | 24.4709%    |
	| (200, 400]   | 14.2186%    |
	| (400, 600]   | 9.5045%     |
	| (600, 800]   | 6.9793%     | 
	| (800, 1000]  | 5.2574%     |
	| (1000, 1200] | 4.1864%     |
	| (1200, 1400] | 3.3699%     |
	| (1400, 1600] | 2.7766%     |
	| (1600, 1800] | 2.3477%     |
    | (1800, 2000] | 1.9550%     |
	
	
### By days since post created

*Note: This is number of days since creation at the time the data dump was created, not from today*

Count of posts by days since creation (top 10) (Covers 64% of broken links):
	
	| Days since Creation | Percentage of Total Broken |
	|---------------------|----------------------------|
	| (1110, 1140]        | 7.2938%                    |
	| (1140, 1170]        | 6.7648%                    |
	| (1470, 1500]        | 6.6579%                    |
	| (1080, 1110]        | 6.6535%                    | 
	| (750, 780]          | 6.5535%                    |
	| (720, 750]          | 6.5516%                    |
	| (1500, 1530]        | 6.3978%                    |
	| (390, 420]          | 5.8508%                    |
	| (360, 390]          | 5.8258%                    |
    | (780, 810]          | 5.5175%                    |

### By Ratio of Views:Days

Ratio Views:Days (top 20) (Covers 90% of broken links):

    | Views:Days Ratio | Percentage of Total Broken |
	|------------------|-------------|
	| (0, 0.25]        | 27.2369%    |
	| (0.25, 0.5]      | 18.8496%    |
	| (0.5, 0.75]      | 11.4321%    |
	| (0.75, 1]        | 7.2481%     | 
	| (1, 1.25]        | 5.1668%     |
	| (1.25, 1.5]      | 3.7907%     |
	| (1.5, 1.75]      | 2.9310%     |
	| (1.75, 2]        | 2.4033%     |
	| (2, 2.25]        | 1.9788%     |
    | (2.25, 2.5]      | 1.6850%     |
    | (2.5, 2.75]      | 1.4080%     |
    | (2.75, 3]        | 1.1879%     |
    | (3, 3.25]        | 1.0654%     |
    | (3.25, 3.5]      | 0.9391%     |
    | (3.5, 3.75]      | 0.8334%     |
    | (3.75, 4]        | 0.7165%     |
    | (4, 4.25]        | 0.6634%     |
    | (4.25, 4.5]      | 0.5789%     |
    | (4.5, 4.75]      | 0.5508%     |
    | (4.75, 5]        | 0.4833%     |


---
 
## Discussion

What can we do with all of this? How do we, as a community, solve the issue of 10% of our outbound links pointing to places on the internet that no longer exist? Assuming that my sample was indicative of the entire data dump, there are close to 600K (150K broken unique links x 4, because I took 1/4 of the data dump as a sample) broken links posted in questions and answers on Stack Overflow. I assume a large number of links posted in comments would be broken as well, but that's an activity for another month.

We encourage posters to provide snippets from their links just in case a link dies. That definitely helps, but the resources behind the links and the (presumably) expanded explanation behind the links are still gone. How can we properly deal with this? 

It looks like there have been a few previous discussions:

 - [Utilize the Wayback API to automatically fix broken links.][5] Development appeared to stall on this due to the large number of edits the Community user would be making. This would also hide posts that depended on said link from being surfaced for the community to fix it.
 - [Link review queue][6]. It was in [alpha][7], but disappeared in early 2014. 
 - [Badge proposal for fixing broken links][8]

---

## Footnotes

1. This is how it ultimately played out. Originally I sent `HEAD` requests, in an effort to save bandwidth. This turned out to waste a whole bunch of time because there are a whole bunch of sites around the internet that return a [`405 Method Not Allowed`][9] when sending a `HEAD` request. The next step was to sent `GET` requests, but utilize the default Python [requests][4] user-agent. A lot of sites were returning `401` or `404` responses to this user agent. 
2. Links to Stack Exchange sites were not counted in the above results. The failures seen are almost 100% due to a question/answer/comment being deleted. The process ran as an anonymous user, thus didn't have any reputation and was served a 404. A user with appropriate permissions *can* still visit the link. I verified a number of 404'd links to Stack Overflow posts and this was the case.
3. The 4th most common failure was to `localhost`. The 16th and 17th most common were `localhost` on ports other than 80. I removed these from the result table with the knowledge that these shouldn't be accessible from the internet.
4. There where 7 total URLs that returned status codes in the `600` and [`700`][10] range. One such site was [code.org][11] with a status code of 752. Sadly, this is not even defined the joke RFC. 


## Follow up

I [posted][12] a proposal on how I think this could be fixed.

 [1]: https://archive.org/details/stackexchange
 [2]: http://meta.stackoverflow.com/a/262040/189134
 [3]: https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods
 [4]: http://docs.python-requests.org/en/latest/
 [5]: http://meta.stackexchange.com/a/198357/186281
 [6]: http://meta.stackexchange.com/questions/224895/what-happened-to-the-broken-link-review-queue
 [7]: http://meta.stackexchange.com/questions/212023/where-can-i-access-the-link-validation-review-queue
 [8]: http://meta.stackexchange.com/questions/174347/badge-request-for-fixing-dead-links-pipefitter
 [9]: https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#4xx_Client_Error
 [10]: https://github.com/joho/7XX-rfc
 [11]: http://learn.code.org/hoc/1
 [status_image]: {attach}images/status_codes.svg
 [12]: {filename}2015_08_07_a-proposal-to-fix-broken-links-on-stack-overflow.md
 [a]: http://meta.stackoverflow.com/q/300916/189134
 [b]: http://meta.stackoverflow.com/users/189134/andy?tab=profile