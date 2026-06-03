+++
title = "Step 1: Learning Rust, setting up your development enviroment"
date = 2026-05-21T19:17:00-04:00
draft = false
description = "Guide to installing Rust, MSVC build tools, VS Code, and Git on Windows."
tags = ["rust", "windows", "rustup", "vs-code", "programming", "dev-environment"]
categories = ["Development", "Tutorials"]
series = ["Learning Rust"]
author = "Gregory Bryant"
# SEO Specific Metadata
meta_title = "How to Install Rust on Windows."
focus_keyword = "install rust on windows"
+++

To get into Windows application programming with **Rust**, you need a few core dependencies before you can start compiling code. 

Because Rust interacts closely with the Windows operating system, it relies on Microsoft's C++ build tools behind the scenes. 

Here is the checklist to install Rust on a Windows machine.

<!--more-->

## Prerequisites: The 4 Essential Tools

To write, compile, and manage Rust applications on Windows, we need to install four pieces of software in a specific order:

1. **Microsoft C++ Build Tools** (The underlying compiler)
2. **Rustup** (The Rust language installer)
3. **VS Code** (The code editor)
4. **Git** (For version control and dependency management)

---

## Install Visual Studio Build Tools

Rust on Windows typically defaults to the **MSVC (Microsoft Visual C++)** toolchain. You do not need to install the massive, full Visual Studio IDE; you only need the command-line build tools.

1. Head to the official [Visual Studio Downloads](https://visualstudio.microsoft.com/downloads/) page.
2. Scroll down to *All Downloads*, expand *Tools for Visual Studio*, and download the **Build Tools for Visual Studio 2022**.
3. Run the installer. In the workloads screen, check the box for **Desktop development with C++**.
4. On the right-hand sidebar, ensure the following components are selected:
   * **MSVC v143** - VS 2022 C++ x64/x86 build tools (or latest)
   * **Windows 11 SDK** (or Windows 10 SDK, matching your operating system)
5. Click **Install** and reboot your PC if prompted.

---

## Install Rust via Rustup

`rustup` is the official management tool for Rust. It handles your compiler (`rustc`), package manager (`cargo`), and standard libraries.

1. Go to [rustup.rs](https://rustup.rs/) and download `rustup-init.exe`.
2. Launch the executable. A command prompt window will open.
3. The installer will automatically detect the MSVC build tools you installed in Step 1.
4. When prompted, type **`1`** and press **Enter** to proceed with the default installation.
5. Close the terminal window once the installation completes.

---

## Configure VS Code and Rust-Analyzer

While you can write Rust in any text editor, Visual Studio Code paired with the right extensions offers an unmatched developer experience.

1. Download and install [Visual Studio Code](https://code.visualstudio.com/).
2. Open VS Code and open the Extensions view by pressing `Ctrl + Shift + X`.
3. Search for and install **rust-analyzer**. 

> **Note:** Do not install the legacy "Rust" extension. `rust-analyzer` is the official, high-performance language server that provides real-time error checking, auto-completion, and code navigation.

---

## Install Git for Windows

The Rust ecosystem relies heavily on Git. Cargo (Rust's package manager) uses Git under the hood to fetch libraries, packages, and dependencies directly from GitHub.

1. Download the installer from [git-scm.com](https://git-scm.com/).
2. Run the installer. You can safely accept the default options. 
3. Ensure that the option **"Git from the command line and also from 3rd-party software"** remains checked during setup.

---

## Verifying Your Rust Setup

With everything installed, let's verify that your system paths are updated and the Rust compiler is responsive. Open a fresh **PowerShell** or **Command Prompt** window and run:

```bash
rustc --version
```

Now with everything working we are redy to move on to [Cargo and the Anatomy of "Hello, World"]({{< relref "learning_rust-cargo.md" >}})