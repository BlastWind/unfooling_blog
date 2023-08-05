---
layout: post
title: Reflecting on my 2022 tech opinions
date: '2023-08-05 12:05:00'
tags:
- idea
---

Last August, I gave 6 tech [opinions](https://unfooling.com/6-tech-opinions-i-formed-during-my-year-long-internship/). I reflect on those now —

> Reading the source code is more responsible and often faster than asking the author, given that you understand the source's architecture. This echoes item 1 in Matt's [reflection](https://matt-rickard.com/reflections-on-10-000-hours-of-programming).

I would like to update this — My new thought is that whether to ask should be based primary on the inherent beauty of the source. If you're reading a spaghetti game engine to figure out some minute detail, might as well ask the author since they didn't publish code that was worth your time. Remember, an idiot admires complexity while a genius admires simplicity.


> Don't force dumber code in the fear of others not understanding. Example: Paul Graham on [Why Lisp?](http://www.paulgraham.com/avg.html). Writing hard (but simple) code makes you smarter and inspires code reviewers. Here's a good article on [hard vs easy, complex vs simple](https://rpeszek.github.io/posts/2022-08-30-code-cognitiveload.html).

I would like to elaborate — When I wrote this, I was relatively new to Haskell, enticed by every single language extension and believed everyone else should be equally passionate as me. But, should you really use dependent types for the sake of it? There's no clear answer — it depends on your team structure (# seniors vs. # juniors, average tenure) and purpose (research, academics). For a well-written essay on this matter, see this [plea](https://www.parsonsmatt.org/2019/12/26/write_junior_code.html) for simple Haskell.


> If you lack knowledge in an established research area (e.g., database, OS, etc), the best way to catch up is to read a textbook.

Oh yea, 100%, read things that people spent years working on. Textbooks and papers often fill this criteria while most blogs don't.

> The quality of a software correlates with how much internal information it has on its documentation. Of course, some software must reveal its internals. However, I find myself revealing unnecessary internal information when I am unable to articulate the usages.

This opinion provides a great question: Did I leak internals because the design of my interface is not abstract enough (the surface area approaches the volume, as what Bartosz Milewski would say)? I continue to use this question on my own projects. 

> 9 pregnant women can't give birth to a baby in one month (from [The Mythical Man Month](https://www.wikiwand.com/en/The_Mythical_Man-Month)). I now understand how true this. Conversely, project managers must decide on this balance, and that's why tech PMs should have tech backgrounds.

Mostly, but I don't think PMs necessarily need tech backgrounds as long as they understand the complexities of software construction.

> And the most important tech opinion: Tech is created by people and for people, so **people \> tech**. Software engineering sits in between computer science and social science. My experience at ClearBlade was enjoyable: People at my work were nice and acknowledged each other's doing, my CTO reminded me to take vacations, and the list goes on.
 
Of the people, for the people, by the people.