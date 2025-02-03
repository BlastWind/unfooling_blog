---
layout: post
title: Verified Parser pt.1 - Vermillion, the Certified LL(1) Parser Generator
featured: false
date: "2025-02-03 02:01:00"
tags: formal-methods
---

Parsers come in two forms. In the first form, they are parser generators.
In the second, they are parser combinators. These two forms have very different backgrounds, but they are dual.

Researchers are interested in similar properties:
1. For parser generators, researchers verify that the parser generator is sound, complete, and unambiguous.
2. For parser combinators, researchers verify that the combinators are total, prefix-secure, and satisfy roundtrip (for binary parsing).

I start this review with an intro to formal methods, then I explain traditional parser generation. Finally, I dive into the source code and present
what the verification code looks like under the hood. In particular, I show how to prove that the [Vermillion](https://github.com/slasser/vermillion) LL(1) parser generator, written in Rocq, must not parse left-recursive grammars. 
In the next post, I explain parser combinators and show how the [Vest](https://github.com/secure-foundations/vest) library proves roundtrip for specific binary parser combinators.

This post is meant for everyone. If you have experience in Coq, you should be able to understand
the concept of abstract, inductive specification that I keep repeating in the verification section.
If you don't, just focus on getting the big ideas. But don't feel defeated if you don't understand the 
proofs themselves — they are mechanized according to complex inductive definitions. 
I, for one, don't understand them completely.

## Background: Formal Methods
In a simplified view, formal methods is about expanding your unit tests to cover the entire input domain. 

But how? 

It is computationally infeasible to run tests for every plausible input.
Instead, to reason about infinity, we have to be inductive.
The philosophy of induction is woven into the programming languages of formal methods
(most of which can be called proof assistants):

1. Structures are defined inductively.

This goes from natural numbers to lists to domain-specific
languages embedded within proof assistants. To elaborate, natural numbers are defined in
their Peano form: `0` is a `nat`, and given `n: nat`, `n + 1` is a nat. For lists,
`[1, 2, 3]` is represented as `Cons 1 (Cons 2 (Cons 3 Nil))`. As for embedded languages,
see the definition of `com` [here](https://softwarefoundations.cis.upenn.edu/lf-current/Imp.html#com).
You are probably more familiar with the latter since we know that code gets parsed into trees
that follow recursive grammar rules. But defining primitives inductively might be the bigger surprise.

2. Proofs can be inductive. 

When every structure in your proof is defined inductively, your proofs can make use of induction!

The other important part about formal methods to mention, in an introduction, is the concept of totality, which means "provably terminating" and "covers entire input domain", i.e., not partial.
Functions in Turing-complete languages, on the other hand, are partial. Haskell will give a warning
for functions not covering the entire input domain, but it's just a warning.

When I started learning about formal methods a few months ago, it took me 
a few days to realize that these "formal method languages" (e.g., Coq, Idris, Isabelle) are
adding a lot of constraints to programming languages.
That is to say, there exist programs in Turing-complete languages that you can't write in these "formal methods languages". 

But it took me a while to understand why, and why such constraints give power.
First, there's this thing called the Curry-Howard Correspondence that makes algebra, logic, and type theory isomorphic. Software folks will find type theory intuitive, but it is more logic and math flavored. 

The Curry-Howard Correspondence leads to the isomorphism between programs and proofs. 
In proof assistants, propositions are types, and proofs are terms (programs). [To prove a proposition, construct a proof program whose type is that proposition](https://softwarefoundations.cis.upenn.edu/lf-current/ProofObjects.html).

Here's the key: Proofs must be provably terminating. This makes sense - 
a proof that loops isn't a proof; a proof must terminate.
Since proofs are programs, programs are provably terminating as well. 
But if our programs are total, our language can't be Turing-complete, since Turing machines are allowed to go into the "loop" state!

So, by adding constraints (totality) to our language, we have made it more powerful (our language can prove things). Developers often first experience this in their encounter with Haskell's type system or Rust's borrow checker: You wage a series of mini-wars against the compiler. 
But when the linter goes green, you have many more guarantees about the correctness of your program.

> Structures, strictures, though they bind,
>
> Strangely liberate the mind.

## Background: Parsing
Parsing is the process of attempting to ingest a raw string into some structured format, according
to some rules. 
The structured format is often a tree, but it need not be. Even the act of lifting
the phone number `(123)-456-7899` into a Haskell record `PhoneNum { area: "123", mid: "456", rest: "7899" }`
is considered parsing.
But traditionally, parsing is focused on parsing programming languages, so the structured format
is a tree. I focus on traditional parsing in this post.

In traditional parsing, parsing follows the definition of a context-free grammar (CFG). A parser 
generator takes in the grammar and outputs a parser, which often comes in the form of some lookup table:
The parsing procedure is general and utilize the specific table.

A context-free grammar is a set of (recursive) generation rules, often written in the Backus-Naur form.
For example, here is the CFG for arithmetic:
```
E -> E + E
E -> E - E
E -> E * E
E -> E / E
E -> (E)
E -> Number
```

The leftmost elements are nonterminals. `E` is the only nonterminal. The right-hand side to the `->` is
called the gamma. Symbols that aren't on the left-hand side are terminals. For example,
`+`, `-`, and even `(` and `)` are terminals.

Each line naturally shows a production, a valid expansion for a nonterminal. You could choose to write
all of the productions for a nonterminal together:

```
E -> E + E | E - E | E * E | E / E | (E) | Number
```

Here is how we can expand an `E` into `3 + 5 + 8`:
```
E -> E + E
E -> E + E + E
E -> Number + Number + Number
E -> 3 + 5 + 8
```
Currently, to check if an input is expandable by the grammar, we mentally create heuristics to choose the
correct productions at each step. But a computer needs something more deterministic. In the ideal case,
the computer knows which production to expand into just by looking at the next input token and remembering 
the context we are in, i.e., which nonterminal we are parsing. This is called top-down, predictive parsing.

This ideal case is possible if our grammar is LL(1). Most parser generators support LR(1), or some stronger versions of it. 
LR(1) is strictly stronger than LL(1). But LL(1) is faster, choosing productions right away, as opposed to shifting 
and reducing in LR(1).

A grammar is LL(1) if it satisfies one property:
1. No productions for the same nonterminal have coinciding first terminals. 

`E` does not satisfy this property. For example, two productions for `E`, `E + E` and `E - E`,
both have `(` as their first terminal.

But, since productions can be nullable (written as `A -> epsilon`), there needs to be one additional criterion:
1. For any nullable nonterminal `A`, the set of candidate first terminals of `A`, denoted `FIRST(A)`, cannot 
intersect with `FOLLOW(A)`, which lists, from all productions, the first token of the symbols that follows `A`.

After computing FIRST and FOLLOW sets for each nonterminal (which can be done through a straightforward fixed-point algorithm), we can construct the parsing lookup table.
The lookup table is a map between `(nonterminal, lookahead token)` and a production, since the goal is to know which production to choose next.

For each production `A -> α`:
- For each terminal t in `FIRST(α)`, add the production to `table(A,t)`.
- If α is nullable, for each terminal t in `FOLLOW(A)`, add `A -> α` to `table(A,t)`

The construction of the `FIRST` and `FOLLOW` maps never throws.
The errors are handled as we add entries: If we try to add a production to a table cell that already contains one,
there is ambiguity. Hence, the grammar is not LL(1) and we throw an error.

During parsing, we maintain a stack of expected symbols (initialized with the start symbol) and an input stream of terminals. At each step:
- If the top of stack is a terminal, match it with input or error
- If top of stack is nonterminal A and next input is t, look up `table(A,t)`:
  - If empty, syntax error
  - If production A → α exists, pop A and push α (reversed) onto stack
- Repeat until stack is empty (accept) or error occurs

An LL(1) grammar is unambiguous: We know exactly which production to choose when we are expanding a nonterminal.
This implies that a string can only have one parse tree.
Our arithmetic language above doesn't satisfy this property;
we can expand `E + E` into `E + E + E` either through expanding the first `E` or the second, which results in 
two parse trees.

Unambiguity is important for programming languages: A program should have exactly one tree! For natural language,
it doesn't matter as much.

## Vermillion, the certified LL(1) parser generator
The general workflow of verifying a parser generator is composed of four parts:
1. Provide an inductive specification for definitions and behaviors in our parser
2. Implement the concrete fixpoints that constructs the parsing table  
3. Show the equivalence of the concrete implementation and the inductive specification
4. Prove properties about parsers. *These properties are exactly the verified surface of our parsers*.

Then, since the concrete implementation is 1-to-1 with the inductive specification, it implies that 
our concrete parsers satisfy the properties proved in 4.

I show real code for each part. You should digest a few of the inductive definitions 
but skim the proofs.

### Using Vermillion
Before going under the hood and studying the verification code, let's first look at how we can 
use the certified parser generator.

The input to the generator is a grammar encoded in Coq. Here's Grammar 3.11 from 
Appel's "Modern Compiler Implementation in ML":

```
S -> if E then S else S
S -> begin S L
S -> print E

L -> end
L -> ; S L

E -> num = num 
```

To encode this grammar, we must first supply the nonterminals, terminals, and the "semantic actions".
The semantic actions are mappings between symbols (nonterminals and terminals) and 
data types in Coq. For example, the terminal `Num` will map to the built-in type `nat`, 
and the nonterminal `S` (a sentence) will map to a `stmt` data type (`stmt` must be defined).
After parsing a string, the resulting tree nodes are semantic actions.

Here are the encodings of nonterminals, terminals, and semantic actions:
```coq
Inductive exp :=
| Cmp_exp : nat -> nat -> exp.

Inductive stmt :=
| If_stmt    : exp -> stmt -> stmt -> stmt
| Begin_stmt : list stmt -> stmt
| Print_stmt : exp -> stmt.

Inductive terminal' :=
| If | Then | Else | Begin | Print | End | Semi | Num | Eq.
  
Inductive nonterminal' :=
| S | L | E.

Definition t_semty (a : terminal) : Type :=
  match a with
  | Num => nat
  | _   => unit
  end.

Definition nt_semty (x : nonterminal) : Type :=
  match x with
  | S => stmt
  | L => list stmt
  | E => exp
  end.
```

Note that `stmt` has three constructors, which corresponds to the number of productions `S` has.

So far, we have only defined the elements of the grammar; we have not yet encoded the productions:
```coq
Definition g311 : grammar :=
  {| start := S ;
     prods := [existT action_ty
                      (S, [T If; NT E; T Then; NT S; T Else; NT S])
                      (fun tup =>
                         match tup with
                         | (_, (e, (_, (s1, (_, (s2, _)))))) =>
                           If_stmt e s1 s2
                         end);
                 
               existT action_ty
                      (S, [T Begin; NT S; NT L])
                      (fun tup =>
                         match tup with
                         | (_, (s, (ss, _))) =>
                           Begin_stmt (s :: ss)
                         end);

               existT action_ty
                      (S, [T Print; NT E])
                      (fun tup =>
                         match tup with
                         | (_, (e, _)) =>
                           Print_stmt e
                         end);
               ... you get the idea
  ]|}
```

The 3.11 grammar has no nullable productions. If you have a nullable production, it will look like:
```coq
existT action_ty
      (A, []) (* Empty list! *)
      (fun tup =>
          match tup with
          | _ => AStruct
          end);
```
Check out this [example](https://github.com/BlastWind/vermillion/blob/master/MyExample.v) in my Vermillion fork where I encode
the following grammar, which has nullable productions:
```
E  -> TE'
E' -> +TE' | epsilon
T  -> FT'
T' -> *FT' | epsilon
F  -> (E)  | Num
```

Now, we can use the parser generator to build a parser (more precisely, the parsing tables),
and then invoke the parse procedure (with the tables) on our inputs.

```coq
(* tt is the unit type *)
Definition example_prog : list token :=
  [tok If tt; tok Num 2; tok Eq tt; tok Num 5; tok Then tt;
     tok Print tt; tok Num 2; tok Eq tt; tok Num 5;
   tok Else tt;
     tok Print tt; tok Num 42; tok Eq tt; tok Num 42].

Compute (match parseTableOf g311 with
         | inl msg => inl msg
         | inr tbl => inr (parse tbl (NT S) example_prog)
         end).

(* You will see a tree made up of semantic action nodes! *)
```

If your grammar is not LL(1), then it will be rejected at `parseTableOf`. If your input
is not recognizable by the parser, then it will give an `inl` error message within `parse`.

### 1. Inductive specifications
Let's start light: Recall, what does it mean for a symbol to be nullable?

A symbol is nullable if it has a production that can go to epsilon. For example, `A` is nullable:
```
A -> b | c | d | epsilon
```
but `A` is also nullable here:
```
A -> B | c
B -> d | epsilon
```

In Coq, we translate the natural language description of "a production can go to epsilon" as:
```coq
Inductive nullable_sym (g : grammar) : symbol -> Prop :=
| NullableSym : forall x ys f,
    In (existT _ (x, ys) f) g.(prods)
    -> nullable_gamma g ys
    -> nullable_sym g (NT x)
with nullable_gamma (g : grammar) : list symbol -> Prop :=
      | NullableNil  : nullable_gamma g []
      | NullableCons : forall hd tl,
          nullable_sym g hd
          -> nullable_gamma g tl
          -> nullable_gamma g (hd :: tl).
```

You are looking at the definition of an inductive proposition. This defines how to
create proofs of a symbol `s` to be a `nullable_sym`: You must supply a nullable gamma `ys`
where `(s, ys)` is a valid pairing in the grammar's production.

Mutually, `nullable_gamma` must be defined: An empty production is trivially a `nullable_gamma`
through the `NullableNil` constructor. More interestingly, a gamma's nullability can be determined by 
the nullability of its head and tail. In summary, the `nullable_sym` inductive prop allows us to "choose
a gamma" to show nullability from, and the `nullable_gamma` inductive prop allows us to break up the 
symbols in the gamma and recurse into `nullable_sym`.

Similar specifications are defined for what it means to be a `first_sym`, a `follow_sym`, then
a `lookahead_for`, and at last culminating in `sys_derives_prefix`:
```coq
    Inductive sym_derives_prefix (g : grammar) : Prop :=  
    | T_sdp :
          (* A terminal can derive an element of the corresponding type (determined with t_semty), *)
          sym_derives_prefix g (T a) [existT _ a v] v r
    | NT_sdp :
        (* A nonterminal can derive a sequence of tokens if:
           1) There exists a production rule for this nonterminal in the grammar
           2) The next token in the input stream is a valid lookahead for this production
           3) The right-hand side of the production (gamma) can derive the token sequence
           The semantic value is computed by applying the production's action to the derived values *)
        In (existT _ (x, gamma) f) g.(prods) (* Multiple productions allowed *)
        -> lookahead_for (peek (w ++ r)) x gamma g
        -> gamma_derives_prefix g gamma w vs r
        -> sym_derives_prefix g (NT x) w (f vs) r
    with gamma_derives_prefix (g : grammar) : Prop :=
         | Nil_gdp :
             (* An empty sequence of symbols derives an empty sequence of tokens *)
             gamma_derives_prefix g [] [] tt r
         | Cons_gdp :
             (* A sequence of symbols derives a token sequence if:
                1) The first symbol derives some prefix of tokens
                2) The rest of the symbols derive the remaining tokens
                The semantic values are paired together as we go *)
             sym_derives_prefix g s wpre v (wsuf ++ r)
             -> gamma_derives_prefix g ss wsuf vs r
             -> gamma_derives_prefix g (s :: ss) (wpre ++ wsuf) (v, vs) r.
```
I omitted the parameters to save space. 
It might be better to see the git diff [view](https://github.com/slasser/vermillion/compare/master...BlastWind:vermillion:master?expand=1)
between my fork and original; I have the same comments there.

`sym_derives_prefix g s w v r` means:
Symbol `s` can derive a sequence of tokens `w` (producing semantic value `v`)
with any remaining tokens `r`, all
according to grammar `g`.

The definition `sym_derives_prefix` is used, for example, in `Corollary LL1_derivation_deterministic`.
Determinism is encoded as: If we can fathom two derivations, i.e., 
`sym_derives_prefix g s w  v  r` and `sym_derives_prefix g s w' v' r'`, then actually,
`w = w' /\ r = r'`.

### 2. The table construction algorithm 
The table construction algorithm is implemented as a series of Rocq fixpoints (total functions). Each fixpoint computation builds a different set/map needed for the final parse table.
These are essentially the Coq implementations of the LL(1) table construction algorithm as described in the parsing background section.
1. `NULLABLE` set (via `mkNullableSet`)

In Vermillion, the FIRST map cannot contain epsilons, instead, emptiness is tracked with a separate, `NULLABLE` set.

```coq
Program Fixpoint mkNullableSet' 
        (ps : list production) 
        (nu : NtSet.t)
        { measure (countNullCands ps nu) } :=
  let nu' := nullablePass ps nu in
  if NtSet.eq_dec nu nu' then
    nu
  else
    mkNullableSet' ps nu'.
```
Each `nullablePass` adds nonterminals to the set if their productions can be nullable. The measure `countNullCands` ensures termination by counting how many candidates remain.

2. `FIRST`, `FOLLOW` map

Similarly, the `FIRST` and `FOLLOW` maps are constructed by individual passes until we reach the fixpoint. We 
keep a count on the remaining candidates to determine the fixpoint.

3. Parsing table
Using the `NULLABLE` set, the `FIRST` and `FOLLOW` maps, we compute the list of entries to add. 
Then, we fold over the list of entries, adding them one by one to the table, throwing
when there are duplicates (by returning an `inl`).

```coq
Definition mkParseTable (ps : list table_entry) :
  Datatypes.sum string parse_table :=
  fold_right addEntry (inr empty_table) ps. (* addEntry throws when we encounter duplicate! *)

Definition parseTableOf (g : grammar) : Datatypes.sum string parse_table :=
  match findDup _ base_production_eq_dec (baseProductions g) with
  | Some b => inl (dupMessage b)
  | None   => 
  let nu    := mkNullableSet g in
    let nu_pf := mkNullableSet_correct g in
    let fi    := mkFirstMap g nu in
    let fi_pf := mkFirstMap_correct g nu nu_pf in
    let fo    := mkFollowMap g nu fi fi_pf in
    let es    := mkEntries nu fi fo g in
    mkParseTable es
  end.
```

### 3. Table construction algorithm makes correct parse tables
And now, we bridge the implementation with the specification. The bridging theorems are `parseTableOf_sound` and `parseTableOf_complete`:

```coq
Theorem parseTableOf_sound : 
  forall (g : grammar) (tbl : parse_table),
    parseTableOf g = inr tbl
    -> parse_table_correct tbl g.

Theorem parseTableOf_complete :
  forall (g : grammar) (tbl : parse_table),
    unique_productions g
    -> parse_table_correct tbl g 
    -> exists (tbl' : parse_table),
      ParseTable.Equal tbl tbl' /\ parseTableOf g = inr tbl'.
```

The first says: If our table construction fixpoint successfully makes a table,
then that table satisfies `parse_table_correct`, meaning:
1. (Soundness) If there is a nonterminal and lookahead `(nt, la)` pair that finds a certain `gamma` in the table, then `nt -> gamma` must be a
production in the grammar with `la` as the `lookahead_for` (inductively defined) `nt` in the `gamma` production.
2. (Completeness) For all `nonterminal -> gamma` productions, if a lookahead `la` is the `lookahead_for` (inductively defined) for the `gamma`, then 
`(nonterminal, la)` is in the table. 

Conversely, if there exists a table satisfying `parse_table_correct`, then there exists a grammar that makes this exact table under `parseTableOf`.

How can these be proved? Correctness is defined as soundness + completeness, let's zoom in on completeness:

```coq
Lemma mkParseTable_complete :
  forall (es  : list table_entry)
          (g   : grammar)
          (tbl : parse_table),
    unique_productions g
    -> entries_correct es g
    -> parse_table_correct tbl g
    -> exists tbl',
        ParseTable.Equal tbl tbl'
        /\ mkParseTable es = inr tbl'.

Theorem parseTableOf_complete :
  forall (g : grammar) (tbl : parse_table),
    unique_productions g
    -> parse_table_correct tbl g 
    -> exists (tbl' : parse_table),
      ParseTable.Equal tbl tbl' /\ parseTableOf g = inr tbl'.
Proof.
  intros g tbl Hu Ht.
  unfold unique_productions in Hu; unfold parseTableOf.
  pose proof Hu as Hu'; eapply NoDup_findDup_None_iff in Hu'; rewrite Hu'.
  eapply mkParseTable_complete; eauto.
  eapply mkEntries_correct; eauto.
  - apply mkNullableSet_correct; auto.
  - apply mkFirstMap_correct.
    apply mkNullableSet_correct; auto.
  - apply mkFollowMap_correct; auto.
    apply mkNullableSet_correct; auto.
Qed.
```

The completeness of the final parse table is encoded in `parseTableOf_complete`. `parseTableOf_complete` depends on `mkParseTable_complete`,
which reduces the goal of showing completeness of `parseTableOf` (constructed by adding entries individually and throwing on duplicates)
to showing `mkEntries_correct`.

This reduction is possible due to this important, bridging lemma:
```coq
Lemma invariant_iff_parse_table_correct :
  forall (g : grammar) (es : list table_entry) (tbl : parse_table),
    entries_correct es g
    -> table_correct_wrt_entries tbl es
        <-> parse_table_correct tbl g.
```

To show `mkEntries_correct`, we ought to show each construction procedure for each substructure is correct.
The key to these proofs is well-founded induction on the number of candidates. For example, 
on each `nullablePass` (implementational), the number of null candidates decreases, 
which coincides with the null candidates increasing in an inductively built nullable.

I don't fully understand the mechanics of these proofs — there are pieces I haven't explained,
but I hope this gives a big enough picture.

### 4. Proving properties about parsers
I present the theorem `LL1_parse_table_impl_no_left_recursion` and its proof.
The theorem is written with inductive definitions. 
But because we have connected the implementational `parseTableOf`
to the inductive definitions, our real algorithms will also enjoy `LL1_parse_table_impl_no_left_recursion`.

Here is the location of the proof in the grand proof tree:

```
└── LL1_derivation_deterministic 
    (Unambiguity)
    ├── parse_terminates_without_error 
    │   └── LL1_parse_table_impl_no_left_recursion <--------------------------------------------- We are here!
    |       (Proves that a correct LL(1) parse table cannot have left recursion)
    │       ├── sized_first_sym_det 
    |           (Shows that the size of first set derivations is deterministic)
    │           └── first_sym_rhs_eqs 
    |               (Proves that if two productions for same nonterminal can derive same first token, they must be identical)
    │               └── no_first_follow_conflict 
    |                   (A symbol cannot have same token in both FIRST and FOLLOW sets if nullable)
```

And here is the theorem and proof:

```coq
Lemma LL1_parse_table_impl_no_left_recursion :
  forall (g : grammar) (tbl : parse_table) 
          (x : nonterminal) (la : lookahead),
    parse_table_correct tbl g
    -> ~ left_recursive g (NT x) la.
```
where `left_recursive` is defined as an inductive proposition:
```coq
Inductive nullable_path (g : grammar) (la : lookahead) :
  symbol -> symbol -> Prop :=
| DirectPath : forall x z gamma f pre suf,
    In (existT _ (x, gamma) f) g.(prods)
    -> gamma = pre ++ NT z :: suf
    -> nullable_gamma g pre
    -> lookahead_for la x gamma g
    -> nullable_path g la (NT x) (NT z)
| IndirectPath : forall x y z gamma f pre suf,
    In (existT _ (x, gamma) f) g.(prods)
    -> gamma = pre ++ NT y :: suf
    -> nullable_gamma g pre
    -> lookahead_for la x gamma g
    -> nullable_path g la (NT y) (NT z)
    -> nullable_path g la (NT x) (NT z).

Definition left_recursive g sym la :=
  nullable_path g la sym sym.
```

For example, in
```
A → ε B c
B → ε C d 
C → e
```

there is a direct nullable path from `A` to `B` and from `B` to `C`. Therefore, transitively, there
is an indirect path from `A` to `C`.

The proof works by contradiction. It assumes left recursion exists and derives a contradiction in two cases.
There are two cases because left recursion can be built with `DirectPath` and `IndirectPath`.

```coq
Proof.
  intros g t x la Ht; unfold not; intros Hlr; red in Hlr.
  (* Get the production that starts the left recursion *)
  assert (Hex : exists gamma f,
             In (existT _ (x, gamma) f) g.(prods)
             /\ lookahead_for la x gamma g).
  { apply nullable_path_exists_production in Hlr; auto. }
  destruct Hex as [gamma [f [Hi Hl]]].
  
  (* Split into two cases based on how the lookahead is derived *)
  destruct Hl as [Hfg | [Hng Hfo]].

  ... to be continued
```

For both cases, the general approach is to say that the length of the nullable path must strictly decrease
as we go down the path. 

For example, in
```
A --nullable-path--> B --nullable-path--> C
```
`A`'s nullable path is 3, `B`'s is 2, and `C`'s is 1.

But since we have left recursion, the path length doesn't decrease once we get back to the same nonterminal, so we 
contradict this:

```coq
  (* Case 1: DirectPath *)
  - assert (Hf : first_sym g la (NT x)) by (inv Hfg; eauto).
    eapply sized_first_sym_np in Hf; eauto.
    destruct Hf as [n [n' [Hf [Hf' Hlt]]]].
    eapply sized_first_sym_det in Hf; eauto.
    lia.

  (* Case 2: IndirectPath *)  
  - assert (Hns : nullable_sym g (NT x)) by eauto.
    eapply exist_decreasing_nullable_sym_sizes_in_null_path in Hns; eauto.
    destruct Hns as [n [n' [Hs [Hs' Hlt]]]].
    eapply sized_nullable_sym_det in Hs; eauto.
    lia.
```

For both branches, before the final `lia`, there are these two hypotheses in the proof state:
```coq
Hlt : n' < n
Hf : n' = n
```

`lia` automatically observes a contradiction, solving our goal.