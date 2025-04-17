Title: What should a software engineering interview look like?
Date: 2023-07-06 15:00
Tags: job, meta, leadership
Category: Leadership
Slug: ideal-engineering-interviews
Summary: I've interviewed a lot of engineers at various stages in their careers. I've utilized existing interview processes, built new ones and improved upon all of those. What do I think an ideal interview process would look like? Read on for details. 
Status: published
Series: Leadership Thoughts

[TOC]

## Some context

I spent nearly five years at PacketFabric. In my role as Vice President of Software Engineering at the company, I rebuilt the hiring pipeline for
the software engineering organization. As the company closed it's Series B round of funding we needed to bring on world class talent quickly.  I moved
on to Woven Teams for a short stint as Director of Engineering where I not only built a tool used by thousands of engineers to get a new job, I hired
for members of my team as well. In my current role at FleetOps, I've been working to ensure this previous experience is utilized to build a 
candidate focused hiring process.

I wrote about my [hunt for a new role in the summer of 2022][1]. This covered my [experiences with various application tracking systems (ATS)][2], 
which [application systems require you to duplicate work][3] and [what job search rejection looks like][4]. In the end I got a job at FleetOps as 
Director of Engineering.

Let's go through what I think an ideal candidate experience should be. Reality means that all of this isn't possible in all environments, but I strive
to ensure as much of this is in place in areas and companies I have a say in. A good candidate experience provides a good feeling about the company
and in any environment - candidate favoring or employer favoring - a happy candidate and new hire will be more effective joining a new company.


## Job Description

Candidates are applying to a role at your company and should know what that role does. Regardless of whether the candidate is a referral from an 
executive or someone who is applying from a job board, the job description sets initial expectations for the role. Many engineers can smell 
marketing hype in a job description. Things like "ninja coder", very vague descriptions or numerous requirements. These are all negatives. A job 
description should be tailored to the role. It can also be reflective of the company's culture without being off putting. 

Job descriptions should include salary ranges. In my experience, the listings with ranges get more applicants. Additionally, it's becoming a legal
requirement in many localities, especially in the United States. This should be a legitimate range. If the range is a hundred thousand dollar spread 
or higher, the believability is pretty low. It signals that the company either doesn't know what they want for this role, that they vastly under pay 
some of their team, vastly over pay some of their team or a combination of all of these. 

Finally, the job description should spell out the application process and expected and realistic timelines. 

## Application

The next step describes a software engineering hiring process. It requires up front work on the part of the company to ensure it goes smoothly. 
The application should be relatively simple. I'm a fan of not weeding out candidates at this stage and immediately sending them to a technical scenario. 
This only works if the process can be automated because a job can get hundreds or thousands of applicants. This should be made clear in the application
and process description that the next step is a technical scenario.

## Technical Assessment

A technical assessment should be real work. Do not do leetcode or code quizzes. If you use either of these, expect them to be easily answered by 
utilizing [ChatGPT][5]. Build a rubric so that every person being evaluated is judged the same way. You should be able to automate some of this
to build in responses that can easily weed out the candidates that are completely inexperienced. 

Scenarios should reflect the type of work they will perform on the job. Build the prompt to fit around a recent problem your team solved. Create a 
problem that is unique to your industry. Utilize the experiences of your existing team to create these scenarios. When building the rubric, do not 
build it so that the only possible solution is the one that your team came up with. There are many possible solutions to a problem. Ensure you get those
to truly see the experiences and expertise your candidates have to offer. 

Build scenarios that have experience levels built in. A junior engineer is going to think about a problem in a different way than a staff engineer. 
The rubric should be reflective of that. 

The technical assessment should be short and timed. At Woven, I told the companies I worked with the entire assessment (usually 2-4 scenarios) should not
last longer than two hours. Candidates are unwilling to take assessments that long. Additionally, untimed scenarios favor candidates that do not have 
other commitments - perhaps another job, family, or hobbies to name a few. Keep candidates on a level field as much as possible.

### Honorium

When I've looked for specialized skillsets or executive roles, I've extended the time frame of scenarios and attached an [honorarium][6]. This will
compensate the candidate for the longer time taken on the application. Unfortunately, free labor is common in the technology industry interview process.
This is to show the company isn't seeking free labor on a problem. It may need to be explained to candidates due to the experiences they have had with
other interview processes.

## Remote work

