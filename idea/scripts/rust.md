# Rust Native Binaries Build Scripts

## Overview
This document covers compiling and releasing pure Rust binaries (`cargo`).

## Common Build Commands
```bash
# Standard release build
cargo build --release

# Cross-compiling
cargo build --release --target x86_64-pc-windows-gnu
```

## Optimizations
- Stripping symbols
- Link Time Optimization (LTO)
- Using `zigbuild` for cross-compilation from Linux to macOS/Windows.
