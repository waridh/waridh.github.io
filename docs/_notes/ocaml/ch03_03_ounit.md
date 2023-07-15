---
title: "CS3110: 03.03 OUnit"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

OUnit is the unit testing framework for OCaml. This is a solution for when your program gets too large for toplevel to test, and you need to design a test suite. Just like with device validation, your test suite needs to contain many unit tests that will test all the different functions and edge cases that your function might run into.

The framework in OUnit is similar to JUnit in Java or HUnit in Haskell. This is the basic workflow of OUnit:
- Write your function in a file `f.ml`. Could have multiple functions.
- Write unit tests in a separate file called `test.ml`.
- Build and run `test` to execute the unit test.

## 3.3.1 Example of OUnit

Example function file `sum.ml`:

```OCaml
let rec sum = function [] -> 0 | h :: t -> h + sum t
```

`test.ml`:

```OCaml
open OUnit2
open Sum

let tests = "Test suite for sum" >::: [
    "empty" >:: (fun _ -> assert_equal 0 (sum []));
    "singleton" >:: (fun _ -> assert_equal 1 (sum (1 :: [])));
    "two_elements" >:: (fun _ -> assert_equal 3 (sum (1 :: 2 :: [])));

]

let _ = run_test_tt_main tests
```

To run these tests properly, you will actually also have to set up dune so that it would include the `ounit2` module.

```Dune
(executable
(name test)
(libraries ounit2))
```

In the `dune-project` file, just keep things the same:

```OCaml
(lang dune 3.4)
```

Now we will get an indicator if a function breaks.

For example, if `sum.ml` `sum` is written like the following:

```OCaml
let rec sum = function
| [] -> 1 (*bug*)
| h :: t -> h + (sum t)

```

You will see an error that looks like this instead:

```OCaml
FFF
==============================================================================
Error: test suite for sum:2:two_elements.

File ".../_build/oUnit-test suite for sum-...#01.log", line 9, characters 1-1:
Error: test suite for sum:2:two_elements (in the log).

Raised at OUnitAssert.assert_failure in file "src/lib/ounit2/advanced/oUnitAssert.ml", line 45, characters 2-27
Called from OUnitRunner.run_one_test.(fun) in file "src/lib/ounit2/advanced/oUnitRunner.ml", line 83, characters 13-26

not equal
------------------------------------------------------------------------------
```

The first line that say `FFF` tells us that three test cases failed.

## 3.3.2 Explanation of OUnit example

open OUnit2 brings the definitions made in the OUnit2 module into scope. The same thing is done with `Sum`. It's capitalized like that to represent the `sum.ml` file.

We also need to make a list of test cases:

```OCaml
[
    "empty" >:: (fun _ -> assert_equal 0 (sum []));
    "singleton" >:: (fun _ -> assert_equal 1 (sum (1 :: [])));
    "two_elements" >:: (fun _ -> assert_equal 3 (sum (1 :: 2 :: [])));
]
```

Each test case has a string that gives it a descriptive name, and a function that probably does an assertion to check if the function output is equal to what we expect. `assert_equal` is a function that is given by `ounit2`, but it is actually not included with the language.

`>:::` and `>::` are separators provided by `ounit2` to setup your test cases. Use this page as reference when trying to build unit tests.

## 3.3.3 Improving OUnit Output

The default error output for OUnit might not be so helpful. In the previous example, we only see that it says `not equal`, but we could make it better. `assert_equal` has an optional argument that lets you view what the expected result is, and also what the wrong output is. This argument is labeled as `printer` and it accepts that function that will convert the assertion compared type into string. Here is an example of that being used on the old test suite:


```OCaml
let tests = "test suite for sum" >::: [
  "empty" >:: (fun _ -> assert_equal 0 (sum []) ~printer:string_of_int);
  "singleton" >:: (fun _ -> assert_equal 1 (sum [1]) ~printer:string_of_int);
  "two_elements" >:: (fun _ -> assert_equal 3 (sum [1; 2]) ~printer:string_of_int);
]
```

It is getting a little messy, and there are now too many repeating code, so with OCaml, we would actually refactor the code to make it more readable:

```OCaml
let make_sum_test name expected_output intput =
    name >:: (fun _ -> assert_equal expected_output (sum input) ~printer:string_of_int)

let tests = "test suite for sum" >:: [
    make_sum_test "empty" 0 [];
    make_sum_test "singleton" 1 (1 :: []);
    make_sum_test "two_elements" 3 (1 :: 2 :: []);
]
```

If we invent the type itself, we might need to make a function that will convert that type into a string.

## 3.3.4 Testing for Exception

Need to look at the section for exceptions

## 3.3.5 Test driven Development

The idea here is that we are developing tests as we are developing code. Doing this will let us catch bugs the moment they appear, and know when we have written code that is correct.

Once again, for example, let's say that we have a type for the days as follows:

```OCaml
type day = Sunday | Monday | Tuesday | Wednesday | Thursday | Friday | Saturday
```

And we want to write a function `val next_weekday : day -> day <fun>` that will return the next workday after a given day.

We would start with writing a broken function:

```OCaml
let next_weekday d = failwith "Unimplemented"
```

Then we start with writing a simple unit test:

```OCaml
let tests = "Testing next_weekday" >::: [
 "tues_after_mon" >:: (fun _ -> assert_equal Tuesday (next_weekday Monday));
]
```

And of course it would fail, so we'd move on to actually implementing the rest of the function:

```OCaml
let next_weekday d = match d with
| Monday -> Tuesday
| Tuesday -> Wednesday
| Wednesday -> Thursday
| Thursday -> Friday
| Friday -> Monday
| _ -> failwith "Unimplemented"
```

And you just keep developing like this. It's a very easy and simple way to build your program.

| [Previous](ch03_02_variants.md) | [Home](ch03_00_data_and_types.md) | [Next](ch03_04_records_and_tuples.md) | 
| ------------------------------- | --------------------------------- | ------------------------------------- |
