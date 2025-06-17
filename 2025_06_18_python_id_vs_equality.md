Title: Python Gotcha: Identity vs Equality - When 'is' Fails Unexpectedly
Date: 2025-06-17 08:00
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-identity-vs-equality
Summary: To a new developer `is` can look like an equality check in Python, especially in poorly written tutorials. I'll an overview of what `is` is and how you should use it in only limited circumstances.
Status: published
Series: Programming Gotchas

[TOC]

## Introduction

In the [comparisons gotcha][1] I wrote a few years ago, I briefly touched on [`is` vs `==`][2].

> Put simply, `is` should ONLY be used if you are checking if two references refer to the same object.
> *Remember, `is` compares object references.*

Even more simply, `is` is *not* checking value. Let's take a look at a couple examples.

## Gotcha

### Integers

Python, specifically CPython, caches the values of `-5` through `256` (inclusive). This means that these small integer values will always refer to the same object. 

Note the phrasing there - "the same object".

Outside of that range, though, the same is not true. 

    >>> a = 100
    >>> b = 100
    >>> a is b
    True
    >>> a = 257
    >>> b = 257
    >>> a is b
    False

In both of the above examples, using `a == b` would have returned `True`. The mistake was assuming that `is` does the same thing. It does not.

### Strings

String interning is a method of storing only one copy of each distinct immutable string value. Immutable strings can't be changed. Not every string will be interned though. Let's take a look:

    >>> a = "Hello"
    >>> b = "Hello"
    >>> a is b
    True
    >>> a = "Hello World"
    >>> b = "Hello World"
    >>> a is b
    False
    >>> a = "Hello_World"
    >>> b = "Hello_World"
    >>> a is b
    True

The first and last example interned the strings, showing that `a` and `b` refer to the same object. But, the second example - `Hello World` - didn't get interned, so `a` and `b` refer to different objects. Why is this?

The short and simply answer is that any string that has only numbers, letters or underscores will be interned. Since `Hello World` contains a `space`, it would not be interned.

## The Solution

To a new developer that has seen tutorials that read `if a is True` or `if b is None`, a conditional for integers or strings following the same pattern _appears_ to be comparing values. If they test it with small, positive numbers or simple one word strings, the assumption holds up. 

But, `==` is for comparing values! Each of the above examples would return `True` by changing the statement to `a == b`.

The few times that `is` is appropriate are when you are checking `True`/`False` or `None`. Otherwise, the _vast_ majority of the time, you want to use an equality check (`==`)

The Python [PEP8 programming recommendations][3] state:

> Comparisons to singletons like `None` should always be done with `is` or `is not`, never the equality operators.

The linters in the Python ecosystem report on the usage of `is` vs `==` too. `flake8` has [E711][4] - `Comparison to None should be 'cond is None:'`. `ruff` has a similar report with it's [`None` comparison][5] check.

I highly recommend a linter for your projects to catch this, and other problems that go against best practices. 

*Remember, `is` compares object references, not object equality*


 [1]: {filename}2023_10_18_python_gotcha_comparisons.md
 [2]: {filename}2023_10_18_python_gotcha_comparisons.md#is-vs
 [3]: https://peps.python.org/pep-0008/#programming-recommendations
 [4]: https://www.flake8rules.com/rules/E711.html
 [5]: https://docs.astral.sh/ruff/rules/none-comparison/