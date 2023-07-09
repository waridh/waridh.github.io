---
title: "CS3110: 03.04 Records and Tuples"
topic: cs3110
teaser: /assets/images/ocaml_teaser.png
---

## 3.4.1 Records

It acts very much like a struct from `C`. You have to define it like you are defining a struct, and then you have to instantiate it to a binding. The fields are named, and they are each assigned a type. Here is a Pokemon related example:

```OCaml
type ptype = TNormal | TFire | TWater
type mon = {name: string; hp: int; ptype: ptype}
```

Here is an expression for it:

```OCaml
{name = "Charmander"; hp = 200; ptype = TFire}
```

```OCaml
- : mon = {name = "Charmander"; hp = 200; ptype = TFire}
```

You define with a `:` so that OCaml would know the type of the field, but when actually creating an instance, you use `=` to assign a value to that field.

Here is the rest of the example which notates how you can get the value from this type:

```OCaml
let c = {name="Charmander"; hp=200; ptype=TFire}
c.hp
```

```OCaml
- : mon = {name = "Charmander"; hp = 200; ptype = TFire}
- : int = 200;
```

You can also use pattern matching:

```OCaml
match c with {name = n; hp = h; ptype = p} -> h
```

```OCaml
- : int = 200
```

There is nice syntactic sugar on this, when you want the variable name and the field name to be the same:

```OCaml

match c with {name; hp; ptype} -> hp
```

```OCaml
- : int = 200
```

### Syntax

```OCaml
{f1 = e1; ...; fn = en}
```

Field access

```OCaml
e.f
```

### Semantics

#### Dynamic Semantics

- for all `i` in `1..n` it holds  that `e1==>v1`, then `{f1 = e1;...;fn = en} ==> {f1 = v1;...; fn = vn}`.
- If `e ==> {...; f = v;...}` then `e.f ==> v`

#### Static Semantics

The order of the record type does not matter. `{f: t1; g: t2}` is entirely equivalent to `{g: t2; f:t1}`. There is a type checking rule for records, but I do not think that it is important as of right now. Come back to 3110 if needed.
