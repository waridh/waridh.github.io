---
layout: single
title: "Rust Book: 06.01 Enumeration"
tags: rust, enum
---
# `enum`

## Definition

Enums let you say that a value if one of a possible list.

For example, you can say that `retangle` is one of possible `shapes` and you can also have `circles` or `triangles`. This is how rust works with `enums`.

Another use-case is IP protocols, as IPv4 and IPv6 are the main protocols that are currently being used often, we are able to say that an IP could be either IPv4 or IPv6. Here is an example:

```rust
enum IPAddr  {
	V4,
	V6,
}
```

So how do we use our type? Here is an example

```rust
let ip_1 = IPAddr::V4;
let ip_2 = IPAddr::V6;
```

This allows for easy function passes, since Rust will see `IPAddr` as the type, so you won't have to specify. Here is that example for passing an `enum` type into a function.

```rust
fn socket(ip_addr: IPAddr)  {}
```

You can call these functions like below:

```rust
socket(IPAddr::V4);
socket(IPAddr::V6);
```

In the above example, we are essentially using the `enum` type as the value that is being passed, but what if we want the type to be able to hold value as well?

Here is an example of how you make the `enum` hold value as well as the type.

```rust
enum IPAddr {
	V4(String),
	V6(String),
}

let home = IPAddr::V4(String::from("180.10.78.189"));
let loopback = IPAddr::V6(String::from("::1"));
```

As you can see, you can attach an actual value type and value into the `enum`. This is a very useful feature. It gets even more advanced, as you can actually just implement tuple class into an `enum` value type.

```rust
enum IPAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}

let home = IPAddr::V4(127, 0, 0, 1);
let loopback = IPAddr::V6(String::from("::1"));
```

It actually gets even more dynamic than this example, as we are able to store whole proper structs inside of `enum`. Here is a code snippet showing us this.

```rust
struct IPv4 {
	// code
}
struct IPv6 {
	// code
}

enum IPAddr {
	V4(IPv4),
	V6(IPv6),
}
```

`enum` are very flexible, enough so that you can place `enum` inside an `enum`! Here is the full example for what you can do inside of an `enum`.

```rust
enum Message {
	Quit,
	Move { x: i8, y: i8},
	Write(String),
	ChangeColor(u8, u8, u8),
}
```

Just like `struct`, `enum` is able to contain methods as well. Here is how you can instantiate those.

```rust
impl Message {
	fn call(&self) {
		// Method stuff
	}
}

let m = Message::Write(String::from("hello"));
m.call;
```

## Options

### `Option` vs `null`

Option is an `enum` that is defined by the standard library. It encodes the scenario where there is or isn't a value. This is very important in a strongly typed language, as it will prevent bugs in the program and reduce confusion. Also, Rust just doesn't have the null reference feature. Good, I suppose. Here is how `Option` is implemented like the following:

```rust
enum Option<T> {
	None,
	Some(T),
}
```

Neat thing is you can just use `None` and `Some` directly because the Option is always built in. `<T>` is a generic type parameter that just allows `Option<T>` to hold any type. Here are how you can assign these enumerations:

```rust
let some_number = Some(5);
let some_char = Some('e');

let absent_number: Option<i32> = None;
```

Time for some types talk. The type of `some_number` is `Option<i32>`, while the type of `some_char` is `Option<char>`. These are different types, but Rust can still infer the type, as long as it is a `Some(T)` enumeration. If we are working with the `None` enumeration, Rust won't be able to tell what type it is, so you would have to do the full type call so that Rust would know what is going on.

## Converting Stored `enum` Values into Regular Values

- [Follow this link to learn about `match`](ch06_02_match.md)
