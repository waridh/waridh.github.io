# 2.4 Functions

## 2.4.1 Function Definitions

Let's do an analysis of some simple let definition:

```OCaml
let x = 42;;
```

This let command has the expression `(42)` in it, but it is itself not an expression. It is rather a definition, and it binds a value to a name. What you will need to understand is that definitions are not expressions nor expressions definitions. They are distinct syntactic classes.

Let's look at the function definition:

```OCaml
let f x = ...
```

Recursive functions need to be explicitly declared, like the following:

```OCaml
let rec f x = ...
```

This is done because most languages assume that the function is a recursive function, but OCaml does not make that assumption. Here is an example of a popular recursive function:

```OCaml
(** [fact n] is [n]!.
    Requires: [n >= 0]. *)
let rec fact n = if n = 0 then 1 else n * fact (n - 1)
val fact : int -> int = <fun>
```

*Note: The OCaml integers are not mathematical integers, but a computing integer with limited bits.* 


Here is another recursive function:

```OCaml
(** [pow x y] is [x] to the power of [y].
     Requires: [y >= 0]. *)
let rec pow x y = if y = 0 then 1 else x * pow x (y - 1)
```

I'll be honest, some of the syntax here is similar to rust. The recursive writing here is quite efficient, I am a big fan.

*Note: When you are annotating the type of a variable in OCaml, you also need to include the bracket. Sometimes, it is simpler to just let the compiler infer the type as to not leave clutter.*

### Syntax

```OCaml
let rec f x1 x2 ... xn = e
```

`f` is a metavariable for function. `rec` is required if the function will be recursive, else it is not needed.

### Mutually recursive functions can be defined with the `and` keyword:

```OCaml
let rec f x1 x2 .. xn = e1
and g y1 ... yn = e2
```

What does this mean? It seems unclear from just the syntax, but the example provided below might shed some more light on what it means:

```OCaml
(** [even n] is whether [n] is even.
    Requires: [n >= 0]. *)
let rec even n =
  n = 0 || odd (n - 1)

(** [odd n] is whether [n] is odd.
    Requires: [n >= 0]. *)
and odd n =
  n <> 0 && even (n - 1);;
```

So you are essentially defining two functions that are both calling each other.

### Function type syntax

```OCaml
t -> u
t1 -> t2 -> u
t1 -> ... -> tn -> u
```

Where `t` is the input type and `u` is the output type. `t1 -> t2 -> u` is a function that takes two inputs.

### Static Semantics

For non-recursive functions, assuming that `x1 : t1`, and `x2 : t2` and ... `xn : tn`, we can conclude that `e : u`, then `f : t1 -> t2 -> ... -> tn -> u`.

## 2.4.2 Anonymous Functions

We often see values that are not bound to a name, like the following:

```OCaml
# 42;;
- : int = 42
```

Or we can choose to bind it to a name where we get:

```OCaml
# let x = 42;;
val x : int = 42
```

Similar to this, OCaml will let you do the same things with functions. Here is an example:

```OCaml
fun x -> x + 1
```

So the `fun` keyword is used to signify an anonymous function. x is the argument, and x + 1 is the expression in the function.

Now we have two ways to write the same function:

```OCaml
let incr x = x + 1;;
let incr = fun x -> x + 1;;
```

Although syntactically different, they have the same semantics.

Anonymous functions are also called Lambda expressions, after Lambda calculus. In lambda calculus, the expression `fun x -> e` would be written $\lambda x.e$. where the $\lambda$ represents an anonymous function.

### Syntax

```OCaml
fun x1 ... xn -> e
```

### Dynamic Semantics

The anonymous function is already a value so no computation needs to be performed.

### Static Semantics

If we assume that `x1 : t1`, `x2 : t2`, and `xn : tn`,  we conclude that `e : u`, then `fun x1 ... xn -> e : t1 -> t2 -> ... -> tn -> u`.

## Function Application

Here is a simplified version of function applications. This is the syntax for it:

```OCaml
e0 e1 e2 ... en
```

The first expression `e0` is the function and it is applied to arguments `e1` to `en`. This is like calling a function in languages like TCL.

### Static Semantics

If we assume that `e1 : t1`, `e2 : t2`, ... `en : tn` and `e0 : t1 -> t2 -> ... -> tn -> u`, then `e0 e1 ... en : u`.

### Dynamic Semantics

To evaluate `e0 e1 ... en`:

1. Evaluate `e0` to a function, and evaluate the rest of the expressions to a value. For `e0` you will either have an anonymous function or a named function `f`. If it is named, you will have to find the definition for the function `f`. Anyways, regardless of if the function that is defined by `e0` is a recursive, non-recursive, or anonymous function, we now have the argument names and the body expressions. The rest of the expression `e1` ... `en` are evaluated to a value `v1` ... `vn`. `f` is also viewed as `let rec f x1 ... xn = e`.
2. Substitute each values `vi` for the corresponding value `xi` in the body `e`. This substitution creates a new expression `e'`.
3. Evaluate `e'` to a new value `v`, which is the result of the evaluation of `e0 e1 ... en`.

