---
title: "CS3110 02.03 Expressions"
layout: single
tag: ocaml
---
# Expressions

If imperative languages are primarily built on commands, then functional languages are primarily built on expressions. Some examples are `2 + 2` or `increment 21`.

The primary purpose of functional languages is to *evaluate* an expression to a *value*. A value is an expression in which there are no more computations left to perform, meaning that in function programming, values are expressions, but not all expressions are values. For example `2`, `true`, and `"two!"` are values.

There are some cases in which expressions do not evaluate to a value. There are two reasons that this would happen:
1. The evaluation raised an exception
2. The evaluation never terminates.

## Primitive Types and Values

Primitives are built on the most basic types: integers, floating point numbers, characters, strings and booleans. They act like other primitive types from other languages.

### Integers

Integer operators act just like those of Rust, so it will not be put in the notes.

OCaml integers are signed twos complement and range from $-2^{62}$ to $2^{62}-1$. This is because OCaml uses 64-bit machine words, but one of the bit has been stolen by the OCaml implementation, and thus it's actually a 63 bit integer. That last bit is used to distinguish the value between a pointer and an integer. There is a library that gets you access to a true 64 bit integer in the **Int64** module. If you want arbitrary precision, then you can use the **Zarith** library. Normally though, the built in **int** type is good enough with the best performance.

### Floats

```OCaml
# 3.;;
- : float = 3.

# 3;;
- : int = 3

```

basically, you will need to put that period in order to specify a float as opposed to an integer. Also, OCaml deliberately does not support operator overloading. Because of this, to do floating point multiplication, you actually need to use `*.` and not `*`.

```OCaml
# 3.14 *. 2;;
- : float = 6.28
```

OCaml will also not automatically convert between floats and int, so you will have to explicitly declare that. This can be seen here:

```OCaml
# 3.14 *. (float_of_int 2);;
- : float = 6.28
```

Because floating point is an estimate, there might be rounding errors that you will have to account for.

### Boolean

Written as `true` or `false`. You can do intersects and unions using `&&` and `||` respectively.

### Character

Written in single quotations like in C, `a`, `b`, `c`. This time it is encoded in Latin-1 which is still the size of a byte per character. You can convert between char in int by using either `char_of_int` or `int_of_char`.

### String

String are written as double quotes: `"abc"` and there is a dedicated string concatenation operator, `^`.

```OCaml

# "abc" ^ "def";;
- : string = "abcdef"
```

There are actually plenty of functions that converts things into strings. Here are a big example list:

```OCaml
# string_of_int 42;;
- : string = "42"
# String.make 1 'z';;
- : string = "z"
# int_of_string "123";;
- : int = 123
```

```OCaml
# "abc".[0];;
- : char = 'a'
```

So you can access chars from string indexing.


## 2.3.2 More expressions

OCaml has two equality operators: `=` and `==`. There are also two inequality operators: `<>` and `!=`. You will notice that there are pretty nice looking syntax for OCaml. `<>` is much more interesting then `!=`. They actually have functional differences. `=` and `<>` are structural equality while `==` and `!=` are physical equality. This will come into effect when working with imperative OCaml.

## 2.3.3 Assertion

Raising exception when the assertion doesn't hold true. Used like you would in C or C++. `assert e` evaluates `e`. When the result is `true` then nothing more happens, but if it does not evaluate to `true` then an exception is raised.

## 2.3.4 If Expressions

The syntax for OCaml simple conditional is:

```OCaml
if e1 then e2 else e3;;
```

Here is a toplevel example for a conditional:

```OCaml
# if 2 + 3 > 4 then "Hooray!" else "Darn!";;
- : string = "Hooray!"
```

The difference from imperative languages here is that the conditional here can be put anywhere like an expression. This means you can directly edit the output from a conditional. Here is an example of that:

```OCaml
# 6 + (if 'a' = 'b' then 1 else 10);;
- : int = 16
```

### else if

Here is how you nest multiple if statements together:

```OCaml
if e1 then e2
else if e3 then e4
else if e5 then e6
...
else en;;
```

Due to the functional nature of the language, the final `else` is always required in OCaml.

### Dynamic Semantics

So the neat thing about what we have been writing with `if e1 then e2 else e3` is that the `e1`, `e2`, and `e3` are actually not OCaml variables, but meta variables representing expressions. To be precise with how OCaml if-then-else works, if `e1` evaluates to true, and if `e2` evaluates to a value `v` then the expression will evaluate to `v`. Otherwise, if `e1` evaluates to false and `e3` evaluates to `v`, then the expression will return `v`. `v` is used to represent a real OCaml value. This is a pretty nice and simple way of representing an expression, but later there will be a more mathematical way of representing such things.

### Static Semantics

The static semantic of an `if` expression:
- If `e1` has type `bool` and `e2` has type `t` and `e3` has type `t` then the expression `if e1 then e2 else e3` has type `t`.

This is a typing rule. It is used to describe how to type check an expression. `t` represent any OCaml type, and since we will be discussing has type a lot, there is a syntax for that as well: `e : t`. This is consistent with the toplevel response to `let` statements.

