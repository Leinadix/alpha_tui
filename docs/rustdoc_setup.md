# Rustdoc Configuration and Setup

Alpha TUI uses Rust's built-in documentation generator `rustdoc` to create comprehensive API documentation. This document explains how to set up, configure, and use rustdoc for the project.

## Overview

Rustdoc is Rust's built-in documentation generator that creates HTML documentation from your Rust source code and documentation comments. It's the standard tool for Rust projects and integrates seamlessly with Cargo.

## Configuration

### Project-Level Configuration

The project uses a `rustdoc.toml` configuration file to customize the documentation output:

```toml
# HTML output configuration
[output.html]
enable-index-page = true
source-sidebar-show = true
default-theme = "ayu"
preferred-dark-theme = "ayu"

# Documentation features
document-private-items = false
document-dependencies = false

# Markdown configuration
[markdown]
enable-gfm = true
enable-smart-punctuation = true
```

### Cargo.toml Configuration

Add documentation metadata to your `Cargo.toml`:

```toml
[package]
name = "alpha_tui"
version = "1.8.1"
edition = "2021"
description = "A runtime environment and debugger for Alpha-Notation"
documentation = "https://lmh01.github.io/alpha_tui/docs/"
repository = "https://github.com/lmh01/alpha_tui"
readme = "README.md"
license = "MIT OR GPL-3.0"
keywords = ["tui", "debugger", "alpha-notation", "runtime"]
categories = ["command-line-utilities", "development-tools::debugging"]

[package.metadata.docs.rs]
all-features = true
rustdoc-args = ["--cfg", "docsrs"]
```

## Documentation Comments

### Basic Documentation Comments

Use `///` for outer documentation comments and `//!` for inner documentation comments:

```rust
//! This module contains the runtime environment for Alpha-Notation programs.
//! 
//! The runtime provides execution environment, memory management, and
//! control flow for Alpha-Notation instructions.

/// Represents the runtime environment for executing Alpha-Notation programs.
/// 
/// The `Runtime` struct manages program execution, memory state, and control flow.
/// It provides methods for step-by-step execution, breakpoint management, and
/// runtime state inspection.
/// 
/// # Examples
/// 
/// ```rust
/// use alpha_tui::runtime::Runtime;
/// 
/// let mut runtime = Runtime::new();
/// runtime.step()?;
/// ```
/// 
/// # Safety
/// 
/// The runtime is safe to use in single-threaded contexts. Multi-threading
/// support is not currently implemented.
pub struct Runtime {
    /// Currently active memory of this runtime
    memory: RuntimeMemory,
    /// Program instructions to execute
    instructions: Vec<Instruction>,
    // ... other fields
}
```

### Advanced Documentation Features

#### Code Examples

```rust
/// Executes a single instruction step.
/// 
/// # Examples
/// 
/// ```rust
/// use alpha_tui::runtime::Runtime;
/// 
/// let mut runtime = Runtime::new();
/// match runtime.step() {
///     Ok(finished) => {
///         if finished {
///             println!("Program completed");
///         }
///     }
///     Err(e) => eprintln!("Runtime error: {}", e),
/// }
/// ```
/// 
/// # Errors
/// 
/// This function returns `RuntimeError` if:
/// - Stack overflow occurs
/// - Invalid instruction is encountered
/// - Memory access violation happens
/// 
/// # Panics
/// 
/// This function panics if the runtime is in an invalid state.
pub fn step(&mut self) -> Result<bool, RuntimeError> {
    // Implementation
}
```

#### Cross-References

```rust
/// Manages memory cells and accumulators.
/// 
/// This struct provides access to:
/// - [`accumulators`](Self::accumulators) - CPU registers
/// - [`memory_cells`](Self::memory_cells) - Named memory locations
/// - [`stack`](Self::stack) - Runtime stack
/// 
/// See also: [`Runtime`] for the main execution environment.
pub struct RuntimeMemory {
    // fields
}
```

## Generating Documentation

### Local Development

Generate documentation locally:

```bash
# Generate documentation for the current package
cargo doc

# Generate documentation with private items
cargo doc --document-private-items

# Generate and open documentation in browser
cargo doc --open

# Generate documentation for all dependencies
cargo doc --document-dependencies
```

### Custom Rustdoc Arguments

Use custom rustdoc arguments:

```bash
# Generate docs with custom theme
cargo doc -- --theme=ayu

# Generate docs with custom CSS
cargo doc -- --extend-css custom.css

# Generate docs with additional lints
cargo doc -- -W missing-docs
```

### GitHub Actions Integration

The project includes automatic documentation generation in `.github/workflows/release.yml`:

```yaml
documentation:
  if: github.ref == 'refs/heads/main' && github.event_name == 'push'
  runs-on: ubuntu-latest
  steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Install Rust
      uses: dtolnay/rust-toolchain@master
      with:
        toolchain: stable

    - name: Generate documentation
      run: cargo doc --all --no-deps

    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./target/doc
        destination_dir: docs
