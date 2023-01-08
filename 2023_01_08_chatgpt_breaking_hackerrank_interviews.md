Title: ChatGPT easily solves HackerRank questions
Date: 2023-01-08 01:15
Tags: technical, job search, chatgpt, ai interviews
Category: Technical Solutions
Slug: solving-more-interview-questions-with-chatgpt
Summary: HackerRank is a common tool companies use in interviews. This post continues the series on how ChatGPT can solve these interview problems.
Status: published
Series: ChatGPT Commentary

[TOC]

## Introduction

It's important that companies evaluate their engineering candidates appropriately when they are hiring. Do other areas of your company 
evaluate their potential hires by restricting access to tools they'll use to perform the job? I suspect not. Why do the same to 
your software engineers?

AI Tooling - things like GitHub's [Copilot][11] and now [ChatGPT][1] - are _tools_ that your engineers can use to do their job. During the hiring 
process, engineers should be evaluated on the skills they have, and that includes the ability to utilize all available tools. I expect AI
assistance to be come as common place as using Stack Overflow to solve a problem. ChatGPT, Copilot, and any future tools doing similar tasks are 
going to be used frequently in the near future.

In this article, I'm testing [ChatGPT][1] against one of the most popular coding assessment services: [HackerRank][10]. I've already covered other assessment services like [LeetCode][4], [TestGorilla][5], [CodeSignal][8] and [Codility][9] in this series. I recommend taking a look at those too.

To the engineers that have found this post and are looking for solutions to your coding assessments, I want to warn you, ChatGPT is an
_assistant_. Think of it as a junior level engineer. You may need to check it's work. [ChatGPT is known to be inaccurate][3], which is why 
[it was banned on Stack Overflow][2]. Don't take it's responses as gospel. Evaluate the responses and make improvements as needed. You'll see I 
had to do that a couple of times with the questions below.

_First a disclaimer_: I was Director of Engineering at [Woven Teams][6] in 2022. Prior to that, [I was a customer of Woven for over two years][7]. 
HackerRank is mentioned multiple times in the Woven blog and is a competitor to Woven.

Final note before I begin, HackerRank presented a couple annoyances during this test because they use LaTeX formatting in their prompts. Simply 
copy and pasting prompts to ChatGPT was not possible, because key parts of the problem would be missing. This meant that I 
spent several minutes per prompt cleaning it up and reinserting the LaTeX characters in plain text. This accounts for the higher solve 
times compared to the competing services in other articles.

## Triangle Quest

I selected a random Medium difficulty Python question for my first prompt: [Triangle Quest][14]. This one was unique in this series of tests. 
The expected solution was to add only a single line of code and I could only use arithmetic operations, a single `for` loop and `print` statement.

![HackerRank - Triangle Quest Prompt][12]

