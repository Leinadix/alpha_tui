# Alpha TUI - A Runtime Environment and Debugger

[![Downloads](https://img.shields.io/github/v/release/lmh01/alpha_tui)](https://github.com/lmh01/alpha_tui/releases)
![Downloads](https://img.shields.io/github/downloads/lmh01/alpha_tui/total)
[![Coverage](https://img.shields.io/codecov/c/github/lmh01/alpha_tui)](https://app.codecov.io/gh/LMH01/alpha_tui)
![Pipeline status](https://img.shields.io/github/actions/workflow/status/lmh01/alpha_tui/rust.yml)
[![License](https://img.shields.io/github/license/lmh01/alpha_tui)](LICENSE)

![Demo](docs/demo.gif)

## Overview

Alpha TUI is a comprehensive runtime environment and debugger for the Alpha-Notation used in Systemnahe Informatik lectures. It provides an interactive terminal-based interface for running, debugging, and analyzing Alpha-Notation programs with features like step-by-step execution, breakpoints, and a playground environment.

## Features

- **Interactive Debugging**: Step through programs line by line with full control
- **Breakpoint Support**: Set breakpoints for targeted debugging
- **Playground Mode**: Interactive environment for testing and learning
- **Comprehensive Error Handling**: Detailed error messages with context
- **Flexible Configuration**: Customizable memory settings and instruction sets
- **Multiple Themes**: Various UI themes including dark mode support
- **Cross-Platform**: Available for Linux, Windows, and macOS

## Quick Start

### Option 1: Download Pre-built Binary

1. Download the [latest release](https://github.com/lmh01/alpha_tui/releases/latest) for your system
2. Extract the archive
3. Run the executable:

```bash
# Linux/macOS
./alpha_tui load examples/programs/faculty.alpha

# Windows
.\alpha_tui.exe load examples/programs/faculty.alpha
```

### Option 2: Build from Source

**Prerequisites:**
- [Rust toolchain](https://rustup.rs/) (1.70.0 or later)
- Git

**Build steps:**

```bash
# Clone the repository
git clone https://github.com/lmh01/alpha_tui.git
cd alpha_tui

# Build the project
cargo build --release

# Run with an example
cargo run --release -- load examples/programs/faculty.alpha
```

### Option 3: Using Nix (NixOS/Linux with Nix)

```bash
# Run directly from GitHub
nix shell github:lmh01/alpha_tui

# Or add to your flake.nix
{
  inputs.alpha_tui.url = "github:lmh01/alpha_tui";
  # ... use in your configuration
}
```

## Usage

### Basic Commands

```bash
# Load and run a program
alpha_tui load program.alpha

# Check program syntax without running
alpha_tui check program.alpha

# Start playground mode
alpha_tui playground

# Show help
alpha_tui --help
```

### Key Bindings (In TUI Mode)

- **`r`** - Run next instruction
- **`c`** - Continue execution
- **`b`** - Set/remove breakpoint
- **`q`** - Quit
- **`h`** - Show help
- **`↑/↓`** - Navigate through program

### Example Programs

The `examples/programs/` directory contains various example programs:

- `faculty.alpha` - Factorial calculation
- `calculate_primes.alpha` - Prime number generation
- `matrix_mult.alpha` - Matrix multiplication
- `syscall_hello_world_demo.alpha` - System call demonstration

## Configuration

### Memory Configuration

Create a `memory_config.json` file to customize memory settings:

```json
{
  "gamma": 1000,
  "beta": 500,
  "alpha": 100
}
```

### Instruction Filtering

Create an `allowed_instructions.json` file to restrict available instructions:

```json
{
  "instructions": ["ADD", "SUB", "MUL", "DIV"],
  "comparisons": ["EQ", "NE", "LT", "GT"],
  "operations": ["LOAD", "STORE", "JUMP"]
}
```

### Themes

Choose from available themes:
- `default` - Standard theme
- `dracula` - Dark theme with purple accents
- `gray` - Minimalist gray theme
- `ayu` - Modern dark theme

```bash
alpha_tui --theme dracula load program.alpha
```

## Documentation

- **[Interface and Usage](docs/interface_and_usage.md)** - Detailed TUI usage guide
- **[Instructions Reference](docs/instructions.md)** - Complete instruction set documentation
- **[CLI Options](docs/cli.md)** - Command-line interface reference
- **[Contributing](CONTRIBUTING.md)** - How to contribute to the project

## API Documentation

Auto-generated API documentation is available at: https://lmh01.github.io/alpha_tui/docs/

## Development

### Project Structure

```
alpha_tui/
├── src/
│   ├── app/          # TUI application logic
│   ├── base.rs       # Core data structures
│   ├── cli.rs        # Command-line interface
│   ├── instructions/ # Instruction parsing and execution
│   ├── runtime/      # Program runtime and execution
│   └── main.rs       # Entry point
├── examples/         # Example programs and configurations
├── tests/           # Test files
├── docs/            # Documentation
└── themes/          # UI themes
```

### Running Tests

```bash
# Run all tests
cargo test

# Run tests with coverage
cargo install cargo-llvm-cov
cargo llvm-cov --html

# Run specific test module
cargo test instructions
```

### Code Quality

```bash
# Format code
cargo fmt

# Run linter
cargo clippy

# Check for outdated dependencies
cargo outdated
```

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Workflow

1. Fork the repository
2. Create a feature branch from `dev`
3. Make your changes
4. Add tests for new functionality
5. Run the test suite
6. Submit a pull request to `dev`

## License

This project is dual-licensed:

- **MIT License** - See [LICENSE-MIT](LICENSE-MIT) for details
- **GPL v3** - See [LICENSE](LICENSE) for details

Choose the license that best fits your needs.

## Support

- **Issues**: [GitHub Issues](https://github.com/lmh01/alpha_tui/issues)
- **Discussions**: [GitHub Discussions](https://github.com/lmh01/alpha_tui/discussions)
- **Documentation**: [Online Docs](https://lmh01.github.io/alpha_tui/docs/)

## Changelog

See [CHANGELOG.md](docs/changelog.md) for a complete list of changes.

## Acknowledgments

- Built with [Ratatui](https://github.com/ratatui-org/ratatui) for the terminal UI
- Uses [Crossterm](https://github.com/crossterm-rs/crossterm) for cross-platform terminal handling
- Error handling powered by [Miette](https://github.com/zkat/miette)