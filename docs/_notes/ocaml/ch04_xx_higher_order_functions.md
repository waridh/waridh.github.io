---
title: "CS3110: Chapter 4 - Higher Order Programming"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

Higher order functions are functions that can take another function as either
a parameter, or return it as an output. This added layer of abstraction can
allow programmers to simplify their code and increase clarity.

## 4.1 Higher-Order function

Here is a basic example of applying a function twice.

```
let double x = x * 2
let square x = x * x

let quad x = double (double x)
let fourth x = square (square x)
```

Notice that the second set of functions are created with a repeat of the same
function twice. This structural pattern can be abstracted out if the language
supports higher-order functions. What we can do in OCaml is the following:

```
let double x = x * 2
let square x = x * x
let twice f x = f (f x)

(* Implementing quad and fouth using twice *)
let quad' x = twice double x
let fourth x = twice square x
```

Currently, there are more lines of code in this implementation, but the
abstraction makes it trivial to understand. This is called the **abstraction principle**, and there is a bigger textbook that describes why it is nice. I will just take that for granted and move on.

Pipelines in OCaml are also a higher order function. Here is some definition examples:

```
let pipeline x f = f x
let (|>) = pipeline
let x = 5 |> double     (* 10 *)
```

There are some more basic examples, but the point here is that higher-order programming allows us to transform functions in ways that allows for good modularity, composition, and so clarity.

### Definition of Higher Order

Formal logic definition:

*First-order quantification* usually refers to universal or existential ($$\forall$$, $$\exists$$) which lets you quantify over a domain.

*Second-order quantification* lets you quantify over properties of a domain. This lets you generate subsets of a domain. Another way to look at it is a like having a function that takes in an element of a domain, and return a boolean value of if the element satisfies the property.

*Third-order* is the properties of properties, and *fourth-order* logic would be properties of properties of properties. *Higher-order* logic is just all types of logic at a higher order than *first-order* logic. Another interesting thing to note is that all *higher-order* logic can be expressed as a *second-order* logic.

### Agenda

Now we are going to cover the following popular higher-order functions:

- Maps - Elements transformed
- Filters - Eliminator of elements
- Folds - Elements combinator

## 6.2 Map

This function will transform all the elements in the input data-structure. The following two examples will have two functions, one implemented separately, and the other using map.

```
let rec add1 = function
| [] -> []
| h :: t -> (h + 1) :: add1 t

let rec concat_bang = function
| [] -> []
| h :: t -> (h ^ "!") :: concat_bang t
```

Those code did not take up that much space, but there are some abstraction that are possible with these functions. The result is the map function, that modifies the entire data-structure. I am putting in the tail recursive solution.

```
let map_aux f acc = function
| [] -> acc
| h :: t -> map_aux f (f h :: acc) t

let map f lst = List.rev (map_aux f [] lst)
```

Tail recursive implementation for the map function will reverse the list, so
another function has to be used to reverse the list again. The trade-off in
time complexity now is that it is a constant slower than the non-tail recursive solution, not that I am showing that one.

## 6.3 Filter

We were able to get through maps fast, and I expect the same for filters.
