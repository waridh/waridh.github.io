---
title: "CS3110: 2.5 Documentation"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

## 2.5.1 How to Document

Here is an example of how to create documentation:

```OCaml
(** [sum lst] is the sum of the elements of [lst]. *)
let rec sum lst = ...
```

The double `**` lets the compiler know that this is OCamldoc comment. The `[]` lets the markup know to render it as a monospace.

OCamldoc also supports documentation tags, like `@author`, `@deprecated`, `@param`, `@return`, etc. For some example, Cornell wants you to put these for their assignments:

```OCaml
(** @author Your Name (your netid) *)
```

There are more documentation tricks in the OCaml manual, but these notes are being created for initial ramp up.

## 2.5.2 What to Document

There is no point in doing basic documentation where you declare the parameters and the return values. Instead, follow this example:

```OCaml
(** [sum lst] is the sum of the elements of the [lst].
    The sum of an empty list is 0.*)
let rec sum lst = ...
```

## 2.5.3 Precondition and Postcondition

```OCaml
(** [lowercase_ascii c] is the lowercase ASCII equivalent of
    character [c]. *)

(** [index s c] is the eindex of the first occurrence of
    character [c] in string [s]. Raises: [Not_found]
    if [c] does not occur in [s]. *)

(** [random_int bound] is a random integer between 0 (inclusive)
    and [bound] (exclusive). Requires: [bound] is greater than 0
    and less than 2^30. *)
```

The `randomm_int` function `Requires` is a kind of precondition. It wants the input to be in a certain range. In the main documentation of that same function it also defines the post condition by saying what the return range must be in.

### Raises

The "Raises" clause in the documentation in `index` is another kind of postcondition. It guarantees that the function raises an exception. It is not a precondition because it states what the function will output depending on the input, but not a bound on the input itself.

| [Previous](ch02_04_functions.md) | [Next](ch02_06_printing.md) | 
| -------------------------------- | --------------------------- |
