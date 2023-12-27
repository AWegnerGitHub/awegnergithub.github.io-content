Title: Ruby Gotcha: Operator Precedence
Date: 2023-10-23 10:45
Tags: technical
Category: Technical Solutions
Slug: ruby-gotcha-operator-precedence
Summary: In Ruby you can use either `and` or `&&`. You can use `or` or `||`. What's is the difference and what's the catch? 
Status: published
Series: Programming Gotchas

[TOC]

## Introduction

In one of my previous roles, the team utilized Ruby. I was not an individual contributor at that point in my career, so my exposure to it was less than much of my team. However, during my tenure, I noticed several "Gotchas" of the Ruby language. Each language has these in their own way, but in this post I'm going to cover one specific one.

What's the difference between `and` and `&&`? What about the difference between `or` and `||`?

## The Gotcha

At first pass, these two blocks of code appear to do the same thing. Let's take a look.

    irb(main):003:0> true && false
    => false
    irb(main):004:0> true and false
    => false

By the rules of boolean logic, both `and` and `&&` statements return `false`.

    irb(main):005:0> true || false
    => true
    irb(main):006:0> true or false
    => true

Boolean logic holds true for the `or` and `||` statements and return `true`.

But, what happens if we assign the result of this to a variable?

    irb(main):007:0> result = true and false
    => false
    irb(main):008:0> result1 = true && false
    => false
    irb(main):009:0> result
    => true
    irb(main):010:0> result1
    => false

Now we see the problem. With the `and` operator, `result = true`. With the `&&` operator, `result1 = false`. 

Looking at the `or`/`||` operators quickly,


    irb(main):011:0> result2 = true or false
    => true
    irb(main):012:0> result3 = true || false
    => true
    irb(main):013:0> result2
    => true
    irb(main):014:0> result3
    => true

This all looks reasonable. But, let's swap the order of our `true`/`false` in the statement.

    irb(main):015:0> result2 = false or true
    => true
    irb(main):016:0> result3 = false || true
    => true
    irb(main):017:0> result2
    => false
    irb(main):018:0> result3
    => true

Here we see the differences again, with the `or` operator setting `result2 = false` and the `||` operator setting `result3 = true`

## What is going on?

`and` and `&&` / `or` and `||` are the same thing, but they have different orders of precedence. The (not well formatted in my opinion) documentation shows the [precedence table for Ruby][1].

The key things to point out here:

 * `&&` and `||` are above `and` and `or`, meaning they will be evaluated first.
 * `=` is also above `and` and `or` (but below `&&` and `||`)

Let's look at one of the examples again:

    irb(main):015:0> result2 = false or true
    => true
    irb(main):017:0> result2
    => false

With the order of precedence in mind, this is what happens:

 * `result2` is assigned the value of `false` because `=` occurs before `or`
 * The remainder of the statement is evaluated as `true`

This was hidden when `true` and `false` was swapped because `result2` was assigned a value of `true` and then the remainder of the statement was also evaluated to `true`.

## How do you prevent this?

There is very few reasons to utilize the English words `and` and `or` in Ruby's logical evaluations. Instead, these should be utilized as flow control modifiers (think `if` and `unless`) and not for boolean logic. 

Here's an example of how it should be utilized for flow control

    irb(main):019:0> numeral = 10 && numeral / 5
    (irb):19:in `<main>': undefined method `/' for nil:NilClass (NoMethodError)
            from C:/Ruby32-x64/lib/ruby/gems/3.2.0/gems/irb-1.6.2/exe/irb:11:in `<top (required)>'
            from C:/Ruby32-x64/bin/irb:33:in `load'
            from C:/Ruby32-x64/bin/irb:33:in `<main>'
    irb(main):020:0> numeral = 10 and numeral / 5
    => 2

In our first example, with `&&`, the statement is broken up like this

    numeral = (10 && numeral) / 5

This won't work because `numeral` doesn't have a value when `&&` is evaluated. Contrast this with the second version where the code:

 * Assigns a value of `10` to `numeral` 
 * Divides `numeral` by `5`

While basic, it should get the point across. Another example could be something like this:

    user = User.find_by_email(email) and user.send_email!

Here, we are only sending an email to the user if we find their information. No information, no email is sent. 


 [1]: https://docs.ruby-lang.org/en/3.2/syntax/precedence_rdoc.html