Title: CoderByte assessments fall to ChatGPT
Date: 2023-02-04 23:30
Tags: technical, job search, chatgpt, ai interviews
Category: Technical Solutions
Slug: chatgpt-beats-more-interview-assessments
Summary: CoderByte falls to ChatGPT just like the more well known interview assessment platforms. Here's my run down, and how I used ChatGPT to solve an easy, medium and hard problem in less than 5 minutes total.
Status: published
Series: ChatGPT Commentary

[TOC]

## Introduction

Today I'll be evaluating [CoderByte][2] and their publicly posted challenges against ChatGPT. I selected an easy, a medium and the single hard
question from their list of [open challenges][3] and ran those through ChatGPT.

In previous articles in this series, I've shown how [ChatGPT][1] can easily solve interview assessments from [LeetCode][leetcode], 
[TestGorilla][gorilla], [CodeSignal][codesignal], [Codility][codility] and [HackerRank][hackerrankg]. I've said it in 
those articles, but I'll say it again: Companies need to evaluate their engineering candidates - all candidates, actually - apprpriately 
when they are hiring. Your engineers are using many tools to do their jobs and AI Tooling is one of the newest tools on the market. Removing 
a tool from your engineers is going to make them less efficient, and _not_ testing your candidates on how they use these tools is 
going to mean you hire someone that had not fully demonstrated their capabilities.

ChatGPT should be treated as a tool, but it's also helpful to think of it as a junior level engineer. You need to check it's work. 
[ChatGPT is known to be inaccurate][pfapi], which is why it remains [banned on Stack Overflow][soban]. There are a few instances where
I had to do that below.

_First a disclaimer_: I was Director of Engineering at [Woven Teams][woven] in 2022. Prior to that, 
[I was a customer of Woven for over two years][mirror]. I do not remember CoderByte being mentioned during my tenure and CoderByte does not 
seem to consider Woven a competitor, as Woven isn't listed as an [alternative on their pricing FAQ page][4].

## Easy - React Button Toggle

While I've tried to be mostly random in my selection of assessments during these tests, I intentionally selected this react 
assessment because I don't know react. I've liked these types of assessment questions in the previous articles because I means I can't 
apply my experience to "nudge" ChatGPT toward a solution. 

The challenge here is to toggle a button between "On" and "Off". You're presented with a very small amount of boiler plate code. 

I simply copy/pasted the prompt and the template into ChatGPT. It sent back this:

    import React, { useState } from 'react';
    import ReactDOM from 'react-dom';

    function Toggle() {
    const [toggle, setToggle] = useState(true);

    function handleClick() {
        setToggle(!toggle);
    }

    return (
        <button onClick={handleClick}>{toggle ? 'ON' : 'OFF'}</button>
    );
    }

    ReactDOM.render(
    <Toggle />,
    document.getElementById('root')
    );

I copied that into the IDE provided by CoderByte and submitted the code.

![CoderByte - Easy Challenge - React Toggle Button Score][5]

This test took two minutes to complete. The bulk of that time was spot checking that ChatGPT didn't mess with the boiler plate code to badly.

![CoderByte - Easy Challenge - React Toggle Button Time Taken][6]

The interesting this about the results is that even with the 10/10, the system says it only scored higher than 12% of other users. Does this mean
that only 12% of the users passing this assessment are getting lower than a 10/10? To me, that's the sign of a bad assessment.

![CoderByte - Easy Challenge - React Toggle Button Score comparison][7]

## Medium - SQL Member Count

The medium challenge I selected was an SQL question. SQL is interesting, in my experience, in that the results returned on an assessment are a 
lot more uniform compared to a non-SQL coding assessment. This one wasn't a difficult one with joins.

The goal of this assessment was to return a list of names in alphabetical order, a count of the number of people that report to them, and the 
average age of their team members. I pasted the prompt, the expected table layout for the output and the table layout being selected from.

The first response was 

    SELECT ReportsTo, COUNT(ID) AS Members, AVG(Age) AS Average_Age
    FROM maintable_YTTQH
    WHERE ReportsTo IS NOT NULL
    GROUP BY ReportsTo
    ORDER BY ReportsTo;

This is where experience, and checking the results are important. The expected output showed that the average age should be an integer. It also showed that the average age column should be named `Average Age` not `Average_Age` (notice the lack of the underscore). I provided this guidance to ChatGPT

> The AVG(Age) should be an integer not a float and The "Average_Age" field should be "Average Age" without an underscore. 

It corrected both problems with a new query which I submitted. 

    SELECT ReportsTo, COUNT(ID) AS Members, ROUND(AVG(Age)) AS "Average Age"
    FROM maintable_YTTQH
    WHERE ReportsTo IS NOT NULL
    GROUP BY ReportsTo
    ORDER BY ReportsTo;