[I am a huge fan of remote work.][7] With the proper company structure, I can hire from a global candidate pool. It's important that the team is 
able to support their new coworkers regardless of their location in the world. I've seen teams and companies expand their developer ranks by slowly 
expanding the timezones they hire from instead of immediately jumping to global. The trade off here is that you are missing candidates that would be 
good for you team immediately and in turn you are slowly training your team to operate on a broader set of timezones and be more asyncronous. 

How the team expands requires careful consideration of team dynamics and executive support. It should also be a discussion the hiring manager is 
prepared to have with candidates. "Remote" does not always mean someone sitting a handful of timezones away. For some it may mean that they like to 
travel and could be in the same timezone as you this month and over the winter they are half a world away. How will you, as a team and as a manager,
respond to this type of situation.

## Onboarding

Once you've made a decision to hire someone, the hiring manager should remain in contact with the candidate. Don't just hand things over to Human 
Resources and assume everything is perfect. Changing jobs and starting a new job is a big deal. Talk with the candidate on next steps. Ensure they are
getting the right equipment to perform their job. Start setting up the first tasks they will be doing when they join the team.

Day one is going to consist of HR work. There is paperwork to fill out, there are things to sign, there are policies to go over, and insurance to sign 
up for (in the US at least). Everyone with a job has done this stuff. Build this time into their first days on the job. Day one also consists of getting
the new employee access to systems and introductions. 

Work with the employee and provide a checklist of things they should be doing and who they should be working with to complete these first items. They 
don't know who in HR to work with because they are new. They don't know their team lead because they are new. The hiring manager should make these 
introductions easy.

Set up expectations for the first week or two, the first month and first three months. They will need to learn the environment, get access to the 
ticketing system and code repositories, and start making their first commits to the code base. But, that's not a day one thing. They will need to 
learn how system A talks to system B. Again, this is not a day one thing. A new user is going to get a firehose of information. Make it somewhat easier
for them by not expecting them to be an expert by the end of week two.

## Intangables

A hiring process has a lot of intangables that can make your candidates feel better or worse about your company. A few examples are:

 1. How many interviews does the candidate have to go through? To many quickly burns out a candidate and makes the entire process feel like a waste of time. To few, or only technical interviews, doesn't give the candidate a sense of the company and the team.
 2. Can the interviewers answer a candidate's questions? Candidates are interviewing you too. Does your interview team have the ability to answer a candidate's questions about the company, role expectations, day to day work, job perks, and more? Is the interview committee excited about a new teammate or are they stressed out/uninterested.
 3. Does the candidate receive updates on their application through out the process? Feedback is important and ghosting a candidate will quickly turn an excited candidate to one that doesn't want to join the team. 
 4. Communication with rejected candidates is important too. Coming back to ghosting a candidate and applicant tracking systems, these can be set up to send candidates emails based on what's happening with their application. If they are rejected, have the system send them a message. Please, PLEASE, don't use a default template for this though. There is a person receiving this email. They deserve more than "we're not moving forward at this time." If the hiring manager has talked to a candidate then this email should be more than the basic template sent to candidates rejected earlier in the process. A rejected candidate may reapply to a future role. How you inform them that you aren't hiring them right now determines if that candidate considers reapplying in the future.
 5. How is the company perceived to the outside world? A candidate wants to join your team, which means their name will be linked with your company for friends and family at least. Future jobs will associate your company with their name. The candidate may ask about this perception - recent news articles, CEO announcements, blog posts - and the interview team should be able to answer these types of questions.

## Conclusion

Interviewing is different at each company, but candidates don't know what the process looks like at each until they are into the process. The earlier
this process is communicated to them, the smoother the candidate's experience will be. This requires that the company and hiring manager have spent time
building the job description, technical assessments, and interview cycles ahead of time. Organization is important.

Communication is more important. The candidate is applying to join your team. How you communicate with them is their first indication on how they will
be communicated with as an employee. 


 [1]: {filename}2022_08_18_looking_for_new_role.md
 [2]: {filename}2022_09_06_job_source_ats.md
 [3]: {filename}2022_09_13_reenter_your_resume.md
 [4]: {filename}2022_10_24_job_search_rejections.md
 [5]: {filename}2022_12_15_get_rid_leetcode_interviews.md
 [6]: https://en.wikipedia.org/wiki/Honorarium
 [7]: {filename}2023_05_23_future_of_remote_work.md