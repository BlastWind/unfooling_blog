---
layout: post
title: Real, Intermediate Haskell Lenses: Part 1
date: '2024-09-14 12:00:00'
tags:
- haskell
---

Recently, I started reading [`swarm`](https://github.com/BlastWind/swarm), a resource gathering + programming game built with Haskell. I went to `swarm`'s [very first commit](https://github.com/swarm-game/swarm/commit/479b10e98685cdc4c7c7f1935b679b6b83819dcf). The 200-liner is able to render the following,

![Sample GIF]({{ site.baseurl }}/assets/img/swarm-first-commit.gif)

and I was already stumped by the code! What in the lens are these snippets —

```haskell
-- r :: Robot, 
-- robotProgram :: Lens' Robot [Command]
-- p :: Command
(r & robotProgram .~ p)

-- world :: Lens' GameState [[Char]]
-- row, col :: Int
preuse $ world . ix row . ix col

-- inventory :: Lens' GameState (Map Item Int)
inventory . at (Resource h) . non 0 += 1
```

What're the functions the fancy symbols like `.~`? And what do combinators such as `preuse` and `ix` do? Hopefully you can understand these after this piece. Kmett's lenses library is focused around the `Lens` type, so I first give you a whirlwind tour of `Lens` (part 1). But to understand the snippet, we need to explore a more general `Traversal` type, and, how `Lens`/`Traversal` interact with `State` (`mtl`-ified with `MonadState`). That will be part 2.


### A Tour of `Lens`
A `Lens` is *basically* type synonym for

```haskell
type Lens s a = forall f. Functor f => (a -> f a) -> s -> f s
```

Function-wise, a `Lens` can be thought of as both a getter and a setter. We can transform a `Lens s a` both 
into a getter and a setter by applying the `Lens` with the right functor. 
`s` is the larger object that *should* contain some `a`.

```haskell
get :: Lens s a -> (s -> a)
get l s = getConst $ l Const s

over :: Lens s a -> (a -> a) -> (s -> s)
over l f a = runIdentity $ l (\b -> Identity (f b)) a
```
 
I explicitly wrapped the `(s -> a)` in `get` to emphasize the
high-order form. I.e., `get` is a transformation from `Lens s a` to a `(s -> a)`. A getter gives you an `a` from a `s`. In `get`'s definition, 
`f` is specialized to `Const` to make everything work.

A setter typically has the form `s -> a -> s`. I.e., `set` takes in 
object `s` and a constant `a` to update some inner field with, and gives
you back the updated `s`. Instead of `set`, we naturally get a more general `over` here, allowing us to consider the previous `a` in the update.

The following **crucial** point will wrap up the background: A `Lens s a` contains the concrete logic 
to "hit" an `a` from a `s`. For example,

```haskell
data Person  = Person { age :: Int, address :: Address }
data Address = Address { streetName :: String }

-- Contains the logic to hit the address from a person 
ex1 :: Lens' Person String
ex1 f s = (\streetName' -> s { address = (address s) { streetName = streetName'} }) <$> f ((streetName . address ) s)

-- Contains the logic to hit a Char from a [Char]. The only way to have a valid definition is if 
-- we choose a specific index
ex2 :: Lens' [Char] Char
ex2 f s = (\a' -> setAt 3 a' s) <$> f (s !! 3)
```

Note, we can also make nonsensical `Lens`, for example

```haskell
ex3 :: Lens' String Int
ex3 f s = s <$ f 0

ex4 :: Lens' [Char] Char
ex4 f s = s <$ f 'a'
```


`ex1 :: Lens' String Int` should provide a means to get/set an `Int` in a `String`, but that's absurd  — `String` doesn't contain an `Int`. However, `Lens'` allow us to write such a vacuous `Lens' String Int`. We provide a concrete `Int` (`0` in this case) in order to lift it into `f Int` via `f 0`. Once we are in the functor, we apply `fmap (const s) :: Int -> String` to build a `f String` (simplified with `<$`). In fact, we can vacuously write a value of type `Lens' s Int`. 


What about `ex2`? It should be possible to (unsafely) get/set a `Char` inside of a `[Char]`. However, our implementation does not update our `s :: [Char]` at all. We are again providing a concrete value (`'a' :: Char` in this case) just to get into `f Char`, which `s <$` latches onto.

**For all concrete `s` and `a`, `Lens' s a` can be written vacuously. It is up to you to provide a reasonable definition.**
After applying its 3 arguments, a sensible `Lens' s a` should update some `a` within the given `s`.
