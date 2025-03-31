Title: Review of Go: The Complete Developer's Guide (Golang) course
Date: 2022-08-28 12:00
Tags: review, technical, learning
Category: Review
Slug: golang-complete-developer-guide-review
Summary: My review of The Complete Developer's guide course for Golang
Status: published
Series: Course Reviews
template: review
revieweditem: Go: The Complete Developer's Guide (Golang)
score: 9.0
price: 109.99
saleprice: 17.99
workload: PT9H

[TOC]

## Introduction

Personally and professionally, I've started bumping into [Golang][go] more and more. I've been told that
it's a good language to learn and know, but until [recently][2] I didn't have a lot of free time to dedicate
to learning a new language. With my down time now, I figure it'd be a great chance to pick up a new skill.

So, I turned to [Go: The Complete Developer's Guide (Golang)][1] by Stephen Grider on Udemy.

## Course Overview

This is a 9 hour course (plus an hour or two for three self paced assignments). It goes from nothing (what's Go?) through
installation to writing way more than a simple Hello, World! Through out the lessons, the instructor is explaining what's
happening and why, not just throwing code into the IDE and having you run it.

The course goes through two main projects, introducing new Go concepts along the way. The first is building a small program
that deals out a hand in a card game. You learn about variables, functions, receiver functions, structs, slices, unit tests, pointers
and more. Nothing feels overwhelming and it is a nice logical progression.

The other project is a web site status checker, which introduces goroutines and channels. These are very important concepts in the language
and the example, while basic, is not contrived. It is very good at introducing there two features, showing common problems and how
to properly handle them.

Throughout the course, the instructor made use of the official documentation and showed how someone new to the language
should utilize it as well. This is an under rated skill and I hope that other engineers appreciate how useful it is to
be able to read official documentation and not immediately go to Stack Overflow for an answer.

I do have a couple very minor complaints. First, the course is using a slightly older version of Go. In the version the
instructor is utilizing the `ioutil` library is still in use. In the version I have installed, that has been deprecated. This
led to a few IDE warnings, and a quick search to determine how to fix it. Once that was resolved though, it was simple enough
to make the few changes needed locally.

My other small complaint is the relatively short lessons. I love bite sized lessons, but when a lesson is between 4 and 10 minutes and the first
minute and last minute are spent summarizing the previous lesson and giving you and idea of what the next lesson will be,
there is a lot of time spend reviewing.

## Conclusion

I have some down time right now and plan on learning a few more items on my back log. Stephen Grider has a nice library of courses
and several of them cover topics I'm interested in learning about. If they are set up the same way as this course, I think I'll
get a lot out of each of those.

This course was a perfect introduction to Go. It covered the basics, assumed that students have some programming knowledge and related
this "new" language to others you've probably utilized. It doesn't dig into the CS101 concepts, other than to briefly mention how certain
things are done (declare variable, function syntax, etc) and instead focuses on the aspects that make Go unique and useful as a language.


[![Go: The Complete Developer's Guide (Golang)][certificate]][courselink]



 [1]: https://www.udemy.com/course/go-the-complete-developers-guide/
 [2]: {filename}2022_08_18_looking_for_new_role.md
 [go]: https://go.dev/
 [certificate]: {attach}images/udemy-complete-dev-guide-golang.jpg
 [courselink]: https://ude.my/UC-e73a8173-1ea5-4974-bc6e-db9c27128677
