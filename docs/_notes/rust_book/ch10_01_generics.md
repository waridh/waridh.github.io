# Generics

Creating a definition for function signatures or structs that will just support all types.

## Vocab

### Generic Type
A placeholder type that can be replaced with any type when the function/method/struct/enum is called.
### Concrete Type
A type that is explicitly defined.

## Function Definition

We can make functions accept generics, which would increase the flexibility of the function, but reduce the closure of the program. Before we come back and talk more about this, here is the code snippet for the function signature for flexible types:

```rust
fn largest<T>(list: &[T]) -> &T {
```

The `<T>` by the function name is what lets Rust know that we are trying to use Generics.

## In Struct Definition

Same as with functions, here is an example:

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}
```

Something to note is that you cannot have mismatched types in a single instance of a struct. For example:

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let wont_work = Point { x: 5, y: 4.0 };
}
```

To solve this, you can actually assign more than one generic type of a struct, like this:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```

Now the two fields are independent.

## In Enums

Here is the syntax for generics in `enum`:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}

```

Haha, as you can see, both `Option` and `Result` uses generics when defining their types. This lets them hold any types.

## In Method Definition

Here is some example code for implementing generics in methods:

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}
```

you need to put the `<T>` on the `impl` as well to show that you are going to be using generic in the method. You can have even greater control of the method, as you can see in the next code snippet:

```rust
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}

```

This snippet means that for specifically instances of `Point<f32>` will have access to this method while all other instance of `Point<T>` will not.

Finally, you can create methods that mixes generics around by labeling the generics for your current struct, and those for a different struct that is being used as an argument. For example:

```rust
struct Point<X1, Y1> {
    x: X1,
    y: Y1,
}

impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c' };

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```

Multiple generics are being used so that the method could have clear and verbose declaration. Note that `X2` and `Y2` are declared on the method and not the `impl` because they are only relevant to the specific method, and mostly as an argument.

## Performance

Good news is that it runs fast. This is because the compiler converts the generics into a concrete declaration at runtime. I love these compiler tricks!

## Citation
- [Rust Book](https://doc.rust-lang.org/book/ch10-01-syntax.html)