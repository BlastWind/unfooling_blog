---
layout: post
title: Art of Copying Art
date: '2024-09-07 08:00:00'
tags:
- art
---

My passion for writing rekindled last week. I go straight to Google. I search "how to become a writer". I see 7 links — 3 blue, 4 purple. I still open the purple reddit link.

> Practice, practice, practice.

or,

> Write a lot. And read a lot.

or, for maximum effectiveness,

> Start.

Well...  Write *what*? And draw *what*? Or compose *what*? I get the gist though, since, once you are serious about learning the craft and start putting in serious time, you naturally and gradually tune to a better learning process. But, I want to alleviate some of that initial anxiety. Some good constraints can liberate. 

I'll propose a linear learning methods that works for many arts. First, an interlude on how machines learn.

### The Machine
Heuristic and inductive biases for the machine's learning process is purposefully designed, sourcing the records of the human mind. Even the most standard algorithm, self-supervised learning, rationally models how we learn. The machine is initially dumb (it is initialized with random weights), but it gets good at, for example drawing, by repeating the following, many, many times:

The machine generates an image. The machine now looks a real artpiece and contrasts its generation with the artpiece (calculating loss). The machine updates itself to better adapt to the artpiece. 

Artists are told to do master studies; writers are told to read. But these processes are inert. I think, what is missing, is the original generation preceding the adaptation.

### The Adversarial Method
Instead of reading, or plain copying, I do this:
1. Select a passage from an author whose style intrigues you. The granularity I experimented with are the early chapters in Mo Yan's *Big Breasts and Wide Hips* (3-4 pages long).
2. Skim the passage. If you have good short term recall of details, return when they vanish. Alternatively, you could skip the skim and elect AI to summarize. 
3. The key step: Instead of attempting at your rendition of the entire passage, localize to things that matters using AI. For example, I want to train my "show not tell", so I paste the passage to Claude, and ask it to replace paragraphs of descriptions with `<summary of paragraph>`.
4. I fill in the `<>`s. 
5. I critically analyze my work to the authors.

See my prompt and an example conversation [here](https://github.com/BlastWind/unfooling_blog/blob/gh-pages/appendix/2024-09-06-how-to-copy.markdown).
