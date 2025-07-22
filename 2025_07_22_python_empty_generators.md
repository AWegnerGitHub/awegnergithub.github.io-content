Title: Python Gotcha: Reusing Generators Returns Nothing
Date: 2025-07-22 09:00
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-reusing-generator-returns-nothing
Summary: Generators provide lazy evaluation for processing large datasets efficiently. However, once a generator is exhausted through iteration, it cannot be reused or reset. Let's cover this common gotcha that trips up developers new to this Python feature.
Status: published
Series: Programming Gotchas

[TOC]

## Introduction

In the previous article, we looked at [logging uncaught exceptions][1]. Let's utilize the log output from that post for another common task: error log file processing. This example is going to be pretty simple, as the error log for the post is tiny, but in a production environment this could be hundreds of thousands of lines, or gigabytes in size. Potentially, that's a lot to shove into memory for processing. But that's where a generator can come in to help.

[Generators][2] do not store their results, instead they maintain state and `yield` the result back to the caller. This means each line in a log can be processed and returned, without loading the entire file. 

### Flashback

As a reminder from the [previous article][1], the log file being used looks like this:

    2025-07-14 22:30:44,061 __main__ INFO Application start
    2025-07-14 22:30:44,061 __main__ CRITICAL uncaught exception, application will terminate.
    Traceback (most recent call last):
    File "/home/andy/main.py", line 31, in <module>
        main()
        ~~~~^^
    File "/home/andy/main.py", line 27, in main
        logger.info(divide(a,b))
                    ~~~~~~^^^^^
    File "/home/andy/main.py", line 21, in divide
        return a/b
            ~^~
    ZeroDivisionError: division by zero

There is one `INFO` entry and one `CRITICAL` entry.

## yield vs return

It's important to understand what makes a generator different from a regular function. The key distinction is the `yield` keyword. When a function contains `yield`, Python treats it as a generator function, which behaves differently from functions that use `return`. 

A regular function with `return` executes completely, returns back a single result and then terminates. A function with `yield` creates a generator object that can pause execution, return a value, and later resume from exactly where it left off. This is what enables the memory-efficient and lazy evaluation that makes generators powerful.

## Gotcha

### Log processing

A generator result can only be utilized one time. Whether you are using a generator to output the next item in a sequence or process a file line by line, once you have passed an iterable or exhausted the generator, it doesn't get reused. This simple generator demonstrates the issue.

    def read_log_lines(filename):
        with open(filename, 'r') as f:
            for line in f:
                if 'CRITICAL' in line:
                    yield line.strip()

    error_logs = read_log_lines('app.log')

    error_count = len(list(error_logs))
    print(f"Found {error_count} CRITICAL lines")

    recent_errors = [log for log in error_logs if '2025' in log]
    print(f"Recent errors: {len(recent_errors)}")

At first glance, it looks like this will read the log file, count the number of errors and then output how many of those were in 2025 (or contain the string `2025`). However, the actual output is different.

    Found 1 CRITICAL lines
    Recent errors: 0

`error_logs` is a generator object. If a function `yield`s, it is a generator. As `error_count` is initialized, it processes the error log and yields back any critical lines. The `list()` function will consume the entire generator (file). A few lines later, the developer wants to see how many of these are recent errors and attempts to go through the `error_logs` generator again. Success! No recent errors!

Right?

No, and looking at the log quickly shows that.

### Fibonacci

Let's use a generator to build the Fibonocci sequence. Spoiler for interviews! In this case, I'm going to use a generator to get the first 10 items. Then print out the first 5 and then try to print the entire list of 10 items.

    def fibonacci(n):
        a, b = 0, 1
        for _ in range(n):
            yield a
            a, b = b, a + b

    fib_numbers = fibonacci(10)

    print("First 5 numbers:")
    for i, num in enumerate(fib_numbers):
        print(num)
        if i >= 4:
            break

    print("Entire List:")
    full_list = list(fib_numbers)
    print(full_list)

The output for this is:

    First 5 numbers:
    0
    1
    1
    2
    3
    Entire List:
    [5, 8, 13, 21, 34]

Notice that the `full_list` variable only contains the items remaining on the generator. Since the first 5 (indexes `0` through `4`) were printed, they are no longer part of the generator. When the full list is printed, only the remaining items can be printed.

## Solution

The solution to the problem is easy enough. Call the generator function again. For example, with the log code from above:

    error_logs = read_log_lines('app.log')
    error_count = len(list(error_logs))
    ...
    error_logs = read_log_lines('app.log')  # Call again and create a new generator
    recent_errors = [log for log in error_logs if '2025' in log]

For the Fibonacci code, you would call `fib_numbers = fibonacci(10)` again before printing the full list.

Obviously, there is a down side here with duplicate processing of the same data due to running the generator twice. This could probably be solved with some logic adjustments to the generator or the code calling the generator, but that'll vary by application depending on what the generator is doing.

### Conclusion

The important thing to take away from this is that once you have iterated over an item in a generator, it's no longer part of a generator. This means that if you want to get clever and see if there are more items in a generator, or determine the next item, you've consumed the next item.

The power of generators, especially when processing large amounts of data, can't be understated. But, at the same time, it's important to know that reusing an exhausted generator or attempting to access a previous item directly from the generator is not going to work. Instead, to reuse generator logic, call the generator function again to create a new generator object, or convert to a list if memory permits and multiple iterations are needed.

 [1]: {filename}2025_07_14_python_log_uncaught_exception.md
 [2]: https://docs.python.org/3/tutorial/classes.html#generators