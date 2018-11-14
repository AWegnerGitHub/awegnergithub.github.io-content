Title: Link Analysis - Technical Explanation
Date: 2015-8-10 23:41
Tags: Stack Exchange, programming
Category: Side Activities
Slug: link-analysis---technical-explanation
Summary: In my last two posts, I've discussed the number of rotten links on Stack Overflow and a proposal to fix said links. In this post, I'm going to discuss how I performed this analysis. 
Status: published
Series: Stack Overflow Link Analysis

[TOC]

## Introduction

In my last two posts, I've discussed the number of [rotten links][1] on Stack Overflow and a [proposal to fix said links][2]. In this post, I'm going to discuss how I performed this analysis. 

## Set up

### The database

The process began by downloading the March 2013 [data dump][3]. I loaded the `posts` into a [MariaDB] instance on my local machine. This was accomplished with a very simple script and patience, as the script took a while to run.

    load xml local infile '/path/to/posts.xml'
    into table posts
    rows identified by '<row>';

### The data 
	
Once this was done, the next step was selecting my random sample of data. I did this by randomly selecting 25% of the days in a year and then pulling all posts for those days across all years of Stack Overflow's existence. The Python script I used to do this was fairly simple:

	from datetime import timedelta, datetime
	from random import randint
	from math import ceil

	def random_date(start, end):
		return start + timedelta(
			seconds=randint(0, int((end - start).total_seconds())))
			
	percentage = 0.25
	days = 366
	
	dayslist = []
	for d in xrange(int(ceil(days*percentage))):
		dayslist.append(random_date(datetime(2008,1,1), datetime(2008,12,31)))
		
At the end of this run, the days that I cared about are in the `dayslist` variable. I used that to pull questions and answers from the database that were created on that month/day combination. In the end, this resulted in just over 25% of the total posts being selected. To ensure that I could replicate the results, I also saved the dates that were selected.

## Parsing the data

The next step was to parse out links from the data. I used the following script to extract anchor text and links from a post. 

	def links_in_post(post):
		"""
		Returns a list of all links found
		:param posts: A list of dictionaries with a 'body' key containing HTML strings
		 [
			{
				'body': "<b>This is HTML</b>"
			},
		]
		:return: A list of tuples containing anchor text and URL
			[
				('Display Text', 'http://example.com')
			]
		"""
		logging.debug("Extracting links...")
		links = []
		images = []
		regexp = "&.+?;"
		list_of_html = re.findall(regexp, post)
		for e in list_of_html:
			if e in invalid_entities:
				h = HTMLParser.HTMLParser()
				unescaped = h.unescape(e) 
				post = post.replace(e, unescaped) 

		doc = html.fromstring(post)
		for link in doc.xpath('//a'):
			links.append(Link(text=link.text_content(), link=link.get('href')))
		for image in doc.xpath('//img'):
			images.append(Link(text=image.get('alt'), link=image.get('src')))
		all_items = links + images
		seen = set()
		unique_items = [item for item in all_items if item[1] not in seen and not seen.add(item[1])]
		return unique_items

The regular expression being utilized, is to strip out HTML entities. This was needed due to weird parsing issues with non-ASCII characters. Fortunately, I wasn't the [first to encounter oddities like this][5]. The list comprehension at the end of the function is returning only unique tuples of anchor text/link. I was surprised how often I'd end up with tuples such as `('this', 'http://google.com')` in the same post. This uniqueness saved a lot of processing time later.

After all links in a post had been extracted, this information and information about the post itself, was saved to the database. If a post had no links, it was not saved. The database consisted of three tables. 

 - Links - This table contains the base URLs seen in all posts. URLs are distinct. It also contains an ID that will be utilized for linking to the other tables.
 - Post Links - This table contains information about links in a post. This includes the specific anchor text/link combinations
 - Link Results - This table contains the results of link status checks
	
Processing the posts was fairly time consuming, but was able to be parallelized easily. That significantly cut down on processing time.

## Checking the links

The most time consuming portion of this entire project was actually checking link status. Each link that appeared in the `Links` table was checked. As I mentioned in my first post, the original idea was to simply send a `HEAD` request to each URL. The idea was to save myself and the end point a tiny amount of bandwidth. I had over a million links to process. I figured a little saved bandwidth wouldn't hurt.

I turns out this isn't a good idea. When I started seeing larger sites as not being accessible, I go suspicious that something was wrong. These sites were returning status 405 errors. This indicates that the method is not allowed. So, I switched to `GET` for every link. The next problem I ran into was that many sites didn't like the default user agent of the spider. They rejected requests with 404 and 401 errors. In the end, I got around this by mimicking Firefox on every request. 

With those kinks worked out, every link was sent a `GET` request that looked to be from a Firefox browser. The process would allow 20 seconds per link. If the link didn't respond in that time limit, it was declared inaccessible. 

A week later, I repeated the process with anything that hadn't returned a status code less than 400. Once more, on the third week, I repeated this with the failed links. At the end of three weeks, I had a list of sites that were inaccessible to me - on a residential connection - three times over a period of three weeks.

## Results

The [SVG image][6] that I created for the write up was generated with Pygal. The tables were the result of various break downs of the data via queries to the status results. 

## Wrap up

I am rather proud of how the results turned out for this project. I went into it expecting about 15% of links to be broken, but I didn't really realize what the meant. Fifteen percent of 21 million total posts is over 3 million. That's a large number. BUT, it also ignored that a large percentage of posts don't contain links. I failed to consider that in my original hypothesis. 

Less than half of my sample had links (2.3M out of 5.6M). Of the 2.3M with links, only 1.5M were unique links. The final result of 10% failed links makes much more sense in this context. Ten percent of 1.5M links means that there are 150K links that are bad. 


 [1]: http://andrewwegner.com/analysis-of-links-posted-to-stack-overflow.html
 [2]: http://andrewwegner.com/a-proposal-to-fix-broken-links-on-stack-overflow.html
 [3]: https://archive.org/details/stackexchange
 [4]: https://mariadb.org/
 [5]: http://stackoverflow.com/a/13939198/189134
 [6]: http://andrewwegner.com/images/status_codes.svg