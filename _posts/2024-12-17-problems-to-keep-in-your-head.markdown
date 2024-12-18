---
layout: post
title: Problems To Keep In Your Head
featured: false
date: "2024-12-17 11:07:00"
tags:
  - idea
---

This post serves two goals:
1. Encourage you to carefully select problems on marinate on,
2. Give you the problems I'm thinking about. I will also 
occasionally update this post with new problems.

### Encouragement
I have observed, smart people can explain intricate, small problems 
succinctly, yet they always leave you with the feeling
that their point sits on top of a great pyramind — built 
carefully over the years, brick by brick, 
composing a vast amount of ideas and knowledge.

They do this with a lot of problems. 
They seem to often take in and explain information in a problem -> solution,
cause -> effect manner. And the very smart ones do this with a lot of small problems 
along with a dozen of big problems. They are ready to
trail into an exposition whenever they speak.

I don't know if these folks do it on purpose. But if you are 
not marinating on selected problems yet, why not make this an intentional habit?
Feynman was the first I know to suggest this —
> "[To be a genius],
> you have to keep a dozen of your favorite problems constantly present
> in your mind, although by and large they will lay in a dormant state.
> Every time you hear or read a new trick or a new result, test it against
> each of your twelve problems to see whether it helps." 

Richard Hamming exclaimed the same —

> [Most great scientists] have something between 10 and 20 important problems for which they are looking for an attack. 
> And when they see a new idea come up, one hears them say "Well that bears on this problem."


### My Problems
I am formulating my initial problem list as of writing (Dec 2024). I stumbled upon these through personal curiosities, but expect
future listings to be inspired by my colleagues, or by you, my reader.

1. The Expression Problem.

Category: Software engineering. 

Enlisted: Dec 2024.

Intro: How can you extend the data type and the functionality orthogonally? I believe 
effect systems in Haskell make for the cleanest approach. But I am far from 
gathering all of the possibilities (other languages? Type-level or term-level?).
I should also make note of architectures/features that don't solve the expression problem.

2. Extent of the Curry-Howard correspondence.

Category: Type theory. 

Enlisted: Dec 2024.

Intro: Curry-Howard correspondance envelopes proof-as-programs. For example,
`exists n. n = 0`, a proposition, is considered a type. When you construct a term
with this type, you have provided a proof for the proposition.

The correspondance runs deeper than proof-as-programs. The polymorphic lambda calculus is the same as
System-F; Many lambda calculi have equivalent power with certain logics;
Logic can be encoded in lambda calculi. 

Where is the extent of this interpretation? I should think about Curry-Howard
whenever I encounter logic in type theory, and vice versa.

3. Integrating formal verifications in Turing-complete languages.

Category: Type theory. 

Enlisted: Dec 2024.

Intro: How can I prove that `forall n: nat, n + 0 = n`, in say, Python? 
The coq approach would be to define `nat` inductively, and prove
statement with induction. But what if we just hook this to a SMT solver?
What do the ergnomics look like? I first came across this problem in June 2023
through the [magmide](https://github.com/magmide/magmide#this-is-an-exciting-idea-how-can-i-help) repo.
I actually emailed the author about what materials to learn, but I put off his suggestions for a 
whole year since I was finishing up senior year. I did complete software foundations volume 1
since my graduation in May, and, coincidentally, I am employed on a program that deals
precisely with this.

4. Building a microbiology for LLMs.

Category: LLM interpretability. 

Intro: I first came across the field in Dec 2023 through the transformer circuits [thread](https://transformer-circuits.pub/).
Reflection questions: "Is this a technique that validates interpretation (refining the microscope's lens)?
Is this fundamental (coining a new cell type)?"

5. Providing more rigor for LLMs.

Category: Category theory, LLM, neurosymbolism. 

Intro: We are at a limit with transformers. Can we provide more rigor for the next generation of LLMs? 
Can you encode logic into [them](How can we provide more rigor for LLMs)? 