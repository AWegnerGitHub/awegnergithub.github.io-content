Title: Review of Sanic - An Asynchronous Web Framework for Pythonistas Udemy course
Date: 2019-01-23 10:00
Tags: review, technical, learning
Category: Review
Slug: sanic-webframework-review
Summary: My review of the Sanic Web Framework fro Pythonistas course on Udemy.
Status: published
Series: Course Reviews
template: review
revieweditem: Sanic - An Asynchronous Web Framework for Pythonista
score: 7.0

[TOC]

## Introduction

At work we are starting to write version 2 of our API. As part of this new version, we're migrating from PHP to Python (hooray!). There are various technical
reasons for this, but I am excited. My technical skills are much (*much*) better in Python. Part of this migration involved decided the framework we'd be using
and after several internal discussions, we settled on [Sanic][1]. The first line of the documentation reads:

> Sanic is a Flask-like Python 3.5+ web server that’s written to go fast.

Great! I used Flask at my previous job. To be clear, Sanic is not *based on* Flask, but it's API is *Flask-like*. Good enjoy to start with. With the framework
set, and previous experience with a similar framework, I wanted to go through how Sanic works.

I turned to Udemy and the [Sanic - an asynchronous web framework for Pythonistas][2] course, created by Szabó Dániel Ernő.

## Course

The course is roughly two hours long and consists of 20 tutorial videos. Each one is less than 10 minutes long. It covers a wide range of features available in
Sanic. The instructor made their code available on [GitHub][3]. I never actually used the GitHub repository though. Instead, I followed along with the brief
tutorials.

For me, this speaks to the instructor's ability to keep the lessons and code short and simple yet effectively show how a single feature works. By making the
code easy enough to type and follow along, the lesson was more effective because I was *doing* instead of just reading code.

The first few lessons are a little rough, because the recordings stutter like the machine that was doing the recording wasn't powerful enough. After 5-6 lessons
this clears up. It's a minor thing but I do feel it's something that should have been fixed before the course was posted. One other annoyance was that each
lesson contains the same boiler plate code:

    import sanic

    app = sanic.Sanic()


    if __name__ == "__main__":
        app.run(host="localhost", port=8000)

The lesson code will then go after the `app` variable is defined. The problem with this boiler plate code is that the instructor types it out every single lesson.
Again, this is minor and it makes each lesson self contained, but I think having this code already written and adding in the important, lesson specific code,
would have made the lesson more succinct. It also would have prevented a few typos that the instructor made and had to spend time correcting.

## Thoughts on the lessons

The lessons themselves were effective at teaching a single Sanic feature. Some were a little short and almost all of them lacked any complexity you'd find in
a real application, but they were quick ways to show how a feature worked and didn't spend a lot of time doing more than that. Lessons covered a variety of
things you'd expect a web framework to handle:

 - Request Parameters
 - Aliases
 - Listeners
 - Configuration
 - Middleware
 - Cookies
 - Streaming files
 - Logging
 - Class methodology
 - Blueprints
 - Request Types
 - Error handling

Ironically, in the error handling lesson, the instructor had an error in their code. This is great, because it helps to see how to troubleshoot errors that Sanic
can throw. My issue is that the instructor left the time where he's looking at another monitor (probably reading documentation) in the video. I feel this time
could have been removed and the troubleshooting steps still been effectively shown.

## Final Thoughts

Overall, I was happy with this course. It's about two hours long and does a good job of showing various aspects of Sanic. I don't recall ever being pointed
to the documentation, which is a little surprising, but it was easy enough to find. The course provides a good foundation of knowing what Sanic can handle. As
of the time of this post, Udemy says there have been ten students that signed up for the course. That is low and the lack of Q&A is also a bit concerning. I
don't know how responsive the instructor is to student questions.

If you have a few hours (and dollars, during a Udemy sale) to spare and wish to learn some basics about Sanic, this is a good course to take. If not, the
[GitHub repository][3] contains all of the lessons you go over.

[![Sanic - An Asynchronous Web Framework for Pythonistas Completion][certificate]][courselink]


 [1]: https://sanic.readthedocs.io/en/latest/
 [2]: https://www.udemy.com/sanic-an-asynchronous-web-framework-for-pythonistas/
 [3]: https://github.com/r3ap3rpy/sanic-web-framework
 [certificate]: {attach}images/udemy-sanic.jpg
 [courselink]: https://ude.my/UC-LYP0VLF7