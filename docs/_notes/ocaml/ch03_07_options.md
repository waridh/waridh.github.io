---
title: "CS3110: 03.07 Options"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

Imagine the exact same concept as `Option` from Rust, but in OCaml. The same use case also applies, we use `option` when we don't know if the function or process is going to return something or not at all. What this does is force the user to write a case for both when there is something, and when there isn't. This improves the robustness of the code, and reduces bugs associated with `null` pointers.

## Usage

`option` is a type constructor, just like a `list`. Here is how you create an instance of an `option`.

```
Some 42
- : int option = Some 42
```

```
None
- : 'a option = None
```

The contents of an `option` can be grabbed by using `match`. Since this is OCaml, you can make a small function to do that like this:

```
let extract o =
  match o with Some i -> string_of_int i | None -> "";;

extract (Some 42);;
- : string = "42"
extract None;;
- : string = ""
```

## Syntax and Semantics

- `t option` is a type for every `t`
- `None` is a value of type `'a option`
- `Some e` is an expression of `t option` if `e : t`. If `e ==> v` then `Some e ==> Some v`
