Title: Running CodeSignal questions through ChatGPT
Date: 2023-01-01 17:00
Tags: technical, job search, chatgpt, ai interviews
Category: Technical Solutions
Slug: chatgpt-breaks-more-interview-questions
Summary: I am going to examine four sample CodeSignal questions and show how ChatGPT solves the problems.
Status: published
Series: ChatGPT Commentary

[TOC]

## Introduction

[ChatGPT][1] has taken over a lot of technology news in the last month. Within days, [Stack Overflow banned ChatGPT][2], due to 
[subtle errors that the chatbot is prone to make][3]. I've also written about how I hope that ChatGPT's existance will change 
the hiring processes in the technology industry, because of how easily it can solve [LeetCode][4] and [TestGorilla][5] technical interview
problems.

Today, I'm going to turn my attention to another company that does interview tests: [CodeSignal][8]. CodeSignal sits on both sides of the 
interview process, with a LeetCode style developer facing set of challenges. Solve problems, increase your score. It also sells services to
companies looking to outsource a portion of their hiring process.

_First a disclaimer_: I was Director of Engineering at [Woven Teams][6] in 2022. Prior to that, [I was a customer of Woven for over two years][7]. 
Woven is a competitor of CodeSignal. 

The questions in this post are taken from [CodeSignal's blog][9], showing off four of their software engineering questions. CodeSignal provides an 
explaination of the question, and a sample response with a quick analysis. 

## Array Manipulation

Surprise! Another array question. I suppose it wouldn't be a generic software engineering interview without a question related to arrays. 

Getting started...

![CodeSignal - Array Manipulation Prompt][10]

> You can see that it covers a basic array traversal and manipulation

A small nitpick - depending on the hirnig process, this sentence can come across as condicending. Depending on where a technical interview fits 
in the process, it's possible that a non-engineer is the one that sees these results first. I would not count on my HR coworkers to understand
"basic array traversal and manipulation", because it's not part of their skillset. 

Let's see how ChatGPT does. I copy and pasted the prompt into ChatGPT and waited a few seconds for a result.

    def solution(a):
        b = []
        for i in range(len(a)):
            # If it's the first element, use 0 as the value for a[i - 1]
            if i == 0:
            b.append(0 + a[i] + a[i + 1])
            # If it's the last element, use 0 as the value for a[i + 1]
            elif i == len(a) - 1:
            b.append(a[i - 1] + a[i] + 0)
            # Otherwise, use the values of a[i - 1], a[i], and a[i + 1]
            else:
            b.append(a[i - 1] + a[i] + a[i + 1])
        return b

It doesn't match the exact response that CodeSignal provides, but it functions with their test case. I am count that as a pass.

## String Pattern Matching

Another common theme in these recent tests I've run is pattern matching. This one, however, at least is more interesting that others. It still 
doesn't seem to reflect a real world problem, and is just a puzzle, but at least it's more unique. 

![CodeSignal - Pattern Matching][11]

> It has the advantage of testing several fundamental programming skills at once: traversing multiple arrays with nested loops, working with subarrays, and performing basic collections/string operations.

We're still sitting in the "basic" tier here, so I suspect that ChatGPT will have no problem. Time to check that assumption by pasting the prompt to the
bot. One thing I did have to modify here is the exponents. They pasted as non-exponents, which would have messed up those constraints at the end. This 
will be a similar problem with a later question. I adjusted those lines slightly to make the numbers exponents.

And, the response back was: 

    def solution(pattern, source):
        # Initialize the count of matching substrings
        count = 0
        # Iterate through the substrings of source with length equal to pattern
        for i in range(len(source) - len(pattern) + 1):
        # Initialize a flag to track whether the current substring matches the pattern
            match = True
        # Iterate through the characters in the pattern and the corresponding characters in the substring
            for j in range(len(pattern)):
        # If the character in the pattern is 0 (vowel), check if the corresponding character in the substring is a vowel
                if pattern[j] == '0':
                    if source[i + j] not in 'aeiouy':
                        match = False
                        break
        # If the character in the pattern is 1 (consonant), check if the corresponding character in the substring is a consonant
                else:
                    if source[i + j] in 'aeiouy':
                        match = False
                        break
        # If the substring matches the pattern, increment the count
            if match:
                count += 1
        return count

A big difference between the response generated and the response provided, is that CodeSignal's response breaks out a portion of the code into
a second function. The ChatGPT response does everything in a single, larger, loop. Personally, I'd take off a point for not breaking this into 
another function. CodeSignal's response is better, but the ChatGPT one functions and passes the test. While the second function is a good idea,
not having it is not a deal breaker for me. This is especially true of a "basic" test, which is probably being used for less senior roles.

## Two-Dimensional Array Traversal

Now we're onto the harder questions. This is a long prompt, with multiple animated gifs, and is multiple pages of scrolling. I'm not going to 
paste the prompt here because the animation is important to understanding the algorithm that needs to be implemented. The general goal though is 
to build a very small scale Tetris board.

> This Tetris-inspired question is challenging. Candidates will have to traverse and manipulate two-dimensional arrays in a non-conventional way. As it’s an advanced question, it tests how well an engineer can understand a complex set of constraints and proceed with a non-trivial implementation.

From my reading of the prompt, and looking at the solution logic, I agree. While this is still a puzzle type question, I don't recall seeing anything
like this previously. The uniqueness, yet similarity to a game that most engineers will know, makes the question interesting to me. 

The animated gifs are useful for a human to see what is expected in the solution, but shouldn't be need by ChatGPT, provided the prompt is descriptive
enough. Copying and pasted the text into ChatGPT, provides the following response:

    def solution(field, figure):
        # Get the height and width of the field
        height = len(field)
        width = len(field[0])
        # Iterate through the columns of the field
        for col in range(width - 2):
            # Iterate through the rows of the field from the bottom up
            for row in range(height - 1, -1, -1):
            # Check if the current cell is occupied
            if field[row][col] == 1:
                # If the cell is occupied, stop checking this column
                break
            # If the figure fits in the current position, check if a full row is formed
            elif all(field[row + i][col + j] + figure[i][j] <= 1 for i in range(3) for j in range(3)):
                # If a full row is formed, return the current column
                if any(all(field[row + i][col + j] == 1 for j in range(3)) for i in range(3)):
                    return col
        # If no full rows are formed, return -1
        return -1

Unfortunately, test cases aren't provided for this one. However, looking at the two responses, there are a couple obvious differences. ChatGPT doesn't
handle the size of the board at all. CodeSignal uses this size in multiple places of their code. ChatGPT also uses `all()` and `any()` checks, where 
CodeSignal uses longer and more complicated `if` statements. Personally, I'd prefer the simiple `if` statements for readability. 

I'm unsure how to score this one without the test cases, but will note that ChatGPT failed CodeSignal's first sentence in the solution explaination:

> This solution starts by defining some dimensions that will be important for the problem. 

I skeptical that without the size of the figure this will work as expected.

## Lookup Table

Lastly, we get to a question with lookup tables. These types of problems can usually be solved via brute force, at the expense of performance. They
make good questions for a more senior developer. Note that this prompt also has exponents that I needed to adjust when pasting to the chat bot.

![CodeSignal - Lookup Tables][12]

> This question tests whether candidates have the problem-solving and data structures skills to use a lookup table (hash set/dictionary) in their programming language of choice. It also involves a bit of tricky logic to avoid double-counting pairs. Finally, this question asks candidates to pay close attention to constraints, testing a key skill for real-world development.

After copying and pasting the prompt into ChatGPT, it spit out a response that attempted to brute force the problem. Depending on how CodeSignal 
presents the problem to a candidate, this could easily be worked around by modifying the prompt to indicate you are looking for a problem with a 
hash table. If that type of response isn't provided by CodeSignal though, ChatGPT would not have presented the expected solution, though it did 
still return expected results. The time trade off would have been seen with different test cases.

I modified the prompt slightly to indicate that I wanted a response with a hash table. ChatGPT complied.

    def solution(numbers):
        # Initialize a hash table to store the numbers that have been seen
        seen = {}
        # Initialize a count of the number of pairs that sum to a power of 2
        count = 0
        # Iterate through the numbers
        for i, x in enumerate(numbers):
            # If x is a power of 2, increment the count
            if x & (x - 1) == 0:
                count += 1
            # Check if x - y is a power of 2 for any y that has been seen
            for y in seen:
            # If x - y is a power of 2, increment the count
                if (x - y) & (x - y - 1) == 0:
                    count += 1
            # Add x to the hash table
            seen[x] = True
        return count

This passes the test cases as well. 

## Results

Of the four sample questions provided by CodeSignal, the "basic" two questions are solved without any difficulty. A third takes a little prompt 
tweaking to indicate that we want a hashtable instead of a simple brute forced solution. The last one is difficult to evaluate without test cases. 
So, of the 3 that can be evaluated all pass easily. Two with very little modification (converting exponents). The third needs a small nudge toward 
the type of solution desired.

## What's this mean?

I've looked at common interview questions from LeetCode, TestGorilla and now CodeSignal. As a hiring manager, I can see that a simple technical 
assessment of skills isn't enough to evaluate a candidate. Fortunately, I knew that already, but I'm hoping that others in the industry are seeing that
tools exist for developers to make their day to day lives easier. Those tools can, literally, do the work for them if you are presenting them with 
basic puzzles during the interview process.

I can already hear people saying that they do face to face coding, so this would never work in their process. You may be right, but ask yourselves this:
do you developers do face to face coding in their day to day work? If not, why are you evaluating your candidates with that type of pressure? 

Adjust your interview process to reflect what you actually need your candidates to demonstrate. A puzzle isn't real work. Give them a real problem
to talk through with your team. Evaluate their ability to solve a real problem so that you can learn about their thought processes, how they approach
a problem, and what resources they utilize.


 [1]: https://openai.com/blog/chatgpt/
 [2]: {filename}2022_12_05_stack_overflow_bans_chatgpt.md
 [3]: {filename}2022_12_20_play_with_chatgpt_and_pf_api.md
 [4]: {filename}2022_12_15_get_rid_leetcode_interviews.md
 [5]: {filename}2022_12_26_chatgpt_breaks_another_interview_exam.md
 [6]: https://www.woventeams.com/
 [7]: {filename}2022_06_04_the_other_side_of_the_mirror.md
 [8]: https://codesignal.com/developers/
 [9]: https://codesignal.com/blog/engineering/example-codesignal-questions/
 [10]: {attach}images/codesignal-array.png
 [11]: {attach}images/codesignal-pattern.png
 [12]: {attach}images/codesignal-lookup.png