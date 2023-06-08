---
layout: post
title: 'String Matching #1: Using Finite Automata'
---

I learned that Finite Automata (FA) can be used to construct a string matching algorithm from [CSLR](https://www.wikiwand.com/en/Introduction_to_Algorithms) Chapter 32. Chapter 32 presents three string matching algoriths: [Robin-Karp](https://www.wikiwand.com/en/Rabin%E2%80%93Karp_algorithm), Finite Automata, and [KMP](https://www.wikiwand.com/en/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm) (Knuth-Morrit-Pratt, introduced as an improvement to FA). While Robin-Karp is mathematically intriguing, I fancy moments in Computer Science where core, theoretical concepts (like FA) are applied. So, I'll write about using FA first, KMP soon, and perhaps Robin-Karp and [Boyer-Moore](https://www.wikiwand.com/en/Boyer%E2%80%93Moore_string-search_algorithm) in the future.

I'm going to introduce the string matching problem, give the brute force algorithm, hint at improvements, introduce FA jargons, present string matching with FA, and at last, formalize it all mathematically.

### The String Matching Problem

Given string `s` and pattern `p`, find all occurrences of `p` in `s` , using an array of starting indices, overlaps allowed.

For example, for `s=abcdbce` and `p=bc`, then, `occurences=[1,4]`

Overlapping example: for `s=aaaa` and `p=aa`, then, `occurences=[0,1,2]` (If no overlaps are allowed, `occurences=[0,2]`)

### Brute Force Algorithm 

Compare each substring of `s` with the same length of `p` with `p`, if equal then it's a match. The total substrings to compare to is `s.length - p.length + 1` &nbsp;Source code [here](https://github.com/BlastWind/unfooling-blog-snippets/tree/main/string-matching).

### Improvements

When I first came across string matching, I noticed one potential improvement from the naive algorithm, which can be seen in the following example:

<!--kg-card-begin: html-->
s=ACGTACGT

p=GXYZ
<!--kg-card-end: html-->

Let's match from back to front, so, match `T` with `Z` first. After the first match, the naive algorithm catches a mismatch, so it goes on the matching the next substring:

<!--kg-card-begin: html-->
s=ACGTACGT

p=&nbsp;&nbsp;GXYZ
<!--kg-card-end: html-->

But, if we preprocess `p` a bit, like build a hash table using characters in `p`. Then, we can **quickly** deduce whether or not a character exists in `p`. When we start from the beginning again, now using this preprocessed `p` knowledge, we query the first character, `T`, against the table, and we notice that the character doesn't even exist. This realization is powerful: If a character doesn't even exist in the pattern, **we can move the pattern pass it:**

<!--kg-card-begin: html-->
s=ACGTACGT

p=&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GXYZ
<!--kg-card-end: html-->

Notice two things

1) `p` is preprocessed without the knowledge of `s`

2) Preprocessed `p` is used at match time in order to skip alignments

This observation is actually the "bad character rule" in Boyer-Moore. Interestingly, FA does not use this rule in match time (although, it sort uses this implicitly during preprocess time). But there is a preprocessing stage to the FA algorithm, and it builds a complicated automata so that matching runtime is always O(n).

### Leap of Faith

In the world of Finite Automata, we have states `q`, transition function `trans`, actions `a`. `trans` has signature: `trans(q, a) -> q`, in other words, it accepts a state, and an action, and propagates the Automata to a new state.

Now, I'm going to give you an analogy so horrible in hope of it being so laughable that it sticks.

<figure class="kg-card kg-embed-card"><iframe width="200" height="113" src="https://www.youtube.com/embed/aCVW-lba644?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen title="EPIC 6 Pass Basketball Trick Shot In A Pool HD"></iframe></figure>

In these pool basketball trick shot videos, there are a couple of friends and one basketball. The objective is simple: a designated friend begins with the basketball, the friend leaps into the pool before passing the ball to another designated friend. When the basketball is in the hands of the last friend, the friend must make a trick shot into the basket.

Let's now translate this to FA language:

- The states `q` consists of each friend and the final state "landed in basket". So for example, `q = { Bob, Amy, Josh, Landed in Basket }`
- The actions `a` include "passing" and "taking the final shot", these actions are fallible.
- The starting state `q0` is the designated friend, so perhaps `Bob` in this case.
- On fail, we restart, returning to the state `Bob`
- The transition function usually maps a friend and the action "passing" to another friend. But, at the final friend, it maps the action "taking a good shot" to "Landed in Basket". 

Note that, each friend in charged of passing could "take the final shot", but, this fails the mission, and we return to Bob.

Keep this analogy in our brain's random access memory:

Similarly, in string matching FA, we designate `p.length + 1` states, where each state corresponds to

Similarly, in string matching FA, only one action can move the state further alone the spine. But,

### Mathematical Formalization