Both these rules and the rules for `let` involves some substitution. This was done on purpose to allow for multiple methods to represent the same semantics. For example `let x = e1 in e2` is semantically identical to `(fun x e2) e1`.

## 2.4.4 Pipelines

OCaml has a built-in infix for function application called the pipeline operator. This is written like this: `|>` which looks like triangle pointing to the right. It's supposed to look like values being sent from the left to the right. Here is an example for better understanding:

```OCaml
let square = fun x -> x * x;;
let inc = fun x -> x + 1;;
```

Here are two ways to square 6:

```OCaml
square (inc 5);;
5 |> inc |> square;;
(* They will both evaluate to 36*)
```

Instead of working by nesting with confusing parenthesis, we are now working with literally an arrow pointing us in the direction that we want to value to travel. This is incredibly clear to readers what is happening.

A good example of why this is a good idea can be seen in the example below:

```OCaml
5 |> inc |> square |> inc |> inc |> square;;
square (inc (inc (square (inc 5))));;
```

So what we just learned here is a just a nicer way to scale function applications to when you have to process through a lot of functions.

## 2.4.5 Polymorphic Functions

*The identity function* is the function that just returns the input.

```OCaml
let id x = x

val id : 'a -> 'a = <fun>

let id = fun x -> x

val id : 'a -> 'a = <fun>
```

`'a` is called a type variable which represent an unknown type. It will always start with a single quote, and the common ones are `'a`, `'b`, and `'c`, which are pronounced alpha, beta, and gamma.

```OCaml
id 42;;
id true;;
id "bigred";;

- : int = 42
- : bool = true
- : string = "bigred"
```

Since you can apply this one function to multiple types, it is a polymorphic function. *poly* for many, and *morphic* for forms.

By adding some manual type annotation, we can force the function to be more restrictive. For example:

```OCaml
let int_id (x : int) = (x : int)
```

^ By doing this, other types cannot be used with this function now. Now another way to declare this `int` only function using the original `id` function is by doing the following:

```OCaml
let int_id' : int -> int = id
val int_id' : int -> int = <fun>
```

So what is happening here? I suppose we bounded the original `'a -> 'a` function to a new name with the restriction of `int -> int`. Supposedly we should be looking at this in terms of behaviour. The function `int_id` promises to take `int` as an input and also have `int` as an output. Even though the original `id` function makes many more promises, namely other types like `bool` as input and output. By binding `id` in this way, we basically throw away the other promises and leave it as only `int`. Doing this is pretty safe, so we can write some functions without types and then morph it into the type we need it to be later.

Doing the converse is not true. We cannot go from `int -> int` to `'a -> 'a`. This will cause many issues when your input is not an `int`, but luckily for us, OCaml is actually smart enough to stop the assignment from going through. This interaction can be thought in terms of function assignments.

```OCaml
let id' : 'a -> 'a = fun x -> x + 1
val id' : int -> int = <fun>
```

So you can see that OCaml automatically assign the type of `int` into the function since the operator being used are only for `int`. This will then prevent us from applying this function to arguments of non-integers.

Here is another example that will help us understand how OCaml is choosing how to do this:

```OCaml
let x = e [in e']
```

OCaml will infer that `x` has the type `t`, and that includes some type variables `'a`, `'b`, etc. Then we are permitted to instantiate those type of variables. We'd then reveal what those type variables would be by using the argument, like in the example of `id 5` revealing `'a` as `int`. We could also reveal the type by using function type annotation, like with `let id_int' : int -> int = id`. Something to be careful about is the instantiation. For `'a -> 'b -> 'a`, you could do `int -> string -> int`, but `int -> string -> bool` would be illegal under this definition.

## 2.4.6 Labeled and Optional Arguments

### Labels

For functions with many arguments, it would be a good idea to label them. Even though there are both types and names, sometimes, it will still be confusing to people reading the code. The example that will be analysed for this topic will be the function `String.sub`.

```OCaml
String.sub;;
- : string -> int -> int -> string = <fun>
```

Just from this alone, it is not clear how you use the function. To avoid needing to consult the documentation, we can label the argument:

```OCaml
let f ~name1:arg1 ~name2:arg2 = arg1 + arg2;;
val f : name1:int -> name2:int -> int = <fun>
```

Now you can apply the function without needing to keep the order of the argument. This is an example of applying this function like that:

```OCaml
f ~name2:3 ~name1:14;;
- : int = 17
```

Now if you want the label to be the same as the variable name, there is a nice little shorthand that OCaml provide:

```OCaml
let f ~name1:name1 ~name2:name2 = name1 + name2;;
let f ~name1 ~name2 = name1 + name2;;
```

Now the one issue with labels is that they can add clutter. If you combine the labels with explicit type annotation, then we'd have this:

```OCaml
let f ~name1:(arg1 : int) ~name2:(arg2 : int) = arg1 + arg2;;
```


### Optional Arguments

Here is the syntax for applying optional arguments:

```OCaml
let f ?name:(arg1=8) arg2 = arg1 + arg2
val f : ?name:int -> int -> int = <fun>
```

Now you can call the function with or without the argument:

```OCaml
f ~name:2 7;;
- : int = 9

