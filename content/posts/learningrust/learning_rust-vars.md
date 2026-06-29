---

title: "Rust: Variables and Immutability"
date: 2026-06-03
draft: false
description: "Learn how Rust handles variables, why immutability is the default, and how using the mut keyword and shadowing can lead to safer, faster systems programming."
tags: ["Rust", "variables", "immutability", "programming", "software development"]
categories: ["Development", "Tutorials", "Rust", "Programming"]
series: ["Learning Rust"]
author: "Gregory Bryant"
meta_title: 'Rust Variables and Immutability'
focus_keyword: "rust variables and immutability"

---

If you are coming to Rust from languages like Python, JavaScript, or C++, you are used to variables acting exactly as their name implies: they vary. You declare a variable, assign it a value, and change that value whenever the program's logic dictates.

Rust takes a different, highly intentional approach. In Rust, variables are immutable by default. Once a value is bound to a name, you cannot change it.

This might feel restrictive at first, but it is one of the foundational design choices that allows Rust to guarantee memory safety and thread safety without sacrificing speed. Let's explore how variables work in Rust, why immutability is the default, and how to safely bypass it when you need to.

### Initializing the Project

Open up VS Code, open your Rust Projects folder, and open a new terminal. In the terminal type:

```bash
cargo new variables
```

This will create a new folder to use for testing out how variables and immuntailty work.

Navigate into your newly created directory:

```bash
cd variables
```

Open the src/main.rs file. This is where we will write all the code for this tutorial.

## The Default State: Locked Down

Let's test Rust's default behavior. Replace the generated "Hello, world!" code in your src/main.rs file with the following:

```rust
fn main() {
    let base_damage = 500;
    println!("The base damage is: {}", base_damage);
}
```

Now, try to compile and execute the program by running this command in your terminal:

```bash
cargo run
```

This compiles and runs perfectly. But what happens if we try to modify base_damage further down in the code?

```rust
fn main() {
    let base_damage = 500;
    println!("The base damage is: {}", base_damage);

    // Attempting to update the value
    base_damage = 600; 
    println!("The modified damage is: {}", base_damage);
}
```

If you run cargo check or cargo run on this, the Rust compiler will stop you in your tracks with a highly descriptive error[E0384]: cannot assign twice to immutable variable.

## Why Does Rust Do This?

Imagine you are building a complex, high performance application like a game engine or a remote streaming client. You have a variable tracking a user's unique connection ID. If a rogue function deep in your codebase accidentally modifies that ID, the application might silently drop the connection or crash entirely.

By making variables immutable by default, Rust eliminates an entire category of bugs where data changes unpredictably. When you read a piece of Rust code and see a standard let binding, you have a compiler backed guarantee that the value will never change unexpectedly

## Breaking the Rules Safely: The mut Keyword

Obviously, software needs to manipulate some variables. A game where a player's health points can never drop is not much of a game. When you need a variable to change, you must explicitly tell the compiler by adding the **mut** (mutable) keyword.

Update your src/main.rs file to look like this:

```rust
fn main() {
    let mut current_health = 1000;
    println!("Player health: {}", current_health);

    // The player takes damage
    current_health = 850; 
    println!("Player took damage! Health is now: {}", current_health);
}
```

Run the code again:

```bash
cargo run
```

The Output:

```plaintext
Player health: 1000
Player took damage! Health is now: 850
```

By requiring **mut**, Rust forces you to communicate your intent. It acts as a clear sign to anyone reading the code that this specific piece of data is expected to change over time.

## Constants vs. Immutable Variables

You might be wondering: If an immutable variable can't be changed, isn't it just a constant? Not quite. While they share similarities, Rust has a separate **const** keyword, and the two are handled differently under the hood:

+ You cannot use **mut** with **const**. Constants are not just immutable by default; they are immutable always.
+ Type annotations are mandatory. You must explicitly state the data type (e.g., u32 for an unsigned 32-bit integer).
+ Evaluation time. Constants can only be set to a constant expression, not the result of a value computed at runtime.

You can define constants at the global scope, outside of your main function:

Update your src/main.rs file to look like this:

```rust
// Constants are typically written in ALL_CAPS with underscores
const MAX_SERVER_CAPACITY: u32 = 10_000; 

fn main() {
    println!("Server capacity is locked at: {}", MAX_SERVER_CAPACITY);
}
```

Run the code again:

```bash
cargo run
```

The Output:

```plaintext
Server capacity is locked at: 10000
```

Constants are useful for hardcoded values that remain static throughout the entire lifecycle of your application, like a maximum frame rate or a physics gravity multiplier.

## The Art of Shadowing

Rust offers a unique feature called shadowing, which allows you to declare a new variable with the same name as a previous variable.

Try running this final block of code in your main.rs file:

```rust
fn main() {
    let x = 5;

    // We shadow the first 'x' by using 'let' again
    let x = x + 1; 

    {
        // Shadowing within a nested scope
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}"); // Prints 12
    }

    println!("The value of x is: {x}"); // Prints 6
}
```

If you run cargo run, you will see that the inner scope prints 12, and the outer scope prints 6.

## Shadowing vs. Mutability

Why use shadowing instead of just making the variable mutable?

+ Preserving Immutability: When you shadow a variable using let, you perform a transformation on the value, but the new variable remains immutable after the transformation is complete.
+ Changing Types: Shadowing allows you to reuse a variable name even if the data type changes.

For example, if you prompt a user for input, you often receive it as a String. If you need to parse that into an integer, shadowing allows you to reuse the same clean variable name instead of inventing arbitrary names like user_input_str and user_input_int.

```rust
// Starting with a String
let spaces = "   "; 
// Shadowing it to become a number (usize)
let spaces = spaces.len();
```

If you tried to do this with **mut**, the compiler would throw an error because you are not allowed to mutate a variable's type, only its value.

## Summary

Understanding how Rust handles data is your first step toward mastering systems programming:

+ **let**: Immutable by default. Safe, predictable, and prevents unwanted side effects.
+ **let mut**: Explicit mutability. Signals that state will change.
+ **const**: Hardcoded, strictly typed values known at compile time.
+ Shadowing (**let** again): Transforms a value or changes its type while keeping the same name.

Embrace the immutability default. It might take a little extra typing upfront, but it pays massive dividends by preventing logic bugs from ever making it to production.

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