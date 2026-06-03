+++
title = 'Cargo and the Anatomy of "Hello, World"'
date = 2026-06-02T19:00:00-04:00
draft = false
description = "Dive into your first Rust program and Cargo file"
tags = ["rust", "cargo", "hello world", "programming", "software development"]
categories = ["Development", "Tutorials", "Rust", "Programming"]
series = ["Learning Rust"]
author = "Gregory Bryant"
meta_title = 'Cargo and the Anatomy of "Hello, World"'
focus_keyword = "rust cargo"
+++

Welcome to day one of writing Rust. You are stepping into a language that powers everything from massive web servers to the core utilities of operating systems.

If following from our previous post, you are ready to dive right in. Otherwise, checkout [Setting up your development enviroment]({{< relref "learning_rust-setup.md" >}}). Let's start with a break down on how Rust projects are structured and get your first program off the ground.

## Meet Cargo: Your New Best Friend

In some languages, compiling code, managing dependencies, and formatting your files require three or four different tools. Rust simplifies this with Cargo.

Cargo is Rust’s build system and package manager. It compiles your code, downloads the libraries your code depends on (called "crates" in the Rust ecosystem), and builds those libraries. You will rarely use the raw Rust compiler (`rustc`) directly; Cargo handles the heavy lifting.

### Initializing the Project

Open up VS Code and click on the Explore button in the left hand bar. Let's create a new Folder called Rust Projects, and then select it. Open up Terminal from the top menu and hit new Terminal if one is not already open.

 To create a new project, type the following command:

```bash
cargo new hello_world
```

Cargo just did a few things behind the scenes. It created a new directory called hello_world, set up a basic project structure, and even initialized a new Git repository automatically to track your version history.

Navigate into your new project director

```bash
cd hello_world
```

## The Anatomy of a Rust Project

If you open the hello_world folder in VS Code, you will see a clean, minimalist structure. Let's dissect exactly what Cargo generated for you.

Cargo.toml: This is your project's manifest file, written in the TOML (Tom's Obvious, Minimal Language) format. It looks like this:

```Ini, TOML
[package]
name = "hello_world"
version = "0.1.0"
edition = "2021"

[dependencies]
```

[package]: This section defines the metadata Cargo needs to compile your program, the name, the version, and the Rust edition to use.

[dependencies]: Right now, it's empty. In the future, when you need to pull in external code (like a library to generate random numbers or build a web server), you will list them here. Cargo will read this file, download the exact versions you need, and link them to your project automatically.

The src Directory: Cargo expects all your source code to live inside the src folder. The top level of your project directory is strictly for configuration files, READMEs, and licensing information.

Inside src, you will find a single file: main.rs.

Dissecting main.rs
Open src/main.rs. You will see the most famous four lines in programming:
```rust
fn main() {
    println!("Hello, world!");
}
```
It looks simple, but there are a few uniquely "Rust" details hiding in here:

fn main(): The fn keyword declares a function. The main function is special: it is the primary entry point of every executable Rust program. The code inside its curly braces {} is the very first thing that runs.

println!: This prints text to the console, but it is not a function. The exclamation mark (!) signifies that println! is a macro. Macros in Rust are essentially code that writes other code. For now, just remember: if you see a !, you are calling a macro instead of a normal function.

"Hello, world!": This is a string literal, passed as an argument to println!.

;: Rust is a statically typed, C-family language. Almost all statements must end with a semicolon, indicating that the expression is complete.

## Compiling and Running
You have the code. Now it is time to execute it.

While you could use cargo build to compile the code and then manually run the executable file it creates, Cargo provides a shortcut that does both in one step.

In your VS Code terminal, make sure you are inside the hello_world directory and type:
```bash
cargo run
```

The Output:

```plaintext
Compiling hello_world v0.1.0 (C:\path\to\your\hello_world)
    Finished dev [unoptimized + debuginfo] target(s) in 1.02s
     Running `target\debug\hello_world.exe`
Hello, world!
```

What just happened? Cargo compiled your main.rs file, created an executable file hidden away in a newly generated target/debug/ folder, and immediately ran it.

You have officially compiled and executed your first Rust program. You now have a working toolchain, a grasp of how Cargo structures a project, and the foundation to start writing real logic.

## Learning Rust

Continue your journey with the next step or visit a previous post you may have missed

[Setting up your development enviroment]({{< relref "learning_rust-setup.md" >}})

[Cargo and the Anatomy of "Hello, World"]({{< relref "learning_rust-cargo.md" >}})

[Rust Variables and Immutability]({{< relref "learning_rust-vars.md" >}})

---