---
title: "Rust: Setting up your development environment"
date: 2026-05-21
draft: false
description:  "Guide to installing Rust, MSVC build tools, VS Code, and Git on Windows."
tags:  ["rust", "windows", "rustup", "vs-code", "programming", "dev-environment", "software development"]
categories:  ["Development", "Tutorials", "Rust", "Programming"]
series:  ["Learning Rust"]
author:  "Gregory Bryant"
meta_title:  "How to Install Rust on Windows."
focus_keyword:  "install rust on windows"

---

To get into Windows application programming with **Rust**, you need a few core dependencies before you can start compiling code. 

Because **Rust** interacts closely with the Windows operating system, it relies on Microsoft's C++ build tools behind the scenes. 

Here is the checklist to install Rust on a Windows machine.

<!--more-->

## Prerequisites: The 4 Essential Tools

To write, compile, and manage Rust applications on Windows, we need to install four pieces of software in a specific order:

1. **Microsoft C++ Build Tools** (The underlying compiler)
2. **Rustup** (The Rust language installer)
3. **VS Code** (The code editor)
4. **Git** (For version control and dependency management)

## Install Visual Studio Build Tools

Rust on Windows typically defaults to the **MSVC (Microsoft Visual C++)** toolchain. You do not need to install the massive, full Visual Studio IDE; you only need the command-line build tools.

- Head to the official [Visual Studio Downloads](https://visualstudio.microsoft.com/downloads/) page.
- Scroll down to *All Downloads*, expand *Tools for Visual Studio*, and download the **Build Tools for Visual Studio 2022**.
- Run the installer. In the workloads screen, check the box for **Desktop development with C++**.
- On the right-hand sidebar, ensure the following components are selected:
  * **MSVC v143** - VS 2022 C++ x64/x86 build tools (or latest)
  * **Windows 11 SDK** (or Windows 10 SDK, matching your operating system)
- Click **Install** and reboot your PC if prompted.

## Install Rust via Rustup

`rustup` is the official management tool for Rust. It handles your compiler (`rustc`), package manager (`cargo`), and standard libraries.

1. Go to [rustup.rs](https://rustup.rs/) and download `rustup-init.exe`.
2. Launch the executable. A command prompt window will open.
3. The installer will automatically detect the MSVC build tools you installed in Step 1.
4. When prompted, type **`1`** and press **Enter** to proceed with the default installation.
5. Close the terminal window once the installation completes.

## Configure VS Code and Rust-Analyzer

While you can write Rust in any text editor, Visual Studio Code paired with the right extensions offers an unmatched developer experience.

1. Download and install [Visual Studio Code](https://code.visualstudio.com/).
2. Open VS Code and open the Extensions view by pressing `Ctrl + Shift + X`.
3. Search for and install **rust-analyzer**. 

> **Note:** Do not install the legacy "Rust" extension. `rust-analyzer` is the official, high-performance language server that provides real-time error checking, auto-completion, and code navigation.

## Install Git for Windows

The Rust ecosystem relies heavily on Git. Cargo (Rust's package manager) uses Git under the hood to fetch libraries, packages, and dependencies directly from GitHub.

1. Download the installer from [git-scm.com](https://git-scm.com/).
2. Run the installer. You can safely accept the default options. 
3. Ensure that the option **"Git from the command line and also from 3rd-party software"** remains checked during setup.

## Verifying Your Rust Setup

With everything installed, let's verify that your system paths are updated and the Rust compiler is responsive. Open a fresh **PowerShell** or **Command Prompt** window and run:

```bash
rustc --version
```

## Learning Rust

Continue your journey with the next step orump ahead to for a refreher

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