---
title: "CS3110: 02.06 Printing"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

OCaml has a printing function that is built in. There are actually different print functions for all the different primitive types: `print_char`, `print_string`, `print_int`, and `print_float`. `print_endline` is just like print string, but it will also output a newline character at the end.

```OCaml
print_endline "Camels are bae"
```

```
Camels are bae
```

```OCaml
- : unit = ()
```

## 2.6.1 Unit

The types of the print functions are something that we have not seen before:

```OCaml
print_endline
- : string -> unit = <fun>
```

`unit` only have one value, and it is `()`. This is like a `void` in Java or a `None` in Python. It doesn't exist in this way in Rust, but there will be something similar later.

## 2.6.2 Semicolon

If you want to print something in sequence, then you can use `let` nesting to do so:

```OCaml
let _ = print_endline "Camels" in
let _ = print_endline "are" in
print_endline "bae"
```

```
Camels
```
```
are
```
```
bae
```

```OCaml
- : unit = ()
```

Just like in Rust, `let _ = e` is a way to evaluate e without binding it to a name. To be even more concise with this method, and tell the compiler that we know that the expression will return a `unit`, we can use the following syntax:

```OCaml
let () = print_endline "Camels" in
let () = print_endline "are" in
print_endline "bae"
```

But since OCaml is a language that loathes boiler plate code, we can take it a step further with the OCaml syntax and do the following to achieve the same printing chain:

```OCaml
print_endline "Camels";
print_endline "are";
print_endline "bae"
```

## 2.6.3 Ignore

If `e1` does not have type `unit`, then `e1`; `e2` will give a warning, since you are discarding potentially useful value. If that is our intent, we would want to use the built-in function `ignore : 'a -> unit` to convert any value into `()`.

```OCaml
(ignore 3); 5
```

```OCaml
- : int = 5
```

`ignore` is such a simple function that we could implement it ourselves:

```OCaml
let ignore = fun x -> ()
```

We can take the declaration further and do the following:

```OCaml
let ignore = fun _ -> ()
```

## 2.6.4 Printf

When we need to do some string formatting before printing, instead of using primitive printing, we could instead do the following:

Use the OCaml `Printf` library and do this:

```OCaml
let print_stat name num =
    Printf.printf "%s: %F\n%!" name num
```

```OCaml
val print_stat : string -> int -> unit = <fun>
```

The first argument for the `Printf.printf` function looks like string, which it kinda is, but the compiler actually looks at it in a different way. There are plain characters and conversion specifiers that start with `%`.

Read the documentation of `Printf` when you need to use them. Everything in that example code should look familiar except for `%!`, which is a conversion specifier that flushes the output buffer.

Like most other languages, `sprintf` variant is also available for converting the printf format into a string instead of printing it.

| [Previous](ch02_05_documentation.md) | [Next](ch02_07_debugging.md) | 
| ------------------------------------ | ---------------------------- |
