Title: Review of Learn Data Analytics with Polars (Python) Course
Date: 2024-01-25 8:30
Tags: review, technical, learning
Category: Review
Slug: learn-analytics-with-polars-review
Summary: A review of the Udemy course: Learn Data Analytics with Polars (Python) in just 2 hours!
Status: published
Series: Course Reviews
template: review
revieweditem: Learn Data Analytics with Polars (Python) in just 2 hours!
score: 9.5
price: 54.99
saleprice: 12.99

[TOC]

## Introduction

I started this entire [Course Reviews][1] series 7 years ago, with a [post about a Pandas course][2]. It went so well, I continued both taking various courses to expand my own knowledge and sharing my experiences with those courses here. It seems fitting to look at a Pandas competitors: [Polars][3].

I've used Pandas for years and it's done the job well enough. But, Polars is gaining traction in the industry and one of the data engineers at work mentioned it as a possible tool to look at. I like to be informed when my engineers make tooling recommendations, so I took it upon myself to learn a little more about Polars. I did so by selecting a course by Kieran Keene, titled [Learn Data Analytics with Polars (Python) in just 2 hours!][course]. I got this course duing another of Udemy's sales for $13.

## About the course

The entire course is run out of [Google Colab][4]. This is nice, because you don't need to set up a virtual environment or install anything to take this course. 

The course is taught with Polars version `0.17.3`, which is from April 2023. I took this course in January 2024, so I used the current version - `0.20.2`. This introduces a few very minor deprecations from the older version, but simply reading the deprecation warning for each tells you how to solve the problem.

Named, the two deprecations that I recall running into:

 * In 0.17 there is the `df.apply()` function. This has been deprecated in favor of `df.map_rows()`
 * The `groupby()` function has been deprecated in favor of `group_by()`

Both take exactly the same arguments, so it was as simple as renaming the instructors function to the modern one. 

The one other minor difference is that the example data has changed at some point between when the course material was recorded and when I took the course. This doesn't change how any of the lectures behave or any of the examples act. It does change a couple results, so I couldn't compare exact numbers between what I received and what the instructor received.

This was not a problem though. The instructor does a very good job of explaining what each lecture is going to teach, shows at least one method of accomplishing the task, and then summarizing the lecture. Through this, it's easy to determine if the results you get with the different sample data returns an accurate result.

I enjoyed the short lectures. I believe the longest single lecture is 11 minutes long, but each builds off of what you've done previously. By introducing these natural break points, it's easy to complete a topic and then spend a couple minutes experimenting with other methods or logic to see how the library behaves.

Additionally, there are multiple ways to perform certain tasks, and the instructor takes the time to explain each of these. Building off of one another, it's easy to determine which is appropriate for the scenario being discussed.

Speaking of scenarios, the course concludes with two short challenges. The goal is to use the knowledge gained from previous lectures to figure out results to two problems. If you followed along through the hour and a half course, these should be pretty easy to figure out but do require combining multiple steps to get to the answer.

## Conclusion

I enjoyed this course, the instructor's teaching method, and two challenges at the end to tie the course back to the lectures. This was a very hands on course and the breaks between lectures and sections encouraged experimenting with the library. 

Experimenting helps me learn more about the tool, and I commend the instructor for building the course in such a way that students could try out the library.

I also found that I liked the Polars library. Pandas has it's quirks and when I dig more into Polars, I'm sure I'll find it has some too. But, Polars feels easier to grasp. The API is easier to understand as you are reading through the code. 

I think I'm going to try out another course to see what else the library can do. That alone should be a complement to both the library and this course - it's encouraged me to keep learning more.

[![Learn Data Analytics with Polars (Python) in just 2 hours! Completion Certificate][certificate]][courselink]


 [1]: https://andrewwegner.com/category/review.html
 [2]: {filename}2016_12_09_review_of_data_analysis_with_pandas_udemy_course.md
 [3]: https://pola.rs/
 [4]: https://colab.research.google.com/
 [course]: https://www.udemy.com/course/unleash-your-polars-python-skills-in-just-2-hours/
 [certificate]: {attach}images/udemy-learn-analytics-polars.jpg
 [courselink]: https://www.udemy.com/certificate/UC-28b98b47-28aa-47f8-9525-f9d85a7ccc2d/