![CoderByte - Medium Challenge - SQL Score][8]

ChatGPT scored another 10/10. 

![CoderByte - Medium Challenge - SQL Time Taken][9]

Even with that extra round trip with ChatGPT, it took less than a minute to complete this assessment. 

![CoderByte - Medium Challenge - SQL Score Comparison][10]

This assessment seems to have a slightly better split of user scores, which is progress I suppose.

## Hard - Bracket Combinations

There is only one hard assessment available publicly. The goal for this assessment is to return the number of combinations you can make with a number 
of pairs of parenthesis. The example provided is:

> The possible combinations of 3 pairs of parenthesis, namely: ()()(), are ()()(), ()(()), (())(), ((())), and (()())

I copied the prompt and boiler plate code into ChatGPT and it returned the following response:

    def BracketCombinations(num):

        from math import factorial
        return factorial(2 * num) // (factorial(num) * factorial(num + 1))

    # keep this function call here 
    print(BracketCombinations(input()))

Experience here says that is odd. Technically speaking, Python will allow you to have that `import` statemnet inside a function, but if I saw one of 
my developers do it, I'd mention it during a code review. I was curious though and submitted it as is. 

I also went back and told ChatGPT:

> Write the function without importing `factorial`

It replied with this function:

    def BracketCombinations(num):
        def fact(n):
            result = 1
            for i in range(1, n + 1):
                result *= i
            return result

        return fact(2 * num) // (fact(num) * fact(num + 1))

    # keep this function call here 
    print(BracketCombinations(int(input())))

I submitted this response too.

I only have one set of images for this challenge, because they both output _exactly_ the same responses. Both solutions scored the same. Both solutions 
were better than 35.1% of other users. Both solutions had an `O(n)` complexity. 

![CoderByte - Hard Challenge - Combination Score and Complexity][11]

Showing the complexity is a nice touch. I will admit that I was expecting the solution that imported `factorial` to be rejected because it's using a 
built in function instead of rolling your own. Good for CoderByte for allowing a developer to use the built in libraries as an engineer would do in 
a production environment, instead of adding a false constraint. 

![CoderByte - Hard Challenge - Combination Time Taken][12]

Copy pasting to CoderByte kept this close to a minute. 


## Final Thoughts

Another code only assessment tool has been shown to be ineffective. The one thing this one does, that others does show publicly, is the 
time it took to solve a problem. I'm very surprised that a majority of these were solved in under ten minutes. Even so, solving a problem in 
less than a minute will probably raise a flag of some kind for a hiring manager giving this type of assessment. I guarentee that time is visbile 
to them.

These three assessments show how well ChatGPT can be in being your virtual junior engineer. With the react question, I have no idea if it's 
the more efficient way to solve the problem but it works. With the SQL question, experience identified two small problems and allowed me to 
provide feedback and get a corrected query in seconds. The third question turned in two different ways of solving the problem. Apparently, according
to CoderByte, bother of which perform exactly the same.

The total time it took me to run these three tests was under five minutes. That's a great tool to have in my pocket.



 [1]: https://openai.com/blog/chatgpt/
 [leetcode]: {filename}2022_12_15_get_rid_leetcode_interviews.md
 [gorilla]: {filename}2022_12_26_chatgpt_breaks_another_interview_exam.md
 [codesignal]: {filename}2023_01_01_chatgpt_codesignal_questions.md
 [codility]: {filename}2023_01_05_chatgpt_codility_sample_question.md
 [hackerrank]: {filename}2023_01_08_chatgpt_breaking_hackerrank_interviews.md
 [soban]: {filename}2022_12_05_stack_overflow_bans_chatgpt.md
 [pfapi]: {filename}2022_12_20_play_with_chatgpt_and_pf_api.md
 [woven]: https://www.woventeams.com/
 [mirror]: {filename}2022_06_04_the_other_side_of_the_mirror.md
 [2]: https://coderbyte.com/developers
 [3]: https://coderbyte.com/challenges
 [4]: https://coderbyte.com/organizations#pricing
 [5]: {attach}images/coderbyte-easy-tests.png
 [6]: {attach}images/coderbyte-easy-minutes.png
 [7]: {attach}images/coderbyte-easy-scores.png
 [8]: {attach}images/coderbyte-medium-tests.png
 [9]: {attach}images/coderbyte-medium-minutes.png
 [10]: {attach}images/coderbyte-medium-scores.png
 [11]: {attach}images/coderbyte-hard-tests-runtime.png
 [12]: {attach}images/coderbyte-hard-solution-solve-time.png
