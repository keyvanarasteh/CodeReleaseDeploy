# Apple Ecosystem (Cupertino)

## Overview
This document covers the code signing, notarization, and distribution processes for macOS and iOS applications.

## macOS Code Signing & Notarization
1. **Certificate Generation**: CSR generation, obtaining the `.cer`, and exporting to `.p12`.
2. **Secrets Configuration**: Encoding `.p12` to Base64 for CI environments.
3. **Notarization**: Using `altool` or `notarytool` with App-Specific Passwords to pass Apple's Gatekeeper.

## iOS Provisioning
1. **Provisioning Profiles**: Managing Development vs. Distribution profiles.
2. **Fastlane Match**: Syncing certificates and profiles across development teams.
3. **TestFlight / App Store Connect**: Automated uploading via the App Store Connect API (`.p8` auth keys).