```

## Documentation Standards

### Module-Level Documentation

Each module should have comprehensive documentation:

```rust
//! Runtime execution environment for Alpha-Notation programs.
//! 
//! This module provides the core runtime functionality including:
//! 
//! - Program execution and stepping
//! - Memory management
//! - Control flow handling
//! - Error handling and recovery
//! 
//! # Architecture
//! 
//! The runtime follows a layered architecture:
//! 
//! ```text
//! ┌─────────────────┐
//! │   Application   │
//! ├─────────────────┤
//! │     Runtime     │
//! ├─────────────────┤
//! │  Instructions   │
//! ├─────────────────┤
//! │   Base Types    │
//! └─────────────────┘
//! ```
//! 
//! # Examples
//! 
//! Basic usage:
//! 
//! ```rust
//! use alpha_tui::runtime::Runtime;
//! 
//! let mut runtime = Runtime::new();
//! while !runtime.finished() {
//!     runtime.step()?;
//! }
//! ```
```

### Function Documentation

All public functions should be documented:

```rust
/// Resets the runtime to its initial state.
/// 
/// This function:
/// 1. Resets the instruction pointer to the beginning
/// 2. Clears the call stack
/// 3. Restores initial memory state
/// 4. Resets runtime counters
/// 
/// # Examples
/// 
/// ```rust
/// let mut runtime = Runtime::new();
/// runtime.run()?;
/// runtime.reset(); // Ready to run again
/// ```
/// 
/// # Performance
/// 
/// This is an O(n) operation where n is the number of memory cells.
pub fn reset(&mut self) {
    // Implementation
}
```

### Error Documentation

Document error conditions thoroughly:

```rust
/// Errors that can occur during runtime execution.
/// 
/// These errors indicate problems during program execution and
/// should be handled appropriately by the caller.
#[derive(Debug, thiserror::Error)]
pub enum RuntimeError {
    /// Stack overflow occurred during execution.
    /// 
    /// This error is thrown when the call stack exceeds the maximum
    /// allowed depth of 65535 entries.
    #[error("Stack overflow: call stack exceeded maximum depth")]
    StackOverflow,
    
    /// Memory access violation.
    /// 
    /// Attempted to access a memory location that doesn't exist
    /// or is not initialized.
    #[error("Memory access violation: {location}")]
    MemoryViolation {
        /// The memory location that was accessed
        location: String,
    },
}
```

## Testing Documentation

### Documentation Tests

Rustdoc can test code examples in documentation:

```rust
/// Calculates the factorial of a number.
/// 
/// # Examples
/// 
/// ```rust
/// use alpha_tui::utils::factorial;
/// 
/// assert_eq!(factorial(0), 1);
/// assert_eq!(factorial(5), 120);
/// ```
/// 
/// # Panics
/// 
/// ```should_panic
/// use alpha_tui::utils::factorial;
/// 
/// factorial(-1); // This will panic
/// ```
pub fn factorial(n: u64) -> u64 {
    match n {
        0 => 1,
        _ => n * factorial(n - 1),
    }
}
```

Run documentation tests:

```bash
# Run all documentation tests
cargo test --doc

# Run documentation tests for a specific module
cargo test --doc runtime
```

## Best Practices

### 1. Write Examples

Always include working examples in your documentation:

```rust
/// # Examples
/// 
/// ```rust
/// use alpha_tui::runtime::Runtime;
/// 
/// let mut runtime = Runtime::new();
/// runtime.load_program("examples/factorial.alpha")?;
/// runtime.run()?;
/// ```
```

### 2. Document Error Conditions

Be explicit about when functions can fail:

```rust
/// # Errors
/// 
/// Returns `RuntimeError` if:
/// - The program contains invalid instructions
/// - Memory access violations occur
/// - Stack overflow happens
```

### 3. Use Intra-doc Links

Link to related items:

```rust
/// See also: [`Runtime::step`] for single-step execution.
```

### 4. Document Safety

For unsafe code, document safety requirements:

```rust
/// # Safety
/// 
/// The caller must ensure that:
/// - The memory region is valid
/// - The pointer is properly aligned
/// - No other threads are accessing the memory
```

### 5. Include Performance Notes

Document performance characteristics:

```rust
/// # Performance
/// 
/// This operation is O(n) where n is the number of instructions.
/// For large programs, consider using [`Runtime::run_async`] instead.
```

## Deployment

Documentation is automatically deployed to GitHub Pages at:
https://lmh01.github.io/alpha_tui/docs/

### Local Preview

To preview documentation locally:

```bash
# Generate and serve documentation
cargo doc --open

# Or use a local server
cd target/doc
python3 -m http.server 8000
```

## Troubleshooting

### Common Issues

1. **Missing Documentation**: Use `cargo doc -W missing-docs` to find undocumented items
2. **Broken Links**: Use `cargo doc -W rustdoc::broken-intra-doc-links` to find broken links
3. **Example Failures**: Use `cargo test --doc` to test documentation examples

### Configuration Issues

If rustdoc isn't finding your configuration:

```bash
# Specify config file explicitly
cargo doc --config rustdoc.toml

# Or use environment variables
RUSTDOCFLAGS="--theme ayu" cargo doc
```

## Contributing

When contributing to the project:

1. Add documentation to all public items
2. Include working examples
3. Document error conditions
4. Test documentation examples
5. Update this guide if needed

For more information, see the [Rustdoc Book](https://doc.rust-lang.org/rustdoc/).