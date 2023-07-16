---
title: "CS3110: 03.08 and Onwards"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

## 3.8 Association List

A simple implementation of maps on OCaml. It is a list of tuple pairs.

```
let d = [("rectangle", 4); ("nonagon", 9); ("icosagon", 20)]
val d : (string * int) list =
[("rectangle", 4); ("nonagon", 9); ("icosagon", 20)]
```

Since this is not a built in type, the programmer needs to implement insertion and lookup themselves. Here is an example of how that can be done:

```
(** [insert k v lst] is an association list that binds key [k] and 
value [v].*)
let insert k v lst = (k, v) :: lst

(** [lookup k lst] is a lookup function that looks for key [k] in
  association list [lst]. It will then return the value [Some v]
  if the key is found and [None] if there is no corresponding key
  value pair. *)
let rec lookup k = function [] -> None | (k', v) :: t ->
  if k = k' then Some v else lookup k t
```

These are somewhat basic implementations since there isn't checks on if the value already exists and the lookup will only return the most recent one. This implementation makes insertion constant time, and lookup being in linear time.

## 3.9 Algebraic Data Types

**Variants** can do more than just act as `enum`. Variants can carry **data**. Here is an example:

```
type point = float * float
type shape = Point of point
  | Circle of point * float (* center and radius *)
  | Rect of point * point (* upper left and lower right corners *)
```

You just need to do pattern matching to utilize the Algebraic Data Types. For example:

```
let area = function
| Point _ -> 0.0
| Circle (_, r) -> Float.pi *. (r *. r)
| Rect ((x1, y1), (x2, y2)) ->
    let w = x2 -. x1 in
    let h = y1 -. y2 in
    w *. h

let center = function
| Point p -> p
| Circle (p, _) -> p
| Rect ((x1, y1), (x2, y2)) ->
    ((x1 +. x2) /. 2. , (y1 +. y2) /. 2)
```

Basically, this is a way of representing a union of multiple data type in a type-safe way.

An important use case of this feature is allowing the built in `list` type to have more than one types in it by making a Variant. Do note that you will have to make a `match` to handle all the cases that are available here.

#### Syntax

Here is the updated syntax for **Variants**:

```
type t = C1 [of t1] | C2 [of t2] | ... | Cn [of tn]
```

Where the words in the square brackets denote optional parameters.

With the new optional data arguments, Variants work very much like option, in that it is now a constructor. You will have to call it like the following example:

```
C1 e
```

when there is a data field attached to it.

#### Dynamic Semantics

- If `e ==> v` then `C e ==> C v`, assuming that `C` is not a constant.
- `c` is already a value if it is a constant.

#### Static Semantics

- If `t = ... | C | ...` then `C : t`
- If `t = ... | C of t' | ...` and if `e : t'` then `C e : t`

#### Pattern matching

New pattern became available for us to use:

```
C p
```

The new definition that has been added for binding is that:

- if `p` matches `v` and produce binding $$b$$, then `C p` matches `C v` and produce binding $$b$$.

### 3.9.1 Catch All Case

Sometimes, you do not want to specify a different pattern matching case for all the possible Variants, so we use a catch all case to grab the rest of the possible cases. In language like Rust, they force the user to do this for all `match` statements. Here is an example:

```
type color = Blue | Red | Green

(* Some code between *)

let string_of_color = function
  | Blue -> "blue"
  | _ -> "red"
```

There is a place for this type of catch all cases, but it might not be robust against future update, and thus not as extensible.

### 3.9.4 Recursive Variants

The Algebraic Data Types that are created here can recursively call itself, or be in *mutual recursion* with another variant. This lets you create data types that are similar to lists, and tuples. Here are some example of creating these variants:

```
type intlist = Nil | Cons of int * intlist

let lst3 = Cons (3, Nil) (* similar to 3 :: [] *)
let lst123 = Cons (1, Cons (2, lst3)) (* similar to 1 :: 2 :: [3]*)

let rec sum (l : intlist) : int =
  match l with
  | Nil -> 0
  | Cons (i, lst) -> i + sum lst

let rec length : intlist -> int = function
  | Nil -> 0
  | Cons (_, lst) -> 1 + length lst

let empty : intlist -> bool = function
  | Nil -> true
  | Cons _ -> false

(* Mutually recursive calls*)
type node = {value : int; next : mylist}
type mylist = Nil : Node of node
```