f 9;;
- : int = 17
```

## 2.4.7 Partial Application

Okay starting the example, we can define an addition function as follows:

```OCaml
let add x y = x + y;;
val add : int -> int -> int = <fun>
```

Right, now here is a similar function that is declared in a way that we haven't seen yet:

```OCaml
let addx x = fun y -> x + y;;
val addx : int -> int -> int = <fun>
```

So what in happening here? The function `addx` takes `x` as an input and returns a function that is `int -> int` that will add `x` to whatever is passed to it. The type of `addx` is `int -> int -> int`, and the type of `add` is also `int -> int -> int`. From the perspective of the type they look like the same function. The thing is that the form of `addx` suggests something interesting.

We can apply `addx` to a single argument:

```OCaml
let add5 = addx 5
val add5 : int -> int = <fun>
```

```OCaml
add5 2;;
- : int = 7
```

Oh wow! It's like a way of storing values in a function. Wait!!! You can do the exact same thing with `add`.

```OCaml
let add5 = add 5;;
val add5 : int -> int = <fun>

add5 2;;
- : int = 7
```

This is called partial application, and it's essentially partially applying the function by just filling it with one of many arguments. Since this feature exists, here is are three semantically equivalent, but syntactically different functions:

```OCaml
let add x y = x + y;;
let add x = fun y -> x + y;;
let add = fun x -> (fun y -> x + y);; (* This is the purely lambda way *)
```

When you really think about it, `add` is just a function that takes an argument `x` and return a function `fun y -> x + y`. Well, this fact revolutionize how you program with OCaml...

## 2.4.8 Function Associativity

So we're at the insane stage in learning about OCaml. The deep state information is...

**EVERY SINGLE OCAML FUNCTION TAKES ONLY ONE ARGUMENT!!!!**

What?!?!?!?! Why? Okay, let's use the `add` example again. What we know is that `let add x y = x + y` is semantically the same as `let add = fun x -> (fun y -> x + y)` and in general:

```OCaml
let f x1 x2 ... xn = e
```

is semantically equivalent to

```OCaml
let f =
    fun x1 ->
        (fun x2 ->
            (...
                (fun xn -> e)...))
```

So when we really think about it, `f` isn't a function that takes multiple arguments, but a function that takes just one. This means that when you look at types:

```OCaml
t1 -> t2 -> t3 -> t4
```

is going to be the same as:

```OCaml

t1 -> (t2 -> (t3 -> t4))
```

Basically, function types are *right associative*, meaning that there will be implicit parentheses around the function types starting from right to left. Basically, the intuition is that functions will take a single argument and return a new function that expects the remaining argument.

Function application are the reverse. It is *left associative*, meaning that there are implicit parentheses around function application from left to right.

```OCaml
e1 e2 e3 e4
((e1 e2) e3 e4)
```

The intuition here is the the left most function grabs the neighbour and turn into a new function.

## 2.4.9 Operators as Functions

The addition operator `+` has type `int -> int -> int`. It is normally `infix`, e.g, `3 + 4`. By putting parentheses around it, we can make it a prefix operator:

```OCaml
( + ) 3 4;;
- : int = 7
```

This is insane, so here is an example that makes use of it:

```OCaml
let add3 = ( + ) 3
val add3 : int -> int = <fun>
add3 2
- : int = 5
```

The same technique works for any built-in operator.

Normally the spaces are unnecessary, we could write `(+)`, but `( + )` is convention due to the multiplication symbol `( * )` since `(*)` would be parsed as the beginning of a comment. Actually, the reverse is also true, and we can define our own infix operator by doing this:


```OCaml
let ( ^^ ) = fun x -> (fun y -> max x y);;
2 ^^ 3;;
- : int = 3
```

The rules are still very not that concrete yet, so avoiding infix operators is a good way to program in OCaml right now.

## 2.4.10 Tail Recursion

```OCaml
(** [count n] is [n], computed by adding 1 to itself [n] times. That is,
    this function counts up from 1 to [n]. *)
let rec count n =
    if n = 0 then 0 else 1 + count (n - 1);;

