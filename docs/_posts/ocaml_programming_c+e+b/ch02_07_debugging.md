# 2.7 Debugging

Debugging comes when everything else has failed. We are going to list the best practices that should be taken before debugging.

## 2.7.1 Defenses against bugs

There are four main methods to defend your program against bugs:
1. **Make bugs impossible** - Bugs that has to do with memory can be completely destroyed by choosing programming languages that are completely memory safe. Some of the programming languages that can achieve this are Rust, and OCaml. The exact same can be said with *type safety*.
2. **Use tools that can find bugs** - There are automated source-code analysis tools that can find bugs. Some examples include *FindBugs* which does bug hunting on Java. *SLAM* is used to find bugs in device drivers. A subfield in CS known as *formal methods* studies how the use mathematics to specify and verify programs, which is to say, prove that there is no bugs in a mathematical way. *Social Methods* like paired programming are also useful tools to find bugs.
3. **Make bugs immediately visible** - Bugs should be able to be delt with as they come up. It's much more efficient to deal with bugs as they appear as opposed to letting the program continue to run and the failure cascades into a different issue. Using assertions is a good way to make bugs appear fast and loud, reducing tracing time.
4. **Extensive testing** - How can you even tell that a specific piece of code has bugs in them? Write tests that would expose the bugs, and confirm that the code does not fail those tests. Unit tests are for relatively small pieces of code, such as an individual function or module, are especially important. OCaml has as unit testing framework called OUnit, which will let you quickly set up tests.

After all these testing failed, you are now forced to start debugging.

## 2.7.2 How to debug in OCaml

1. **Distill the bug into small test cases** - Instead of working with the entire system, work with a smaller test case to save time, and increase focus on the issue.
2. **Employ the scientific method** - i) Formulate a hypothesis on why the bug is occurring, ii) Design an experiment to confirm if that is the issue, and regardless of what the result is, record it. iii) Based on what you learned from the previous experiment, reform the hypothesis and restart the cycle until you have found the cause of the bug.
3. **Fix the bug** - Might be a typo, might be a design flaw, just apply the fix. If this is a copy paste error, would you have to refactor your code??
4. **Permanently add the small test case to your test suite** - The test case you used to catch the bug should now be a part of the test case.

## 2.7.3 Debugging in OCaml

Here are some debugging technique for OCaml. Some of these are also used in other languages:

### Print statement

The classic debugging method, print statements lets us see what the values at a certain point in the program's runtime is. If you want to see what the value in a certain function:

```OCaml
let inc x = x + 1
```

You can just add the following line:

```OCaml
let inc x =
    let () = print_int x in
    x + 1
```

### Function traces

If you want the trace of a recursive calls and returns used for a function, use the #trace directive:

```OCaml
# let rec fib x = if x <= 1 then 1 else fib (x - 1) + fib (x - 2);;
# #trace fib;;
```

if you evaluate `fib 2`, you will see the following output:

```
fib <-- 2
fib <-- 0
fib --> 1
fib <-- 1
fib --> 1
fib --> 2
```

and if you want to stop tracing, then you can use the `#untrace` directive.

### Debugging tool

At the worse case, you can use the OCaml debugging tool called `ocamldebug`.

## 2.7.4 Defensive Programming

As a part of trying to make bugs appear as early as possible, we can link this idea with preconditions.

Let's look at this function documentation:

```OCaml
(** [random_int] is a random integer between 0 (inclusive)
    and [bound] (exclusive). Requires: [bound] is graeter than 0
    and less than 2^30. *)
```

To help us debug better, we could also code in an input check on the function so that if the input breaks the precondition, we raise an exception. Here are three example designs that will do that:

```OCaml
(* Design 1 *)
let random_int bound =
    assert (bound > 0 && bound < 1 lsl 30);
    (* Rest of code *)
(* Design 2 *)
let random_int bound =
    if not (bound > 0 && bound < 1 lsl 30)
    then invalid_arg "bound";
    (* Rest of code *)
(* Design 3 *)
let random_int bound =
    if not (bound > 0 && bound < 1 lsl 30)
    then failwith "bound";
    (* Rest of Code *)
```

The second one is the most informative to the client since the built in function will tell them that it is an invalid argument, and not just an error without description. The first design is best for debugging your own code.

Checking the precondition is computationally cheap, so it would be a good idea to do this instead of waiting for later. When you implement these defensive programming technique, be sure to inform the postcondition portion of the documentation:

```OCaml
(** [random_int bound] is a random integer between 0 (inclusive)
    and [bound] (exclusive). Raises: [Invalid_argument "bound"]
    unless bound is greater than 0 and less than 2^30. *)
```


| [Previous](ch02_06_printing.md) | 
| ------------------------------- |