As previously mentioned, there were a few LaTeX things to clean up, but then ChatGPT returned a single line of code to add to the `for` loop provided.
The whole code is:

    for i in range(1,int(input())): #More than 2 lines will result in 0 score. Do not leave a blank line also
        print((i * (10 ** i - 1)) // 9)

I submitted the code and passed all test cases.

![HackerRank - Triangle Quest Results][13]

I was surprised by this one. I was expecting to encounter some problems with the constraint of only being able to use arithemtic operations. It 
turns out, ChatGPT caught that constraint and solved the problem without any issue.

## Iterables and Iterators

I wanted to try another Medium difficulty problem, that added more than a single line of code. The constraints on the previous question put 
the problem well into "puzzle" territory, and I am not a fan of puzzles in technical interviews. The second medium question selected was 
[Iterables and Iterators][15].

![HackerRank - Iterables and Iterators Prompt][16]

The first result returned didn't take input. Instead, it hard coded values like so:

    # given list of letters
    letters = ['a', 'a', 'c', 'd']

    # number of indices to select
    k = 2

The interesting thing with this solution was that it ignored the first input row - 
"The first line contains the integer N, denoting the length of the list". I don't disagree with this change. We should be able to determine the length 
of the list from the list itself.

However, the solution required taking input from STDIN, so I let ChatGPT know this by pasting in the only bit of information provided in the 
code here:

> # Enter your code here. Read input from STDIN. Print output to STDOUT

ChatGPT made the neccessary change and provided a new code block.

    import itertools

    # read input from stdin
    n = int(input())
    letters = input().split()
    k = int(input())

    # number of combinations of indices
    total_combinations = len(list(itertools.combinations(range(1, len(letters)+1), k)))

    # number of combinations of indices that do not contain the letter 'a'
    combinations_without_a = list(filter(lambda x: 'a' not in [letters[i-1] for i in x], itertools.combinations(range(1, len(letters)+1), k)))

    # probability that none of the selected indices contain the letter 'a'
    probability_without_a = len(combinations_without_a) / total_combinations

    # probability that at least one of the selected indices contain the letter 'a'
    probability_with_a = 1 - probability_without_a

    # print the probability to three decimal places
    print("{:.3f}".format(probability_with_a))

I submitted this code and it also passed each of the test cases. This solution also ignored using the `n` input, but did accept it as input.

![HackerRank - Iterables and Iterators Results][17]

## Validating Postal Codes

At this point, I've shown that ChatGPT can do medium level problems. So, I selected a Hard problem: [Validating Postal Codes][18]. 
The intersting constraint on this one is that `if` statements aren't allowed. The goal here is to provide two regular expressions to solve the problem.

I've built some nasty regular expressions over the years. I've talked about [SmokeDetector][21] in the past, and it's role in removing spam from 
the Stack Exchange network. It is built on regular expressions. Lots and lots of regular expressions. So, I felt pretty comfortable with 
this question and wanted to see how ChatGPT would handle it.

![HackerRank - Validating Postal Codes Prompt][19]

The second regular expression, the alternating repeating digit pair, didn't work the first time. 

    regex_alternating_repetitive_digit_pair = r'(\d)\d\1'

For those that know regular expressions, that is not going to work. I pointed this out and it corrected itself, and explained the meaning of the
new regular expression. The two final regular expressions are

    regex_integer_in_range = r"^[1-9]\d{5}$"	# Do not delete 'r'.
    regex_alternating_repetitive_digit_pair = r"(\d)(?=\d\1)"	# Do not delete 'r'.

This is using a positive lookahead assertion, which ChatGPT explains as:

> This regular expression will match any digit `(\d)` followed by another digit, with the same digit `(\1)` appearing immediately after that. The `(?=...)` syntax is a positive lookahead assertion, which means that it will only match a digit if it is followed by the specified pattern, but the pattern will not be included in the overall match. The parentheses around `\d` capture the digit so that it can be referred to as `\1` later on.

I submitted the two regexes above, and passed the test cases.

![HackerRank - Validating Postal Codes Result][20]

## Final Thoughts

The majority of my time on these was spent cleaning up the LaTeX formatting. I mentioned a few prompt clarifications needed to solve problems in the 
returned code too - one required accepting user input and the other was an invalid regular expression. Pointing this out to ChatGPT got the 
problem resolved immediately.

This is exactly the type of thing that I'd expect an engineer to need to do with the AI assistant (or more junior engineer). Hiring processes
must adapt to the existance of AI assistants. Evaluate your candidates and their ability to do more than simply write code. 


 [1]: https://openai.com/blog/chatgpt/
 [2]: {filename}2022_12_05_stack_overflow_bans_chatgpt.md
 [3]: {filename}2022_12_20_play_with_chatgpt_and_pf_api.md
 [4]: {filename}2022_12_15_get_rid_leetcode_interviews.md
 [5]: {filename}2022_12_26_chatgpt_breaks_another_interview_exam.md
 [6]: https://www.woventeams.com/
 [7]: {filename}2022_06_04_the_other_side_of_the_mirror.md
 [8]: {filename}2023_01_01_chatgpt_codesignal_questions.md
 [9]: {filename}2023_01_05_chatgpt_codility_sample_question.md
 [10]: https://www.hackerrank.com/
 [11]: https://github.com/features/copilot
 [12]: {attach}images/hackerrank-triangle-prompt.png
 [13]: {attach}images/hackerrank-triange-result.png
 [14]: https://www.hackerrank.com/challenges/python-quest-1/problem?isFullScreen=true
 [15]: https://www.hackerrank.com/challenges/iterables-and-iterators/problem?isFullScreen=true
 [16]: {attach}images/hackerrank-iterables-prompt.png
 [17]: {attach}images/hackerrank-iterables-result.png
 [18]: https://www.hackerrank.com/challenges/validating-postalcode/problem?isFullScreen=true
 [19]: {attach}images/hackerrank-postal-prompt.png
 [20]: {attach}images/hackerrank-postal-result.png
 [21]: {filename}2017_02_19_can-a-machine-be-taught-to-flag-spam-automatically.md