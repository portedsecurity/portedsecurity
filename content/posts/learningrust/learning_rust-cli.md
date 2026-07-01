---
title: "Rust: CLI Tutorial, Building Your First Command Line Tool"
date: 2026-07-01
draft: false
description: "Bring your Rust skills together. Learn how to parse command-line arguments, read files, structure data, and build a fully functional CLI application."
categories: ["Rust", "Programming"]
tags: ["Rust basics", "CLI", "command line", "file I/O", "software development", "tutorial"]
series: ["Learning Rust"]
author: "Gregory Bryant"
meta_title: 'Rust: CLI Tutorial, Building Your First Command Line Tool'
focus_keyword: "Rust CLI Tutorial"
---

Over the past nine steps, you have went over variables, memory ownership, control flow, custom data types, collections, error handling, and file organization.

Today, we are going to combine those concepts into a single, functional Command Line Interface (CLI) application.

Our project will be a **System Scanner**. It will accept a file name from the terminal, open and read that file, parse the data into custom Structs, and output a formatted report.

Let's set up our final project. Open your terminal and run:

```bash
cargo new system_scanner
cd system_scanner
```

## 1. Preparing the Data File

Before we can read a file, we need a file to read. Let's create a simple text document that simulates a list of network servers our program needs to check.

Create a new file in the absolute root of your `system_scanner` folder (at the same level as your `Cargo.toml` file, *not* inside the `src` folder) and name it `server_list.txt`.

Add the following text to it:

```plaintext
// File: server_list.txt

Server_Alpha
Server_Beta
Server_Gamma
```

## 2. Parsing Command-Line Arguments

When you run a command like `cargo run`, `run` is an argument passed to the `cargo` program. We want our program to accept arguments too, so the user can tell it exactly which file to scan.

To do this, we use the `env` module from the standard library to collect the arguments into a Vector (`Vec<String>`).

Open your `src/main.rs` file and replace the default code with this:

```rust
// File: src/main.rs

use std::env;
use std::process;

fn main() {
    // Collect the command line arguments into a Vector
    let args: Vec<String> = env::args().collect();

    // The first argument (index 0) is always the path to the executable itself.
    // The second argument (index 1) will be the file we want to read.
    if args.len() < 2 {
        println!("Error: Missing file path argument.");
        println!("Usage: cargo run -- <file_path>");

        // This safely terminates the program with an error code of 1
        process::exit(1); 
    }

    // We borrow the string at index 1
    let file_path = &args[1];
    println!("Target file identified: {}", file_path);
}
```

### Passing Arguments via Cargo

To test this, we need to pass an argument through Cargo down into our program. We do this by adding two dashes `--`, followed by our arguments. Run this in your terminal:

```bash
cargo run -- server_list.txt
```

You should see this successful output:

```plaintext
Target file identified: server_list.txt
```

## 3. Reading the File and Handling Errors

Now that we have the file path, we need to read the contents of `server_list.txt`.

As we learned in Step 7, reading a file is risky, the file might be missing, or we might not have permission to read it. The `fs::read_to_string` function returns a `Result` Enum, which we will handle safely using a `match` statement.

Let's expand our `src/main.rs` file. Add the `fs` module to your imports at the top, and add the file-reading logic below your argument checks:

```rust
// File: src/main.rs

use std::env;
use std::fs;
use std::process;

fn main() {
    let args: Vec<String> = env::args().collect();

    if args.len() < 2 {
        println!("Error: Missing file path argument.");
        println!("Usage: cargo run -- <file_path>");
        process::exit(1); 
    }

    let file_path = &args[1];
    println!("Scanning file: {}\n", file_path);

    // Attempt to read the file. If it fails, we gracefully print the error and exit.
    let contents = match fs::read_to_string(file_path) {
        Ok(text) => text,
        Err(error) => {
            println!("Failed to read the file: {}", error);
            process::exit(1);
        }
    };

    println!("File contents successfully loaded into memory!");
}
```

If you misspell the file name (e.g., `cargo run -- missing.txt`), your robust error handling will catch it perfectly without a chaotic crash. Run this in your terminal:

```bash
cargo run -- server_list.txt
```

You should see this successful output:

```plaintext
Scanning file: server_list.txt

File contents successfully loaded into memory!
```

## 4. Structuring the Data

Finally, let's take the raw text data we just loaded, structure it using a custom `Server` struct, and use an `impl` block to give it a display method. We will loop through the file line-by-line, create a `Server` instance for each line, and store them all in a new Vector.

Here is your complete, final `src/main.rs` file to finish the project:

```rust
// File: src/main.rs (Complete Code)

use std::env;
use std::fs;
use std::process;

// Define our custom data structure
struct Server {
    name: String,
    status: String,
}

// Attach behavior to our Struct
impl Server {
    fn display_status(&self) {
        println!("[-] {}: {}", self.name, self.status);
    }
}

fn main() {
    let args: Vec<String> = env::args().collect();

    if args.len() < 2 {
        println!("Error: Missing file path argument.");
        println!("Usage: cargo run -- <file_path>");
        process::exit(1);
    }

    let file_path = &args[1];
    println!("Scanning file: {}\n", file_path);

    let contents = match fs::read_to_string(file_path) {
        Ok(text) => text,
        Err(error) => {
            println!("Failed to read the file: {}", error);
            process::exit(1);
        }
    };

    // Create an empty Vector to hold our structured Server data
    let mut server_records: Vec<Server> = Vec::new();

    // Loop through the raw text string, line by line
    for line in contents.lines() {
        let current_server = Server {
            name: String::from(line),
            status: String::from("ONLINE"), // Hardcoded for this simulation
        };

        // Push the new struct into our Vector
        server_records.push(current_server);
    }

    // Output the final formatted report
    println!("--- Scan Results ---");
    for record in &server_records {
        record.display_status();
    }
}
```

### The Final Execution

Run your finished program in the terminal:

```bash
cargo run -- server_list.txt
```

The Output:

```plaintext
Compiling system_scanner v0.1.0 (C:\path\to\your\system_scanner)
    Finished dev [unoptimized + debuginfo] target(s) in 0.85s
     Running `target\debug\system_scanner.exe server_list.txt`
Scanning file: server_list.txt

--- Scan Results ---
[-] Server_Alpha: ONLINE
[-] Server_Beta: ONLINE
[-] Server_Gamma: ONLINE
```

## Conclusion

Take a moment to appreciate what you just built.

In this single file, you navigated command-line environments, managed heap memory (`String` and `Vec`), safely bypassed a potential crash using Pattern Matching (`match` on a `Result`), attached encapsulated behavior to custom data (`impl` on a `Struct`), and structured a loop to generate a dynamic UI in the terminal.

You didn't just write a script; you wrote a mathematically proven, memory-safe systems application.

Rust is a deeply challenging language, but the compiler is your best friend. By forcing you to handle errors explicitly, define data strictly, and respect memory ownership, Rust ensures that if your code compiles, it works.

Welcome to the world of systems programming. You are officially a Rustacean.

# Learning Rust

We've reached the end of this series, if you've missed or need to revisit any of our previous post.

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


