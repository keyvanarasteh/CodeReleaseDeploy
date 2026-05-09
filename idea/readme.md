# Release & Deploy Ideas (Platform KB)

This folder contains the core knowledge base for code signing, packaging, and store deployment across all major ecosystems.

## Table of Contents

### 1. Platform Deployment Guides
Detailed guides on developer accounts, code signing identities, and store submission.
- 🍎 **[Apple Ecosystem (Cupertino)](cupertino.md)**: macOS DMG/App code signing, iOS Provisioning Profiles, App Store Connect, TestFlight.
- 🤖 **[Google Play (Android)](google-play.md)**: Keystore generation, App Bundles (AAB), Google Play Console API.
- 🪟 **[Microsoft (Windows)](microsoft.md)**: Authenticode signing, MSIX packaging, Microsoft Partner Center.
- 🐧 **[Linux Distributions](linux.md)**: Snapcraft, Flatpak, AppImage creation, DEB/RPM packaging.

### 2. CI/CD & Automation
Automating the deployment processes described in the platform guides.
- ⚙️ **[GitHub Actions Automation](automation/readme.md)**: Multi-platform release pipelines, GitHub Secrets management, Telegram CI reporting.
- 📜 **[Release Scripts](scripts/readme.md)**: Bash and Python utilities for version bumping, asset packaging, and local signing.
