Title: Python Gotcha: Mutable Default Optional Arguments
Date: 2023-10-06 10:15
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-optional-default-arguments
Summary: A quick walk through of why mutable defaults on optional arguments is bad in Python.
Status: published

[TOC]

## The Problem

Without running this in your IDE, what does the following code output? Forgive the use of `vins`, I work in the logistics field and deal with vehicle identification numbers frequently.

    def add_vin(this_vin: str, vins: list = []):
        if this_vin not in vins:
            vins.append(this_vin)
        return vins

    add_vin("VIN1")
    add_vin("VIN2")
    print(add_vin("VIN3"))

You probably saw that `vins` is an optional argument and that it defaults to an empty list. Run this function three times without passing it a list to append to, and it's reasonable to assume that you have a final result of a list with a single item contained in it - `VIN3`

Unfortunately, that's wrong. The output is actually `["VIN1", "VIN2", "VIN3"]`

Why?

## What's going on?

In Python, default arguments are bound to a function at the time the function is defined. This is only done once. If you don't pass the optional argument in to the function call, the same list is changed when the function is called. This means that in the example above, a new list is not created each time the function is called without the optional `vins` argument.

We can see this by slightly modifying the code above to show the object ID we are operating on.

    def add_vin(this_vin, vins=[]):
        if this_vin not in vins:
            print(f"id={id(vins)}")    # Print the object ID we are appending to
            vins.append(this_vin)
        return vins

Which outputs the following as it adds each VIN.

    id=2954990867712
    id=2954990867712
    id=2954990867712
    ['VIN1', 'VIN2', 'VIN3']

Each time `add_vin` was called without passing it a list to operate on, the function will operate on the same default list.

## Another example

The example above is basic, but gets the point across. However, let's look at something a bit more common. Connecting to a database with a connection object at the parameter.

    def open_database(connection = make_connection(host='example.com')):
        # do database things with `connection`
        connection.close()

Well done. You've closed your connection. Your DBA will thank you. However, what happens next time you call `open_database()` in your code? After all, your connection information is there by default, you don't need to pass a new connection, right?

Again, wrong. You've closed the `connection` and it's the default argument. The next call to `open_database` will utilize the same `connection` object. A connection that is closed. Your database call will fail.

## Last Example

One last example:

    import datetime, time

    def print_datetime(dt = datetime.datetime.now()):
        return str(dt)

    print(print_datetime())
    time.sleep(10)
    print(print_datetime())

This is going to print the current datetime, sleep for 10 seconds, and print the new datetime - right?! 

No. 

    2023-10-06 08:45:21.392973
    2023-10-06 08:45:21.392973

Why?! 

Again, an optional default argument is only bound once. Since a new value isn't passed to the function on the second call, the exact same value is used the second time through. Whoops. 

## What can I do about this?

The best option, in my opinion, is to set the default values to `None`. This is going to make it very obvious that there is a problem. In the first example, you'll get an exception because you can't append to `None`. In the second, your database calls will error because `None` doesn't contain connection information. In the third, you'll print `None` instead of an expected `datatime`. 

You could pass an immutable object like a tuple or a frozenset, but even these have caveats to be aware of. For example, a tuple can contain something mutable in it (ie. a list). Personally, I prefer to utilize `None`.