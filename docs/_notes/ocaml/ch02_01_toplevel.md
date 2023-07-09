---
title: "CS3110 02.01 OCaml Toplevel"
layout: single
tags: cs3110
---
# The OCaml Toplevel

The Toplevel is like a calculator or a command-line interface to OCaml. It's similar to JShell for Java, or the interactive Python interpreter. The toplevel is useful for trying out small piece of code without going to the trouble of launching the OCaml compiler, but its not very good for working with larger programs.

The toplevel is more like a read-eval-print-loop. It can read the programmer input, evaluate, print the result, and repeat.

To start the toplevel, just run `utop` in the terminal. `ctrl-D` is how you exit the toplevel, but you can also type in `#quit;;`. In the toplevel, you need to put the `#` before a command.

## 2.1.1 Types and Values

You can type in expressions in the OCaml toplevel. End an expression with a double semi-colon `;;` and then press the return key. OCaml will evaluate the expression and tell you the resulting value, and the value's type.

```OCaml
# 42;;
- : int = 42
```

Binding values to a variable can be done with `let` just like rust.

```OCaml
# let x = 42;;

val x : int = 42
```

And utop is nice enough to describe the value and type of what we declared.

## 2.1.2 Functions

This is a functional programming language, you better be good at making functions. A function can be declared as simply as doing this:

```OCaml
# let increment x = x + 1;;

val increment : int -> int = <fun>
```

You can see that they use int as an input and int as an output. `<fun>` represent an unprintable function value.

```OCaml
# increment 0;;
- : int = 1

# increment(21);;
- : int = 22

# increment( increment(5));;
- : int = 7
```

In OCaml vocab, we apply a function instead of call it. Just also know that OCaml doesn't require parenthesis or whitespace, and the preferred style is to use less parenthesis.

## 2.1.3 Loading Code in Toplevel

Toplevel doesn't just act as an interpreter, but also a quasi terminal. You can use toplevel command by putting `#` in from of the word, and toplevel will register that as something you are trying to make it do.

Something simple that might be used very often for debugging would be: 
```OCaml
#use "ocamlfile.ml"
```

## 2.1.4 Workflow with Toplevel

1. Edit the code in the file
2. Load the code into toplevel using `#use`
3. Interactive testing
4. Exit toplevel


| [Previous](ch02_00_basics_of_ocaml.md) | [Next](ch02_02_compiling_ocaml.md) | 
| -------------------------------------- | ---------------------------------- |
