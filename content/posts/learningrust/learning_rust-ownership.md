---
title: "Rust: Ownership Explained, Memory Safety"
date: 2026-06-09
draft: false
description: "Discover Rust's defining feature: Ownership. Learn how Rust guarantees memory safety, prevents access violations, and eliminates the need for a garbage collector."
tags: ["Rust", "programming", "software development", "ownership", "borrow checker", "memory management", "systems programming"]
categories: ["Development", "Tutorials", "Rust", "Programming"]
series: ["Learning Rust"]
author: "Gregory Bryant"
meta_title: "Rust Ownership Explained"
focus_keyword: "Rust Ownership Explained"

---

If you have ever spent hours digging through Windows Error Reporting logs trying to decipher a sudden BEX64 crash, or watched an application completely lock up due to a mysterious memory access violation, you have been the victim of poor memory management.

In traditional systems programming languages like C or C++, developers have to manually allocate and free memory. If they forget to free it, the program bloats and crashes (a memory leak). If they free it too early and try to use it again, the application violently terminates (a use-after-free error, often throwing that dreaded `0xc0000005` exception code).

Other languages, like C#, Python or JavaScript, solve this by using a "Garbage Collector" an automated background process that cleans up unused memory. While safe, this process periodically pauses your program, which is unacceptable for high-performance applications like game engines or low-latency remote streaming clients.

Rust introduces a third, revolutionary paradigm: **Ownership**. It guarantees memory safety at compile time without a garbage collector. Let's explore how it works.

## The Three Rules of Ownership

To understand Ownership, you only need to memorize three fundamental rules. The Rust compiler enforces these rules relentlessly.

1. **Each value in Rust has a variable that is called its \*owner\*.**
2. **There can only be one owner at a time.**
3. **When the owner goes out of scope, the value will be dropped (its memory is freed).**

Let's see what this looks like in practice. Create a new project (`cargo new ownership`), open `src/main.rs`, and let's write some code.

## The Concept of "Moving" Data

Primitive data types of a known, fixed size (like the integers and booleans we covered in Step 2) are stored on the **Stack**. They are incredibly fast to access and are easily copied.

However, complex data that can grow or shrink (like a dynamic text `String`) is stored on the **Heap**. Rust handles Heap data very differently.

Look at this example:

```rust
fn main() {
    let original_string = String::from("Hello, World!");

    // We assign the value to a new variable
    let new_string = original_string;

    // What happens if we try to use the original variable?
    println!("{}", original_string); 
}
```

Move into the project by running `cd .\ownership\` and run `cargo run`, the compiler will output a lot of information. Here is exactly what you will see:

```plaintext
warning: unused variable: `new_string`
 --> src\main.rs:5:9
  |
5 |     let new_string = original_string;
  |         ^^^^^^^^^^ help: if this is intentional, prefix it with an underscore: `_new_string`
  |
  = note: `#[warn(unused_variables)]` (part of `#[warn(unused)]`) on by default

error[E0382]: borrow of moved value: `original_string`
 --> src\main.rs:8:20
  |
2 |     let original_string = String::from("Hello, World!");
  |         --------------- move occurs because `original_string` has type `String`, which does not implement the `Copy` trait
...
5 |     let new_string = original_string;
  |                      --------------- value moved here
...
8 |     println!("{}", original_string);
  |                    ^^^^^^^^^^^^^^^ value borrowed here after move

For more information about this error, try `rustc --explain E0382`.
warning: `ownership` (bin "ownership") generated 1 warning
error: could not compile `ownership` (bin "ownership") due to 1 previous error; 1 warning emitted
```

### Breaking Down the Output

Cargo reads top-to-bottom and reports everything it finds, ending with a summary of the damage.

**1. The Warning (`unused variable`)** Rust hates "dead code." It noticed that we created `new_string` but never actually used it anywhere. The `note` simply tells you that this specific warning is turned on by default. As the compiler helpfully suggests, if you want to create a variable and intentionally not use it yet, you prefix it with an underscore: `let _new_string = original_string;`.

**2. The Fatal Error (`E0382`)** Directly below the warning is the real issue—the red error that stopped the build.

In other languages, `new_string` would either become a complete duplicate of the data, or both variables would act as pointers looking at the same piece of memory.

Rust says **no**. According to Rule #2, there can only be *one owner at a time*. When we wrote `let new_string = original_string;`, Rust transferred the ownership. The `original_string` variable was completely invalidated to prevent accidental double-free errors later on. We call this a **Move**.

**3. The Summary** The final three lines tell you exactly how to get more help (`rustc --explain E0382` is an incredibly useful command to run in your terminal for deep dives into specific errors) and summarizes that the build failed due to 1 error and 1 warning.

## Borrowing: How to Share Data

If moving data invalidates the old variable, how do we ever pass data around to different functions? What if we just want to look at the data without taking ownership of it?

The answer is **Borrowing**. You can borrow a value by creating a **Reference** to it, using the ampersand (`&`) symbol.

Let's fix our broken code:

```rust
fn main() {
    let original_string = String::from("Hello, World!");

    // We pass a reference using '&' instead of moving the value
    let borrowed_string = &original_string;

    // Now both can be used!
    println!("The original is still valid: {}", original_string);
    println!("The borrowed reference sees: {}", borrowed_string);
}
```

By adding the `&`, `borrowed_string` does not take ownership. It merely points to the data owned by `original_string`. Because it doesn't own the data, when `borrowed_string` goes out of scope, the memory is not dropped.

## The Borrow Checker in Action

You can also pass references into functions, which is the most common way to handle data in Rust.

```rust
fn main() {
    let my_name = String::from("Rustacean");

    // We pass a reference (&) to the function
    calculate_length(&my_name);

    // Because we only passed a reference, we still own my_name!
    println!("I still have my name: {}", my_name);
}

// The function signature explicitly expects a reference (&String)
fn calculate_length(text: &String) {
    let length = text.len();
    println!("The length of '{}' is {}.", text, length);
}
```

The system that enforces these rules is called the **Borrow Checker**. It is famously strict. If you try to modify a borrowed value without explicit permission (using `&mut`), or if you try to borrow a value that has already been dropped, the Borrow Checker will stop your program from compiling.

### Embracing the Strictness

Fighting the Borrow Checker is a rite of passage for every new Rust developer. When the compiler yells at you, it is not being pedantic; it is actively preventing a memory access violation that would have crashed your application in production.

Once you internalize the rules of Ownership and Borrowing, you will write code that is not only blazingly fast but mathematically guaranteed to be memory-safe.

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

[Rust: CLI Tutorial, Building Your First Command Line Tool]({{< relref "learning_rust-cli.md" >}})
