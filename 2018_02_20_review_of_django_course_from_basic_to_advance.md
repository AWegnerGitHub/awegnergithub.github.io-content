Title: Review of Udemy's Django Course from Basics to Advance
Date: 2018-02-20 22:45
Tags: review, technical, learning
Category: Review
Slug: django-course-basic-to-advance
Summary: My review of the Django Course from Basics to Advance course on Udemy.  
Status: published
Series: Course Reviews
template: review
revieweditem: Django Course from Basics to Advance
score: 0.5

[TOC]

## Introduction

With the [new server][1] coming along nicely, I wanted to take a quick refresher on Django. Last summer I went through another [Django course][2]. That
was a decent course. I didn't need anything that intensive or that focused on the non-Django portions though. I have a couple plans for web control panels
that will help me manage the aspects of the new server I care most about. I decided to take a look at [Django Course from Basics to Advance][3] by Bucky Roberts.
It is billed as a five hour course and seemed to hit major aspects of the framework. It was released in January 2018, so it was brand new at the time I signed up.

Spoiler alert: *Avoid this course*. It's a waste of money. The bad grammar in the title should have been the red flag that made me avoid it. Unfortunately, that was
only the start of my issues with the course.

## Thoughts on the course

Literally ten seconds into the first lecture: "If you guys were wondering what the F Django is, it's not a movie, well it is a movie." It only goes
down hill from there. A few complaints:

 - A majority of the lectures start with "Alright Hauses"
 - "Think of [Django] like PHP, if you guys are fuzzy on it, but cooler."
 - Lecture 3 includes a rant about what an "app" is and how everyone is developing "apps" now.
 - In lecture 10, the instructor starts with a rant about an overweight airline passenger sitting next to them on a plane
 - Insults and mocking of front end developers
 - One of the lectures ends with "To be honest, my mom keeps texting me and it's kind of annoying."
 - Another lecture is interrupted with this train of thought: "There is a dog barking outside. It's annoying me."
 - Finally, one lecture ends with "Thank you guys for watching. Don't forget to subscribe!"

It's a YouTube channel. The instructor uploaded their YouTube channel to Udemy. Not only that, they uploaded two year old tutorial videos. It's not
even current!

There are technical issues with the course too:

 - The instructor is using Django 1.9. There is three years out of date at this point.
 - Installation instructions tell users to use administration or `sudo` rights for all Python libraries
 - Installation instructions are done using `easy_install` not `pip`

In the `urls.py` files that define URL routes, there are lines that look like this:

    url(r'^admin/', admin.site.urls)

The instructor attempts to describe the `r` in front of `'^admin/'` by saying:

> Whenever I type "r", it means 'regular expression'.

This is just flat wrong. `r` indicates a [raw string][4]. From the documentation:

> Both string and bytes literals may optionally be prefixed with a letter 'r' or 'R'; such strings are called raw strings and treat backslashes as literal characters.

## Final Thoughts

Don't waste your time with this. Don't look at it. Don't think about it. This course is a YouTube channel. If you really want to watch a two year old series running a
three year old version of Django [watch it on YouTube][5]. It's not worth it. The information is outdated and wrong in many spots. The instructor uploaded this course for
a quick dollar. They didn't build this course for Udemy. They built it for YouTube and you can tell. The quality of content here is not what I've come to expect from
Udemy course.

Fortunately for me, Udemy Support was very helpful when I requested a refund. I listed most of the complaints I did above and mentioned it course was just a
YouTube channel that had been uploaded to Udemy. I was issued a refund in under an hour. I also left my first public review of a course on Udemy so that other students
know they can get the same content elsewhere.

![Course Review][6]

I still want to find a quick refresher. I'll have to go hunting for that later.

---

 [1]: {filename}2018_02_12_a_new_server_for_the_house.md
 [2]: {filename}2017_06_28_review_of_django_fullstack_bootcamp.md
 [3]: https://www.udemy.com/django-course-from-basics-to-advance/
 [4]: https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals
 [5]: https://www.youtube.com/watch?v=qgGIqRFvFFk&list=PL6gx4Cwl9DGBlmzzFcLgDhKTTfNLfX1IK
 [6]: {attach}images/django-course-basics-to-advance-review-screenshot.png
