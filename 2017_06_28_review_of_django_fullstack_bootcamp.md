Title: Review of Udemy's Python and Django Full Stack Web Developer Bootcamp course
Date: 2017-06-28 6:16
Tags: review, technical, learning
Category: Review
Slug: django-fullstack-bootcamp-course-review
Summary: My review of the Python and Django Full Stack Web Developer Bootcamp course on Udemy.  
Status: published
Series: Course Reviews
template: review
revieweditem: Python and Django Full Stack Web Developer Bootcamp
score: 7.0

[TOC]

## Introduction

After ploughing through [two machine learning][1] [prerequisite courses][2], I wanted to have a change in content for the next
course I took. I've used Django in the past to build [IRVING][3] - a dashboard that allows users to run queries against many
database types and display the results in one location. The majority of this was done almost 6 years ago though. It was built
to assist in a job I no longer hold. Since then, I've ignored it and haven't used Django for a major project. I have used it to
test a small snippet of code here or there, but never a complete application.

I remember liking Django. To that end, I decided to take Jose Portilla's [Python and Django Full Stack Web Developer Bootcamp][4]
course on Udemy.

## Thoughts on the course

I am conflicted on how to rate this course. On the one hand, it covers a lot in the 30ish hours that the lectures take. Starting at
the very basics and moving through more advanced Django topics, it covers HTML and CSS then moves to Bootstrap, Javascript, the DOM,
jQuery, and Python before finally getting to Django. On the other hand, it moves through those basic lectures very slowly. Django isn't
even reached until two thirds of the way through the course.

The lectures were a combination of hands on coding and an explanation of what the code is doing. The instructor types slightly
faster than me. This was a problem only at transitions between files, for example moving between an HTML file and a CSS file. I found myself
rewinding the video lectures many times around these transitions.

One thing that I liked very early on in the lecture was the introduction of GitHub's [ATOM text editor][5]. I've used this once before, for a job
interview that went very poorly (another story, for another time). I'd heard good things about it though. I used it throughout the course as a way to
force myself to use something other than PyCharm. Now that the course is complete, I have decided that I like ATOM, but not enough to switch from
PyCharm. It does have some nice features, especially with plugins, that I may utilize when building boiler plate code and templates though.

## Foundation vs Django content

The foundational material - HTML, CSS, Bootstrap, Javascript, jQuery and even the Python - dragged on a little long for my taste. As I
mentioned above, this foundation took up the first two thirds of the course. I'd have prefered it if more time could be spent covering
other aspects of Django, especially some of the concepts that are thrown at the student in the last clone project.

The Django content, itself, was great. It started at basics like setting up a project and single application within the project and
moved through URL routing, templates, views and briefly touched on the admin side. There were advanced topics on class based views and
the debug toolbar, both of which are important when developing.

I can't help feeling that important things were missed though. For example, the admin side is barely touched. It is incredibly capable, yet the
most that is done in this course with the admin backend is registering a model to appear. Groups and permissions aren't touched at all. Customizing
views in the backend aren't mentioned, either. Another thing that I was hoping would be covered as part of the "and much,much more!" bullet in the
course description was both [signals][6] and [channels][7], but they are not mentioned.

Clone projects are useful in giving students a "real world" example that they are familiar with to bring everything they've learned together. There
are two such projects in this course, a blog and a message board/social media site. The blog one was a logical extension of the 5 parts that were
covered by Django. The message board, however, wasn't as useful. In an effort to be more "real world", the instructor missed a lot of steps in the
preceeding foundational lectures. This project introduces multiple applications in the same project, but never introduced that concept previously
and doesn't expand on it much here. Instead, the project becomes a combination of speed coding to keep up with what's happening in the lecture and
copy and pasting code from the notes when the instructor does the same thing. I appreciate the more realistic project, but for all the time that was
spent building the foundation of Django knowledge, there is a big gap between the last lecture and this clone.

## Final Thoughts

This course was worthwhile, once I made it through the foundational courses. It was a great refresher for what I'd done 6 years ago. Either I did
things very inefficiently then, or Django has had a *ton* of improvements (or both), but looking back at IRVING, I see many things that could be
improved. I'm not sure if I actually will though, just due to not using the application any longer.

The long foundational courses either should have been condensed. I would have rather had more focus on Django and covered more topics there. This
is especially true after completing the second clone project. New concepts were used in this project, but glossed over as "there is excellent
documentation on this", instead of the in depth explanations that were provided earlier in the course.

I'd recommend this course for the Django topics. However, if you are coming into the course with any type of web development background (even the
relatively basic one I have), be prepared to be bored during the first half of the course.

---

[![Python and Django Full Stack Web Developer Bootcamp Award][8]][9]



 [1]: {filename}2017_04_20_review_of_deep_learning_prereq_numpy.md
 [2]: {filename}2017_05_03_review_of_deep_learning_prereq_regression.md
 [3]: https://github.com/AWegnerGitHub/IRVING
 [4]: https://www.udemy.com/python-and-django-full-stack-web-developer-bootcamp/learn/v4/overview
 [5]: https://atom.io/
 [6]: https://docs.djangoproject.com/en/1.11/topics/signals/
 [7]: https://www.djangoproject.com/weblog/2016/sep/09/channels-adopted-official-django-project/
 [8]: {attach}images/udemy-django-full-stack-bootcamp.jpg
 [9]: https://ude.my/UC-1VGWNREH
