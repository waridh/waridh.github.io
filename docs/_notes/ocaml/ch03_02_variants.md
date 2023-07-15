---
title: "CS3110: 03.02 Variants"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

Literally `enum` from other programming languages. Here is the syntax for them:

```OCaml
type day = Sun | Mon | Tue | Wed | Thu | Fri | Sat
d = Tue

val d : day = Tue
```

To access and use these, you just have to do the following:

```OCaml
let int_of_day d = match d with Sun -> 1 | Mon -> 2 | Tue -> 3
| Wed -> 4 | Thu -> 5 | Fri -> 6 | Sat -> 7

val int_of_day : day -> int = <fun>
```

As you can see, you are just doing some pattern matching.

### Static Semantics

- If `t` is a type defined as `type t = ... | C | ...` then `C : t`.

## 3.2.1 Scope

Suppose that there are two types with overlapping constructor names, for example:

```OCaml
type a = C | D
type b = D | E
let x = D
```

What happens here? Simple, the type that is defined later will win. In this example `x : b`. This makes it good convention to put a prefix character on the constructor names, such as:

```OCaml
type ptype = TNormal | TFire | TWater
```

It is less likely to have a constructor collision with other `enum`.

| [Previous](ch03_01_lists.md) | [Home](ch03_00_data_and_types) | [Next](ch03_03_ounit.md) | 
| ---------------------------- | ------------------------------ | ------------------------ |
