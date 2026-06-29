---
title: "Rust: Modules and Traits, Organizing Code and Shared Behavior"
date: 2026-06-10
draft: false
description: "Stop writing massive single-file programs. Learn how to organize your Rust code using modules, manage visibility with 'pub', and share behavior using Traits."
categories: ["Rust", "Programming"]
tags: ["Rust basics", "modules", "traits", "interfaces", "code organization", "software development"]

series: ["Learning Rust"]
author: "Gregory Bryant"
meta_title: "Rust: Modules and Traits"
focus_keyword: "Rust Modules and Traits"

---

When you first start learning to program, it is perfectly normal to dump all your variables, functions, and structs into a single file. But as your application grows from a small script into a fully-fledged system, a 1,000-line `main.rs` file becomes a nightmare to navigate.

To build scalable software, you need two things: a way to split your code into multiple files, and a way to define shared behaviors across different custom data types.

In Rust, we achieve this using **Modules** and **Traits**.

Let's create a new project to learn how to structure a real-world codebase:

```bash
cargo new code_organization
cd code_organization
```

## Organizing Files with Modules

In Rust, the module system allows you to split your code across different files and directories. A module is essentially a container for functions, structs, and traits.

Let's imagine we are building a server application, and we want a dedicated file to handle our system logging.

### 1. Creating a New File

Inside your `src` directory, create a new file named `logger.rs`. Your folder structure should look like this:

```plaintext
src/
├── main.rs
└── logger.rs
```

### 2. Writing the Module Code

Open `logger.rs` and add a simple function:

```rust
// src/logger.rs

pub fn log_warning(message: &str) {
    println!("[WARNING]: {}", message);
}

fn log_internal_error() {
    println!("[ERROR]: This function is private!");
}
```

Notice the **`pub`** keyword before `fn log_warning`. By default, everything in Rust (functions, structs, fields) is **private**. It cannot be accessed by other files. Adding `pub` makes the function public, allowing our main file to see and use it.

### 3. Linking the Module to `main.rs`

Now, open your `src/main.rs` file. We need to tell Rust that the `logger.rs` file exists and bring it into scope.

```rust
// src/main.rs

// This tells Rust to look for a file named logger.rs (or a folder named logger)
mod logger; 

fn main() {
    println!("Starting the server...");

    // We use the double colon (::) to access functions inside the module
    logger::log_warning("Disk space is running low.");

    // logger::log_internal_error(); // Un-commenting this would cause a compile error!
}
```

Run `cargo run` and you will see your multi-file project compile perfectly. The `mod` keyword acts as a bridge, cleanly separating your logic into manageable pieces.

## Defining Shared Behavior with Traits

Now that our files are organized, let's talk about organizing our data logic.

Imagine you have two completely different Structs: a `User` and a `SystemAlert`.

```rust
struct User {
    username: String,
    role: String,
}

struct SystemAlert {
    alert_code: u32,
    urgency: String,
}
```

Even though these are different types of data, you might want them both to be "Printable" or "Loggable" so you can easily output their details to the terminal in a standard format.

In other languages (like Java or C#), you would use an "Interface." In Rust, we use a **Trait**. A Trait tells the Rust compiler about functionality a particular type *must* have.

### 1. Defining the Trait

Let's define a Trait called `Summary`. Add this to your `src/main.rs`:

```rust
// We define the Trait and the function signatures it requires
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Notice that we don't write the actual code for the function here; we just define the *signature* (its name, inputs, and outputs). We are creating a contract. Any struct that implements this trait *must* provide a `summarize` function returning a `String`.

### 2. Implementing the Trait

Now, let's implement our `Summary` trait for both of our structs:

```rust
// Implementing Summary for the User struct
impl Summary for User {
    fn summarize(&self) -> String {
        format!("USER: {} (Role: {})", self.username, self.role)
    }
}

// Implementing Summary for the SystemAlert struct
impl Summary for SystemAlert {
    fn summarize(&self) -> String {
        format!("ALERT [{}]: Level {}", self.alert_code, self.urgency)
    }
}
```

*(Note: We use the `format!` macro here. It works exactly like `println!`, but instead of printing to the terminal, it returns the formatted `String`.)*

### 3. Using the Trait

Now both distinct structs share a guaranteed behavior. Let's test it in our `main` function:

```rust
fn main() {
    let admin = User {
        username: String::from("SysAdmin_Bob"),
        role: String::from("Administrator"),
    };

    let cpu_warning = SystemAlert {
        alert_code: 404,
        urgency: String::from("Critical"),
    };

    // Because both implement the Summary trait, we know we can call .summarize()
    println!("{}", admin.summarize());
    println!("{}", cpu_warning.summarize());
}
```

```rust
fn main() {
    let admin = User {
        username: String::from("SysAdmin_Bob"),
        role: String::from("Administrator"),
    };

    let cpu_warning = SystemAlert {
        alert_code: 404,
        urgency: String::from("Critical"),
    };

    // Because both implement the Summary trait, we know we can call .summarize()
    println!("{}", admin.summarize());
    println!("{}", cpu_warning.summarize());
}
```

The Output:

```plaintext
USER: SysAdmin_Bob (Role: Administrator)
ALERT [404]: Level Critical
```

## Traits as Parameters

The true power of Traits is that you can write functions that accept *any* struct, as long as that struct implements a specific trait.

```rust
// This function doesn't care if you pass it a User or a SystemAlert.
// It only cares that the item implements the Summary trait.
fn print_to_dashboard(item: &impl Summary) {
    println!("DASHBOARD UPDATE: {}", item.summarize());
}
```

This allows you to write incredibly flexible, reusable code. You can add ten new structs to your application later, and as long as you implement the `Summary` trait for them, `print_to_dashboard` will be able to handle them without any modifications.

## Summary

As you prepare to write larger applications, remember these structural pillars:

- Use **`mod`** to pull in code from other files.

- Use **`pub`** to control exactly what functions and data are visible to the rest of the application (privacy by default).

- Use **Traits** to define shared capabilities (like a contract) across entirely different custom data types.

You now have the complete Rust toolkit. You know how to manage memory, handle errors, build custom data, and organize your files.

Learning Rust

Continue your journey with the next step or visit a previous post you may have missed

[Rust: Setting up your development environment]({{< relref "learning_rust-setup.md" >}})

[Rust: Cargo and the Anatomy of "Hello, World"]({{< relref "learning_rust-cargo.md" >}})

[Rust: Variables and Immutability]({{< relref "learning_rust-vars.md" >}})

[Rust: Control Flow, Making Decisions with If, Else, and Loops]({{< relref "learning_rust-flow.md" >}})

[Rust: Ownership Explained, Memory Safety]({{< relref "learning_rust-ownership.md" >}})

[Rust: Structs and Enums, Modeling Custom Data]({{< relref "learning_rust-structs.md" >}})

[Rust: Pattern Matching, Mastering the Match Operator]({{< relref "learning_rust-pattern.md" >}})

[Rust: Error Handling, Ditching Exceptions for Result and Option]({{< relref "learning_rust-error.md" >}})
