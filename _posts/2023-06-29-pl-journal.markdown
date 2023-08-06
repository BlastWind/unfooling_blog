---
layout: post
title: Programming Languages Journal
featured: true
date: '2023-06-28 12:00:00'
tags:
- project
---

### Foreword

I have come to a point in my Haskell journey where I should start learning about other programming languages (PL) fields. Towards the end of my [FP journal](https://unfooling.com/fp-journal/), I started diving into Category Theory and formal verifications, endeavours that shouldn't be documented in a "FP Journal". As such, I've decided to start a PL journal, documenting all of my PL ventures. Unlike the previous journal, I do not plan to elaborate on the concepts I made sense of, as these elaborations often turn into word rambles in the moment.  

## June

**28th**

Understanding the relationships between LTL (Linear Temporal Logic), Haskell, model-checkers, and proof assistants. Conclusion: TLA+ (model checker) can be used to model LTL statements and verify them. There exists LTL libraries in Haskell but they appear to be more data modelling purposes.


**29th**

- Chris (from work) sent me two papers: 
	- [Runtime Verification for LTL and TLTL](https://cs.uwaterloo.ca/~bbonakda/teaching/CS745/papers/RV.pdf)
	- [Alternating Automata and Program Verification](https://www.cs.rice.edu/~vardi/papers/vol1000.pdf)
	These papers supposedly describe how LTL statements can really be translated (down?) to a FSM (Finite State Machine). From there, any language can be used to implement this FSM (since a simple FSM is just a `while` loop + `switch` statements).
	I will zoom in on these tomorrow.
- Watched a video on [linear logic](https://www.youtube.com/watch?v=FqDHSIpWJRw&t=331s&ab_channel=Serokell). Everyday I'm hearing 5 new jargons, at the end of this week I ought to layout a taxonomy of the topics.

## July 

**1st to 14th**
- Read the first 10 pages of the Lamport's TLA [paper](https://lamport.azurewebsites.net/pubs/lamport-actions.pdf) that covers TLA motivation and semantics.
- Watched the [first lecture](https://www.youtube.com/watch?v=VHWEldcSx14&list=PLhZdSWbNhIbCQKxUta0VrDGg3gLguedIh&index=1&ab_channel=songsong) of RWTH Aachen University's Model Checking course.
- Talked to ChatGPT a lot to derive a taxonomy of formal verifications technique. Lectures do better jobs covering these types of information though — ChatGPT don't really come up with memorable sayings. 
- Came across [Linear Types](https://www.youtube.com/watch?v=FqDHSIpWJRw&t=14s&ab_channel=Serokell), didn't understand much.
- Submitted my first paper ever (it's on program analysis) to SCAM'23! I can reveal more if it gets accepted.
- Read the intro of Pierce's [Software Foundations](https://softwarefoundations.cis.upenn.edu/lf-current/Basics.html).
- Read the first two chapter of Pierce's TAPL — currently, I don't find type theory as interesting as formal verifications. Though, I do believe that enjoyment is a linear combinations of the interestingness of the topic's pure abstractions, the topic's applicability, and one's current understanding of the field. When it comes to type theory, I don't have any of these.

**15th to 19th**
- Read the docs of the `servant` library thoroughly: It's the first library I encountered that uses `DataKinds`.
- Read up on the expression problem (encountered through `servant`'s paper). Supposedly, Object Algebras of OOP or advanced Haskell type features can address it.
- Going back to reading Milewski's Category Theory for Programmers because I should be knowing what Kelsili Arrow and natural transformations are at this point. 
	- Finished Chapter 3. Interesting idea: Think of functions in their uncurried form more. If I think of `mappend` as `m -> (m -> m)`, I can see it as a mapping between an element of a monoid set to a function acting on that set!



**Week 4**
- Listened to two episodes of the Haskell Interlude podcast: One with Gabriella Gonzalez, and one with Graham Hutton. Professor Hutton's honesty and passion was, simply put, inspiring. The also-inspiring Gabriella mentioned two interesting things I'll look into: Monad Reader and Dhall.
- Listened to Conal Elliot talking about [the lost elegance of computation](https://www.typetheoryforall.com/2022/05/09/17-The-Lost-Elegance-of-Computation-(Conal-Elliott).html). Unique and poignant philosophy, inspired by Conal's passion, I ought to read John Backus's famou 1977 Turing award [lecture](https://dl.acm.org/doi/pdf/10.1145/359576.359579) and read into FRP.
- Read Monad Reader issue [19](https://themonadreader.files.wordpress.com/2011/10/issue19.pdf); learned about the `Coroutine` monad; I was able to use `Iteratee` and `Generator` to successfully implement a basic sketch of a program for work.


## August 
**Week 1**
- Spent a weekend in FRP via Heinrich's [guide](https://github.com/HeinrichApfelmus/frp-guides). Big idea: `Behaviors` are continous and `Events` are discrete, but, they can be easily composed and transformed to one another.
- React-banana uses `MonadMoment`, which is also a `MonadFix`. I tried reading [MonadFix is Time Travel](https://elvishjerricco.github.io/2017/08/22/monadfix-is-time-travel.html). I only partially understood what's going on even with after reading another [article](https://www.parsonsmatt.org/2016/10/26/grokking_fix.html) because I lack a complete understanding of laziness, thunks, and `seq`, so, I started reading Haskell wikibook's laziness [article](https://en.wikibooks.org/wiki/Haskell/Laziness).
