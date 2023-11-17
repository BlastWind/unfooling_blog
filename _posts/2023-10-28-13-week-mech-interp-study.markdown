---
layout: post
title: 13 week Mech Interp Study Journal
featured: true
date: '2023-10-28 12:00:00'
tags:
- project
---
### Foreword
<!-- Cross all history, we might just be living in the most anticipatory time. Billions of neurons, organized as subgraphs and circuitries, firing millions of "AI will revolutionize..."s everyday. But most of us, even those that happen to study computer science, and even those that happen to study artificial intelligence, seems to not grasp the gravity of the moment. Indeed, so much happened so quickly that perhaps, our circuitries have overfitted on the irregular progression of science.

I read my first alignment paper this week, and it has reignited my once diminished passion for AI and machine learning. Though my future occupation in formal verifications does not have too much correspondance to AI yet, I have, as a scientist, a moral responsibility to understand this field fully. Of course, as an undergrad who's writing his thesis on LLMs -->
Mech Interp is super cool! I'm gonna set aside 13 weeks to study it. In these 13 weeks, I plan to master basic ML math, master transformers, read a couple mech interp papers in depth, and hopefully carry out some small-scaled, original mech interp exploration.

Note, this is a direct mirror of my personal log in my Obsidian vault, and some formatting might not be appropriate (I will format everything at the end).

### Timeline
**Target**: 10/28/2023 to 1/26/2023 (end of my winter break)

### Undated Day 1
- Read some basic definitions of *Linear Algebra Done Right*.
### Undated Day 2
- Finished *Linear Algebra Done Right* Chapter 1. Re-learnt subspaces, sums of subspaces, direct sums.
- Looking ahead, to solidfy my linear algebra for MI, I should master Chapter 1-3, 5-6, and 7d. More importantly, I need to spend some time everyday in finding connections of the niche linear algebra techniques are used for ML.
- I found one such connection today (querying ChatGPT): Gradients should be initialized with orthonormal matrices in order to preserve the magnitude of gradients.

### Day 1
- Read *Linear Algebra Done Right* Chapter 2 up until the last definition. I think basis and theorems regarding bases drilled in the definition of linear independence and span.
- Tomorrow, I just want to finish up the definition, do a few problems. Then, I want to skim some high-level material on why bases matters for machine learning; in particular, I hear a lot about "orthonormal" bases in ML, when and why do they come up?
- [x] Why bases matter for machine learning - high level view. 📅 2023-10-28 ✅ 2023-10-28

### Day 2
- Yesterday i assigned the "why bases matters" task, I have a high-level answer: To address the curse of dimensionality and other dimension related issues. Keywords are privileged, non-privileged bases, PCA, SVD. 
	- Resources: https://harrisonpim.com/blog/privileged-vs-non-privileged-bases-in-machine-learning


### Day 3
Skipped

### Day 4
- Did 15 exercises for [[Chapter 2]]. I didn't try too hard, I looked at the solution if I can't solve in 5 mins. 

### Day 5
Skipped

### Day 6
Skipped

### Day 7 (11-03-2023)
Wrote MLP forward stuff.

Watched this great [video](https://www.youtube.com/watch?v=Gey9CG6R6w8) by WhyML on Skip connection and residual blocks.

- [x] Review Batch Normaliztaion, Optimizers, Regularization, Gradient Clipping, Weight Constraints, Weight Normalization, Layer Normalization 📅 2023-11-04 ✅ 2023-11-09
- [x] What is Skip Connections and Residual Blocks? 📅 2023-11-03 ✅ 2023-11-04
- [x] Understand the gradients that are taken. 📅 2023-11-12 ✅ 2023-11-15

### Day 8 (11-04-2023)
Studied backpropagation. I used multiple resources: The original sparse encoder article, some random article by ML-dawn, and Brilliant Wiki. But, when I revisited 3b1b's video, that's the one that made it all click. Tomorrow, I plan to rewatch the video and take a lot of notes.

### Day 9 (11-05-2023)
Rewatched 3b1b's video and I think all the formulae individually makes sense. Tomorrow, I'll try to
- Derive the gradients from scratch
- If I get them right, give a shot at drawing derivative diagrams for a MLP
- Implement backprop in MLP.

### Day 10-18 (11-06-2023 to 11-14-2023)
Skipped. Not gonna lie, played a little too much minesweeper.

### Day 19
- Deleted my online minesweeper account. Started 2 weeks ago and sunk 35 hours into it, no good, those hours will now be redirected to mech interp.
- Watched Neel Nanda's [MI and Math](https://www.youtube.com/watch?v=bZvPLRZx-V8) talk and annotated it. The slides are [here](https://docs.google.com/presentation/d/1hQHsWcoxtBQKj4mTgFtMA9KgDjkyAcjEWjxDwXq5EPg/edit#slide=id.g24176faf163_0_225)
- 40 mins into ML street talk episode with Neel Nanda. Take aways:
	- A biologist's getting hands dirty view might be more appropriate than a mathematician's view. A good quality of a mech interp researcher is someone who can *handle* a surprise. 

My goals this week is to start understanding transformers well, read this paper, and get a sense of the other techniques I gotta study (dimensionality reduction, Fourier transform even?).


**Idea**: Analyze transformers that implement simple regexes / text manipulation patterns?

### Day 20
- Read the [Attention](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/) blog post, it is a prior of the [Illustrated Transformers](https://jalammar.github.io/illustrated-transformer/) guide. 

Some knowledge tasks. 
- [ ] Understand: What is the Curse of Dimensionality? 📅 2023-11-17 
- [ ] Master PCA 📅 2023-11-19 

Inspired by Neel's talks yesterday, I will run a mech interp notebook this week to get a feel of the tools.
- [ ] Run, completely, one mech interp notebook 📅 2023-11-17 