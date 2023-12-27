Title: Python Gotcha: Join vs Concat
Date: 2023-10-12 10:45
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-join-vs-concat
Summary: Append 100,000 strings together using join or concat - Which is faster?
Status: published
Series: Programming Gotchas

[TOC]

## The Problem

Here's a somewhat contrivied example to demonstrate a problem. I need to create a string with the word `word` 100,000 times. What's the fastest way to generate this string? I could use string concatenation (simply `+` the strings to one another). I could [`join`][1] a list with 100,000 items. 

My proposed code for this example is below

    def concat_string(word: str, iterations: int = 100000) -> str:
        final_string = ""
        for i in range(iterations):
            final_string += word
        return final_string

    def join_string(word: str, iterations: int = 100000) -> str:
        final_string = []
        for i in range(iterations):
            final_string.append(word)
        return "".join(final_string)

## Results

In `concat_string`, I iterate 100,000 items with each iteration adding my `word` to the end of the string. In `join_string`, I append my `word` to a list on each iteration and then join the list into a string at the end. 

Running each function through the built in profiler (`cProfile`) shows how these two functions perform.


    >>> cProfile.run('concat_string("word ")')

          4 function calls in 1.026 seconds

    Ordered by: standard name

    ncalls  tottime  percall  cumtime  percall filename:lineno(function)
         1    0.000    0.000    1.026    1.026 <string>:1(<module>)
         1    1.026    1.026    1.026    1.026 test.py:9(concat_string)
         1    0.000    0.000    1.026    1.026 {built-in method builtins.exec}
         1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}

    >>> cProfile.run('join_string("word ")')

          100005 function calls in 0.013 seconds

    Ordered by: standard name

    ncalls  tottime  percall  cumtime  percall filename:lineno(function)
         1    0.000    0.000    0.013    0.013 <string>:1(<module>)
         1    0.009    0.009    0.013    0.013 test.py:16(join_string)
         1    0.000    0.000    0.013    0.013 {built-in method builtins.exec}
    100000    0.004    0.000    0.004    0.000 {method 'append' of 'list' objects}
         1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
         1    0.000    0.000    0.000    0.000 {method 'join' of 'str' objects}

## What's happening here?

`join` is more than 75x than the concatination method. Why?

String are immutable objects in Python. I talked about these in my last [Gotcha Article about default parameters][2]. This immutability means that a string can't be changed. `concat_string` does appear to be changing the string with each `+` action, but under the hood, Python has to create a new string object each iteration through the loop. That means there are 99,999 temporary string values - creating and discarding almost all of them immediately on the next iteration during the concatenation action. 

`join_string` on the other hand, is appending 100,000 string objects to a list. But, only one list is created. The final `join` is only doing a _single_ concatenation with all 100,000 strings.

## What are the implications of this?

While this is a contrived example to show the problem, there are real performance impacts to string immutability that may not be obvious. There are other places where a new string is created commonly used in Python. A couple examples are `f-strings`, `%s` format specifiers and `.format()`. Each of these create a brand new string. 

This doesn't mean you should avoid these, as the performance impact is only really obvious in situations where you are appending _a lot_ of strings together. However, if you have a string formatting line in a loop, it's a potential area to focus on for performance improvements.


 [1]: https://docs.python.org/3/library/stdtypes.html#str.join
 [2]: {filename}2023_10_06_python_gotcha_default_optional.md