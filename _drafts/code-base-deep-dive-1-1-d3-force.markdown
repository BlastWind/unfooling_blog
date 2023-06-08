---
layout: post
title: 'Code Base Deep Dive #1.1: d3'
---

I have a LONG history with d3. My early programming memories were of resolving d3 bugs in my web app due to the annoying `.enter`, `.join`, and `.exit` patterns (yea sorry, it was not writing games on a commodore 64).

Today I will slay my dragon, conquer the old beast of my most primordial fear with my keyboard and IDE.

Questions:

- Internals of `select`, `join`, `enter`, `exit`, `update`, `merge`
- chained `.` syntax
- `.call(drag)`
- `.data`
- How does `d3.event.ctrlKey` know when it's pressed.
