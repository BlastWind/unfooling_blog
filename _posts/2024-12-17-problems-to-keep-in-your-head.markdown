---
layout: post
title: Problems in Your Head, Problems in My Head
featured: false
date: "2024-12-17 11:07:00"
tags:
  - idea
---

This post serves three goals:
1. Encourage you to marinate on problems,
2. Prelude a practical information organization framework,
3. Give you my problems.

I will occasionally update this post with new problems.

### Encouragement
I have observed, smart people can explain intricate, problems 
succinctly and can fluidly back up their points 
with examples and stories. 

They seem to communicate and store information in a natural
problem-solution, cause-effect framework. Their brain can
constantly juggle numerous problems. Their neural connectivity
between concepts are strong.

My thesis: It is the exact act of constantly juggling problems 
over a long period of time,
connecting them with other concepts, infusing them with stories,
that makes you smart. It also helps you appear smart, as 
that information is lucid and ready.

And while the geniuses might be unconsciously doing this, 
there is nothing preventing us from consciously adapting
this practice. Feynman was the first I know to suggest this —
> "[To be a genius],
> you have to keep a dozen of your favorite problems constantly present
> in your mind, although by and large they will lay in a dormant state.
> Every time you hear or read a new trick or a new result, test it against
> each of your twelve problems to see whether it helps." 

Richard Hamming exclaimed the same —

> [Most great scientists] have something between 10 and 20 important problems for which they are looking for an attack. 
> And when they see a new idea come up, one hears them say "Well that bears on this problem."

Pick your problems and set a daily reminder to resurface them.

### Organizing Information At Large
There are big problems, small ideas, quick sketches... How do we organize all of these?
How do we ingest information? Can we become fluid with all of them?
Should the problems we pick only be big challenges?

I present some thoughts and untested hypotheses on these but leave this section partial.
Developing a total framework for genius science thinking deserves a whole book.

1. You only know what you can teach. Before you pick your problems, 
discover what you know by heart: What can you teach a class on, right now?
What are you confident in writing a textbook on?

2. There are two tiers of information: Fundamental and junk. 
Information you deem fundamental make up what you know. 
Since what you know is a good chunk of who you are, these information
**make up your identity**. Therefore, they are very, very important.
After ingestion, they should be queued for prepping into 
a format ready for scientific communication. They are composed 
into stories or written in prose. They should be revisited intermittently (active recall).
You should consciously classify information as either fundamental or junk.
Most information lie in the no mans land of "might be useful". 
But, you aren't going to retrieve or use the "might be useful" unless you need it,
so, with time, they are as good as garbage.

3. Your big problems are your super fundamental information. They are your organs, your chakras.
When enlisting new fundamental information, you should consider how it
relates to your big problems. 

4. It may be useful to embark on your "Great Reconstruction". I will experiment with this.
I plan to go through my obsidian app and delete all information that "might be useful".
I will then label each note as uningested, slowly go through them, reingesting them
with the rules I posed in 2).

5. Information can be recalled easier if it maintains casual relationships with other information.
When I studied Algebra last year, I justified the algebraic rules for an "ideal" as the precise
constraints that allow rings to quotient over during canonical decomposition. This is not 
the way it's taught, but the two hours I spent finding this casual relationship allowed me 
to still remember this fact. 


### My Big Problems
I am formulating my initial problem list as of writing (Dec 2024). I stumbled upon these through personal curiosities, but expect
future listings to be inspired by my colleagues, or by you, my reader.

##### The Expression Problem.

Category: Software engineering. 

Enlisted: Dec 2024.

Intro: How can you expand the datatype and extend the functionality orthogonally?
As of right now, I know effect systems in Haskell to make for the cleanest approach.
But I need to investigate other languages and type-level programming.

##### Extent of the Curry-Howard correspondence.

Category: Type theory. 

Enlisted: Dec 2024.

Intro: Curry-Howard correspondance envelopes proof-as-programs. For example,
`exists n. n = 0`, a proposition, is considered a type. When you construct a term
with this type, you have provided a proof for the proposition.

But the correspondance runs deeper than proof-as-programs. The polymorphic lambda calculus is the same as
System-F; Many lambda calculi have equivalent power with certain logics;
Logic can be encoded in lambda calculi.

Where is the boundary of this interpretation?

##### Integrating formal verifications in Turing-complete languages.

Category: Type theory. 

Enlisted: Dec 2024.

Intro: How can I prove that `forall n: nat, n + 0 = n`, in say, Python? 
The coq approach would be to define `nat` inductively, and prove
statement with induction. But what if we just hook this to a SMT solver?
Or, maybe `forall n: nat, n + 0` doesn't matter, it's not something 
real software writes?
I first came across this problem in June 2023
through the [magmide](https://github.com/magmide/magmide#this-is-an-exciting-idea-how-can-i-help) repo.
I actually conversed with the author through emails as a complete noob at the time. Coincidentally,
I am employed on a program that deals precisely with this.

##### Building a microscope and microbiology for LLMs.

Category: LLM interpretability. 

Enlisted: Dec 2024.

Intro: I first came across the field in Dec 2023 through the transformer circuits [thread](https://transformer-circuits.pub/).

##### Providing more rigor for LLMs.

Category: Category theory, LLM, neurosymbolism. 

Intro: We are at a limit with transformers. Can we provide more [rigor](https://arxiv.org/abs/2104.13478) for the next generation of LLMs? 
Can you encode logic into [them](https://link.springer.com/book/10.1007/978-3-031-71167-1)? 

##### Something Quantum.

Category: Quantum.

I know nothing right now. But I'll slowly develop this. I'm definitely more interested on the algorithmic application side.