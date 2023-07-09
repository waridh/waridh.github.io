---
tags: rust, enum
---
# `if let`

`if let` was created as a less verbose way to match patterns while being able to ignore others. Here is an example using `match` that shows a scenario in which `if let` would be useful:

```rust
    let config_max = Some(3u8);
    match config_max {
        Some(max) => println!("The maximum is configured to be {}", max),
        _ => (),  // This is an annoying boilerplate code added.
    }

```

There is a shorter way to write this by using `if let`. Here is an example on how that is done.

```rust
    let config_max = Some(3u8);
    if let Some(max) = config_max {
        println!("The maximum is configured to be {}", max);
    }

```

Essentially, this is match with some ignore and some different syntax. It's like a match with let syntax. The one issue with `if let` is that it is not as exhaustive as `match`, but there is less typing and boilerplates.

It's not all doom however, as you can add `else` to the end of `if let` and cover the other cases. Here is an example for that.

```rust
    let mut count = 0;
    if let Coin::Quarter(state) = coin {
        println!("State quarter from {:?}!", state);
    } else {
        count += 1;
    }

```

And here is the `match` equivalent for that same piece of code. I think that `match` becomes less verbose than `if let` when it gets to this point.

```rust
    let mut count = 0;
    match coin {
        Coin::Quarter(state) => println!("State quarter from {:?}!", state),
        _ => count += 1,
    }

```

# Pointers
- [Chapter 6: home](ch06_00_enums.md)
- [Previously: match](ch06_02_match.md)