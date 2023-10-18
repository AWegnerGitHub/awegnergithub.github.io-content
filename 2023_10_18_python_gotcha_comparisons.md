Title: Python Gotcha: Comparisons
Date: 2023-10-18 10:15
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-comparisons
Summary: Comparing two numerical variables in Python can have surprising results if you aren't aware of some common gotchas. This post covers a couple of the common ones.
Status: published
Series: Programming Gotchas

[TOC]

## Float Equality Comparisons

Like every other programming language, Python can't accurately represent floating-point numbers. I'm sure many Computer Science students have lost many hours learning how [floating representation works][1]. I remember that class well.

In any case, let's get into the problem with comparing `float` values in Python and how to handle it.

    >>> 0.1 + 0.2 == 0.3
    False

You, me, and anyone with a few years of elementary school under their belt can see that this _should be_ `True`.

What's happening here? We can see the problem by breaking down the component parts of this comparison.

    >>> 0.3
    0.3
    >>> 0.1 + 0.2
    0.30000000000000004

And now we see the floating point representation that is causing a problem.

So, how do we deal with this?

### Decimal

There are a couple options, both of which have their drawbacks. Let's start with [`Decimal`][2].

> The decimal module provides support for fast correctly rounded decimal floating point arithmetic.

This sounds good, but an important gotcha here too is how it handles numerals vs. strings.

    >>> from decimal import Decimal
    >>> Decimal(0.1)
    Decimal('0.1000000000000000055511151231257827021181583404541015625')
    >>> Decimal('0.1')
    Decimal('0.1')

This means, to accomplish the comparison above, we need to wrap each of our numbers in a string.

    >>> Decimal('0.1') + Decimal('0.2') == Decimal('0.3')
    True

That's annoying, but does function. 

### isclose

Python 3.5 implemented [PEP 485][3] to test for approximate equality. This is done in the [`isclose`][4] function. 

    >>> import math
    >>> math.isclose(0.1+0.2,0.3)
    True

That's cleaner than wrapping everything in strings. But, it also is more verbose than a simple `==` statement. It makes your code less clean, but does provide accurate comparisons.

## is vs. ==

Another comparison that I've commonly seen misapplied is developers using `is` when they mean `==`. Put simply, `is` should ONLY be used if you are checking if two references refer to the same object. `==` is used to compare value by calling underlying `__eq__` methods.

Let's see this in action:

    >>> a = [1, 2, 3]
    >>> b = a
    >>> c = [1, 2, 3]
    >>> d = [3, 2, 1]
    >>> a == b
    True
    >>> a == c
    True
    >>> a == d
    False

So far, nothing unusual. `a` has the same values of `b` and `c` and different values from `d`. Now let's use `is`

    >>> a is b
    True
    >>> b is a
    True
    >>> a is c
    False
    >>> a is d
    False
    >>> b is c
    False

Here, the only `True` statements are the comparison between `a` and `b`. This is because `b` was initialized with the statement `b = a`. The other two variables were initialized as their own statements and values. **Remember, `is` compares object references. If they are the same, it returned `True`.**

    >>> id(a), id(b), id(c), id(d)
    (2267170738432, 2267170738432, 2267170545600, 2267170359040)

Since `a` and `b` are the same object, we get a `True` on their comparison. The others are different, hence the `False`.

## nan == nan

`nan`, or Not a Number, is a floating point value that can not be converted to anything other than a float and is considered not equal to all other values. It's a common way to represent missing values in a data set.

There is a key phrase in that description above that is the basis for this Gotcha:

> is considered not equal to all other values

It's common for software to check if two values are equal to one another prior to taking an action. For `nan`, that does not work:

    >>> float('nan') == float('nan')
    False

This prevents code like this from entering the `if` block of an `if/else` statement

    >>> a = float('nan')
    >>> b = float('nan')
    >>> if a == b:
    ...   .. ## Do something if equal
    ... else:
    ...   .. ## Do something if not equal

In this example, they are _never_ equal. 

This leads to an interesting, if not unintutive, way of checking if a variable is a `nan` value. Since `nan` is not equal to _all_ other values, it is not equal to itself.

    >>> a != a
    True

Like I said, "Interesting". But, when your code is looked at by others it's also "confusing". There is an easier way to show that you are checking for a `nan` value. [`isnan`][5]

    >>> import math
    >>> a = float('nan')
    >>> b = 5
    >>> c = float('infinity')
    >>> math.isnan(a)
    True
    >>> math.isnan(b)
    False
    >>> math.isnan(c)
    False

To me, that's a much clearer check that we want to see if the value is `nan`. It's likely you aren't just passing `nan` to a single variable. You're probably using a library like [NumPy][6] or [Pandas][7]. In that case, you have functions in each of those libraries that can check for `nan` in a performant way.

* In NumPy the function has the same name but in the NumPy library: `numpy.isnan(value)`. 
* In Pandas, the function has a slightly different name: `pandas.isna(value)`

### Conclusion

Comparisons aren't always as straight forward as we'd like. I covered a few common comparison problems in Python here. 

Floating point comparisons are common across languages. Python has a few ways of making this easier for developers. I recommend utilizing `isclose` as it keeps the code a bit cleaner and eliminates the need to wrap numbers in strings if using the `Decimal` module.

`is` should _only_ being used to check if two items are referring to the same object. In any other case, it's not doing the check you want it to be doing. 

Lastly, `nan` is equal to _nothing else_. It's important to be aware of that before you start comparing values in your dataset to one another. 


 [1]: https://www.doc.ic.ac.uk/~eedwards/compsys/float/
 [2]: https://docs.python.org/3/library/decimal.html
 [3]: https://peps.python.org/pep-0485/
 [4]: https://docs.python.org/3/library/math.html#math.isclose
 [5]: https://docs.python.org/3/library/math.html#math.isnan
 [6]: https://numpy.org/
 [7]: https://pandas.pydata.org/