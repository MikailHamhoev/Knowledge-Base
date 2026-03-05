* Toolchain

** Installation and Compiler

*** rustup
Installer and Manager. It installs the compiler, standard libraries, and Cargo.
Allows easy switching between different versions of Rust: stable, beta, and nightly.

*** rustc
Compiler. Takes .rc files and compiles them into a binary executable. Rarely used directly because Cargo handles it.

** Cargo - Build System and Package Manager
Handles building code, downloading dependencies, and running tests.

*** Creating a Project

**** cargo new my_project
Creates a new folder with a standard structure. Genearates Cargo.toml - project manifest file, and a src folder.

*** Building and Running

**** cargo build
Compiles the project and its dependencies.

**** cargo run
Builds and then runs the projects.

**** cargo build --release
Compiles with optimizations to create a final, fast binary.

*** Dependencies

**** Crate
A Rust package or library.

**** crates.io
The official registry where people share Rust packages. To use a dependency, we have to add its name and version to the Cargo.toml file. Cargo downloads it automatically the next time you build.

*** Toolchain Management (rustup)

**** rustup update
Updates all installed toolchains.

**** rustup default nightly
Switches the default compiler to "nightly" version.

**** rustup target add
Adds support for compiling to a different OS or architecture.

*** Documentation (rustdoc)

**** rustdoc
Default built-in documentation generator.

**** /// 
Used above the code to create documentation comments.

**** cargo doc
Generates HTML documentation.

**** cargo doc --open
Generates the documentation and opens it in the browser.