Title: Taking Codility's sample interview exam using ChatGPT
Date: 2023-01-05 01:00
Tags: technical, job search, chatgpt, ai interviews
Category: Technical Solutions
Slug: chatgpt-continues-beating-interview-questions
Summary: I'm continuing my look at technical interview exam questions, this time with Codility, and using ChatGPT to show the process needs to change
Status: published
Series: ChatGPT Commentary

[TOC]

## Introduction

It was announced on January 3, 2023 that [New York City has banned students and teachers from using ChatGPT][9], because of "concerns about negative 
impacts on student learning, and concerns regarding the safety and accuracy of content". This continues the daily news stories 
about [ChatGPT][1] that have come out over the past several weeks. I suspect that 2023 is going to be a massive year for public 
awareness of "AI". Especially with GPT-4's expected release in a month or so. In the meantime, 
[ChatGPT remains banned over on Stack Overflow][2]. The [accuracy of ChatGPT][3] was also mentioned by New 
York City in their banning of the tool in the school system. 

My perspective is that this is a tool that software engineers can utilize, but they have to understand that it is just that - a tool. It is 
only as good as the engineer that wields it. A good engineer will be able to utilize it very effectively. Hiring managers need to be aware of 
what this tool is capable of, though. I've already shown that it can easily solve common interview questions that the software industry uses
in their hiring process. I showed that questions from [LeetCode][4], [TestGorilla][5], and [CodeSignal][8] are easily solved by this tool in 
previous articles in this series.

Today, I'm going to look at another service, similar to these: [Codility][10]. Much like CodeSignal, Codility sits on both sides of the interview
process. It has a developer facing set of challenges and learning material. It also sells services to companies seeking to streamline a portion
of their hiring process.

_First a disclaimer_: I was Director of Engineering at [Woven Teams][6] in 2022. Prior to that, [I was a customer of Woven for over two years][7]. 
Woven is a competitor of Codility. 

The question in this post is taken from [Codility's sample test][11]. I will not be sharing the exact prompt from the sample test, as Codility has
a disclaimer in the prompt clearly stating that disclousure is prohibited. Fair enough.

It'd be a shame if they hid all of their technical exam questions behind such a disclaimer though. Especially when this 
[sample question sits on Stack Overflow][12] with over 400k views and has over 100 answers. It also generated it's own 
[meta question][13]. 

This is just one reason that these types of questions aren't great for technical interview questions. Exam questions will always escape the 
exam environment. A student, a job candidate, or someone interested in the content will be able to find it and the answers after it's out
there.

## The problem

If you want to see the full prompt, take a look at the [Stack Overflow question][12]. But, briefly, the problem to solve is to find the 
smallest positve integer that does not appear in an array of numbers. We are provided three sample arrays with positive, negative and duplicate entires.

I copy and pasted the prompt into ChatGPT and got a short function back.

    def solution(A):
        # Create a set of all the positive integers in the list
        positive_int_set = set([x for x in A if x > 0])

        # Iterate through the positive integers, starting at 1
        i = 1
        while i in positive_int_set:
            i += 1

        # Return the first integer that is not in the set
        return i

I pasted this back into the testing environment and ran the code.

    Compilation successful.

    Example test:   [1, 3, 6, 4, 1, 2]
    OK

    Example test:   [1, 2, 3]
    OK

    Example test:   [-1, -3]
    OK

    Your code is syntactically correct and works properly on the example test.
    Note that the example tests are not part of your score. On submission at least 8 test cases not shown here will assess your solution.

## Results

The built in test cases worked as expected. After I submitted the code, I need to wait about 5 minutes for the hidden test cases to be processed and 
receive my results. 

![Codility - Missing Integer Sample Test][14]

I passed the 3 example test cases, 5 additional "correctness" test cases and 4 performance test cases. 100%

Hooray! ChatGPT wins again. Total time taken - 4 minutes (plus 5 minutes for Codility to do it's evaluation). 

## Change your interview processes

Engineers are smart individuals. They, generally, enjoy new technologies too. ChatGPT is a new technology that is going to be a valuable
tool in their future. GitHub has had [Copilot][15] for a while now. ChatGPT brings a new tool to the toolbelt. It should be expected by 
hiring managers that your current engineering team and engineers you hire will use such tools. 

Interview processes need to adapt to the existance of these tools. If you are asking a question that a simple tool can solve, are you 
really determining an engineer's ability? Do your counterparts ask someone in finanace to calculate a percentage and not expect the candidate 
to use, at least, a calculator? 

Hiring processes much adapt. Evaluate your candidates, especially the more experienced candidates, with something other than an exam question that 
their newest tool can solve. Your engineer is going to be doing more than writing code and implementing an algorithm. Evaluate _those_ skills. Those 
are the ones you really need on your team. 

If you are curious about the other common interview systems and how they perform against ChatGPT, read one of the articles in the series below.


 [1]: https://openai.com/blog/chatgpt/
 [2]: {filename}2022_12_05_stack_overflow_bans_chatgpt.md
 [3]: {filename}2022_12_20_play_with_chatgpt_and_pf_api.md
 [4]: {filename}2022_12_15_get_rid_leetcode_interviews.md
 [5]: {filename}2022_12_26_chatgpt_breaks_another_interview_exam.md
 [6]: https://www.woventeams.com/
 [7]: {filename}2022_06_04_the_other_side_of_the_mirror.md
 [8]: {filename}2023_01_01_chatgpt_codesignal_questions.md
 [9]: https://ny.chalkbeat.org/2023/1/3/23537987/nyc-schools-ban-chatgpt-writing-artificial-intelligence
 [10]: https://www.codility.com/
 [11]: https://app.codility.com/demo/take-sample-test/
 [12]: https://stackoverflow.com/q/51719848/189134
 [13]: https://meta.stackoverflow.com/q/418024/189134
 [14]: {attach}images/codility-results.png
 [15]: https://github.com/features/copilot