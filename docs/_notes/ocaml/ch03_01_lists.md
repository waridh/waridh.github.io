---
title: "CS3110: 03.01 Lists"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

Conceptually, we know what a list is, but how is it done is OCaml? Lists in OCaml are implemented as a singly linked list. These list enjoy first class status in the language. There are a lot of macro supports for lists in OCaml, so it is easy to use.

## 3.1.1 Building a List

There are three syntactic forms for building a list in OCaml:

```OCaml
[]
e1 :: e2
[e1; e2; ...; en]
```

`[]` is an empty list, and it is pronounced `nil`.  Given a list `lst` and an element `elt`, we can prepend the list by writing `elt :: lst`. The `::` is called a cons, and it stands for constructor. Only the first element of the list is called the head, the rest are called the tail.

Another cool thing is that the following are equivalent:

```OCaml
[e1; e2; ...; en]
e1 :: e2 :: ... :: en :: []
```

Now since the element of the list is actually arbitrary, then we could actually nest the list as deep as we like. e.g., `[[[]]; [[1; 2; 3]]]`.

### Dynamic semantics

- [] is already a value
- If `e1` evaluates to value `v1` and if `e2` evaluates to value `v2`, then `e1 :: e2` evaluates to `v1 :: v2`.

Through some mathematical derivation, we end up with the rule:

- If `ei` evaluates to `vi` for all `i`, then `[e1; ...; ei]` evaluates to `[v1; ...; vi]`.

We are now sick of saying evaluates to, we instead we are going to be using a new notation: `==>`. This is not OCaml syntax, Cornell just decided that they did not want to write boiler plate text.

- If `e1 ==> v1` and if `e2 ==> v2` then `e1 :: e2 ==> v1 :: v2`.
- If `ei ==> vi` for all `i`, then `[e1; ...; ei] ==> [v1; ...; vi]`.

### Static Semantics

All the elements in a list must have the same type. If an element type is `t`, then the type of the list is `t list`. Types in lists should be read from right to left, so a list of list of `t`'s would be `t list list`. `list` isn't a type. but instead a type constructor. It takes in a type and generate a new type. It is like a function that operates on a type instead of a value.

Type-checking rules:

- `[] : 'a list`
- If `e1 : t` and `e2 : t list` then `e1 :: e2 : t list`.

On that `nil`, the compiler will assume generic until something is passed into in, in which the type of the list will be defined.

## 3.1.2 Accessing a list

You can really only build a list with either a `nil` or a `cons`. If we want to look inside a list, we actually also have to define what happens on empty content, and if it's non-empty. This is *pattern matching* and we see it in Rust as well.

Here is an example of pattern matching used to get the sum of a list:

```OCaml
let rec sum lst =
    match lst with
    | [] -> 0
    | h :: t -> h + sum t

val sum : int list -> int = <fun>
```

As you can see here, iterating through a list was made recursive. Let `h` be the head element and `t` be the tail element. Other names could be use, but the underlaying principle of matching still applies.

Actually, just to note, we never need newlines when defining these functions, they were just added for better readability. Second point is that the first `|` is optional. A faster way to write this function would be the following:

```OCaml
let rec sum lst = match lst with [] -> 0 | h :: t -> h + sum t
```

Here is an example of writing a function that calculates the length of a list:

```OCaml
let rec length lst = match lst with [] -> 0 | h :: t -> 1 + length t
```

```OCaml
val length : 'a list -> int = <fun>
```

Actually, if we are not going to be using the value, but still show it's presence in pattern matching, you can use `_`.

```OCaml
let rec length lst = match lst with [] -> 0 | _ :: t -> 1 + length t
```

What is nice here is that OCaml has a built in library with a lot of these functions already. Finding the length of a list can be done using the function `List.length`.

Another list function example:

```OCaml
let rec append lst1 lst2 =
    match lst1 with [] -> lst2 | h :: t -> h :: append t lst2
```

```OCaml
val append : 'a list -> 'a list -> 'a list = <fun>
```

Learning how to create this function is useful, but like before, the function has already been implemented by OCaml, and it is the operator `@`. Here is an example of the usage:

```OCaml
lst1 @ lst2
```

Here is another example of examining if a list is empty:

```OCaml
let empty lst = match lst with [] -> true | h :: t -> false
```

```OCaml
val empty : 'a list -> bool = <fun>
```

It's not even a recursive function because the checking method is simple and does not need traversal.

Another thing to think about is that induction and recursion are very similar. In induction with natural numbers, we start with the base case of 0, and an inductive case of $n + 1$. In recursion, we have a base case of 0/[], and a recursive case of non-empty.

Back to the topic of lists, there are two library functions for getting the head and getting the tail of a list, `List.hd` and `List.tl`. This issue with these functions is that they will return an error when used on an empty list. Unlike Rust, the compiler will not argue with you when there are some cases that are not handled.

## 3.1.3 (Not) Mutating Lists

Something big to note is that lists are actually immutable and OCaml programmers will have to just consume and return a new list every time that they want to change it. Here is a function that will return the same list, but with the first element incremented by one.

```OCaml
let inc_first lst = match lst with [] -> [] | h :: t -> h + 1 :: t
```

So we are actually returning a new list every time we want to change a pre-existing list. The good thing about the OCaml compiler is that it has smart people working on it, so it will not be wasteful when dealing with cases like these. Instead of copying the whole list to change one element, it will share the tail list with the original list while changing the head.

## 3.1.4 List pattern matching

Starting by looking closer at pattern matching:

```OCaml
match e with
| p1 -> e1
| p2 -> e2
| ...
| pn -> en
```

