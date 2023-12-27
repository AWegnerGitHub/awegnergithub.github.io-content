Title: Python Gotcha: List Copy Problems
Date: 2023-12-20 15:30
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-list-copy
Summary: Copying a Python list (or any mutable object) isn't as simple as setting one equal to another. Let's discuss a better way to do this.
Status: published
Series: Programming Gotchas

[TOC]

## Introduction

This problem isn't limited to only `list`s. Any mutable object in Python can be impacted by this "gotcha". The problem is setting one mutable object equal to another doesn't make a copy. It assigns a new variable to the same object as the original. Let's take a look and see how this appears.

## The First Gotcha

The first example is pretty simple. Let's make a copy of our list

    >>> list1 = [1,2,3,4,5]
    >>> list2 = list1
    >>> list1
    [1, 2, 3, 4, 5]
    >>> list2
    [1, 2, 3, 4, 5]

That looks like I have two lists, both with the same set of values. But, let's modify the second list and change the first element in the list to be `6`

    >>> list2
    [6, 2, 3, 4, 5]

But what about `list1`? It also has changed.

    >>> list1
    [6, 2, 3, 4, 5]

This is occuring because, as mentioned, `list2` is assigned to the same object as `list1`. Any change to either, changes the object that both are assigned to.

    >>> print(f"list1 id: {id(list1)}")
    list1 id: 1497534800448
    >>> print(f"list2 id: {id(list2)}")
    list2 id: 1497534800448

## The First Solution

This can be solved a few different ways. The important take away, though, is that you have to create a new object with the original values. I can think of a few ways to handle this. This article doesn't consider performance for these basic lists.

One option is to utilize the `list()` method and pass it the initial values from `list1`

    >>> list3 = list(list1)
    >>> list3
    [6, 2, 3, 4, 5]
    >>> list3[0] = 0
    >>> list3
    [0, 2, 3, 4, 5]
    >>> list1
    [6, 2, 3, 4, 5]
    >>> print(f"list1 id: {id(list1)}")
    list1 id: 1497534800448
    >>> print(f"list3 id: {id(list3)}")
    list3 id: 1497531282368

A second option is to `slice()` the entire original list into a new variable.

    >>> list4 = list1[:]
    >>> list4
    [6, 2, 3, 4, 5]
    >>> list4[0] = 0
    >>> list4
    [0, 2, 3, 4, 5]
    >>> list1
    [6, 2, 3, 4, 5]
    >>> print(f"list1 id: {id(list1)}")
    list1 id: 1497534800448
    >>> print(f"list4 id: {id(list4)}")
    list4 id: 1497534798208

Lastly, the `list` object has a `copy()` method that you can utilize. 

    >>> list5 = list1.copy()
    >>> list5[0] = 0
    >>> list5
    [0, 2, 3, 4, 5]
    >>> list1
    [6, 2, 3, 4, 5]
    >>> print(f"list1 id: {id(list1)}")
    list1 id: 1497534800448
    >>> print(f"list5 id: {id(list5)}")
    list5 id: 1497534799552

With each of these three options, you can see that we created new objects because each has a unique object id. Changing one of these copies does not change the original's values.

## The Second Gotcha

You, a savvy developer, know all this though. You also have a more advanced data structure. You have a _list of lists_. 

    [
        ['first element', 1, 2, 3],
        ['second element', 4, 5, 6],
        ['third element', 7, 8, 9],
    ]

Since you know about this gotcha, you are going to make a new object and copy the values to the new object.

    >>> deeplist1 = [
    ...     ['first element', 1, 2, 3],
    ...     ['second element', 4, 5, 6],
    ...     ['third element', 7, 8, 9],
    ... ]
    >>> deeplist2 = deeplist1.copy()
    >>> print(f"deeplist1 id: {id(deeplist1)}")
    deeplist1 id: 1497534571008
    >>> print(f"deeplist2 id: {id(deeplist2)}")
    deeplist2 id: 1497534577536

Now, with two different objects, is it safe to modify an element in one of the sublists? Turns out, it is not.

    >>> deeplist2[0][0] = "NOT 3RD"
    >>> deeplist2
    [['NOT 3RD', 1, 2, 3], ['second element', 4, 5, 6], ['third element', 7, 8, 9]]
    >>> deeplist1
    [['NOT 3RD', 1, 2, 3], ['second element', 4, 5, 6], ['third element', 7, 8, 9]]

But why? The ids of `deeplist1` and `deeplist2` are different. This is true, but the sublists are not. Each sublist is assigned the same object id as the original.

    >>> id(deeplist1[0][0]), id(deeplist2[0][0])
    (1497534796864, 1497534796864)

## The Second Solution

Utilize the [`deepcopy()`][1] method to make a copy of your original object.

    >>> import copy
    >>> deeplist3 = copy.deepcopy(deeplist1)
    >>> deeplist3[0][0] = "NEW VALUE"
    >>> deeplist3
    [['NEW VALUE', 1, 2, 3], ['second element', 4, 5, 6], ['third element', 7, 8, 9]]
    >>> deeplist1
    [['NOT 3RD', 1, 2, 3], ['second element', 4, 5, 6], ['third element', 7, 8, 9]]

Using `deepcopy()` we can safely make a copy of our list of lists and modify both the original and the copy without impacting the other.

 [1]: https://docs.python.org/3/library/copy.html#copy.deepcopy