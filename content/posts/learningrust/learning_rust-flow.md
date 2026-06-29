---

title: "Rust: Control Flow, Making Decisions with If, Else, and Loops"
date: 2026-06-04
draft: false
description: "Master Rust control flow. Learn how to use if/else statements, the powerful 'loop' construct, while, and for loops to control program execution paths."
tags: ["Rust", "programming", "software development", "control flow", "loops", "if else"]
categories: ["Development", "Tutorials", "Rust", "Programming"]
series: ["Learning Rust"]
author: "Gregory Bryant"
meta_title: "Rust If, Else, and Loops"
focus_keyword: "Rust If, Else, and Loops"

---

Programs are not just top-to-bottom lists of instructions. For software to be useful, it needs to react to data, make decisions, and repeat tasks. A game needs to keep running until the player quits. A server needs to respond differently to an admin than to a guest.

The mechanisms that guide a program through these decisions are called **Control Flow**.

Let's go over how Rust handles branching logic (`if`/`else`) and repetition (`loops`). We will also build a classic beginner project: a number-guessing game that utilizes an infinite loop and conditional breaking.

## Branching Logic: `if` and `else`

The most fundamental decision making tool in programming is the `if` statement. It evaluates a condition (which must result in a Boolean: `true` or `false`) and executes a block of code only if that condition is met.

Here is the basic syntax:

```rust
fn main() {
    let player_level = 15;
    let required_level = 10;

    if player_level >= required_level {
        println!("You may enter the dungeon.");
    } else {
        println!("You are not high enough level.");
    }
}
```

### The "No Parentheses" Rule

If you are coming from C++ or JavaScript, you will immediately notice something missing: **Rust does not require parentheses around the condition**. However, the curly braces `{}` are strictly mandatory, even if you are only executing a single line of code. This eliminates the dangling `else` bugs that plague other languages.

### Multiple Conditions with `else if`

You can chain multiple conditions together to handle complex decisions.

```rust
fn main() {
    let dps = 15000;

    if dps > 20000 {
        println!("Top tier damage output.");
    } else if dps > 10000 {
        println!("Solid rotation, respectable damage.");
    } else {
        println!("You might be missing some animation clips.");
    }
}
```

## The Engines of Repetition: Loops

Rust has three kinds of loops: `loop`, `while`, and `for`.

#### 1. `loop`: The Infinite Standard

The `loop` keyword is Rust’s simplest loop. It tells the program to execute a block of code over and over again forever, or until you explicitly tell it to stop using the `break` keyword.

```rust
fn main() {
    let mut counter = 0;

    loop {
        counter += 1;
        println!("Loop number: {}", counter);

        if counter == 5 {
            println!("Breaking out of the loop!");
            break; // This stops the loop
        }
    }
}
```

*When to use it:* `loop` is perfect for game loops, main application event listeners, or any scenario where you must repeatedly ask for user input until they provide something valid.

#### 2. `while`: The Conditional Loop

A `while` loop runs *as long as* a specific condition remains true. It is essentially an `if` statement combined with a `loop`.

```rust
fn main() {
    let mut countdown = 3;

    while countdown > 0 {
        println!("{}...", countdown);
        countdown -= 1;
    }

    println!("LIFTOFF!");
}
```

#### 3. `for`: Iterating over Collections

When you need to loop through a specific set of items (like the elements in an array or a range of numbers), `for` is the safest and most concise choice. It guarantees you will not accidentally step out of bounds.

```rust
fn main() {
    // Loop through numbers 1 to 5 (the range is exclusive of the last number)
    for number in 1..6 {
        println!("Number: {}", number);
    }
}
```

## The Project: A Number Guessing Game

Open VS Code and open your Rust Projects folder. Create a new project by running `cargo new guessing_game` in your terminal, and navigate into it (`cd guessing_game`). Then, replace the contents of `src/main.rs` with the following code.

```rust
use std::io;

fn main() {
    println!("Welcome to the Guessing Game!");
    let secret_number = 42; // Hardcoded for now!

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        // Reading input from the terminal
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        // Shadowing the guess variable to convert it from String to integer (u32)
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("Please type a number!");
                continue; // Skips the rest of the loop and starts over
            }
        };

        println!("You guessed: {}", guess);

        // Our control flow logic
        if guess < secret_number {
            println!("Too small!");
        } else if guess > secret_number {
            println!("Too big!");
        } else {
            println!("You win!");
            break; // Exits the loop
        }
    }
}
```

### Breaking Down the Code

- **`use std::io;`**: By default, Rust automatically brings only a few essential items into scope (a collection known as the "prelude"). To read user input from the terminal, we have to explicitly bring the input/output library (`io`) from the standard library (`std`) into scope at the top of our file.
- **`loop { ... }`**: We wrap the entire asking process in an infinite loop. The program will not exit until the user wins.
- **`continue`**: If the user types "hello" instead of a number, our parsing logic catches the error (`Err`) and calls `continue`. This tells the program to skip the `if/else` checks and jump immediately back to the top of the loop to ask again.
- **`break`**: When the `guess` matches the `secret_number`, the `else` block executes, printing "You win!" and hitting the `break` keyword, successfully ending the program.

Control flow is what transforms a static script into an interactive application. By mastering `if/else` and loops, you now have the tools to build complex logic and direct how your programs behave.

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