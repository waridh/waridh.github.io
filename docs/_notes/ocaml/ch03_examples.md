---
title: "CS3110: 03.__ Examples"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

## 3.11 Trees

You can quickly represent a binary tree using both Variants and Records in OCaml. Here is the example that is given in class.

### Tuple Implementation

```
(* Declaring the variant *)
type 'a tree =
| Leaf
| Node of 'a * 'a tree * 'a tree

(*
         4
       /   \
      2     5
     / \   / \
    1   3 6   7
*)
let t =
Node(
  4,
  Node(2,
  Node(1, Leaf, Leaf),
  Node(3, Leaf, Leaf)),
  Node(5,
  Node(6, Leaf, Leaf),
  Node(7, Leaf, Leaf))
)
```

Now here is a function that counts the number of nodes in the tree:

```
let num_node = function
| Leaf -> 0
| Node(_, l, r) -> 1 + (num_node l) + (num_node r)
```

### Record Implementation \w Mutual Recursion

```
type 'a tree =
| Leaf
| Node of 'a node
and 'a node = {
  value: 'a;
  left: 'a tree;
  right: 'a tree
}

(*
      2
     / \
    1   3
*)
let t =
  Node {
    value = 2;
    left = Node {value = 1; left = Leaf; right = Leaf};
    right = Node {value = 3; left = Leaf; right = Leaf};
  }
```

Here is an example of recursive searching over the tree for the existence of a member:

```
let mem x = function
| Leaf -> false
| Node{value; left; right} -> value = x || mem x left || mem x right
```

This function will return a boolean confirming the existence of the member.

To complete the example, here is a function that places the nodes in `preorder` ordering in a list:

```
let preorder t = 
  let rec pre_acc acc = function
  | Leaf -> acc
  | Node{value; left; right} -> value :: (pre_acc (pre_acc acc right) left)
  in pre_acc [] t
```

## 3.12 Example: Natural Numbers

This example shows the way one should think about functional programming by breaking down numbers to its fundamental definitions. This is how you do so. Begin with defining the variant for natural numbers as the following:

```
type nat = Zero | Succ of nat
```

The idea being represent here is that all natural numbers are recursively subsequent numbers of zero. The way this model works is like this following example:

```
let zero = Zero
let one = Succ zero
let two = Succ one
let three = Succ three
```

You can then make a function to find the predecessor of the current natural value input by doing the following:

```
let iszero = function
  | Zero -> true
  | Succ _ -> false

let pred = function
  | Zero -> failwith "Zero does not have a predecessor"
  | Succ m -> m
```

To go a little further, we can also implement addition with with this natural number variant that we created.

```
let add n1 n2 =
  match n1 with
  | Zero -> n2
  | Succ pred -> add pred (Succ n2)
```

How the addition function work is we are incrementally moving value from the left side to the right side and then finally returning the right side.

You can write a function to convert between primitive int and `nat` and vice versa by doing the following:

```
let nat_to_int = function
| None -> 0
| Succ pred -> 1 + nat_to_int pred

let int_to_nat = function
| i when i = 0 -> Zero
| i when i > 0 -> Succ (int_to_nat (i - 1))
| _ -> failwith "int_to_nat is undefined on negative values"
```

Finally, when we want to check if the value is even or odd, we can use a mutually recursive function:

```
let rec even = function Zero -> true | Succ m -> odd m
and odd = function Zero -> false | Succ m -> even m 
```

This looks like classic induction in formal logic.
