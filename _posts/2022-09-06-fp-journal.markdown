---
layout: post
title: Functional Programming Journal
featured: false
date: '2022-09-06 14:29:26'
tags:
- project
excerpt: Heyo!

---

### Foreword

> It is not only the violin that shapes the violinist, we are all shaped by the tools we train ourselves to use, and in this respect programming languages have a devious influence: they shape our thinking habits. - Dijkstra

At the time of writing this foreword (9/6/2022), I have functionally programmed ~200 hrs (mainly, regurgitating and practicing [this](https://github.com/MostlyAdequate/mostly-adequate-guide)). I love it, it's a challenge, it makes me think clearer and feel more powerful. Two months ago, I started dabbing in Haskell a bit and am 10 chapters into [this](https://haskellbook.com/). As Euclid deemed that "there is no loyal road to geometry", the modern FP wizards deem that there is "no loyal road to FP". But, I still want to serve this community with my personal learning notes. Perhaps you, my reader, will stumble upon this journal years after my initial post, and at least find some comfort in that there are others after the same destination, who followed through and prevailed (hopefully).

I want to maintain the entire journal in this single post. I shall debrief daily, but I'll compile the debriefs each month so that this place doesn't get too cluttered.

Edit (11/28/2022): I am not going to compile the journals at the end of each month. I think a compilation will dull the sentiments of the journal! However, to avoid clutter, I'll open up a new journal post every few months.

### September 2022

**6th**

`foldr, foldl, fold'`: foldr can short circuit. foldr, foldl are lazy, stack overflow prone. foldl' is strict (opposite of lazy), hangs in infinite loop because no stack overflow.

7th

HPFFP pg. 590-642: Interesting `Product`, `Sum` types (`data Sum a b = First a | Second b`). First time coming across kinds, cardinality (despite being familiar with sum and product type).

8th

HPFFP pg. 643-681: Function type is the exponent operator (in regards to sum and product types). Higher kinded types are similar to higher order functions. Lists are polymorphic.

9th

First half of [Input and Output](http://learnyouahaskell.com/input-and-output). Solved 5 basic codeforces problems according to this [list](https://github.com/anurudhp/CPHaskell) left. Biggest lesson from doing these exercises: Type can be inferenced using knowledge of the next function invoked, this is how `read` can be written to not use explicit casts; `interact $`.

10th

HPFFP pg. 681-690. Started studying binary trees, yet to comprehend [Foldable instance for Tree](https://riptutorial.com/haskell/example/2556/an-instance-of-foldable-for-a-binary-tree) implementation:

     foldr f acc Leaf = acc
     foldr f acc (Node l x r) = foldr f (f x (foldr f acc r)) l

Tomorrow I want to understand everything in the previously attached linked deeply.

11th

Understood yesterday's snippet: To fold a tree, recursively fold its left subtree with the accumulator value being the result of the input function applied to the current root node's value and the result of folding the right subtree.

Started doing HPFFP Ch.11 exercises, stuck on Ciphers problem. Watched 90 minutes of [this](https://www.youtube.com/watch?v=N9RUqGYuGfw): How a custom Parser type (which has instances for Functors and Applicatives) can parse json (also the topic of HPFFP Ch.24). I'll refrain from finishing the entire video, need to build up to understanding everything.

12th

Used `zipWith` to iterate two Foldables at once to complete the cipher (thanks to my python knowledge); &nbsp;Learned `as-patterns` to bind original variable while pattern matching it.

14th

HPFFP Ch.15 reading. Types can have more than one monoidal operation.

15th

HPFFP Ch.13 reading and exercises: Built hangman!

16th

Read almost all of HPFFP Ch.14 on testing, still internalizing `Arbitrary` and `Gen`. Solved code forces 2A in haskell, used `Data.Map` for the first time.

16-27th: Busy with other stuff. Read half of the chapter on Functors.

28th: Lisp s-expressions. Stumbled upon this because I'm working on a call graph generator which uses tree-sitter, a parser with a query language formatted with s-expressions.

### October 2022

5th: Q1 - Q4 of [99 problems](https://wiki.haskell.org/H-99:_Ninety-Nine_Haskell_Problems). Ignore first argument in function application by using const. Ignore second argument with a `const . (+1)`. So damn cool.

6th: Went back to `foldr, foldl, fold'`. I learned that `seq` can be used to enforce strictness in the lazy realm of haskell.

15th: `(fmap . fmap)` penetrates the value of the second Functor. More can be chained. I was able to trace the type of this expression, but man, I don't have much intuition of it.

16th: Finished HPFFP Functor Chapter. Instead of pattern matching on `Just` and `Nothing`, just use a single `fmap`! Understand functors more, like why `fmap` only hits the second argument in `Either`, tuples, etc.

17th: HPFFP p.1052-p.1067. Obtained basics of `Applicative`. It was tough writing the pedagogical `Applicative` instance for `Constant x` , had to look up both how to write `pure` and `<*>`, it all makes sense now though.

18th-22nd: HPFFP p.1067-1206 (currently on Monad exercises). Built a simple [tic tac toe](https://github.com/BlastWind/hic-hac-hoe). Internalized `<$>`, `<*>`, `>>=`. I need, however, a deep understanding of "algebras that aren't other algebras". In other words, concrete examples of say, Applicative instances that aren't Monad instances.

28th: Read over how to use quickcheck in HPFFP again, did some Monad exercises.

29th: HPFFP p.1212-1258: Learned to build a URL shortener! Also TIL `replicateM 5 [1,2,3]`effectively generates all length 5 permutations with elements from `[1,2,3]`. This is because `replicateM` performs the monad 5 times, and list monads combine on mappend.

30th-31st: HPFFP Ch20 Foldable and Ch21 Traversable (p.1259-1314). Fun and easy.

### November 2022

1st: HPFFP Ch22 Reader Monad. Tbh, this [article](https://engineering.dollarshaveclub.com/the-reader-monad-example-motivation-542c54ccfaa8#77ff) offered a great explanation. There is, more to the reader monad than resolving "prop drilling" (term used by the ReactJS community), but it is one of its greater uses.

2nd: I found that similar to how `fmap . fmap` targets a value beneath nested functors, `foldMap . foldMap` targets a value beneath nested `foldables`. The concept might be understood quicker if one were to just write `fmap (fmap f)` and `foldMap (foldMap f)` though. The `(.)` syntax, while terse, doesn't imply nestedness. However, I'm but a novice, I think stronger readers can understand quickly that `fmap` can be seen as `(a->b) -> (f a -> f b)`, so the second `fmap` of `(fmap . fmap)` is the function the first `fmap` accepts, which is functorial `(f a -> f b)`.

3rd: HPFFP Ch 25 and partial Ch 26. I'm getting a hang of monad transformers thanks to [this](https://stackoverflow.com/questions/32579133/simplest-non-trivial-monad-transformer-example-for-dummies-iomaybe). What I understand is that `MaybeT` `return Nothing` in its `>>=` when the first part's nested Maybe is indeed a `Nothing`. What I still need are more examples. It seems like `MaybeT`, in the stackoverflow example, solves the staircase problem because the Maybe-ness is embedded in the `>>=`, and thus embedded between the lines of a `do` block. This prevents the need to pattern match on `Maybe`, and thus prevents the staircase problem.

4th: Watch this little [gem](https://www.youtube.com/watch?v=A-qGGag3Mt8) on how IO is defined using the state monad. Haven't understood everything about it yet. I also traced through a state monad in this [video](https://www.youtube.com/watch?v=DaU6BAV7Z94&t=1215s), also still trying to understand it.

5th-9th: Went through the [Turnstile State example](https://en.wikibooks.org/wiki/Haskell/Understanding_monads/State#Accessing_the_State). In addition to the essence of state, I also dabbled in `foldM`, `mapM`, `filterM`, and `sequence`.

11th: Wrote the instance for the monad transformers `ReaderT` and `StateT`. Turns out, I can't write an `Applicative` for `StateT` unless the inner higher typed variable is a `Monad`. This is because, to get from `s -> m (s, a->b)` and `s -> m(s, a)` to `s -> m(s, b)`, I can apply the `s` to the first expression to get `m (s, a->b)` , but then, to get the `s` out of this expression in order to apply it to the second expression, `m` must be a Monad.

12th-13th: More on monad transformers, read through the MonadTrans section in HPFFP but didn't really internalize much, so I plan to read [this](https://en.wikibooks.org/wiki/Haskell/Monad_transformers).

14th: The reason we do `MaybeT $ do ...` when defining the monad instance for `MaybeT` is that `do` must be in the other monad, `m`'s context, and not in `MaybeT m`'s (because it `MaybeT` monad is yet to be defined). "There is no magic formula to create a transformer version of a monad; the form depends on what makes sense in the context of its non-transformer type." We can write liftM with the bind or do notation.

15th: Read this little [piece](http://matija.me/2020/11/05/haskell-monad-transformers-intro/) on monad transformers. Now reading FP Complete's [piece](https://www.fpcomplete.com/haskell/tutorial/monad-transformers/) on it. The `foldTerminate` function utilizing Either monad is something I haven't seen before. I'm going to carefully trace through and unwrap it tomorrow.

18th: Read the `StateEither` example of FP Complete's piece of monad transformers.

19th: I am getting a hang on monad transformers. The `StateEither`'s `Applicative` and `Monad` instances required no explicit usage of `State` functions, it just uses `return` and `>>=`, which all monads have. So really, many monads can wrap `Either`.

21st: Summary of the FP Complete's article so far:

    step 1: Observe that we can write foldTerminate whose fold f returns EitherWe can then write sumTillNegative, with negative case returning (Left acc)
    step 2: Avoid explicitly patterning matching on Either by using Either's monad and do notation.
    step 3: Notice that fold can be implemented with the State monad. So, foldTerminate can be implementedwith a State monad whose outputs were Either.
    step 4: We make a StateEither type and write the algebra instances; we write the helper methods
    step 5: We notice that it is possible to write the algebra instances with methods that all monads have (>>=, return).
    step 6: We make EitherT and write algebra instances and an MonadTrans instance for it; we write the helper methods
    step 7: The helper methods, like modify, do not need to be defined on Either. So we write modifyM that uses the polymorphic lift.

22nd: Exercise #1 on the FP Complete article

23rd: Took me 2 hrs to write this for the article's Exercise #2:
```haskell
foldTerminateM :: Monad m=> (b -> a -> m(Either b b)) -> b -> [a] -> m b

foldTerminateM f accum (x:xs) = fmap (either id id) $ runExceptT $ do
                                    b <- ExceptT $ f accum x
                                    lift $ foldTerminateM f b xs

foldTerminateM f accum [] = return accum
```
I was getting confused at first tackling this problem since I was **using the wrong "lifting action"**. Specifically, I was doing `return $ foldTerminateM f b xs` instead of `lift $ foldTerminateM f b xs`.

26th: Exercise 3. Going to take a detour to study [Lenses](https://www.fpcomplete.com/haskell/tutorial/lens/) since exercise 4 depends on it.

28th: 20 minutes into SJP on [Lenses](https://www.youtube.com/watch?v=k-QwBL9Dia0), implemented my own Bad Lens example.

29th: I decided to read some real Haskell code, think I've been in the ivory tower for too long! To start, I'm reading [2048 written in Haskell](https://github.com/8Gitbrix/2048Haskell) with the TUI library [bricks](https://github.com/jtdaugherty/brick). Two nuggets I learned thus far: 1) Use `forkIO` to start a background thread. 2) Use `uncurry` to apply a tuple as the first two args to a function. Overall, it's very surprising how easy it is to write 2048 with Haskell in a framework. `bricks` allows for a very declarative style of programming, where you predefined all your event handlers and drawing functions in one `App` object. When I'm experienced enough, I will read `bricks`.

30th: Started working on a tic-tac-toe clone, which I affectionately named `hic-hac-hoe`. I am using `bricks`, so this is TUI, and so I envision users moving around the highlight block with the arrow keys, pressing `enter` to make a move. I have already implemented a dummy cell with text "highlight", that moves around in the tic-tac-toe grid on arrow key inputs. The code is very similar to the 2048 haskell clone.

### December 2022

1st, 2nd:

<figure class="kg-card kg-image-card kg-card-hascaption"><img src=" __GHOST_URL__ /content/images/2022/12/demo.gif" class="kg-image" alt loading="lazy" width="600" height="338" srcset=" __GHOST_URL__ /content/images/2022/12/demo.gif 600w"><figcaption>Done!</figcaption></figure>

3rd: I started looking around for networking libraries. The author of `bricks` recommended `zeromq-haskell` to me, so I probably will look a lot into it, but it doesn't have much documentations since it is a binding.

4th: I browsed for multiplayer haskell games. Unfortunately, most of them use web server frameworks like `Yesod`, `scotty`. which I don't need since I just need some simple messaging relaying. I am going to follow haskell wiki's `Network.Socket` only "[implement a chat server](https://wiki.haskell.org/Implement_a_chat_server)" tutorial first, and then maybe dig into `zeromq` if I think I need more.

5th: I took a detour and did a thought experiment on how to model a call graph generator application in Haskell. I got stuck, so I [asked](https://softwareengineering.stackexchange.com/questions/442688/haskell-data-modeling-an-internal-app-that-interfaces-with-a-3rd-party-library). Also, I do not plan to study much Haskell in the coming 2 weeks. I have finals and research papers to read! During the winter break, however, I plan to basically Haskell every waking hour and also start working through [TAPL](https://www.cis.upenn.edu/~bcpierce/tapl/).

14th: I haven't touched FP in the last 10 days due to finals. However, I have learned in the last 10 days that becoming acquainted with PL jargons serves me better in the longer run than working through something specific like TAPL. So, during my winter break, I'll instead work with [PLP](https://www.cs.rochester.edu/~scott/pragmatics/) (Programming Language Pragmatics), which is, actually, the current textbook of my PL class.

25th: Merry Christmas! I began digging into `vty`, a `ncurses` alternative written in Haskell. `vty` is a precursor of `brick`, which I plan to dig into in the future.

### February 2023

7th, 8th: I've taken a huge break from FP due to school but now I'm back. I began digging into the Parsec library and learned the basics of Parsec via this [article](https://book.realworldhaskell.org/read/using-parsec.html). Since I eventually want to produce a code-base-deep-dive video on Parsec, I am reading earlier papers on Monadic Parser Combinators to gain a thorough understanding. I am 10 pages into Hutton's [work](https://www.cs.nott.ac.uk/~pszgmh/monparsing.pdf). The most important improvement that Parsec since made is better error handling and performance.

### Apr 2023
From Feb 8th to Apr 2nd: 
- Read [A History of Haskell: being lazy with class](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.168.4008&rep=rep1&type=pdf)
- Read [It's Easy As 1, 2, 3](https://www.cs.nott.ac.uk/~pszgmh/123.pdf)
    - Inspired me to read the Semantic Algebras chapter of [Denotational Semantics, A Methodology for Language Development](https://www.google.com/search?q=schmidt+denotational+semantics&oq=schimdt+denotational+semantics&aqs=chrome..69i57j33i10i160j33i299.6393j0j7&sourceid=chrome&ie=UTF-8&si=AMnBZoGh_HM3J_-U1hyC2-ep11yDw5aKTM8ASg3cFDT_bpkmN-nz5Mys77G29AQiHAXdbAd7TklX5aUP33W1kspyQShjpUO0COnYw2TFvAFB9cDkpqyU_igPXLp5DmM54gWv62A3829WebxaSMHIzi44AILQgKHgUcLtdgjqSa_Gtpqc0NXnW-3Z1XETi1Y3HKkzrB5rNFE4&ictx=1&ved=2ahUKEwjZuOfJsYz-AhVbE1kFHcvoC78QnZMFegQIWRAC)

- For my undergrad research on *Interpreting Language Models for Functional Programming*, I defined a taxonomy for Haskell (mapping Haskell grammar rules to general categories, e.g., `algebraic data types` belongs to the category `type system`). In defining this taxonomy, I came across many Haskell extensions that was complex for me to organize. Thankfully, I discovered [Thinking with Types, Type-Level Programming in Haskell](https://thinkingwithtypes.com/). I am only one chapter in, but I plan to finish this book.

Apr 3rd: A few pages of Thinking in Types (I abbreviate as TiT from now).
Fully applied (saturated) TypeClasses always end in `Constraint`, this single fact seems to be the missing piece for me to internalize the non-extended Haskell's type system. Previously, I learned from `HPFFP` that 
a type constructor like `Maybe` has kind `* -> *`, but I think I never
remembered these stuff well because I didn't internalize where TypeClasses fit in the whole scheme.

May 8th: Sorry! Another big skip. I didn't do much last month. Today, I reread
TiT Chapter 1, took notes, and did the exercises. The big idea I learned is that the Curry-Howard correspondence/isomorphism allows us to analyze mathematical theorems through the lens of functional programming. We get surprising results, for example, somehow, the power of power rule corresponds to currying.

May 9th: Reread TiT Ch2.1 and 2.2. This time around, I've cleared up many concepts, for example
- `(->)` is higher kind
- Exact difference between `TYPE` (`*`) and type
As last time, I got stuck when reading the admin token example. But I did more digging into the example and learned:
- `Proxy` is a type constructor whose only type variable is poly-kinded. That's how it's able to pass any type around.
- One usage of `forall` is to allow for type variables to show up in data constructors but not in their type constructors.
```haskell
data Worker x y = forall b. Buffer b => Worker { buffer :: b, input :: x, output :: y} 
```
- Scoped Type Variables allow free variables to be re-used in the scope of a function. 
```haskell
mkpair :: forall a b. a -> b -> (a, b)
mkpair aa bb = (ida aa, bb) 
    where 
      -- ida :: b -> b would error!
      ida :: a -> a -- Refers to "a" in the type signature.
      ida = id
```
Yet, I still don't understand the admin token example. I checked out TiT's source repo and it didn't have that part filled in. However, I did find more code around the TicTacToe example in Ch1 that taught me a few more things. I'll skip this example for now.

May 10th: Finishes TiT Ch2. Learned about how lists and tuples behave in type-level programming. I don't have the intuition down, but I kept querying `GHCi` to 
get practice. Here're some of my queries and results: 
```haskell
:set -XDataKinds

> :t [] 
[] :: [a]
> :k []
[] :: * -> *
> :k [Bool]
[Bool] :: *
> :k '[]
'[] :: [a]
> :t '[] -- throws error.
> :k '[ Bool ]
'[ Bool ] :: [*]
> :k '[ 'True ]
'[ 'True ] :: [Bool]
```
I stumbled half an hour into Ch3, couldn't understand much. This textbook is not beginner friendly.

May 11th: Finished TiT Ch3-Ch4.2. Confused about the following two statements from the Ch3, they seem conflicting with each other.
1) Variance is a property of a type in relation to one of its type constructors.
2) The variance of a type `T a` with respect to its type variable `a` is ...
By the definition of 1), it seems off to say 2). I skip this for now.
Coolest thing I learned is the positive/negative positioning trick to quickly determine a type's variance.

Then, I read the FPComplete's [Covariance and Contravariance](https://www.fpcomplete.com/blog/2016/11/covariance-contravariance/) article and learned the mappings between Haskell typeclasses and variance
(e.g., `Profunctor` is a bifunctor where the first argument is contravariant and the second argument is covariant). 

Onto Ch4 —
- I filled in a knowledge gap of `forall` from before. I knew that `forall` can be applied in ADT declaration. But when there're applied to functions, they moreso expand the type variables' scope to the entire function (in conjunction with `XScopedTypeVariables`).
- `XTypeApplications` allow us to apply types to polymorphic functions in the same way we can apply value arguments to functions. 

May 12th: Finished TiT Ch4.3-Ch5. Learned about how to write a heterogeneous list and that the GADTs syntax is just syntactic sugar upon type equalities. So, I challenged myself to write `HList` with the latter: 

```haskell
data HList (xs :: [Type]) 
    = HNil
    | forall t ts. (xs ~ (t ': ts)) => t (:#) HList ts
```
This allows for functions like 

```haskell
showBool :: HList '[_1, Bool, _2] -> String
```

Afterwards, I wrote the `Eq`, `Show`, and `Ord` instances for `HList` because GHC can't derive GADTs instances well. To do this efficiently, I also learned to write the closed type family `All` to fold `[TYPE]` into a `CONSTRAINT`. 

May 13th: Finished TiT Ch6-Ch6.3. 

Biggest takeaway: Universally quantified type variables are instantiated at the call site while existentially quantified type variables can be instantiated in the function definition. With existentially quantified type variables, we can take in a truly polymorphic functions, and then instantiate it if needed. 

May 18th: TiT Ch6.3

6.4 started talking about the Continuation Monad. I'm can't figure
out the essence despite knowing how to write the instances. I looked for other sources, finally landing on [this]((https://www.haskellforall.com/2012/12/the-continuation-monad.html)). The author suggests that to understand a Monad's purpose, I must study what its *Kleisli arrows* do, something I don't know much about.

So starting tomorrow, I'll take a detour from TiT and into [Category Theory 3.2: Kleisli category](https://www.youtube.com/watch?v=i9CU4CuHADQ) and the lectures before it. Then, I'll finish the article, and then back to TiT.

May 19th: Category Theory Playlist 1.1-2.1

I dulged in 2 hours of Category Theory lecture today. 

Bartosz Milewski is an excellent lecturer. I absorbed more about the epistemology of Category Theory and Physics more than Category Theory itself. I grasped the axioms of a category, understood an example category pertaining to programming, studied the category of sets, and learned about some relationships between sets and categories (isomorphism corresponds to function invertibility). 

I wonder if the power of type level programming follows: 

If I have a def that does: 


def false(): return False

The return type can only be annotated as "Bool", even though we know it can ever be only False.