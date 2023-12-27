Title: Review of Unreal Engine 5 Generative Motion Graphics VFX Course
Date: 2023-04-14 09:15
Tags: review, technical, learning, unreal engine
Category: Review
Slug: generative-motion-graphics-review
Summary: A review of the Udemy course: Unreal Engine 5 | Generative Motion Graphics / VFX
Status: published
Series: Course Reviews
template: review
revieweditem: Unreal Engine 5 | Generative Motion Graphics / VFX Course
score: 7.5
price: 74.99
saleprice: 15.99

[TOC]

## Introduction

After my last venture into Unreal Engine where [I built a simple 2D platformer][1], I wanted to continue learning about blue prints,
but take a set of lessons that would be relatively quick. I found a great course for that in Yu Fujishiro's 
[Unreal Engine 5 | Generative Motion Graphics / VFX][course] course on Udemy.

The course is billed as an hour and a half course that will help the learner gain comfort in using the Unreal Engine as an art
and design tool. Here's the result of my work from this course, and I am happy with the result.

::: youtube vTW2w9IHRVE

## Course Overview

The hour and a half estimated time is slightly misleading, as the instructor is approaching the course expecting that the 
student has some experience. Fortunately, the [handful of Unreal Engine courses][2] I've taken since last year were enough for 
me to feel comfortable in this course.

The first "5 minute" section of the course depends on that knowledge (and your ability to Google) to set up and configure the project.
It's not difficult. I will save you a bit of time on one step though. This minute and a half [video on installing the DLSS plugin][3] 
was more useful than pages of text from NVidia. 

With that out of the way, it's onto the course material. The instructor is clear about the what and the why we are doing things.
I appreciated the explaination of small bits of logic and the migration from hard coded sections to more dynamic blue prints. It 
was a good way to introduce a concept and the same type of thing I've done when teaching beginners. 

I have one issue with my blue print logic that the instructor didn't seem to have. I'm not sure if it's due to different versions
of the engine. I was using 5.1.1 and I believe they were using 5.0. 

![Cube Material Emission not equal to 0][4]

See that emission node down on the lower left? In the course, the instructor has this set to a default of `0.0`. If I left this at 
`0.0`, my cubes were all black. They'd start with the color I selected, but on the first split the children would be black cubes
instead of the expected shifting hue of colors. I couldn't figure out what I did wrong, but adjusting the default solved my
problem.

The only other oddness - at least to me - was the process of exporting a high quality render. In a previous course I 
[exported a video as MP4][5], I learned that through Google and a plugin. This course exports as an EXR file format. I've never
utilized this before so was initially confused by the giant dump of individual files I received. 

Fortunately, [DaVinci Resolve (and the course I took on it)][6] can handle this without problems. This is how I created the 
video show casing my work above.

## Conclusion

This course was short and sweet. It took me approximately two and a half hours to get through. This accounts for environment set up,
playing around with various colors and values to make the composition unique, and the emission bug I mentioned above. I was looking 
for a short course that focused on a single "thing" and was pleased with how this turned out.

There are few things that I didn't like were the assumption that the students had experience. Normally this wouldn't be a problem, but
the instructor does reference "previous courses" a handful of times but they only have this course on Udemy at the time of publishing
this post. Another negative was the instructor's push of a paid for Unreal Plugin. This was for electronic wires, versus the curvy 
ones from the image above. The electronic wires allow for a more compact blue print - which I can see being valuable in a very large
project like the 2D Platformer - but in a short course it does cause the wires the overlap and can be difficult to follow. It also
costs an additional $13.

Overall, this was an entertaining course and I am happy with the output I produced. It was very satisfying the first time I pressed
play and watched my cube split, flash, lauch a few small sparks, and repeat. I recommend this course for someone with a basic 
Unreal Engine experience and a few hours to learn.


[![Unreal Engine 5 | Generative Motion Graphics / VFX Completion Certificate][certificate]][courselink]


 [1]: {filename}2023_04_11_make_2d_platformer_in_unreal5.md
 [2]: https://andrewwegner.com/tag/unreal-engine.html
 [3]: https://www.youtube.com/watch?v=BBx0a6rNgvI
 [4]: {attach}images/cube_material_emission.png
 [5]: {filename}2022_10_18_beginners_building_environment.md
 [6]: {filename}2023_03_03_davinci_beginner_to_advanced.md
 [course]: https://www.udemy.com/course/ue5-procedural-vfx-motion-graphics/
 [certificate]: {attach}images/udemy-generative-motion-graphics.jpg
 [courselink]: https://www.udemy.com/certificate/UC-ce71bf81-06b7-41f4-bbbd-934670454295/
