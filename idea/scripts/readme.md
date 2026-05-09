# Release & Deploy Scripts

This directory is intended to house utility scripts (Bash, Python, Node.js) that assist with the release, code signing, and deployment lifecycle. 

## Table of Contents

1. [Version Management](#version-management)
2. [Code Signing Utilities](#code-signing-utilities)
3. [Asset Packaging](#asset-packaging)
4. [Deployment Automation](#deployment-automation)

---

## Version Management
Scripts to automatically bump versions across multiple manifest files (`package.json`, `Cargo.toml`, `tauri.conf.json`, `pubspec.yaml`, etc.), sync tags with git, and generate changelogs.

- `bump_version.sh` - Standardizes version incrementing (major/minor/patch).
- `generate_changelog.py` - Parses git commits to generate release notes.

## Code Signing Utilities
Helper scripts designed to be executed both locally and within CI/CD pipelines to manage certificates and signing.

- `decode_apple_cert.sh` - Takes Base64 encoded `.p12` from env vars, decodes it, and adds it to a temporary keychain.
- `sign_windows_exe.ps1` - PowerShell script to invoke `signtool.exe` with timestamp server configurations.

## Asset Packaging
Scripts for creating universal binaries, zipping assets, or wrapping applications.

- `create_macos_universal.sh` - Uses `lipo` to combine `x86_64` and `aarch64` binaries into a single macOS universal app.
- `bundle_assets.py` - Collects build artifacts, renames them by version/arch, and prepares a `.zip` or `.tar.gz` for release.

## Deployment Automation
Scripts that interface directly with store APIs or custom servers.

- `upload_to_testflight.sh` - Uses `altool` or `xcrun` to upload `.ipa` or `.pkg` files to App Store Connect.
- `publish_snap.sh` - Uses `snapcraft upload` to push the `.snap` file to the Snap Store channels.
