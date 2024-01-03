Title: Python Gotcha: Modifying a list while iterating
Date: 2024-01-03 15:00
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-modify-list-while-iterating
Summary: Python makes it easy to modify a list while you are iterating through it's elements. This will bite you. Read on to find out how and what can be done about it.
Status: published
Series: Programming Gotchas

[TOC]

## Introduction

A fairly common task an engineer may need to do is to remove all numbers from a list below a threshold. It's a simple enough task that anyone could accomplish - iterate over the list, check the value, if it's below the threshold remove it. With Python you can do this in a `for` loop.

## Gotcha

Let's check code for this simple problem.

    numbers_list = [0, 9, 2, 1, 5, 1, 10, 4, 3]
    for index, value in enumerate(numbers_list):
        print(index, value, value < 5, 'Deleted' if value < 5 else 'Kept')
        if value < 5:
            del(numbers_list[index])

    print(numbers_list)

It'd be reasonable to expect an final list that looks like this:

    [9, 5, 10]

But, the actual output is:

    [9, 1, 5, 10, 3]

What is _happening_? 

## Explaination

The (unconsious?) assumption that the developer is making with this piece of code is that the final list _must_ be in `numbers_list`. Since we're able to iterate and modify, there is no need for a temporary variable or a need to worry about [list copy gotchas][1]. But, let's go through this one iteration at a time.

    Starting list: [0, 9, 2, 1, 5, 1, 10, 4, 3]
    
    | Iteration | Index | Value | Value < 5? | Action | List after iteration      |
    | --------- | ----- | ----- | ---------- | ------ | ------------------------- |
    | 0         | 0     | 0     | True       | Delete | [9, 2, 1, 5, 1, 10, 4, 3] |
    | 1         | 1     | 2     | True       | Delete | [9, 1, 5, 1, 10, 4, 3]    |
    | 2         | 2     | 5     | False      | Keep   | [9, 1, 5, 1, 10, 4, 3]    |
    | 3         | 3     | 1     | True       | Delete | [9, 1, 5, 10, 4, 3]       |
    | 4         | 4     | 4     | True       | Delete | [9, 1, 5, 10, 3]          |

    End of iteration as list has no more elements

Looking at this, it's a little clearer what's going on. Let's go through there one iteration at a time.

- On the first iteration, the value of `Index 0` is `0`, so it's deleted. _And the problem starts now_.
- On the next iteration, the value of `Index 1` is `2` (**not 9**). In the previous iteration, `Index 0` was deleted, which means everything has moved up one index. The value of `9` was never checked. Instead, we look at a value of `2`. It's less than our threshold so it's also deleted.
- On the third iteration, the value of `Index 2` is `5`. Again, we've skipped a value (`1`) and it's never even checked. This iteration doesn't result in a deletion because `5` is not less than `5`.
- On the forth iteration, the value of `Index 3` is `1`. This is deleted as expected.
- On the fifth iteration, the value of `Index 4` is `4`. We've skipped another value (`10`) and this iteration's value is less than our threshold so it's deleted. 
- There is no further iteration, because the last one has made the final value the last index, so the final value is never checked. 

We skipped checking several values because we messed with indexes that were either being operated on or hadn't been operated on yet. This results in unexpected behavior.

## Solution

A quick solution is to add the values that exceed our threshold to another variable:

    numbers_list = [0, 9, 2, 1, 5, 1, 10, 4, 3]
    larger_list = []
    for index, value in enumerate(numbers_list):
        if value >= 5:
            larger_list.append(value)

    print(larger_list)

This results in the expected list of `[9, 5, 10]`

Another solution, assuming that the result _needs_ to sit in a variable with the same name, is to use a list comprehension. This does the same thing as above, but keeps the code more compact. It also works because the final result is assigned to a new variable object with the same name.

    numbers_list = [0, 9, 2, 1, 5, 1, 10, 4, 3]
    numbers_list = [nums for nums in numbers_list if nums >= 5]
    print(numbers_list)

This results in the expected list of `[9, 5, 10]`

 [1]:  [2]: {filename}2023_12_20_python_list_copy_gotcha.md