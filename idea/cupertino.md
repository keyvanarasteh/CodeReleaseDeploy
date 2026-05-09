# Apple Ecosystem (Cupertino)

## Overview
This document covers the complete, end-to-end process of Code Signing and Notarization for macOS applications to bypass the Gatekeeper "unidentified developer" warning.

## Why is Signing Necessary?
macOS Gatekeeper blocks unsigned applications to protect users. To ensure a seamless user experience, we must:
1. **Code Sign** the application using an Apple `Developer ID` certificate.
2. **Notarize** the application by passing Apple's automated malware scanning.
3. **Staple** the notarization ticket to the `.dmg` so it can be verified offline.

---

## Apple Developer Requirements

| Requirement | Description |
|-------------|-------------|
| **Apple Developer Program** | Active paid membership ($99/year). |
| **Developer ID Application** | Certificate created via Certificates, Identifiers & Profiles. |
| **App-Specific Password** | Generated from `appleid.apple.com` → Security. |
| **Team ID** | 10-character ID found in Membership (e.g., `YOUR_TEAM_ID`). |

---

## Certificate Generation Process

### 1. Certificate Signing Request (CSR)
1. Open **Keychain Access** on your Mac.
2. Navigate to `Certificate Assistant` → `Request Certificate from a Certificate Authority...`
3. Enter your email and Common Name (e.g., `Keyvan Arasteh`).
4. Select `Saved to disk` and save the `.certSigningRequest` file.

### 2. Developer ID Application Certificate
1. Go to the **Apple Developer Portal** → `Certificates, IDs & Profiles` → `Certificates`.
2. Click `+` to create a new certificate and select **Developer ID Application**.
3. Upload the CSR file generated in Step 1.
4. Download the resulting `.cer` file.
5. Double-click the `.cer` file to import it into your macOS Keychain.

### 3. Export to `.p12`
1. Open **Keychain Access** and go to `My Certificates`.
2. Locate the imported certificate, usually named: `Developer ID Application: Your Name (YOUR_TEAM_ID)`.
3. Right-click and select **Export**.
4. Choose the `.p12` Personal Information Exchange format.
5. Set a strong password (this will become your `APPLE_CERTIFICATE_PASSWORD` secret).

### 4. Base64 Encoding
To use the certificate in GitHub Actions, it must be Base64 encoded as a single string.

```bash
# macOS (pbcopy copies it directly to clipboard)
base64 -i "certificate.p12" | pbcopy

# Linux (disable line wrapping)
base64 -w 0 "certificate.p12" > /tmp/cert.b64
```

---

## GitHub Secrets Configuration

Map your Apple Developer credentials to your repository secrets exactly as follows:

| Secret Name | Value |
|-------------|-------|
| `APPLE_CERTIFICATE` | Base64-encoded `.p12` string |
| `APPLE_CERTIFICATE_PASSWORD` | The password you set during the `.p12` export |
| `APPLE_SIGNING_IDENTITY` | The exact Common Name in Keychain (e.g., `Developer ID Application: Keyvan Arasteh (TEAMID)`) |
| `APPLE_ID` | Your Apple Developer Account email |
| `APPLE_PASSWORD` | App-specific password generated from appleid.apple.com |
| `APPLE_TEAM_ID` | Your 10-character Team ID |

> ⚠️ **Best Practice:** Generate a unique App-Specific Password for each repository/project rather than sharing one across all pipelines.

---

## Workflow Integration (Tauri Example)

When using the `tauri-apps/tauri-action`, passing these environment variables allows the action to handle the entire macOS signing lifecycle automatically.

```yaml
- name: Build and Release Tauri app
  uses: tauri-apps/tauri-action@v0
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    # Apple Code Signing & Notarization
    APPLE_CERTIFICATE: ${{ secrets.APPLE_CERTIFICATE }}
    APPLE_CERTIFICATE_PASSWORD: ${{ secrets.APPLE_CERTIFICATE_PASSWORD }}
    APPLE_SIGNING_IDENTITY: ${{ secrets.APPLE_SIGNING_IDENTITY }}
    APPLE_ID: ${{ secrets.APPLE_ID }}
    APPLE_PASSWORD: ${{ secrets.APPLE_PASSWORD }}
    APPLE_TEAM_ID: ${{ secrets.APPLE_TEAM_ID }}
```

**What the Action does under the hood:**
1. Creates a temporary CI macOS Keychain.
2. Imports the decoded `.p12` certificate.
3. Code signs the `.app` bundle using `/usr/bin/codesign`.
4. Packages into a `.dmg`.
5. Uploads the `.dmg` to Apple for Notarization using `xcrun notarytool`.
6. Staples the approved ticket to the DMG using `xcrun stapler`.

---

## Troubleshooting Guide

| Error | Cause | Solution |
|-------|-------|----------|
| `failed to resolve signing identity` | Mismatch between `.p12` identity and `APPLE_SIGNING_IDENTITY`. | Open Keychain, copy the exact certificate name and update the secret. |
| `The certificate is not trusted` | Missing Apple Intermediate Certificates in CI. | Usually handled by actions, but you may need to manually install the Apple WWDR CA certificate. |
| `notarization failed` | Incorrect App-Specific Password. | Revoke the old one at appleid.apple.com and generate a new one. |
