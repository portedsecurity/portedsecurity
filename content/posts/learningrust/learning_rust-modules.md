---
title: "Rust: Modules and Traits, Organizing Code and Shared Behavior"
date: 2026-06-29
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

Let's create a new project to learn how to structure a real-world codebase. Open your terminal and run:

```bash
cargo new code_organization
cd code_organization
```

## Organizing Files with Modules

In Rust, the module system allows you to split your code across different files and directories. A module is essentially a container for functions, structs, and traits.

Let's imagine we are building a server application, and we want a dedicated file strictly for handling our system logging.

### 1. Creating a New File

Inside your `src` directory, create a new file named `logger.rs`. Your folder structure should now look exactly like this:

```plaintext
src/
├── main.rs
└── logger.rs
```

### 2. Writing the Module Code

Open your newly created `src/logger.rs` file and add these two functions:

```rust
pub fn log_warning(message: &str) {
    println!("[WARNING]: {}", message);
}

// We add this attribute to tell the compiler not to warn us about this unused function
#[allow(dead_code)]
fn log_internal_error() {
    println!("[ERROR]: This function is private!");
}
```

Notice the **`pub`** keyword before `fn log_warning`. By default, everything in Rust (functions, structs, fields) is **private**. It cannot be accessed by other files. Adding `pub` makes the function public, allowing our main file to see and use it.

*Note: We also added `#[allow(dead_code)]` above our private function. Because we never actually call this private function in our project, the Rust compiler will aggressively throw a yellow warning in the terminal. This attribute politely tells the compiler we are leaving it there intentionally.*

### 3. Linking the Module

Now, open your `src/main.rs` file. We need to tell Rust that the `logger.rs` file exists and bring it into scope. Replace the default "Hello, world!" code with this:

```rust
// This tells Rust to look for a file named logger.rs and load it
mod logger; 

fn main() {
    println!("Starting the server...");

    // We use the double colon (::) to access functions inside the module
    logger::log_warning("Disk space is running low.");
}
```

Run `cargo run`. You will see your multi-file project compile perfectly. The `mod` keyword acts as a bridge, cleanly separating your logic into manageable pieces.

## Defining Shared Behavior with Traits

Now that our files are organized, let's talk about organizing our data logic.

Imagine you have two completely different Structs: a `User` and a `SystemAlert`. Even though these are different types of data, you might want them both to be "Printable" or "Loggable" so you can easily output their details to the terminal in a standard format.

In other languages (like Java or C#), you would use an "Interface." In Rust, we use a **Trait**. A Trait tells the Rust compiler about functionality a particular type *must* have.

In a massive production application, you would put these structs in their own separate files. However, for this tutorial, we are going to add all of the following code directly to your `src/main.rs` file so we can easily see how it all connects.

### 1. Defining the Data and the Trait

In `src/main.rs`, add the following code right below your `mod logger;` declaration:

```rust
struct User {
    username: String,
    role: String,
}

struct SystemAlert {
    alert_code: u32,
    urgency: String,
}

// We define the Trait and the function signatures it requires
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Notice that we don't write the actual code for the `summarize` function inside the trait; we just define the *signature* (its name, inputs, and outputs). We are creating a contract. Any struct that implements this trait *must* provide a `summarize` function returning a `String`.

### 2. Implementing the Trait

Continue in `src/main.rs` by implementing our new `Summary` trait for both of the structs. Add this right below the trait definition:

```rust
impl Summary for User {
    fn summarize(&self) -> String {
        format!("USER: {} (Role: {})", self.username, self.role)
    }
}

impl Summary for SystemAlert {
    fn summarize(&self) -> String {
        format!("ALERT [{}]: Level {}", self.alert_code, self.urgency)
    }
}
```

*(Note: We use the `format!` macro here. It works exactly like `println!`, but instead of printing to the terminal, it returns the formatted `String`.)*

### 3. Traits as Parameters

The true power of Traits is that you can write functions that accept *any* struct, as long as that struct implements a specific trait. Add this helper function to `src/main.rs`, right above your `main` function:

```rust
// This function doesn't care if you pass it a User or a SystemAlert.
// It only cares that the item implements the Summary trait.
fn print_to_dashboard(item: &impl Summary) {
    println!("DASHBOARD UPDATE: {}", item.summarize());
}
```

## Putting It All Together

To make sure everything is in the right place, here is exactly what your complete, final `src/main.rs` file should look like. It brings together the external logger module, our structs, the shared trait, and tests them all in the `main` function:

```rust
mod logger; 

struct User {
    username: String,
    role: String,
}

struct SystemAlert {
    alert_code: u32,
    urgency: String,
}

pub trait Summary {
    fn summarize(&self) -> String;
}

impl Summary for User {
    fn summarize(&self) -> String {
        format!("USER: {} (Role: {})", self.username, self.role)
    }
}

impl Summary for SystemAlert {
    fn summarize(&self) -> String {
        format!("ALERT [{}]: Level {}", self.alert_code, self.urgency)
    }
}

fn print_to_dashboard(item: &impl Summary) {
    println!("DASHBOARD UPDATE: {}", item.summarize());
}

fn main() {
    println!("Starting the server...\n");

    // Using our external module
    logger::log_warning("Disk space is running low.\n");

    let admin = User {
        username: String::from("SysAdmin_Bob"),
        role: String::from("Administrator"),
    };

    let cpu_warning = SystemAlert {
        alert_code: 404,
        urgency: String::from("Critical"),
    };

    // Testing the direct summarize calls
    println!("--- Direct Trait Calls ---");
    println!("{}", admin.summarize());
    println!("{}", cpu_warning.summarize());

    // Testing the function that accepts the Trait
    println!("\n--- Trait Parameter Calls ---");
    print_to_dashboard(&admin);
    print_to_dashboard(&cpu_warning);
}
```

Run `cargo run` one last time. You will see both the logger module and the trait logic execute.

## Summary

As you prepare to write larger applications, remember these structural pillars:

- Use **`mod`** to pull in code from other files.

- Use **`pub`** to control exactly what functions and data are visible to the rest of the application (privacy by default).

- Use **Traits** to define shared capabilities (like a contract) across entirely different custom data types.

You now have the complete Rust toolkit. You know how to manage memory, handle errors, build custom data, and organize your files.

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
