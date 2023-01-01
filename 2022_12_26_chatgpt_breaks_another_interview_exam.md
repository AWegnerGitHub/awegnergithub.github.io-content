Title: Breaking TestGorilla interview questions with ChatGPT
Date: 2022-12-26 10:45
Tags: technical, job search
Category: Technical Solutions
Slug: breaking-the-interview-with-chatgpt
Summary: Using ChatGPT, I'm going to solve a couple interview questions from TestGorilla quickly, easily and with nothing more than copying and pasting.
Status: published
Series: ChatGPT Commentary

[TOC]

## Introduction

I've written quiet a bit about [ChatGPT][1] in the last month. As of this post, it is still [banned on Stack Overflow][2]. I believe I showed
why with my previous post. In it, I showed that [ChatGPT is prone to adding subtle errors][3] (and in some cases not so subtle), which is bad on
Q&A site. 

Today's post follows in the footsteps of my writing about how I hope ChatGPT inpires software engineering hiring managers to change their 
hiring process. I wrote about how [ChatGPT can solve LeetCode][4] problems. Today's post is going to focus on a company called [TestGorilla][5] and a 
couple of their assessments.

_First a disclaimer_: I was Director of Engineering at [Woven Teams][6] in 2022. Prior to that, [I was a customer of Woven for over two years][7]. 
Woven is a competitor of TestGorilla. That said, I do not recall TestGorilla being discussed during my employment at Woven. 
I only became aware of them during my job search this year. I'd be happy to do this same type of analysis on a Woven assessment, but as far 
as I am aware, their prompts are not presented publicly. TestGorilla's prompts used in this article are taken directly from their website. I did not 
speak with either company prior to writing this article.

## Assessment One - PHP Arrays

The first assessment I want to look at is related to PHP Arrays. The [prompt][8] itself if fairly simple.

![TestGorilla - PHP Array Sample Prompt][9]

This is billed as:

> PHP (coding): intermediate-level algorithms
> This PHP test evaluates candidates' coding skills through a short coding assignment. The test requires candidates to write code that meets certain requirements, and candidates can run their code to test if the result matches their expectations.

The estimated time for this test is 30 minutes. 

Well, let's see if ChatGPT is an intermediate level PHP programmer. Pasting the prompt into ChatGPT quickly spits out a result, in Python. I'm not
surprised since almost all of my testing has been in Python. I provided one additional set of directions for ChatGPT to utilize, based on the boilerplate
code provided.

> Write this in PHP using the following as a template:

    <?php function sum_array( $no1, $no2 ) {
    
    $array = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
        //Insert your code here 
    }
    ?>

The result provided looks functional, based on very outdated PHP syntax knowledge.

    <?php

    function sum_array($no1, $no2) {
    $array = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

    if ($no1 < 0 || $no2 < 0) {
        return -1;
    } else if ($no1 > $no2) {
        return 0;
    } else {
        $sum = 0;
        for ($i = 0; $i < count($array); $i++) {
        if ($array[$i] >= $no1 && $array[$i] <= $no2) {
            $sum += $array[$i];
        }
        }
        return $sum;
    }
    }

    ?>

I copy and pasted that into TestGorilla's preview page and ran it to see how it'd do. 

    Task executed successfully.

    ---------------------------------
        Input parameters: ((int) 10, (int) 20)
    Result: (30)
    OK
    ---------------------------------
        Input parameters: ((int) 10, (int) 30)
    Result: (60)
    OK
    ---------------------------------
        Input parameters: ((int) 30, (int) 50)
    Result: (120)
    OK
    ---------------------------------
        Input parameters: ((int) 90, (int) 100)
    Result: (190)
    OK


Success! One intermediate PHP assessment completed. Total time taken: 2 minutes. 

## Assessment Two - Kotlin Arrays

This [assessment][10] is listed as a "New Test" by TestGorilla at the time of this post. The challenge here, if ChatGPT fails, is that I have never
used Kotlin. 

![TestGorilla - Kotlin Array Sample Prompt][11]

This is billed as:

> Kotlin (Coding): Working with Arrays
> This Kotlin Coding test evaluates candidates’ ability to work with arrays. The test gives candidates 30 minutes to create a short algorithm involving arrays while taking into consideration all the requirements provided.

This is billed as a 20 minute assessment. It is also billed as an intermediate level assessment. 

I pasted the prompt into ChatGPT and also provided the boiler plate code block. The prompt doesn't explicitly say "Kotlin" anywhere, and I didn't mention
it either. The only hint provided was the boiler plate code. ChatGPT picked up the language and returned a result.

    fun getUniqueStudentIds(a: Array<Int>): Array<Int> {
    if (a.any { it < 50000 || it > 99999 }) {
        return arrayOf(-1)
    }
    return a.distinct().toTypedArray()
    }

Running this on TestGorilla's preview page passes the built in test cases:

    Task executed successfully.

    ---------------------------------
    Input parameters: (arrayOf(52311, 52311, 55125, 55125, 52128, 52130, 52128) )
    Result: (52311, 55125, 52128, 52130)
    OK
    ---------------------------------
    Input parameters: (arrayOf(59100, 58200, 58200, 51190, 58200) )
    Result: (59100, 58200, 51190)
    OK
    ---------------------------------
    Input parameters: (arrayOf(54321, 54321, 54321, 54321, 54321) )
    Result: (54321)
    OK
    ---------------------------------
    Input parameters: (arrayOf(54321, 53421, 53241, 53214, 54231, 54213, 54312) )
    Result: (54321, 53421, 53241, 53214, 54231, 54213, 54312)
    OK

Success! Total time: 1 Minute

## Evaluation

Despite not knowing Kotlin, I can look at the code provided and see that improvements can be made to the code. The prompt says 

> Note that all student IDs are 5-digit numbers beginning with the number 5. Return -1 as a single element in the array in the case of any invalid input.

The `if (a.any { it < 50000 || it > 99999 })` statement can be adjusted to reflect the prompt's specification better. Additionally, there isn't a 
built in test case for invalid data, but I suspect that it'll fail based on the expected return.

Depending on how TestGorilla is evaluating these edge cases on the paid version of this exam, this may be picked up. On the free preview version, though
it is not. 

## Hiring process

Once again, the reader should be able to see that a simple technical evaluation of software engineers in the hiring process is no longer sufficient. It 
took me longer to determine which prompts I wanted to evaluate for this post than it did to solve with a couple copy and pastes into ChatGPT. 

The hiring process must be more than "solve this code challenge". Hiring managers need to move past code only assessments. Instead, focus on a 
candidate's ability to technically assess and judge based on a context specific problem. These types of problems require a candidate to show their
experience, versus their ability to Google, ask Stack Overflow or use ChatGPT to provide a problem prompt. Evaluate your candidates based on the 
work you want them to actually do. 



 [1]: https://openai.com/blog/chatgpt/
 [2]: {filename}2022_12_05_stack_overflow_bans_chatgpt.md
 [3]: {filename}2022_12_20_play_with_chatgpt_and_pf_api.md
 [4]: {filename}2022_12_15_get_rid_leetcode_interviews.md
 [5]: https://www.testgorilla.com/
 [6]: https://www.woventeams.com/
 [7]: {filename}2022_06_04_the_other_side_of_the_mirror.md
 [8]: https://app.testgorilla.com/preview/105976?language=en
 [9]: {attach}images/testgorilla-php-array.png
 [10]: https://app.testgorilla.com/preview/788899?language=en
 [11]: {attach}images/testgorilla-kotlin-array.png