```OCaml
# let x = 42;;
val x : int = 42

```

## Let Statements

We know that let can be used to assign values to a variable, but there is also another use for it. You can use `let` as an expressions:

```OCaml
# let x = 42 in x + 1
- : int = 43
```

So what is happening here is that the x is bound to the value of 42, and then it got immediately used in the `x + 1` expression. This is actually a different interaction than defining a value, since those do not evaluate to an expression. This is what will not work:

```OCaml
# (let x = 42) + 1;;

File "[24]", line 1, characters 11-12:
1 | (let x = 42) + 1
               ^
Error: Syntax error
```

Here is the weird thing. If you make this a `let` expression instead of a `let` definition, then you will get the desired interaction:

```OCaml
# (let x = 42 in x) + 1
- : int = 43
```

To become even more fundamental in our understanding, think of `let` definitions as `let` expressions that have not provided the body of the expression yet. It looks a little insane, but here is how it works:

```OCaml
# let a = "big";;
# let b = "red";;
# let c = a ^ b;;
...
```

Is understood by OCaml the same way as;

```OCaml
let a = "big" in
let b = "red" in
let c = a ^ b in
...
```

### Syntax

```OCaml
let x = e1 in e2
```

Here, `x` is just an identifier. What's funny here is that identifiers are written in `snake_case` and not `camelCase`. `e1` here will act like a binding expression, and `e2` is the *body expression*.

### Dynamic Semantic

To evaluate `let x = e1 in e2`.

1. Evaluate `e1` to value `v1`.
2. Substitute `v1` for `x` in `e2` yielding new expression `e2'`.
3. Evaluate `e2'` to a value `v2`.
4. The result of evaluating the let expression is `v2`.

### Static Semantic

- If `e1 : t1` and if under the assumption that `x : t1` it holds that `e2 : t2` then `(let x = e1 in e2) : t2`.


```OCaml
let x : t = e1 in e2
```

## 2.3.6 Scope

`Let` bindings will only be effective in the block of code that they occur. This is the normal scope behaviours that you would see in a modern language. For example:

```OCaml
let x = 42 in
    (*y does not have meaning here*)
    x + (let y = "3110" in 
        (*y has meaning here*)
        int_of_string y
    );;
```

The scope of a variable is the area in which the name is useful. It's how expressions like this can run validly:

```OCaml
let x = 5 in
    ((let x = 6 in x) + x)
```

But don't code like this, with bad naming like this, no-one collaborating would actually be able to do things.

Actually, you would likely trace the expression to return 11. This is because the name doesn't really matter in the this thing called the *Principle of Name Irrelevance* where the name of the variable does not inherently matter. The following two functions are the same:

$$
f(x) = x^2
$$
$$
f(y) = y^2
$$

To see this in OCaml, we can represent it with

```OCaml
let f x = x * x;;

let f y = y * y;;
```

This is called *alpha equivalence*.

So there is actually a term for the way that you can implement the *Principle of Name Irrelevance*. 

```OCaml
let x = 5 in (let x = 6 in x) + x;;
let x = 5 in (let y = 6 in y) + x;;
```

These two expressions are equivalent since we are using the *principle of name irrelevance*. There is a common name for this phenomenon; a binding of a variable *shadows* any old binding. Look, if you already have an understanding of scope, you don't have to learn a new metaphor to get better at it.

Shadowing is not mutable assignment. 

```OCaml
let x = 5 in ((let x = 6 in x) + x)
let x = 5 in (x + (let x = 6 in x))
```

and in toplevel:

```OCaml

# let x = 42;;
val x : int = 42
# let x = 22;;
val x : int = 22
```

The thing with this example is, even though it looks like `x` is mutable, it actually isn't and we have just entered a new scope. This is effectively doing the following:

```OCaml
let x = 42 in
  let x = 22 in
    ... (* whatever else is typed in the toplevel *)
```

Now here is a utop that is worth looking at to get a better understanding of how scopes and let expressions work in OCaml;

```OCaml
# let x = 42;;
val x : int = 42
# let f y = x + y;;
val f : int -> int = <fun>
# f 0;;
: int = 42
# let x = 22;;
val x : int = 22
# f 0;;
- : int = 42  (* x did not mutate! *)
```

So in a previous scope, you create a function from a value, and then in the new scope, you create a new value, the function was defined in a different scope and it will use that value present in that scope. This prevents incorrect and unexpected results from the function to occur.

## 2.3.7 Type Annotation

OCaml infers the type of every expression without the programmer needing to annotate what it is, but it can still be useful to annotate the type of the expression. This can be done like the following:

```OCaml
# (5 : int);;
- : int = 5
```

This will act more like a correctness check, since annotating the wrong type will cause a compile time error to occur.

```OCaml
# (5 : float);;
File "[27]", line 1, characters 1-2:
1 | (5 : float)
     ^
Error: This expression has type int but an expression was expected of type
         float
  Hint: Did you mean `5.'?
```

### Syntax

```OCaml
(e : t)
```

| [Previous](ch02_02_compiling_ocaml.md) | [Next](ch02_04_functions.md) | 
| -------------------------------------- | ---------------------------- |