Each `| pi -> ei` is called a branch or case for a pattern match. In our current knowledge, `pi` could be:
- A variable
- An underscore `_` which is called a wildcard.
- `[]`
- `p1 :: p2`
- `[p1; ...; pn]`

No variable name can appear more than once in a pattern match e.g., `x :: x` cannot be used.

### Dynamic Semantics with Pattern Matching

Pattern matching is made up of two inter-related tasks. The first being that matching, where you are trying to find patterns with the same shapes. The second is about the variable binding introduced by the pattern.

For example:

```OCaml
match 1 :: [] with [] -> false | h :: t -> h >= 1 && List.length t = 0
- : bool = true
```

If we are looking at the second branch, we see that `h -> 1` and `t -> []`. So to further dig deeper into the semantics of how match works, we can try:

- The pattern `x` matches any value `v` and produces a binding `x -> v`.
- The pattern `_` matches any value and produces no bindings.
- The pattern `[]` matches the value `[]` and produce no binding.
- If `p1` matches `v1` and produces a set of bindings $b_1$ and `p2` matches `v2` and produces a set of binding $b_2$ then `p1 :: p2` matches `v1 :: v2` and produces the set $b_1 \cup b_2$. For this to work, `v2` has to be a list since it's on the right hand side of `::`. The union of $b_1$ and $b_2$ will never have an issue of a repeating element because you cannot use the same names in match.
- If for all `i` in `1..n`, it holds that `pi` matches `vi` then `[p1; ...; pn]` matches with `[v1; ...; vn]` and produces the set $\bigcup_i b_i$ of bindings.

Alright, so let's analyse the mechanics of matching with the following example:

```OCaml
match e with p1 -> e1 | ... | pn -> en
```

- Evaluate `e` to `v`
- Match `v` with all `pi` in the order that they appear in the match.
- If `v` does not match with any `pi` then raise a `Match_failure` exception.
- Stop trying to match on a match, and let `pi` be the pattern and $b_i$ be the variable binding produced by matching `v` with `pi`.
- Substitute that binding inside of `ei` and produce the new expression `e'`.
- Evaluate the expression `e'` to a value `v'`.
- `v'` is the result of the match expression.

### Static Semantics

- If `e : ta` and for all `i`, it holds that `pi : ta` and `ei : tb` then `(match e with p1 -> e1 | ... | pn -> en) : tb`.

### Additional Static Checking

Other than the type checking rule, the compiler will also check for two other rules:

1. **Exhaustiveness:** Making sure that there isn't an unhandled case.
2. **Unused Branched:** The compiler makes sure that there isn't a pointless case being programed.

## 3.1.5 Deep Pattern Matching

Patterns can be nested, so you can do some more logical checks that gets you nice answers:

- `_ :: []`. Matches all lists with exactly one element.
- `_ :: _`.  Matches all lists with at least one elements.
- `_ :: _ :: []`. Matches lists with exactly two elements.
- `_ :: _ :: _ :: _`. Matches lists with at least three elements.

## 3.1.6 Immediate Matches

So some sugar coding that OCaml has for when you are immediately pattern matching against the final arguments. Instead of doing the the following:

```OCaml
let rec sum lst = 
match lst with [] -> 0 | h :: t -> h + sum t
```

You can write the following instead:

```OCaml
let rec sum = function [] -> 0 | h :: t -> h + sum t
```

## 3.1.7 OCamldoc and List syntax

Learning how to document with lists would also be a nice little addition to our skillset, so here it is:

```OCaml
(** [hd lst] returns the first element of the [lst].
    Raises: [Failure "hd"] if [lst = []]. *)
```

## 3.1.8 List Comprehensions

OCaml does not need this feature, so we don't need to talk about it. Higher order programming takes care of that. This feature will be talked about later.

## 3.1.9 Tail Recursion

So how do we use tail recursion with list and matching? Here is two examples:

```OCaml
let rec sum (l : int list) : int = 
match l with
| [] -> 0
| x :: xs -> x + (sum xs)

let rec sum_plus_acc (acc : int) (l : int list) : int =
match l with
| [] -> acc
| x :: xs -> sum_plus_acc (acc + x) xs

let sum_plus : int list -> int = sum_plus_acc 0
```

```OCaml
val sum : int list -> int = <fun>
val sum_plus_acc : int -> int list -> int = <fun>
val sum_plus : int list -> int = <fun>
```

When the function is going to work on a really long list, we should design it to be tail recursive, so that we don't have a large space complexity.

*Note: Tail recursion is not strictly better than the regular implementation, especially when working with functions that deal with smaller sizes as the overhead associated with specifically reversing a list.*

Here is an example of a function that will create a list of an arbitrary size:

```OCaml
(** [from i j l] is the list containing the integers from [i] to [j],
    inclusive, followed by the list [l].
    Example:  [from 1 3 [0] = [1; 2; 3; 0]] *)
let rec from i j l = if i > j then l else from i (j - 1) (j :: l)

(** [i -- j] is the list containing the integers from [i] to [j], inclusive. *)
let ( -- ) i j = from i j []

let long_list = 0 -- 1_000_000
```

What we should understand here is that it is tail recursive, and the design of OCaml actually makes use create the list from the last element backwards. It will grow from the `1_000_001` element and back to the first.

But also, if you want to do something like this, there is actually a library code that already does the same thing:

```OCaml
List.init 1_000_000 Fun.id
```

What this expression does is that it takes `List.init len f` and create a list of length `len` and the elements being:

```OCaml
[f 0; f 1; ...; f (len - 1)]
```

And this is still all done tail recursively.

| Previous | [Home](ch03_00_data_and_types.md) | [Next](ch03_02_variants.md) | 
| -------- | --------------------------------- | --------------------------- |
