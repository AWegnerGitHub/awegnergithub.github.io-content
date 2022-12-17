Title: Review of Travis CI Tutorial Udemy course
Date: 2019-07-09 14:30
Tags: review, technical, learning
Category: Review
Slug: travis-ci-tutorial-review
Summary: My review of the Travis CI Tutorial course on Udemy.
Status: published
Series: Course Reviews
template: review
revieweditem: Travis CI Tutoria
score: 2.0

[TOC]

## Introduction

I've written about Travis CI before. I've reported a bug that makes it fairly easy
to see [environment variables in Travis CI][2] or even unintentionally transfer
them if you transfer your repository. I [build this blog using Travis CI][3]. I've
[used Travis CI to set up an OpenShift application][4]. [SmokeDetector][5] uses it to
manage it's blacklists. In short, I have experience with Travis CI.

I was surprised when I saw a course being offered for free recently on Udemy that
deals with Travis CI. Since it was free and only a couple hours long, I figured I'd
give it a shot. I enrolled in the course.

This is my review of [Travis CI Tutorial][1] by Vaga Notes.

## Course

This course starts with no instructor introduction, no explanation of what
the course is about, or goals for the course. If you don't know what Travis CI is,
don't expect to learn that here. You are expected to know what it is and what
it does. Honestly, you should probably have used it before too.

All coding done for this course is gone in the GitHub web editor. This bothered
me initially, but after thinking about it, not so much. Most of the "real code"
needed for this course is simply editing the `.travis.yml` file and the instructor
saves a lot of time by *not* dealing with git and GitHub more than necessary.

Unfortunately, I think that is going to be the last good thing I have to say
about this course. The rest can be summed up with one word: Inconsistent.

The sound is inconsistent. From one lesson to another, the instructor goes from
being whisper quiet at highest volume to so loud it hurts. The microphone being
used picks up static and most annoyingly, the instructor keeps coughing into the
mic. Sound *within* a lesson is also inconsistent. In one lesson - Lesson 13 "Build Stages" -
it went from quiet to loud and back multiple times.

Testing of the course material was inconsistent. The instructor recording their
videos an hour after they had done it the first time, in some cases. This
short time between doing it the first time and recording it for the tutorial
is seen in how the instructor handles unexpected delays and failing builds.

The presentation is inconsistent. On several videos, the instructor obviously
spent time making an intro and outro for the lecture. It's a few second animation
and sound effect that encapsulates the lesson. On others, they jump right into the
material or awkwardly end the lesson. If the instructor had spent more time
giving the course a unified look and feel through the entire course, it'd come
across as more polished. Instead, it looks like it was thrown together haphazardly.

Which brings me to the content. This is also inconsistent. There are points where
the instructor didn't have a script at worse and only high level bullet points
at best. They mumble their way through an explanation or series of interface
options. They live code - with typos - their way through set up. They navigate to
GitHub using Google Search, but accidentally click to quickly and get a previous
search result (that could have been embarrassing).

Along with the live coded examples and an after thought of an explanation of
what each step means, there is very bad advice given in some locations.

The first example of this is setting up environment variables (see my post
on [Travis CI environment variables][2]). While setting up the GitHub token, so
that Travis can deploy GitHub Pages, the advice given is to "just give it all
the access". NO! NO NO NO NO NO! NO!. Especially when combined with the next
step they took, which isn't even described.

The instructor moves over to Travis CI from GitHub to put in the environment variable.
They put in the name of the variable and the value. Then they change the "Display
this value in build log" from the default "Off" to "On" *and don't say why*!.

Moments later a build is pushed, the Travis CI log is shown, and there is the
GitHub token that had "all the access" displayed in the build log. Anyone can
come along and use that token to do anything to your GitHub repository.

## Final Thoughts

This course is now listed for $20. It's not worth it. Go find something on YouTube,
look at another repository that is already using Travis CI or Google for an example.
Any of those options are going to be more useful to you - and more consistent - than
this course. With the bad sound, half finished video introductions, and horrible
advice on token generation the only thing you'll be doing if you take this course is
play with your volume constantly and learn the wrong way to set up environment variables.

This course feels like it was recorded while the instructor was home sick with a
minor cold and got bored with their video editing software.

[![Software Testing Masterclass (2019) - From Notice to Expert][certificate]][courselink]


 [1]: https://www.udemy.com/travis-ci-tutorial/
 [2]: {filename}2018_02_28_do_not_trust_travisci_environment_variables.md
 [3]: {filename}2018_11_15_autobuilding_this_blog.md
 [4]: {filename}2015_12_11_how-i-set-up-openshift-travisci-and-flask.md
 [5]: {filename}2017_02_19_can-a-machine-be-taught-to-flag-spam-automatically.md
 [certificate]: {attach}images/udemy-travis-ci.jpg
 [courselink]: https://ude.my/UC-IJRCAV24