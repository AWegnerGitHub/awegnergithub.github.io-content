Title: Python Gotcha: Logging an uncaught exception
Date: 2025-07-14 23:00
Tags: technical
Category: Technical Solutions
Slug: python-gotcha-logging-uncaught-exception
Summary: Uncaught exceptions will crash an application. If you don't know how to log these, it can be difficult to troubleshoot such a crash. Let's walk through this gotcha and see how to fix it.
Status: published
Series: Programming Gotchas

[TOC]

## Introduction

A well build application will use logging instead of `print` statements. An exceptionally well built one will log in such a way that additional context is added to each log message and be consumable by a log aggregation service. Perhaps I'll write up such an article in the future. For now though, let's focus on a single problem. 

Here is some sample code to demonstrate the problem.

    import logging

    logger = logging.getLogger(__name__)
    logger.setLevel(logging.INFO)

    handler = logging.FileHandler("app.log")
    formatter = logging.Formatter("%(asctime)s %(name)s %(levelname)s %(message)s")
    handler.setFormatter(formatter)
    logger.addHandler(handler)

    def divide(a, b):
        return a/b

    def main():
        logger.info("Application start")
        a = 10
        b = 0
        logger.info(divide(a,b))
        logger.info("Application end")

    if __name__ == "__main__":
        main()

Briefly, this sets up the `app.log` file to receive our log messages. The `main` function is going to divide two numbers, log the result, and end the program. Pretty simple.

Except, in this case, it is dividing by zero. This throws an error and crashes the program.

The console spits out a stack trace

    Traceback (most recent call last):
    File "/home/andy/main.py", line 22, in <module>
        main()
        ~~~~^^
    File "/home/andy/main.py", line 18, in main
        logger.info(divide(a,b))
                    ~~~~~~^^^^^
    File "/home/andy/main.py", line 12, in divide
        return a/b
            ~^~
    ZeroDivisionError: division by zero

The `app.log` file contains a single line:

    2025-07-14 22:20:04,551 __main__ INFO Application start

## Gotcha

Where is the gotcha here? The stack trace is right there!

You are, of course, right. However, imagine that this was not a simple application, but instead a production application that sends logs to a central service. Your application crashed and now one was watching the console. Your `app.log` file has no information. It says the application started and then...nothing. What happened? Is it still running?

As you dig through running processes, or check a `/health` end point for responses, you find out that it isn't running. That took a lot of time, and production isn't responding.

You've lost all visibility to what happened in your application at the most critical moment. When it crashed and spit out a stack trace, you want as much detail as you can get.

## Solution

The solution is [sys.excepthook][1]. This is called when any exception is raised and uncaught, except for `SystemExit`. It's pretty easy to utilize as well. A few small changes to the above code will allow us to log this completely unexpected `ZeroDivisionError`. 

    import logging
    import sys

    logger = logging.getLogger(__name__)
    logger.setLevel(logging.INFO)

    handler = logging.FileHandler("app.log")
    formatter = logging.Formatter("%(asctime)s %(name)s %(levelname)s %(message)s")
    handler.setFormatter(formatter)
    logger.addHandler(handler)

    def handle_uncaught_exception(exc_type, exc_value, exc_traceback):
        logger.critical(
            "uncaught exception, application will terminate.",
            exc_info=(exc_type, exc_value, exc_traceback),
        )
    sys.excepthook = handle_uncaught_exception


    def divide(a, b):
        return a/b

    def main():
        logger.info("Application start")
        a = 10
        b = 0
        logger.info(divide(a,b))
        logger.info("Application end")

    if __name__ == "__main__":
        main()

The important bit is the new `handle_uncaught_exception` function and the `sys.excepthook` line (with appropriate `import` statement). 

Someone running this in the console will notice that there is not a stack trace dumped to the console now. Instead, our log contains important information:

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

This is great! Now when troubleshooting this failing application and looking at the logs, we can easily see that an exception occurred. Additionally, with proper updates to the logging, more context can be provide such as the values of `a` and `b`. While it's easy enough to figure out that `b` is `0` in this simple example, the context in a larger production application could save a ton of troubleshooting time.

## Conclusion

Logging is vital to knowing what your application is doing. But, it's even more important to determining why it stopped working. If your logs aren't providing information when the application crashes, it's functionally useless. By implementing an `excepthook`, you can catch and properly log uncaught exceptions.

I know some of you are coming up with alternatives. Terrible ideas like wrapping the entire main block in a `try/except`. There are legitimate reasons to throw an exception. In this case, a `ZeroDivisionError` is a great exception to catch. But, you'd want to do it around as small of a code block as possible.

This is a clean way to catch truly unexcepted exceptions, not something a developer could have anticipated and, perhaps, fixed with additional input validation.


 [1]: https://docs.python.org/3/library/sys.html#sys.excepthook