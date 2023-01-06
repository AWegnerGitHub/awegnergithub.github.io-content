Title: ChatGPT is here - Use that to stop asking LeetCode questions during interviews
Date: 2022-12-15 23:45
Tags: job, leadership, job search, chatgpt, ai interviews
Category: Jobs
Slug: chatgpt-should-end-leetcode-interviews
Summary: ChatGPT's availability and ability to answer basic questions should end LeetCode use in software engineering interviews. Right?
Status: published
Series: ChatGPT Commentary

[TOC]

## Introduction

I have interviewed a lot of software engineers looking for a new position. I've talked with college students looking for
an internship, junior engineers just getting started, staff engineers looking for a new role, managers and directors to
lead these teams. In every one of those interviews, I'd told the candidate that I _hate_ puzzle questions (ie.
"How many windows are in Seattle?") and I hate LeetCode style questions.

It turns out that [ChatGPT][1] is good at solving these problems.

Good. Maybe that will kill LeetCode interview questions once and for all. There are so many better ways to determine if an
engineer is the right fit.

## ChatGPT vs LeetCode

We have [temporarily banned ChatGPT over on Stack Overflow][2], because it can and does provide subtly incorrect answers.
It's been so bad that we called up two new moderators from the previous election today. One of my mod [colleagues provided some
context][3] around this today too.

> We get a few hundred CGPT flags per day, leading to hundreds or even thousands of post deletions per day. There's often one flag per user (not accounting overlap from several users), but each flag can lead to anywhere between a couple and tens of deletions per user.

LeetCode, however, checks your answer right away. They don't have the same problem as Stack Overflow. I wanted to see just how good 
this new chatbot was, and took it for a spin against a random LeetCode problem.

I randomly selected a medium difficulty problem with a 58% acceptance rate. My victim question:
[1143. Longest Common Subsequence][4]

### Getting started

The first step in passing this test, is selecting the entire prompt, copying it and pasting it directly into LeetCode's UI. I didn't read
the prompt and I didn't format it. Just a blind copy and paste. I also provided one instruction:

> Provide python code

Press enter and wait 30 seconds or so.

### The response

Within seconds I was provided this block of code


    def longestCommonSubsequence(text1: str, text2: str) -> int:
        # Convert text1 and text2 to lists of characters
        s1 = list(text1)
        s2 = list(text2)
        
        # Initialize the dynamic programming table
        n = len(s1)
        m = len(s2)
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        
        # Fill the dynamic programming table
        for i in range(1, n+1):
            for j in range(1, m+1):
                if s1[i-1] == s2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        
        # Return the length of the longest common subsequence
        return dp[n][m]


It likes the phrase ["dynamic programming table"][5]. Along with this code, it even explains its reasoning for going this route. Maybe 
our intrepid AI can teach a junior dev a new trick or two while doing this puzzle. 

![ChatGPT explains its code][6]
![ChatGPT explains its algorithm][7]

Then it shows that tests pass...very confidently.

> All the tests passed, so this implementation is correct

### The result

I'm trying to show that ChatGPT can do this, so at this point I haven't read the prose above. I copy and pasted the code it returned
and clicked "Submit".

I waited a few seconds for all tests to run and got my results.

![ChatGPT beats over 96% of other implementations in runtime performance and 71% in memory performance][8]

ChatGPT crushed this. It performs better than 96% of other submissions in terms of run time performance. It uses less memory than 71% of
other submissions.

## Can we please stop using LeetCode now?

The total time it took to perform this test was under 5 minutes. I randomly selected a problem, copy and pasted the prompt, waited for a response
from the chatbot, copy and pasted the code response, waited for the tests to run. Complete.

As a hiring manager, I'd have wasted my time and the candidates time having them do this problem now. Instead, we can talk about 
an actual problem they will be solving. We can do a real world problem instead this. There are better ways to interview an engineer
and I'm hoping ChatGPT makes that abundantly clear to hiring managers everywhere. 

We're going to have to adapt our hiring processes, but that's a good thing. My recent experience shows that at most companies, it's bad anyway.
Now we have an opportunity to shine some sunlight on it and make it better.

In the meantime, if you continue to use LeetCode or something similar to evaluate your candidate's technical skills, your are doing your 
team a huge diservice. Candidates know about this tool, and they know that the easiest way to get an interview is to play the game. There 
have been tens of thousands of layoffs in the tech industry in the last six months. I guarentee candidates are looking for every advantage they can
get, to move to their next role.





 [1]: https://openai.com/blog/chatgpt/
 [2]: {filename}2022_12_05_stack_overflow_bans_chatgpt.md
 [3]: https://meta.stackoverflow.com/questions/421619/2022-community-moderator-election-results-now-with-two-more-mods#comment939591_421619
 [4]: https://leetcode.com/problems/longest-common-subsequence/description/
 [5]: https://guides.codepath.com/compsci/DP-Table
 [6]: {attach}images/leetcode-response1-1.png
 [7]: {attach}images/leetcode-response1-2.png
 [8]: {attach}images/leetcode-implementation.png