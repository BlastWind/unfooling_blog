---
layout: post
title: Polya's 4 steps to problem solving
date: '2022-08-05 18:48:29'
tags:
- bit
---

How do I determine the time till the next timer tick, given the start time of the timer and its tick cycle? Let's go through the four steps of mathematician Polya's problem solving technique to address this.

### 1. You have to understand the problem

This is a simple problem. Let's understand it with a few examples. If `start = Aug 1st` and `cycle = 1 month` is once per month, then the answer is around 25 days (currently writing this on Aug 5th). If `start=Aug 1st` ,`cycle = 1 day`, and `currentTime = Aug 5th 2pm`, then the answer is 10 hours.

If you can't even make examples, you are far from understanding. So, start with making examples.

### 2. Find the connection between the data and the unknown. 

Let's find connections by observing the examples.

In the case of `start = Aug 1st` and `cycle = 1 month`, I observe that the current time, `Aug 5th`, subtracted by the `start`, is 5 days. 5 days is less than 1 month, and, I calculate `1 month - 5 days = 25 days`

In the case of `start = Aug 1st` and `cycle = 1 day`, I observe that the current time, `Aug 5th and 2pm`, subtracted by the `start`, is 5 days and 14 hours. 5 days and 12 hours is greater than 1 day, so, I `mod` 5 days and 14 hours by 1 day, obtaining 14 hours. Then, I calculate `1 day - 14 hours = 10 hours`.

In this step, I suggest not to think about the edge cases too much. I have made this mistake when solving binary search problems: The edge cases were so daunting I altered my correct but too general logic to match them.

### 3. Carry out the plan

In code, this is

    const diff = current - start 
    const timeTill = diff > cycle ? cycle - (diff % cycle) : cycle - diff  

This is mere translation. If you can't do this step well, you should invest more time in studying data structures and the features of your language of choice.

### 4. Looking back 

Can you derive the result differently? **Can you use the result, or the method, for some other problem?**

This is the most important step for growth.

Turns out, we can abstract something out. We can write a `mumble` function, that calculates the distance to the beginning of the next cycle **.** This is like the opposite of `mod`, which calculates the distance from the beginning of the last cycle. I was introduced to the `mumble` function by the seminal textbook Concrete Mathematics.

So,

    mod x y = x - y * floor (x y) 
    mumble x y = y * ceil (x y) - x

or in Javascript:

    const mod = (x, y) => x - Math.floor(x/y)
    const mumble = (x, y) => Math.ceiling(x/y) - x

With this, the solution is cleaner:

    const diff = current - start
    const timeTill = mumble(diff, cycle)

