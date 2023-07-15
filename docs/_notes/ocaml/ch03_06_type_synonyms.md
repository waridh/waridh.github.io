---
title: "CS3110: 03.06 Type Synonyms"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

Creating a new name for a pre-existing type. Useful for quickly representing a
longer declaration with a known set. Here are some examples:

```
type point = float * float
type vector = float list
type matrix = float list list
```

The way that this works on OCaml is that a `point` type will fit anywhere a `float * float` will, and vice-versa.

Here is an example of applications for this:

```
let get_x = fun (x, _) -> x

let p1 : point = (1., 2.)
let p2 : float * float = (1., 3.)

let a = get_x p1
let b = get_x p2
```