val count : int -> int = <fun>
```

We counting to 10 is no problem, and neither is counting to 100,000:

```OCaml
count 10
count 100_000
```

The real issue occurs when we are trying to count to 1,000,000:

```OCaml
Stack overflow during evaluation (looping recursion?)
```

I can tell that it is because we are creating multiple functions in the stack and it just kept growing because it's trying to get into the base case.

This is indeed the case, and the operating system is what limits it. This is for the sake of safety so that users will not run programs that will take up all the memory available on the system and crash it.

### Tail Recursion

There is a solution to this type of recursive calls, and it is called *tail-call optimization*. This is like dynamic programming from bottom up, but the programmer will have to do some work to get this to happen.

So let's have a look at why the `count` function was having issues:

```OCaml
let rec count n =
    if n = 0 then 0 else 1 + count (n - 1)
```

So the issue here is that after the recursive call to `count (n - 1)`, there is still calculations remaining  in this function application, since the function still needs to add `1` to the result of the recursive call. So how could we rewrite the function so that it would not need to do more computation after the recursive call? Here is an example:

```OCaml
let rec count_aux n acc =
    if n = 0 then acc else coun_aux (n - 1) (acc + 1)

let count_tr n = count_aux n 0
```

```OCaml
val count_aux : int -> int -> int = <fun>

val count_tr : int -> int = <fun>
```

So here is the difference with this version vs the original `count`. We are also passing an accumulator in that will increment each time it enters another layer of recursion. This is the new return and it is a smart way of keeping the program optimized as you don't need to go back in layers of recursions to grab or calculate something.

And after setting up the `acc` to `0`, we can bind it to a new name that will function just like the original `count` but this time, it will actually not fill up the call stack thanks to the OCaml compiler. The OCaml compiler will notice when a recursive call is in *tail position*, which is the technical term for "there is no more computation after it returns". The recursive call to `count_aux` is in tail position but `count` is not.

What the OCaml compiler does to recursive calls in tail position is that it will just reuse the stack frame that did the recursive call to the function, saving both space and time. This is insane because this will just reduce the stack space requirement from linear to constant. Incredible, I think that we are coming back to using recursion in our programs again.

*Note: As a new programming, do not fixate too much on tail recursion. Like with all of engineering, the first draft should not be the final draft, and sometimes, you should put time and energy into getting it working before you make it fast.*

### How to do tail recursions?

1. Change the main function into the helper function and add an extra augment: the accumulator, often named `acc`.
2. Write a new "main" version of the function that will call the helper function. It will pass the original base case's return value as the initial value of the accumulator.
3. Change the helper function to return the accumulator in the best case.
4. Change the helper function's recursive case. It now needs to do extra work on the accumulator argument before the recursive call, and this step is the hardest one.

#### Example

```OCaml
(* [fact n] is [n] factorial *)
let rec fact n =
  if n = 0 then 1 else n * fact (n - 1)
```

```OCaml
let rec fact_aux n acc1 acc2 =
    if n = 0 then acc1 else fact (n - 1) (acc1 * acc2) (acc2 + 1);;

let fact_tr n = fact_aux n 1 1;;
```

I was able to solve it using two arguments, but I'm not sure if there is a more optimized answer. Yeah, there was, I was too fixated on a pattern that was restrictive.

```OCaml
let rec fact_aux n acc =
    if n = 0 then acc else fact (n - 1) (acc * n);;

let fact_tr n = fact_aux n 1;;

val fact_aux : int -> int -> int = <fun>
val fact_tr : int -> int = <fun>
```

Whoop, factorials are large, so at `fact 50` we would already get an integer overflow. That's alright though, we could just use the `Zarith` library to get larger integers. Install using `opam install zarith`.

To import the `zarith` library, do the following:

```OCaml
#require "zarith.top";;

let rec zfact_aux n acc =
    if Z.equal n Z.zero then acc else zfact_aux (Z.pred n) (Z.mul acc n);;

let zfact_tr n = zfact_aux n Z.one;;

zfact_tr (Z.of_in 50);;

val zfact_aux : Z.t -> Z.t -> Z.t = <fun>
val zfact_tr : Z.t -> Z.t = <fun>
```

What is insane here is that this will indeed calculate `zfact_tr 1_000_000`, but it might take several minutes.

#### Quick facts about Zarith

- `Z` is used to represent numbers, like they do in math.
- `Z.n` where `n` is the name in the `Z`. This is like a module, and it might be, but I don't know yet.
- `Z.t` is the big integer type.
- `Z.equal` for equality, `Z.zero` for a big integer zero, `Z.pred` for predecessor, which is just subtracting by one. `Z.mul` is the multiplication function, `Z.one` makes a big integer one, and `Z.of_int` converts a integer primitive into a big integer.

| [Previous](ch02_03_expressions.md) | [Next](ch02_05_documentation.md) | 
| ---------------------------------- | -------------------------------- |
