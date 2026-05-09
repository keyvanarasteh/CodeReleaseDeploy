# Crates.io Deployment Scripts

## Overview
This document covers the automation and scripting for publishing Rust libraries to `crates.io`.

## Prerequisites
- **Cargo Token**: You must generate an API token from crates.io and add it to your environment or GitHub Secrets (`CARGO_REGISTRY_TOKEN`).

## Deployment Commands
```bash
# Authenticate with Cargo (if not using an env var)
cargo login $CARGO_REGISTRY_TOKEN

# Dry run publish (highly recommended)
cargo publish --dry-run

# Publish the crate
cargo publish
```

## GitHub Actions Automation Example
```yaml
- name: Publish to crates.io
  env:
    CARGO_REGISTRY_TOKEN: ${{ secrets.CARGO_REGISTRY_TOKEN }}
  run: cargo publish
```
