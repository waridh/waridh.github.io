# Traits

## Defining shared behavior

Traits define a functionality a particular type has, and can be shared with other types. Defining shared behavior in an abstract way. We can use *trait bounds* to define a generic to be any type with that certain behaviour.

## Defining a `trait`

A behavior of a type consists of the methods that we call on that type. Different types share the same behavior if we call the same method on all those types. Trait definition are a good way to group method signature together to define a set of behaviors to accomplish some purpose.

Lets say that you have a group of structs that hold different types and amount of texts. `NewsArticle` hold news story from a particular location and `Tweets` are 280 words long, with metadata to indicate what type of post the tweet is.

## Citation
- [Rust book](https://doc.rust-lang.org/book/ch10-02-traits.html)