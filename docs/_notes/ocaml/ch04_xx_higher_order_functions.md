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
| [] -> List.rev acc
| h :: t -> map_aux f (f h :: acc) t

let map f lst = map_aux f [] lst
```

Tail recursive implementation for the map function will reverse the list, so
another function has to be used to reverse the list again. The trade-off in
time complexity now is that it is a constant slower than the non-tail recursive solution, not that I am showing that one.

## 6.3 Filter

We were able to get through maps fast, and I expect the same for filters.

Motivation is somewhat simple. We want to remove certain elements from a given data structure. How can we do this? I'll just give the example of how you could implement a filter now, since it is relatively simple to understand.

```
let filter_aux p acc = function
| [] -> List.rev acc
| h :: t -> if p h then filter p (h :: acc) t else filter p acc t
```

Nothing too complicated here. Just making sure to skip the element of the list if it doesn't satisfy the predicate.

## 6.4 Fold

Things start to get complicated here, since folds have ordering intricacies.

A fold is a function that will combine all elements in a data structure using an input function to define how the combination should happen.

Here is the example of creating functions that do not use fold:

```
let rec sum = function
| [] -> 0
| h :: t -> h + (sum t)

let rec concat = function
| [] -> ""
| h :: t -> h ^ concat t
```

Moving on to how this can be implemented in higher order, simplifying the implementation:

```
let rec combine f init lst = match lst with
| [] -> init
| h :: t -> f h (combine f init lst)

let sum = combine ( + ) 0
let concat = combine ( ^ ) ""
```

So there are actually two layers of abstraction when it comes to folds. You need to separate out the base case, as to comply with the type of the function. After that, the function used to combine the values must be specified.

Now here is where ordering matter. What we made was called a **fold right**, and that is because the operations are being done from right to left. Here is an example of how to operation is ordered when you use a fold right:

$$
\left(a + \left(b + \left(c + init\right)\right)\right)
$$

The problem with this solution is that it is not tail recursive. We can make a change in the code to fix that by changing the way that the accumulator is handled. The following example shows how we can do that:

```
let rec combine_tr f acc = function
| [] -> acc
| h :: t -> combine_tr f (f acc h) t
```

This implementation is now recursive, but the ordering of the application has changed. The operation now runs as follows:

$$
\left(\left(\left(init+a\right)+b\right)+ c\right)
$$

This makes the result different between fold left and fold right while using the same inputs. The implementation of `combine_tr` is actually a library function called **fold left**, and it does its order of operation from left to right. Best advantage in using fold left is that it is tail recursive. Also, you can reverse lists with it, so that is pretty cool.

### Examples

Right, here are some examples of how fold can be used:

```
let length lst =
  List.fold_left (fun acc _ -> acc + 1) 0 lst
let rev lst =
  List.fold_left (fun acc h -> h :: acc ) [] lst
let map f lst =
  List.fold_right (fun h acc -> f h :: acc) lst []
let filter p lst =
  List.fold_right (fun h acc -> if p h then h :: acc else acc) lst []
```

So what fold guarantees is that the recursive travel is not done correctly. What might be a bit more difficult with fold is that it will not always be immediately clear to a programmer what is happening in the code.

### 6.4.8 Fold vs Recursive vs Library

Here is the jot note:

- For all implementation, there is a trade-off. For some functions, the recursive and library implementation might return without having to go through the entire data structure, but if you are using fold left, you know that it is going to run in linear time with a tail recursion.

## 6.5 Beyond Lists

Remember this?

```
type 'a tree =
  | Leaf
  | Node of 'a * 'a tree * 'a tree
```

Here are some example of doing filters, maps, and folds on this structure (not in that order).

### Map

```
let tree_map f = function
  | Leaf -> Leaf
  | Node (a, b, c) -> Node (f a, tree_map f b, tree_map f c)
```
