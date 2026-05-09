# Google Play Ecosystem (Android)

## Overview
This document covers building, signing, and releasing Android apps to the Google Play Store.

## KeyStore Management
1. **Generating a Keystore**: Creating a `.jks` or `.keystore` file securely.
2. **Play App Signing**: Opting into Google Play App Signing (upload key vs. deployment key).
3. **CI/CD Secrets**: Encoding the `.jks` file to Base64, and storing the Keystore Password, Key Alias, and Key Password as GitHub Secrets.

## Packaging
- **AAB (Android App Bundle)**: The required format for modern Play Store releases.
- **APK**: Fallback for direct sideloading or alternative stores.

## Google Play Console API
- **Service Accounts**: Creating a GCP Service Account with a `.json` key to access the Play Developer API.
- **Fastlane / Gradle Play Publisher**: Automating rollouts to Alpha, Beta, and Production tracks.
