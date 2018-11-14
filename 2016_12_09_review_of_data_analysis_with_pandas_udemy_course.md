Title: Review of Udemy's 'Data Analysis with Pandas' course
Date: 2016-12-09 22:54
Tags: review, technical, learning
Category: Review
Slug: data-analysis-with-pandas-review
Summary: My review of "Data Analysis with Pandas" on Udemy.  
Status: published
Series: Course Reviews
template: review
revieweditem: Data Analysis with Pandas
score: 9.5

[TOC]

It's been a few years since I finished my formal education. I've been getting the itch to take a few more structured
training courses, but don't have the desire to commit to another full university degree. Fortunately, there are a lot
of places online that now offer training and college courses (both free and paid). I'll be picking out a few and going
through them as my free time permits. I'll be sharing my thoughts on the courses in a series of posts with a review of
the course and, if available, a link to show what I produced during the course.

## Data Analysis with Pandas

I recently completed [Data Analysis with Pandas][1] by Boris Paskhaver on Udemy. I use pandas both at work and in
personal projects and constantly find new things the library can do. This course is billed as a 19 hour course spanning
173 lectures. The curriculum looked like it covered both basics and topics I was less familiar with. It also helped
that the instructor was offering a coupon code on reddit to take the course for free. I decided to take the course.

*Spoiler*: Even if this wasn't free, this class was worth the price. It covered a large part of pandas and did so in a
way that didn't make me tune out the instructor, even after 19 hours.

## Thoughts on the lessons

The course starts a bit slow with your basic "How to install" tutorials. The entire course is done in a series of
[Jupyter Notebooks][2] and, obviously, requires that you have Python (the instructor uses 3.5), pandas and a few
other modules installed before we get to the good stuff. The instructor uses OS X during all lectures, but that has
no effect on how things are communicated to the student. Everything is done in the Jupyter Notebooks or at a [conda][3]
prompt. This works the same across all major operating systems. The only reason I felt this was slow was because I
already had all of the requirements installed due to using the library before. I understand the need for this lesson,
though, and can't hold it against the instructor for needing to include this.

One thing that I liked about the introduction was that the instructor provided a series of CSV files we'd use
throughout the rest of the lessons. These CSVs were varied in size and composition. I thought this was a great way to
keep everyone on the same page. I've seen a lot of tutorials on pandas around the internet and most of them depend on
generating random data. By providing everyone with a set of CSVs it is much easier to focus on specific aspects of the
data and how certain functions work.

### Series and DataFrames

The course really begins in Section 2 which is on the pandas [Series][4] object. It covers what a series is, various
methods of creating one, and then goes over the various methods and attributes you can use on a series object. The
module covers 21 lessons and lays the foundation for the entire pandas library. Section 3, 4, and 5 cover
[DataFrames][5]. These sections cover 41 lessons. DataFrames are the heart of the pandas library. This is the object
you'll use most of the time, which explains the number of lessons that focus on DataFrames.

These lessons cover everything from selecting a series (column) in a data frame, adding a new column, dropping null
values and sorting values. More advanced topics include various ways of filtering a dataframe to only the data you are
interested in, applying a function to all values and working with either index labels or index positions. These modules
are the heart of the entire course.

### Text data and DateTime data

Sections 6 and 10 deal with text data and datetime data. One complaint I had about these two sections was that they
were separated so much. Both types of data need to be operated on the same way in pandas. Namely, you have to add
either `.str` or `.dt` when calling a string or datetime function. I also think that datetimes are important enough
and used frequently enough that getting a lesson in on how to properly use them early would make more sense.

That complaint aside, both sections cover their content well. There isn't anything ground breaking here, especially
if you've done any sort of work with either strings or dates in Python. The functions introduced all work as you'd
expect based on that experience.

The DateTime section also provides information on how to work with date offsets and time deltas. Adding and
subtracting days/weeks/months is always important and the lessons cover how to do so pretty well.

### Advanced Features

Sections 7, 8 and 9 cover aspects of pandas that many will use as well. These cover ways to join and group multiple data
sets into a single data frame and the benefits of each method.

MultiIndexes are not something I've used a lot, but after these 14 lessons I have already thought of ways I can
improve my code at work to utilize these.

### Panels

Section 11 covers a topic that was brand new to me: Panels. I have never used them or heard of them. A series is a 1D
dataset, a dataframe is a 2D dataset and a panel is a 3D dataset. It is a group of dataframes. The instructor explained
this concept very clearly and worked through multiple examples of how to build and use a panel.

In this lesson, the panel we utilized was created by calling Google Finance for multiple companies. However, the panel
that was returned was not formatted the way I expected and it bothered me for several of the lessons. At first, this
was mentioned by the instructor in passing, and then seemed to be ignored. However, toward the end of the section, the
course covered ways of reforming the panel to be in a different format.

### Input/Output and Visualization

Sections 12 and 13 covered the portions of pandas that my users see at work. The end result of data manipulation. I
produce visuals or excel documents and they are happy. The lessons cover how to export to CSVs and Excel (you need
an `ExcelWriter`). It also covers the four most common visualizations I produce at my job - line charts, bar charts,
pie charts and histograms.

Outside of the scope of this course, but something I'll look into, is what else `matplotlib` can do. We did very
little to customize our plots and I know that the library can do so much more.

### Options

The final section covers a few of the miscellaneous options pandas has. There isn't anything exciting in this section
and these 4 lessons are short. Placing them at the end is a decent spot for them, as it reminds you to go look at the
documentation to see what other options are available (there are a ton).

## Final Thoughts

While I used a coupon code to take this course for free, I feel it was worth the listed price. The course covers 19
hours and 172 lessons. The may seem overwhelming at first, but the lessons are short, with most falling in the 4-7
minute range. There are a couple that reach the 10+ minute point, but they still held my attention the entire lesson.

The instructor has the lessons structured in a way that allows you to pause and return at the start of any lesson
without having to reexecute a long series of code. Most of the lessons are self contained, where a dataset is either
reimported at the start of each lesson or a new one is introduced.

The only Section that didn't seem to follow this pattern was the visualization module. That module kept using code
from previous lessons. The section is less than an hour, but if you take a break you may need to rerun some of your
code in this section.

There are 4 "quizzes" in this course. If you've worked through the lessons with the instructor, the questions are
very easy.

I've now spent 19 hours (re)learning pandas and I feel that I've still just scratched the surface. I've learned a lot
that I'll be taking back to my code, but throughout the course I still got the impression there is much more to pandas.

## Notebooks

The notebooks that I created during this course are all available on [GitHub][2]

[![Completion Award][6]][7]



 [1]: https://www.udemy.com/data-analysis-with-pandas/learn/v4/overview
 [2]: https://github.com/AWegnerGitHub/Data-Analysis-with-Pandas
 [3]: https://www.continuum.io/downloads
 [4]: http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html
 [5]: http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html
 [6]: {attach}images/udemy-data-analysis-pandas-completion.jpg
 [7]: https://ude.my/UC-FB6LLMB5
