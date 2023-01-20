---
layout: post
title: Replace for loops with iterables and .map
date: '2022-08-14 19:12:03'
tags:
- bit
---

Yesterday I participated in a weekly leetcode contest and came across this [question](https://leetcode.com/problems/largest-local-values-in-a-matrix/description/):

<figure class="kg-card kg-code-card"><pre><code>You are given an n x n integer matrix grid.

Generate an integer matrix maxLocal of size (n - 2) x (n - 2) such that:

maxLocal[i][j] is equal to the largest value of the 3 x 3 matrix in grid centered around row i + 1 and column j + 1.
In other words, we want to find the largest value in every contiguous 3 x 3 matrix in grid.

Return the generated matrix.</code></pre>
<figcaption>Amounts to building a max value grid out of each 3x3 square.</figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img src=" __GHOST_URL__ /content/images/2022/08/image-4.png" class="kg-image" alt loading="lazy" width="364" height="206"><figcaption>Example <code>maxLocal</code> matrix</figcaption></figure>

Here's the straight forward iterative solution:

    const largestLocalIterative = (grid) => {
        const res = [...Array(grid.length - 2)].map((_) =>
            Array(grid.length - 2).fill(0)
        );
        for (let row = 0; row < grid.length - 2; row++)
            for (let col = 0; col < grid.length - 2; col++)
                for (let i = row; i < row + 3; i++)
                    for (let j = col; j < col + 3; j++)
                        res[row][col] = Math.max(res[row][col], grid[i][j]);
        return res;
    };

However, we can spice it up with a functional spin:

    function* range(start, end) {
        for (let i = start; i < end; i++) yield i;
    }
    
    const largestLocal = (grid) =>
        [...range(0, grid.length - 2)].map((row) =>
            [...range(0, grid.length - 2)].map((col) =>
                Math.max(
                    ...grid
                        .slice(row, row + 3)
                        .flatMap((rowObj) => rowObj.slice(col, col + 3))
                )
            )
        );

Here's are some breakdowns:

The **Outer loop** iterates over each cell in the `maxLocal` grid.

The iterative approach

    for (let row = 0; row < grid.length - 2; row++)
        for (let col = 0; col < grid.length - 2; col++)
            // access to row, col

is equivalent to

    [...range(0, grid.length - 2)].map((row) =>
        [...range(0, grid.length - 2)].map((col) =>
            // access to row, col
        )
    );

The **Inner loops** fills the current cell's value in the `maxLocal` grid.

The iterative approach

    for (let i = row; i < row + 3; i++)
        for (let j = col; j < col + 3; j++)
            res[row][col] = Math.max(res[row][col], grid[i][j]);

is equivalent to

    Math.max(
        ...grid
            .slice(row, row + 3)
            .flatMap((rowObj) => rowObj.slice(col, col + 3))
    )

Note the cute `range` generator I wrote. It is similar to python's `range`. However, I decided to use Javascript instead of Python because of the built in `.map` and `.flatMap` on the native array object. &nbsp;

I can even define an generator directly on the native Number object.

    Number.prototype[Symbol.iterator] = function* () {
        for (var i = 0; i < this; i++) yield i;
    };

This allows me to drop the `range` function in certain situations. For example, I can do `[...(grid.length - 2)]` instead of `[...range(0, grid.length - 2)]`.

**Update 8/24/2022:**

I realized that I can write a cleaner solution in python since it has `list comprehension`:

    def largestLocal(self, grid: List[List[int]]) -> List[List[int]]:
        R, C = len(grid) - 2, len(grid) - 2
        return [[max(max(row[c: c + 3]) for row in grid[r: r + 3])
               for c in range(C)] for r in range(R)]

