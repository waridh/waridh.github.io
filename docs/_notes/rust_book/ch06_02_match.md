---
tags: rust, enum, match
---
# `match` Flow Control

## Overview

Regularly used to match an `enum` type with a value.

## Description

Here is an example of how basic flow control would work when you do not have a value attached to the `enum`.

```rust
enum Coin {
	Penny,
	Nickle,
	Dime,
	Quarter,
}

fn value_in_cent(coin: Coin) -> u8 {
	match coin {
		Coin::Penny => 1,
		Coin::Nickle => 5,
		Coin::Dime => 10,
		Coin::Quarter => 25,
	}
}
```

This is how you would unravel an `enum` back into a regular value, when there are many options. If binary, you could just use a regular conditional. Now we can also multiline code on a match, view the following.

```rust
fn value_in_cent(coin: Coin) -> u8 {
	match coin {
		Coin::Penny => {
			println!("You found a lucky penny!");
			1
		}
		Coin::Nickle => 5,
		Coin::Dime => 10,
		Coin::Quarter => 25,
	}
}
```

Now the question is, "how do we bind these patterns when they have values??". So here is the code snippet that contains an `enum` that has an `enum` as one of its value attached.

```rust
enum US_STATE {
	Alabama,
	Alaska,
}

enum Coin {
	Penny,
	Nickle,
	Dime,
	Quarter(US_STATE),
}
```

So we want to unravel this, but how?? Here it how.

```rust
fn value_in_cents(coin: Coin) -> u8 {
	match coin {
		Coin::Penny => 1,
		Coin::Nickle => 5,
		Coin::Dime => 10,
		Coin::Quarter(state) => {
			println!("State quarter from {:?}!", state);
			25
		}
	}
}
```

For this code snippet, I believe that the code explains the concept well enough.

When working with the `Option<T>` `enum`,  and we want to extract the value from `Some`, or be able to handle `None`, we need to use `match` as well.

Here is a code snippet that should act as an adequate example:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
	match x {
		None => None,
		Some(i) => Some(i+1),
	}
}

let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
```

## Match are exhaustive

What do we mean? Stay tuned, we'll be right back!

## Pointer
- [Chapter 6: home](ch06_00_enums.md)
- [Next: `if let`](ch06_03_if_let.md)
- [Previously: `enum`](ch06_01_enums.md)