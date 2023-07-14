---
title: "CS3110: 03.04 Records and Tuples"
topic: cs3110
header:
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

The order of the record type does not matter. `{f: t1; g: t2}` is entirely equivalent to `{g: t2; f: t1}`.

The record types must be defined before the type is used, since OCaml would not be able to do good type inference otherwise.

- For all i in 1..n, it holds that `ei : ti`, and if `t` is defined to be `{f1 : t1; ...; fn : tn}`, then `{f1 = e1; ...; fn = en} : t`.
- If `e : t1` and if `t1` is defined to be `{...; f : t2; ...}`, then `e.f : t2`.

### Record Copy

Here is some syntactic sugar for creating a modified copy of a pre-existing record.

```OCaml
{e with f1 = e1; ...; fn = en}
```

The semantic of this syntax is that not all fields in the record needs to be specified on here. Those that are not specified will be kept the same as the reference record that is placed before the `with`. The fields that are specified are changed to match the new values.

### Pattern Matching

New legal pattern:

```OCaml
{f1=p1; ...; fn=pn}
```

This can be extended to create a new binding. The pattern `{f1=p1; ...; fn=pn}` will match with the record value `{f1=v1; ...; fn=vn; ...}`. It's important to note that the pattern does not have to have all the fields in the record type present to match with a record.

## 3.4.2 Tuples

A record that is identified by position. Much like tuples in other languages.

```OCaml
(1, 2, 10)
(true, "Hello")
([1; 2; 3], (0.5, 'X'))
```

As seen by the example, the elements of a tuple do not have to be the same type, and a tuple can hold a tuple. Tuple declaration is simple, simply write the tuple and the items in them.

Matching is simple and quick as well:

```OCaml
match (1, 2, 3) with (x, y, z) -> x + y + z
- : int = 6
```

This is all the important notes in the scope of my understanding of tuples. If the specifics of the definition of the syntax and both semantics are needed, use [this page](https://cs3110.github.io/textbook/chapters/data/records_tuples.html) as a reference.

## 3.4.3 Variants vs tuples and records

A mathematical way of looking at variants vs records is that variants are one-of types and records are each-of types. One-of types can only represent one of a set of many, while each-of represents all of them at the same time. Variants in OCaml are also known as a **tagged union** in mathematics, and it is an algebraic data-type. This is similar to a **disjoint union** which you would know of if you have taken probability.
