---
layout: post
title: Thrown Error Falls through Promise.catch
date: '2022-08-03 03:25:54'
tags:
- bit
---

Today I wrote an unhandled error. See if you can find it first:

    const foo = (shouldThrow: boolean) => {
    	if(shouldThrow) throw new Error("BAD!")
    	return Promise.resolve("ok")
    }
    
    foo(true).catch(() => console.log("caught"))

`.catch` does not catch `throw`s. So, the thrown `new Error("BAD!")` is unhandled.

Here's a way to get around this:

    const foo = async(shouldThrow: boolean) => {
    	if(shouldThrow) throw new Error("BAD!")
    	return Promise.resolve("ok")
    }
    
    foo(true).catch(() => console.log("caught"))

Nothing's changed, except, I added a little `async`. This is powerful. Although `async` internally maps to a `Promise`, I like to think of the keyword `async` as a modifier to place a function into an "asynchronous" pool (distinct from the normal, "synchronous" pool). And I also like to think of `.catch` as only being able to catch errors from this asynchronous pool. This is why that the first examples resulted in an unhandled error: The error thrown is synchronous.

Now, here's the annoying part: Neither **Typescript nor Eslint warned me.** My actual code was more complex, hiding a `throw` among nested checks.

I wonder if Typescript should lint this. If a plain `throw new Error()` gives the type `never`:

    // () => never
    const err = () => { throw new Error("BAD!") }

I wonder why the following is the case:

    // infers () => number
    const foo = () => {
        throw new Error("Bad!"); 
        return 1
    }
    
    // infers () => number | string
    const bar = (condition: boolean) => {
        throw new Error("Bad!"); 
        return condition ? 1 : "asdf"
    }

Instead of

    // maybe () => number | never is better
    const foo = () => {
        throw new Error("Bad!"); 
        return 1
    }
    
    // maybe () => number | string | never is better
    const bar = (condition: boolean) => {
        throw new Error("Bad!"); 
        return condition ? 1 : "asdf"
    }

`never` is documented as "not having a reachable endpoint". And, it seems to   
[correspond to an empty set](https://stackoverflow.com/a/64246079/9007785) in Set Theory. So, under these constructs, it wouldn't make sense to return a union of `never` with something else. But, should these constructs stand in the first place? `never` never gets used (no pun never intended), so perhaps, when designing a new language with the same notion, `never` should be loosened.

