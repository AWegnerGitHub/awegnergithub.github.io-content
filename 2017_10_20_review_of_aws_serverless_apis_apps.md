Title: Review of Udemy's AWS Serverless APIs & Apps - A Complete Introduction course
Date: 2017-10-20 11:17
Tags: review, technical, learning
Category: Review
Slug: aws-serverless-apis-apps-review
Summary: My review of the AWS Serverless APIs & Apps course on Udemy.  
Status: published
Series: Course Reviews
template: review
revieweditem: AWS Serverless APIs & Apps - A Complete Introduction
score: 8.5

[TOC]

## Introduction

I've been involved with the [SmokeDetector][1] spam hunting project for several years. In that time It's grown from
a small Python script to a large combination of Python, Ruby and user scripts. The infrastructure hasn't really changed
in that time, though, and it's starting to show. SmokeDetector, the Python script, is run by multiple users so that
random network problems that kill the instance don't take the entire system off line. Arguably, the project should
work on fixing this instability, but the alternative that we came up with works too.

At least it did. Now this distributed way of running is starting to cause issues. Keeping each instance in sync
with one another is a very difficult problem. We've built a decent solution to this for handling spam patterns,
but we've failed to replicate this for other things like permission sets or notifications. Adding this involves
integration with GitHub and a series of new commands. It's painful.

One of the recent discussions to solve this problem was a centralized database. That brings me to this course.

The [AWS Serverless APIs & Apps - A Complete Introduction][courselink] by Maximilian Schwarzmüller on Udemy
looked like a good overview for AWS lambda and DynamoDB, two solutions the SmokeDetector project was looking
into using.

## Thoughts on the course

This course provides a good overview of Amazon's API Gateway, AWS Lambda and several other Amazon services. There
is a little bit of coding involved - using NodeJS - but a large majority is dedicated to working in the various Amazon
consoles. Except for one optional lecture, the entire course can be completed using Amazon's free tier. This is
great for a course and one of the factors I used to determine if I really wanted to take the course. The instructor
is very clear, throughout the course, which aspects cost additional money so that no unexpected charges show up
from Amazon.

The course covers the API Gateway, AWS permissions, AWS Lambda, body mapping templates, DynamoDB, authentication with
Cognito and hosting a serverless SPA. At first glance this seems like a lot to cover in an 8 hour course. The instructor
has broken these into small digestable lectures that do a good job of introducing each new service, explaining the theory
behind each and then using the service to implement functionality into the course long project.

A course long project is a good way to show how each of these Amazon services are useful to a larger project. Instead of
focusing on a small example project where it is difficult to expand the course work into the real world, a real world
project with various requirements is used. This project uses each of the services I mentioned above in a natural way.
Implementing new functionality doesn't feel forced, instead it is something that users would expect from a modern application.

I am very happy with this course. Everything was very well presented. The project was useful because I can easily understand
how each of the Amazon services function. I also have a good idea how the console works and how to troubleshoot problems
because of the work that was done in this course.

My only complaint with regard to this course is that the video lectures are interrupted with text based lectures consisting
of more details and links to documentation. I appreciate these links and used several of them, but the interruption of the
videos to present these broke the nice flow of the lectures. Presenting these links at the end of the section, I think would be
less of an interruption. This was a minor thing, though, and doesn't take away from how much I enjoyed the course.

## Final Thoughts

I feel that I have a decent understanding of Amazon's services at this point. I am looking forward to applying this new
knowledge and, hopefully, improving the SmokeDetector infrastructure at the same time. I highly recommend this course
if you are thinking of migrating anything to Amazon. I feel much more comfortable about moving aspects of a project
to this new solution.

---

[![AWS Serverless APIs and Apps Completion][certificate]][courselink]



 [certificate]: {attach}images/udemy-aws-serverless-apis-apps.jpg
 [courselink]: https://ude.my/UC-1ESFUC2V
 [1]: {filename}2017_02_19_can-a-machine-be-taught-to-flag-spam-automatically.md
