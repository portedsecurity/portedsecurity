---
title:  "Rust: Structs and Enums, Modeling Custom Data"
date:  2026-06-15
draft: false
description:  "Learn how to model complex, real-world data in Rust using Structs and Enums. Group related fields, define strict variants, and attach behavior to your data."
tags: ["Rust", "structs", "enums", "data modeling", "systems programming", "software development"]
categories: ["Development", "Tutorials", "Rust", "Programming"]
series: ["Learning Rust"]
author: "Gregory Bryant"
meta_title: 'Rust Structs and Enums: Modeling Custom Data'
focus_keyword: "Rust Structs and Enums: Modeling Custom Data"

---

# 

So far, we have worked with primitive data types: integers, booleans, and strings. While these are the building blocks of software, they are rarely enough to model real-world concepts on their own.

If you are building an MMO server, a character is not just a `String` or a `u32`. A character is a complex entity with health, power reserves, a specific role, and an inventory. Managing all these distinct variables separately would quickly turn your code into an unreadable mess.

To solve this, Rust provides two incredibly powerful tools for creating custom data types: **Structs** and **Enums**.

Let's set up a new project to see how they work. Open your terminal and run:

```bash
cargo new custom_data
cd custom_data
```

## ## Grouping Data with Structs

A **Struct** (short for structure) allows you to package multiple related values together into a single, cohesive unit. If you are coming from object-oriented languages, structs are similar to the data attributes of a Class.

Let's model a player character optimized for a Might-based DPS build. Open `src/main.rs` and define a struct outside of your `main` function:

```rust
// We add this line so Rust knows how to print our Struct to the terminal later
#[derive(Debug)]
struct Player {
    name: String,
    health: u32,
    power: u32,
    might: u32,
}
```

We have defined the *shape* of our data, but we haven't created any actual data yet. To do that, we create an **instance** of the struct inside our `main` function:

```rust
fn main() {
    // Creating an instance of our Player struct
    let character = Player {
        name: String::from("Voltage"),
        health: 2500,
        power: 1000,
        might: 4500,
    };

    // Accessing individual fields using dot notation
    println!("{} has {} might.", character.name, character.might);
}
```

If you run `cargo run`, it will neatly print: `Voltage has 4500 might.`

### Mutating Structs

Just like standard variables, struct instances are immutable by default. If you want a player to take damage or spend power, the *entire* instance must be declared as mutable. You cannot make just one field mutable.

```rust
let mut character = Player { /* ... */ };
character.health = 2000; // This is only allowed because the instance is 'mut'
```

## Defining Possibilities with Enums

While Structs group multiple fields together using *AND* (a player has health *and* power *and* might), **Enums** allow you to define a type by enumerating its possible variants using *OR*. A value can only be exactly one variant at a time.

Imagine we want to assign a powerset to our character. A player could be Electric, Mental, Fire, or Ice, but they cannot be all of them at once. This is the perfect use case for an Enum.

Let's add an Enum to our file and update our `Player` struct to include it:

```rust
#[derive(Debug)]
enum Powerset {
    Electric,
    Mental,
    Fire,
    Ice,
}

#[derive(Debug)]
struct Player {
    name: String,
    health: u32,
    power: u32,
    might: u32,
    powerset: Powerset, // Using our custom Enum as a type!
}
```

Now, when we instantiate our player, we use double colons (`::`) to access the specific Enum variant:

```rust
fn main() {
    let character = Player {
        name: String::from("Voltage"),
        health: 2500,
        power: 1000,
        might: 4500,
        powerset: Powerset::Electric,
    };

    // Using the {:?} formatter to print the whole struct for debugging
    println!("{:#?}", character);
}
```

If you run `cargo run` now, the `#[derive(Debug)]` attribute we added earlier allows Rust to pretty-print your entire custom data structure to the terminal:

```rust
Player {
    name: "Voltage",
    health: 2500,
    power: 1000,
    might: 4500,
    powerset: Electric,
}
```

## Adding Behavior: The `impl` Block

Data is great, but we usually want our data to *do* something. In Rust, you attach functions to your Structs and Enums using an **`impl` (implementation) block**.

Functions defined inside an `impl` block are called **methods**. Let's give our character the ability to cast an ability, draining their power and calculating damage based on their might stat.

Here is the complete, final code for `src/main.rs`:

```rust
#[derive(Debug)]
enum Powerset {
    Electric,
    Mental,
}

#[derive(Debug)]
struct Player {
    name: String,
    health: u32,
    power: u32,
    might: u32,
    powerset: Powerset,
}

// The impl block for our Player struct
impl Player {
    // The &mut self parameter means this method borrows the instance 
    // and is allowed to change its data.
    fn cast_ability(&mut self, power_cost: u32, damage_multiplier: f32) {
        if self.power >= power_cost {
            self.power -= power_cost;

            // Calculate total damage (casting might as an f32 for the math, then back to u32)
            let total_damage = (self.might as f32 * damage_multiplier) as u32;

            println!("{} casts an ability! It deals {} damage.", self.name, total_damage);
            println!("Remaining power: {}", self.power);
        } else {
            println!("Not enough power to cast!");
        }
    }
}

fn main() {
    let mut character = Player {
        name: String::from("Voltage"),
        health: 2500,
        power: 1000,
        might: 4500,
        powerset: Powerset::Electric,
    };

    // Calling the method
    character.cast_ability(300, 1.5);
}
```

The Output:

```plaintext
Voltage casts an ability! It deals 6750 damage.
Remaining power: 700
```

## Summary

By combining Structs, Enums, and `impl` blocks, you have moved beyond simple scripts and entered the realm of true software architecture.

- Use **Structs** when you need to group related attributes together (A *has a* B, C, and D).

- Use **Enums** when a value can be exactly one of several possibilities (A *is either* X, Y, or Z).

- Use **`impl` blocks** to tie logic and behavior directly to your custom data types.

In the next post, we will look at how to handle Enums elegantly using Rust's incredibly powerful `match` control flow operator.

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