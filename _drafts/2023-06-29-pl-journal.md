---
layout: post
title: Programming Languages Journal
featured: true
date: '2022-09-06 14:29:26'
tags:
- project
excerpt: Heyo!

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