# Enums, pattern matching, Collections, Error handling

Source: [Rust Book](https://doc.rust-lang.org/book/ch06-00-enums.html)

## Enums

With enums you can define a type that has multiple possible variants. We'll first look at how enums can be defined, then we'll look at a special enum, called Option, which which expresses that a value can be either something or nothing. Then we’ll look at how pattern matching in the match expression makes it easy to run different code for different values of an enum. Finally, we’ll cover how the if let construct is another convenient and concise idiom available to you to handle enums in your code.

Enums are a feature in many languages, but their capabilities differ in each language. Rust’s enums are most similar to algebraic data types in functional languages, such as F#, OCaml, and Haskell.

*Enum examples*

### The Option enum

The Option type is used in many places because it encodes the very common scenario in which a value could be something or it could be nothing. Expressing this concept in terms of the type system means the compiler can check whether you’ve handled all the cases you should be handling; this functionality can prevent bugs that are extremely common in other programming languages.

Programming language design is often thought of in terms of which features you include, but the features you exclude are important too. Rust doesn’t have the null feature that many other languages have. Null is a value that means there is no value there. In languages with null, variables can always be in one of two states: null or not-null.

In his 2009 presentation “Null References: The Billion Dollar Mistake,” Tony Hoare, the inventor of null, has this to say:

> I call it my billion-dollar mistake. At that time, I was designing the first comprehensive type system for references in an object-oriented language. My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn’t resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.

The problem with null values is that if you try to use a null value as a not-null value, you’ll get an error of some kind. Because this null or not-null property is pervasive, it’s extremely easy to make this kind of error.

As such, Rust does not have nulls, but it does have an enum that can encode the concept of a value being present or absent. This enum is `Option<T>`, and it is defined by the standard library as follows:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

The `<T>` syntax is a feature of Rust we haven’t talked about yet. It’s a generic type parameter. 

Here's some basic usage of the option type. 

*Option example*

In order to add these values you have to convert an `Option<T>` to a T. Generally, this helps catch one of the most common issues with null: assuming that something isn’t null when it actually is.

## Pattern matching

One of Rust's most important control flow operators is called match, which allows you to compare some value against a series of pattens and then execute code based on which pattern matches. Patterns can be made up of literal values, variable names, wildcards, and many other things; Chapter 18 covers all the different kinds of patterns and what they do. The power of match comes from the expressiveness of the patterns and the fact that the compiler confirms that all possible cases are handled.

*Pattern example*

### Concise control flow with `if` `let`

The if let syntax lets you combine if and let into a less verbose way to handle values that match one pattern while ignoring the rest. Without it we might do something like this.

*If let example*

### Error handling

Rust groups errors into two major categories: recoverable and unrecoverable errors. For a recoverable error, such as a file not found error, it’s reasonable to report the problem to the user and retry the operation. Unrecoverable errors are always symptoms of bugs, like trying to access a location beyond the end of an array.

Rust doesn’t have exceptions. Instead, it has the type `Result<T, E>` for recoverable errors and the panic! macro that stops execution when the program encounters an unrecoverable error.

Result enum is defined as having two variants, Ok and Err, as follows:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```
*Error example*

### Shortcuts for panic on error

Using match can sometimes be a bit verbose, and doesn't always express the intent of a code block well. In some cases we may prefer to use the `unwrap()` or `expect()` methods. 

The Result<T, E> type has many helper methods defined on it to do various tasks. One of those methods, called unwrap, is a shortcut method. 

*Shortcut example*

### The panic macro

We briefly above saw the `panic!` macro. This macro is used to generate an unrecoverable error. Here is an example. 

*Panic example*

### Error propogation

When writing a function whose implementation may fail, it is generally advised to return the error to the calling code as opposed to handling it in the function, so the calling code has more control. This is known as propogating the error. 

*Propagation example*

### Panic vs Result

How do you know when to call `panic!` and when to return a `Result`? When you choose to return a Result value, you give the calling code options rather than making the decision for it. The calling code could choose to attempt to recover in a way that’s appropriate for its situation, or it could decide that an Err value in this case is unrecoverable, so it can call panic! and turn your recoverable error into an unrecoverable one. Therefore, returning Result is a good default choice when you’re defining a function that might fail.

## Errors in collections

Rust’s standard library includes a number of very useful data structures called collections. Most other data types represent one specific value, but collections can contain multiple values. Unlike the built-in array and tuple types, the data these collections point to is stored on the heap, which means the amount of data does not need to be known at compile time and can grow or shrink as the program runs. 

We've already spent a lot of time using the `Vec` type in the class examples and projects. As a reminder, vectors can be initialized in multiple ways:

*Collections example*
