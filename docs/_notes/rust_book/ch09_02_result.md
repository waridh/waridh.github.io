# Recoverable Errors with Result

This is us installing error handling for easy errors that can be dealt with. So, `Result` is actually an `enum`, just like `Option`. It is defined as follows:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}

```

You can see it has two state, `Ok(T)` and `Err(E)`. Both `E` and `T` are just generic type parameters, it could really just be anything.

The success case will return an `Ok(T)` and a failure case will return an `Err(E)`.

Here is an example that uses a function that returns a `Result` type in the following code snippet.

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");
}
```

`File::open()` returns a `Result<T, E>`. The `T` is `std::fs::File`, which is the standard file handle. The `E` in this case is a `std::io::Error` type. Using this system, functions will now have an error handling system that isn't just returning either a 0 or a -1. This makes the language more clear and concise.

So after making the error handling, the code block should look like this:

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}", error),
    };
}
```

Just like the `Option` `enum`, there is no need to specify the `enum` type for `Result`.

## Matching on Different Errors

If we have different error types that could occur, we could create a nested `match` chain to handle those cases. Here is a code example that does just that:

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => {
                panic!("Problem opening the file: {:?}", other_error);
            }
        },
    };
}
```

`ErrorKind` is an enum from the standard library, and allows you to determine the type of error. This is useful when we are trying to specifically handle the case when the file is not found and you want to handle that case.

## Alternative to using `match`

`match` can be verbose and doesn't communicate the intent very well. There are helper methods for the `Result<T, E>` type which can let us deal with passing value more concisely.

### `unwrap`

The unwrap method will return the value of the `Result` contains an `Ok`, and if it has an `Err`, it will `panic!`.

### `expect`

The expect method lets us choose the `panic!` error message, and otherwise works like `unwrap`. Honestly, `expect` is just better `unwrap`.

## Propagating Errors

When a function calls something that could fail, instead of letting the function handle it, let the program calling that function handle it instead would allow for better modularity. This is what happens in the POSIX protocol. Here is an example of propagating error being implemented:

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    // By just opening it, you will get the Result
    let username_file_result = File::open("hello.txt");

    // This is getting the content returned from the Result
    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e), // Just returning to error back up a layer
    };

    // Allocating the username string.
    let mut username = String::new();

    // Returning the thing. So we are seeing if the method was successful in writing to the username. I guess the File library is just rearlly robust like that.
    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e), // Also return the error back up a layer
    }
}

```

### `?`

Epic shortcut for error propagation. Here is a code that does the same thing as the code block above, but in way less lines:

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

From what I see, I think that the `?` will either let the value stored in the `Ok` be used normally, or it will return an `Err`.

The difference between `?` and `match` is `?` will only return a single error type per function, while `match` could be much more fine grain.

#### Where can `?` be used?

You can actually use `?` on `Option<T>` as well. You can only use `?` with `Option<T>` on a function that returns an `Option<T>`. Basically, if the value is `None`, then the function will just return that and take itself off the stack. If the value is `Some`, then the value inside is returned to the function and the function continues.

Something you should be careful about is that `?` will not automatically convert between `Result<T, E>` and `Option<T>`.

#### Doing a funny with main()

So far the `main()` we've worked with returns a `()`. The main function is special because it is where the program enters and exits, and thus there is a restriction on what the `return` type can be.

`main()` can also return a `Result<(), E>`. Here is a code example having a return type of `Result<(), Box<dyn Err>>`:

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

`Box<dyn Error>` is a trait object. For now it means any kind of error. Using `?` on a `Result` in main will now return that specific error without discriminating. Well, we are back to `C` and if the program exits on `Ok()` then the program will end with exit code 0, and if you exit with `Box<dyn Error>`, it will exit with a non-zero integer for C errors.

## Citation

- [Rust Book link](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html)