All mutual recursions need either a variant or a record to "go through" or else it is invalid. Just another note, **records** can also be recursive like variants.

### 3.9.5 Parameterized Variants

You can further generalize the variants to take on generics. This can be seen in the following example:

```
type a' mylist = Nil | Cons of a' * a' mylist
let lst3 = Cons (3, Nil) (* Like [3] *)
let lsth = Cons ('h', Nil) (* Like ['h'] *)
```

Some functions do not change in the way that they are written, such as `length` or `empty`, while other functions that interacts with the element of the list will convert the variant into an actual type.

```
let rec length = function
| Nil -> 0
| Cons (_, t) -> 1 + length t

let rec empty = function
| Nil -> true
| Cons _ -> false

(* The following function will be converted into an [int] form*)
let rec sum : int mylist -> int =
| Nil -> 0
| Cons (h, t) -> h + sum t
```

### 3.9.6 Polymorphic Variants

Functionally, it works like an *anonymous variant*, where there is no need to declare the variants beforehand. This is useful for function outputs that do not get used again often. The output still must be handled using match like a regular variant output, but there is less boilerplate. Here are examples to demonstrate why it's useful:

```
type fin_or_inf = Infinity | Finite of int
let f = function
| 0 -> Infinity
| 1 -> Finite 1
| n -> Finite (-n)

(* Rewriting without needing to declare the variants beforehand *)
let f = function
| 0 -> `Infinity
| 1 -> `Finite 1
| n -> `Finite (-n)
val f : int -> [> `Finite of int | `Infinity] = <fun>
```

`[>]` represents the set of constructors that must be handled.

### 3.9.7 Built-in Variants

Just like Rust, we have the `option` variant, but the linked list present `list` is also a variant.

## 3.10 Exceptions

Just like variants, but used for exceptions. The syntax is identical, here are some example:

```
exception e of t
```

Where `e` is the constructor and `t` is the type. Just like variants, the data is optional.

The syntax for creating an exception is also the same as variants:

```
Failure "Something went wrong"
```

Start with a constructor name and follow it up with the data value that is being included. To raise an exception, just use the `raise` function:

```
raise e
```

Where `e` is an exception type. Now since you can raise exceptions, you can also catch it using the following syntax:

```
try e with
| p1 -> e1
| ...
| pn -> en
```

So if `e` raises an exception, the `try` will do some pattern matching to catch it, but if it does not, then the entire statement will evaluate to the value that `e` does.

Really, `exceptions` are just extensible variants, which just means it is a variant that has already been defined, but new definitions can be added to it after the original declaration.

### 3.10.2 Semantics

The semantics for raising an exception is not different from regular evaluation, so please to refer to those or the 3110 page.

### 3.10.3 Pattern Matching

There are two ways to integrate exceptions with pattern matching. Here are the two ways, the examples are self explanatory:

```
match List.hd [] with
  | [] -> "empty"
  | _ :: _ -> "non-empty"
  | exception (Failure s) -> s

try
  match List.hd [] with
  | [] -> "empty"
  | _ :: _ -> "non-empty"
with
| Failure s -> s
```

## 3.10.4 Exceptions and OUnit

You should be unit testing exceptions as well as the regular functionalities. What is nice about OUnit is that it provides a function to do exception assertions. The following is an example for how that is done:

```
open OUnit2

let tests = "suite" >::: [
  "empty" >:: (fun _ -> assert_raises (Failure "hd") (fun () -> List.hd []));
]

let _ = run_test_tt_main tests
```

What this will do is test if the function is raising the desired exceptions or not.

Something to note is that `fun () -> e` that is used in the second argument of the assertion statement is of type `unit -> 'a` is sometimes called a thunk. What this does is delay the evaluation of the program. The purpose here is that we do not want the `List.hd` to evaluate before `assert_raises` is ready to catch it.
