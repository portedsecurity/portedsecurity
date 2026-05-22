+++
title = "Understanding Variables, Mutability, and Data Types in Rust"
date = 2026-05-21T21:00:00-04:00
draft = false
description = "Dive into the core of Rust programming on Windows. Learn how Rust handles variables, strict mutability, and basic data types safely."
tags = ["rust", "variables", "data-types", "programming"]
categories = ["Development", "Tutorials", "Learning Rust"]
author = "Gregory Bryant"
meta_title = "Rust Variables and Data Types: A Beginner's Guide"
focus_keyword = "rust variables"
+++

In our last post, we successfully compiled and ran our first Rust application on Windows using Cargo. Now, it is time to look at the actual code. 

If you are coming from languages like Python, JavaScript, or C++, Rust's approach to memory and variables might feel a little strict at first. However, these strict rules are exactly why Rust is immune to so many common crashes and security bugs.

Let's dive into how Rust handles storing data, starting with variables and mutability.

## The Golden Rule: Immutability by Default

In most programming languages, when you create a variable, you can change its value whenever you want. In Rust, **variables are immutable by default**. Once a value is bound to a name, you cannot change it.

Open your `src/main.rs` file and try this code:

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    
    x = 6; // This will cause an error!
    println!("The value of x is now: {}", x);
}
```

If you run cargo run, the Microsoft C++ build tools (which Rust relies on in Windows) will stop and throw a massive error reading: cannot assign twice to immutable variable x.

Rust does this for safety. If part of your code assumes a value will never change, but another part changes it, you get a bug. Rust prevents this at the compiler level.

Making Variables Mutable
If you actually want to change a variable's value, you have to explicitly ask for permission using the mut keyword:

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    
    x = 6; // This works perfectly now!
    println!("The value of x is now: {}", x);
}
```
By typing mut, you are signaling to other programmers (and the compiler) that this value will change during the program's lifecycle.

Shadowing: A Rust Superpower
What if you want to perform a transformation on a variable, but you want it to remain immutable afterward? Rust offers a feature called Shadowing.

You can declare a new variable with the same name as a previous variable using the let keyword again:

```rust
fn main() {
    let spaces = "   "; // This is a string of spaces
    let spaces = spaces.len(); // We shadow it to become an integer (3)
    
    println!("There are {} spaces.", spaces);
}
```
Shadowing is completely different from making a variable mut. With shadowing, we are actually creating a brand new variable, which means we can even change the type of the data (from text to a number), which mut does not allow.

Basic Data Types
Rust is a statically typed language, meaning it must know the types of all variables at compile time. Usually, the compiler is smart enough to guess the type based on the value you assign. But occasionally, you must annotate it.

Here are the four primary scalar data types you will use every day in Windows app development:

1. Integers
Numbers without a fractional component. They can be signed (can be negative, i) or unsigned (only positive, u), and vary by the amount of memory they take up (8-bit to 128-bit).

let age: u32 = 30; // Unsigned 32-bit integer (can only be positive)
let temperature: i32 = -15; // Signed 32-bit integer (can be negative)

Note: i32 is the default integer type in Rust.

2. Floating-Point Numbers
Numbers with decimal points. Rust has two types: f32 and f64.

let pi = 3.14159; // Defaults to f64 for better precision
let speed: f32 = 65.5; // Single-precision float

3. Booleans
Used for logic, booleans are strictly either true or false.

let is_windows_11: bool = true;
let is_crashing: bool = false;

4. Characters
Rust's char type is four bytes in size and represents a Unicode Scalar Value. This means it can represent a lot more than just ASCII—it can handle accented letters, Chinese/Japanese/Korean characters, and even emojis.

```rust
let letter: char = 'A';
let rust_crab: char = '🦀';
```
Summary
You now know how to store data safely in Rust. You understand that variables are locked (immutable) by default to prevent bugs, how to unlock them with mut, and how to