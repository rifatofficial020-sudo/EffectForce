# EffectForce

**EffectForce** is an open-source motion graphics and compositing application built with Rust.

> EffectForce is based on the EffectCraft codebase. This README documents the Cargo workspace workflows and build commands. Some platform-specific requirements and package names may change as the project evolves.

<p align="center">
  <img src="docs/brand/effectcraft.svg" alt="EffectForce logo" width="180">
</p>

<p align="center">
  <a href="https://github.com/rifatofficial020-sudo/EffectForce">GitHub Repository</a>
  ·
  <a href="https://github.com/rifatofficial020-sudo/EffectForce/issues">Report a Bug</a>
  ·
  <a href="https://github.com/rifatofficial020-sudo/EffectForce/releases">Releases</a>
</p>

---

## Table of Contents

- [About](#about)
- [Features](#features)
- [Technology](#technology)
- [Requirements](#requirements)
- [Get the Source](#get-the-source)
- [Build and Run](#build-and-run)
- [Build Commands Reference](#build-commands-reference)
- [Windows Build](#windows-build)
- [Linux Build](#linux-build)
- [macOS Build](#macos-build)
- [Web Build](#web-build)
- [Command-Line Rendering](#command-line-rendering)
- [Tests and Code Quality](#tests-and-code-quality)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## About

EffectForce is a desktop creative application for motion graphics and compositing. The project uses a Rust workspace managed by Cargo, allowing the application, command-line renderer, and supporting tools to be built independently.

The repository is maintained at:

**https://github.com/rifatofficial020-sudo/EffectForce**

## Features

The project is intended for a motion-graphics and compositing workflow. The exact feature set depends on the current implementation in this repository. Explore the source tree and release notes for the latest supported tools and formats.

## Technology

- **Rust** — application and core implementation
- **Cargo** — dependency management, workspace builds, tests, and release builds
- **WebAssembly / browser tooling** — for the web build workflow where supported
- **Git** — source control

## Requirements

- Rust toolchain **1.95 or newer** (as required by the upstream EffectCraft build instructions; verify the repository's current `rust-toolchain.toml` or CI configuration if present).
- Cargo, installed with Rust.
- Git.
- Platform-native build tools and libraries required by Rust dependencies.

Check your installed versions:

```bash
rustc --version
cargo --version
git --version
```

Install or update Rust using [rustup](https://rustup.rs/).

```bash
rustup update stable
rustup default stable
```

## Get the Source

```bash
git clone https://github.com/rifatofficial020-sudo/EffectForce.git
cd EffectForce
```

If you already cloned the repository:

```bash
git pull
```

## Build and Run

### Run the desktop application (development)

```bash
cargo run -p effectcraft
```

### Build the desktop application (debug)

```bash
cargo build -p effectcraft
```

### Build the desktop application (optimized release)

```bash
cargo build --release -p effectcraft
```

### Run the demo project

```bash
cargo run --release -p effectcraft -- --demo
```

> The package name `effectcraft` is retained here because the upstream workspace commands use that Cargo package name. Renaming the visible product does not automatically rename Cargo packages. Confirm package names with `cargo metadata --no-deps --format-version 1` before changing these commands.

## Build Commands Reference

| Goal | Command |
|---|---|
| Check Rust installation | `rustc --version` |
| Check Cargo installation | `cargo --version` |
| Download dependencies | `cargo fetch` |
| Check workspace quickly | `cargo check --workspace` |
| Build workspace (debug) | `cargo build --workspace` |
| Build workspace (release) | `cargo build --workspace --release` |
| Run desktop app | `cargo run -p effectcraft` |
| Run desktop app (release) | `cargo run --release -p effectcraft` |
| Run demo | `cargo run --release -p effectcraft -- --demo` |
| Build CLI package | `cargo build --release -p effectcraft-cli` |
| Run all tests | `cargo test --workspace` |
| Format check | `cargo fmt --all -- --check` |
| Apply formatting | `cargo fmt --all` |
| Run Clippy | `cargo clippy --workspace --all-targets --all-features -- -D warnings` |
| Show workspace packages | `cargo metadata --no-deps --format-version 1` |
| Show available Cargo aliases | `cargo --list` |

Some commands can depend on optional features or native system libraries. If a package name differs in this fork, use `cargo metadata` or inspect the root `Cargo.toml` workspace members.

## Windows Build

### 1. Install prerequisites

Install:

- Rust using [rustup.rs](https://rustup.rs/)
- Git for Windows
- Visual Studio Build Tools with the **Desktop development with C++** workload, if the selected Rust target or native dependencies require the MSVC linker.

For an MSVC build, install the stable MSVC toolchain:

```powershell
rustup toolchain install stable-x86_64-pc-windows-msvc
rustup default stable-x86_64-pc-windows-msvc
rustup target add x86_64-pc-windows-msvc
```

Verify:

```powershell
rustc -V
cargo -V
rustup show
```

### 2. Clone and build

```powershell
git clone https://github.com/rifatofficial020-sudo/EffectForce.git
cd EffectForce
cargo fetch
cargo check --workspace
cargo build --release -p effectcraft
```

### 3. Run

```powershell
cargo run -p effectcraft
```

The release executable is normally written under:

```text
target\release\
```

The exact executable name and output location depend on the binary targets declared by the workspace.

### Windows GNU toolchain note

If you intentionally use the GNU target with MSYS2/MinGW-w64, install and configure a matching 64-bit toolchain and ensure its `bin` directory is on `PATH`. Do not mix MSVC and GNU linker environments in the same build. If Cargo reports `link.exe` not found, the MSVC linker environment is missing; use the MSVC build tools or switch deliberately to a supported GNU target.

## Linux Build

Install Rust with rustup, then install the native dependencies required by the project.

For Debian / Ubuntu, the upstream EffectCraft instructions identify ALSA development/runtime metadata as a requirement for desktop preview audio:

```bash
sudo apt update
sudo apt install build-essential pkg-config libasound2-dev
```

For Fedora:

```bash
sudo dnf install gcc gcc-c++ make pkgconf-pkg-config alsa-lib-devel
```

For Arch Linux:

```bash
sudo pacman -S --needed base-devel pkgconf alsa-lib
```

Then build:

```bash
git clone https://github.com/rifatofficial020-sudo/EffectForce.git
cd EffectForce
cargo fetch
cargo check --workspace
cargo build --release -p effectcraft
```

If the build reports that `alsa.pc` cannot be found, install the ALSA development package for your distribution and verify that `pkg-config --modversion alsa` works.

## macOS Build

1. Install Xcode Command Line Tools:

```bash
xcode-select --install
```

2. Install Rust from [rustup.rs](https://rustup.rs/).

3. Clone and build:

```bash
git clone https://github.com/rifatofficial020-sudo/EffectForce.git
cd EffectForce
cargo fetch
cargo check --workspace
cargo build --release -p effectcraft
```

## Web Build

The upstream project provides an `xtask` web workflow. From the repository root:

```bash
cargo xtask web --serve 8765
```

Then open:

```text
http://127.0.0.1:8765/
```

See the project's web documentation (for example, `docs/web.md`) for browser build prerequisites, supported targets, and deployment details.

## Command-Line Rendering

Build and run the headless renderer:

```bash
cargo run --release -p effectcraft-cli -- render --out intro.mp4
```

This command uses the CLI's default input/demo behavior as implemented by the current version. Check the CLI help for the exact supported arguments:

```bash
cargo run -p effectcraft-cli -- --help
cargo run -p effectcraft-cli -- render --help
```

## Tests and Code Quality

Run the complete workspace test suite:

```bash
cargo test --workspace
```

Check formatting:

```bash
cargo fmt --all -- --check
```

Automatically format the code:

```bash
cargo fmt --all
```

Run Clippy:

```bash
cargo clippy --workspace --all-targets --all-features -- -D warnings
```

The upstream project also documents a comprehensive CI task:

```bash
cargo xtask ci
```

Use this command if the `xtask` CI task is present in this repository. It may run formatting, linting, tests, layering checks, asset-attribution checks, and WebAssembly validation.

## Troubleshooting

### `linker 'link.exe' not found`

The Rust MSVC target is selected, but the MSVC linker is not available in the current terminal. Install Visual Studio Build Tools with the C++ workload and launch a terminal with the MSVC environment configured. Alternatively, configure a supported GNU target and its matching MinGW toolchain.

### ALSA / `alsa.pc` not found (Linux)

Install the ALSA development package:

```bash
# Ubuntu / Debian
sudo apt install libasound2-dev

# Fedora
sudo dnf install alsa-lib-devel

# Arch
sudo pacman -S alsa-lib
```

### Cargo cannot find a package

List workspace packages:

```bash
cargo metadata --no-deps --format-version 1
```

Or inspect package names in the root and member `Cargo.toml` files. Cargo package names are not necessarily the same as the repository name.

### Start with a clean rebuild

Try this only if you want Cargo to rebuild generated artifacts; it does not delete source files:

```bash
cargo clean
cargo build --release -p effectcraft
```

## Contributing

Contributions, bug reports, and feature suggestions are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Make your changes.
4. Run formatting, tests, and relevant build checks.
5. Commit your changes and open a pull request.

Example:

```bash
git switch -c feature/my-change
cargo fmt --all
cargo test --workspace
git add -A
git commit -m "Describe your change"
git push -u origin feature/my-change
```

Before submitting a pull request, review the repository's contribution guidelines and any agent/development instructions in the `docs/` directory.

## License

Check the repository's `LICENSE` file and the license notices of individual components and assets for the exact licensing terms. Do not assume that every dependency, logo, font, or bundled asset has the same license.

---

<p align="center">
  Built with Rust · Maintained by the EffectForce community
</p>
