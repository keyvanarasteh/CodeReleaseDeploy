# Tauri v2 Build & Release Scripts

## Overview
This document covers the CLI commands and scripts necessary to build, bundle, and sign Tauri v2 applications.

## Common Build Commands
```bash
# Build for current platform
bun tauri build

# Build for specific target (e.g., Apple Silicon)
bun tauri build --target aarch64-apple-darwin
```

## Release Artifacts
- **macOS**: `.dmg`, `.app`
- **Windows**: `.exe`, `.msi`
- **Linux**: `.deb`, `.AppImage`, `.rpm`
