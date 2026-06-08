---
layout: post
title: Improve Code Reading with Software?
date: '2022-09-04 01:58:45'
tags:
- idea
---

A quick search of "software to help read code" led me to the download links of different IDEs. Interesting. While there are thousands of books that teaches how to read books, among the millions of software, there doesn't seem to be many that helps with reading software.

Let's brainstorm some features for such a tool:

- Code Focus: Select a few lines of code to focus, this will fade away all variables and functions that have nothing to do with the selected code. If branching occurs in the selection, use our tool to quickly denote the truth values of the logical expressions in order to focus on the right flow. 

Interlude: Why do we read FOSS? I read FOSS to learn how to write similar software, intricate design choices and architecture, and hidden nuggets. Here is an [example](https://github.com/JimLiu/recoil-paint/blob/master/src/recoil/defaults.js) of a hidden nugget: JS code can modify global variables (`gId`) of another file. Although I have wrote something similar to this in C, this is still eye-opening in JS, and textbooks/manuals rarely provide examples as specific as you will find in real code.

- Auto commenting the code patterns detected.

Now, onto intricate design choices and architecture. I don't know how our tool can deduce a software's "macroarchitecture" (where its components exists on a computer, how do its moving parts communicate with each other, etc). However, our tool can certainly detect code patterns: For example, our tool might tag a class as "Singleton" when it detects a private constructor. **Furthermore, while copilot translates idea -\> code, our tool can probably utilize the inverse: code -\> idea** , and auto comment the idea.

(Update Sep 4)

- Tutorial mode.

If our software becomes a standard way of browsing code, we can enforce special tutorial annotation comments that software authors write to provide a tutorial mode" for new readers. For example, authors can give functions a certain `@tutorial-item-order-1`.

So, the flow looks something like this: Readers realize that the software is compliant with our tool's tutorial mode -\> They begin the tutorial -\> A code slideshow begins, moving forward in the slides jumps to the next piece of code the authors annotated.

- Special annotations for a sea of possibilities

If our software becomes a standard way of browsing code, we can enforce special annotation comments that software authors write to provide a better experience for code readers. For example, one annotation could be `@hack`. Readers can browse all interesting hacks and gotchas in a slideshow format. Furthermore, `hacks` might be tinted in red for emphasis.

Another annotation I thought about is `@tutorial-item-order`. The authors can provide this annotation for code to be browsed in a guided, sequential order in a tutorial mode. I have personally commented my repos this way for the ease of new readers to better follow the code flow.

At last, I think such a software should be packaged as a vscode extension.

