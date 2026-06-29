---
title: "Rust: Collections, Mastering Vectors and Strings"
date: 2026-06-25
draft: false
description: "Learn how to store dynamic lists of data in Rust using Vectors and Strings. Understand heap allocation, memory safety, and common collection methods."
categories: ["Rust", "Programming"]
tags: ["Rust basics", "collections", "vectors", "strings", "heap memory", "software development"]
series: ["Learning Rust"]
author: "Gregory Bryant"
meta_title: 'Rust Collections Mastering Vectors and Strings'
focus_keyword: "Mastering Vectors and Strings"
---

So far in this series, we have worked with data types that have a fixed size known at compile time, like a `u32` integer or a boolean. These are fast and stored neatly on the "Stack."

But what if you are building an application where you don't know how much data you will have until the program is actually running? If you are building a To-Do list, you don't know if the user will add 3 items or 300.

For dynamic, changing data, Rust provides **Collections**. These are stored on the "Heap," meaning they can expand and contract at runtime. Today, we will look at the two most common collections: **Vectors** and **Strings**.

Let's set up a new project:

```bash
cargo new collections_guide
cd collections_guide
```

## Vectors: The Growable Array

A Vector (`Vec<T>`) allows you to store a list of items of the same type next to each other in memory. It is similar to an Array in JavaScript or a List in Python.

Open your `src/main.rs` and let's create our first Vector.

### Creating and Modifying a Vector

Because a Vector is designed to grow, you will almost always declare it as mutable (`mut`). You can create a new, empty Vector using the `Vec::new()` function.

```rust
fn main() {
    // Create a new, empty Vector that holds Strings
    let mut tasks: Vec<String> = Vec::new();

    // Adding items using the .push() method
    tasks.push(String::from("Buy groceries"));
    tasks.push(String::from("Finish Rust tutorial"));
    tasks.push(String::from("Walk the dog"));

    println!("I have {} tasks on my list.", tasks.len());
}
```

If you run `cargo run`, it will output: `I have 3 tasks on my list.`

*Note: Because Rust is strictly typed, a `Vec<String>` can only hold Strings. You cannot push a `u32` into it.*

### The `vec!` Macro Shortcut

If you already know exactly what data you want to put in your Vector when you create it, you can use the `vec!` macro for a much cleaner syntax:

```rust
fn main() {
    // The vec! macro infers the type automatically
    let mut high_scores = vec![10500, 9800, 7200];

    // You can still push to it later!
    high_scores.push(11200); 

    println!("The scores are: {:?}", high_scores);
}
```

### Reading Safely from a Vector

How do we get data *out* of a Vector? You might be tempted to use index notation (e.g., `high_scores[0]` to get the first item).

While you can do that, it is dangerous. If you ask for `high_scores[100]` and there are only 4 items, your program will instantly panic and crash (an "out of bounds" error).

Instead, Rust provides a safer method using the `.get()` function. `.get()` returns an `Option` enum, which we learned about in the last post!

```rust
fn main() {
    let servers = vec!["US-East", "EU-West", "Asia-Pacific"];

    // .get() takes the index and returns an Option
    let request = servers.get(5);

    match request {
        Some(server_name) => println!("Connecting to {}...", server_name),
        None => println!("Error: Server index out of bounds!"),
    }
}
```

Because we used `.get()` and a `match` statement, asking for a non-existent item gracefully prints an error instead of crashing the application.

## Strings: A Specialized Collection

You have been using `String::from()` throughout this entire series, but did you know that a `String` is actually just a specialized Collection? Under the hood, a `String` is essentially a Vector of bytes (`Vec<u8>`) that has been verified to be valid UTF-8 text.

Because it behaves like a Vector, you can push and modify it dynamically.

```rust
fn main() {
    // Creating a mutable, empty String
    let mut system_log = String::new();

    // Appending a string slice
    system_log.push_str("System Booting...");

    // Appending a single character
    system_log.push('\n'); 

    system_log.push_str("Loading Modules...");

    println!("{}", system_log);
}
```

### String Slices (`&str`) vs. `String`

This is a major tripping point for beginners, so let's clarify the difference between the two text types in Rust:

1. **`String`**: Growable, mutable, stored on the Heap, and *owned* by a variable. (Created via `String::from()` or `String::new()`).

2. **`&str` (String Slice)**: Fixed size, immutable, and usually a *reference* to a piece of data stored elsewhere.

When you write hardcoded text like `let msg = "Hello";`, that is a string slice (`&str`). It is baked directly into the final executable file of your program. It cannot grow or change.

If you need to modify text, always convert it to an owned `String`.

## Looping Over Collections

The most common thing you will do with a collection is loop through it to process every item. We do this using the `for` loop we learned in Step 3.

```rust
fn main() {
    let mut system_alerts = vec![
        String::from("Disk Space Low"),
        String::from("Updates Available"),
    ];

    // We loop over a REFERENCE to the vector (&system_alerts)
    // so the loop doesn't take ownership of the data.
    for alert in &system_alerts {
        println!("ALERT: {}", alert);
    }

    // Because we only passed a reference, we can still use the vector here!
    system_alerts.clear(); 
}
```

## Summary

When you don't know the exact size of your data at compile time, Collections are your go-to tools:

- **Vectors (`Vec<T>`)** store dynamic lists of items of the same type.

- **Strings (`String`)** are essentially Vectors of verified text that can grow and change.

- Always try to access Vector elements safely using `.get()` to prevent "out of bounds" crashes.

- Loop over Collections using references (`&`) to avoid accidentally transferring Ownership.

With control flow, error handling, and now dynamic data storage under your belt, you are ready to start writing highly capable applications. In the next post, we will look at how to organize all this code using Traits and Modules.

## Learning Rust

Continue your journey with the next step or visit a previous post you may have missed

[Rust: Setting up your development environment]({{< relref "learning_rust-setup.md" >}})

[Rust: Cargo and the Anatomy of "Hello, World"]({{< relref "learning_rust-cargo.md" >}})

[Rust: Variables and Immutability]({{< relref "learning_rust-vars.md" >}})

[Rust: Control Flow, Making Decisions with If, Else, and Loops]({{< relref "learning_rust-flow.md" >}})

[Rust: Ownership Explained, Memory Safety]({{< relref "learning_rust-ownership.md" >}})

[Rust: Structs and Enums, Modeling Custom Data]({{< relref "learning_rust-structs.md" >}})

[Rust: Pattern Matching, Mastering the Match Operator]({{< relref "learning_rust-pattern.md" >}})

[Rust: Error Handling, Ditching Exceptions for Result and Option]({{< relref "learning_rust-error.md" >}})

[Rust: Collections, Mastering Vectors and Strings]({{< relref "learning_rust-collections.md" >}})

[Rust: Modules and Traits, Organizing Code and Shared Behavior]({{< relref "learning_rust-modules.md" >}})
