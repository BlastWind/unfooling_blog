---
layout: post
title: Practical Guide to Haskell's Effectful library
featured: false
date: "2024-12-15 19:00:00"
tags:
  - haskell
---

Objective 1: Make sense of the following types: 
```haskell
interpret :: (HasCallStack, DispatchOf e ~ Dynamic) 
  => EffectHandler e es
  -> Eff (e : es) a
  -> Eff es a
```

```haskell
reinterpret :: (HasCallStack, DispatchOf e ~ Dynamic)
  => (Eff handlerEs a -> Eff es b)
  -> EffectHandler e handlerEs
  -> Eff (e : es) a
  -> Eff es b
```

Objective 2: What is `Eff` and `Effect`?



A `EffectHandler e es` takes in a local environment that handled effects `es`, the operation `e` (more precisely, `e (Eff localEs) a`), and is able to return `Eff es a`.


`g` should be a state.

`evalState Map :: Eff (State Map ': es) a -> Eff es a`

```haskell
reinterpret (Eff (State Map ': es) a -> Eff es a) ::
     EffectHandler e (State Map ': es)
  -> Eff (e : es) a
  -> Eff es a 

  