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

### Step 1. Certificate Signing Request (CSR) — 📷 NEEDS RECORDING (macOS Keychain Access)
1. Open **Keychain Access** on your Mac.
2. Navigate to `Certificate Assistant` → `Request Certificate from a Certificate Authority...`
3. Enter your email and Common Name (e.g., `Keyvan Arasteh`).
4. Select `Saved to disk` and save the `.certSigningRequest` file.

<!-- 🖼️ IMAGE PLACEHOLDER: temp/04_keychain_csr_request.png -->
<!-- 🎬 VIDEO PLACEHOLDER: temp/04_keychain_csr_request_video.webp -->

### Step 2. Developer ID Application Certificate — ✅ RECORDED
1. Go to the **Apple Developer Portal** → `Certificates, IDs & Profiles` → `Certificates`.
2. Click `+` to create a new certificate and select **Developer ID Application**.
3. Upload the CSR file generated in Step 1.
4. Download the resulting `.cer` file.
5. Double-click the `.cer` file to import it into your macOS Keychain.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/01_apple_developer_select_cert.png -->
<!-- Video:      temp/01_apple_developer_video.webp -->

### Step 3. Export to `.p12` — 📷 NEEDS RECORDING (macOS Keychain Access)
1. Open **Keychain Access** and go to `My Certificates`.
2. Locate the imported certificate, usually named: `Developer ID Application: Your Name (YOUR_TEAM_ID)`.
3. Right-click and select **Export**.
4. Choose the `.p12` Personal Information Exchange format.
5. Set a strong password (this will become your `APPLE_CERTIFICATE_PASSWORD` secret).

<!-- 🖼️ IMAGE PLACEHOLDER: temp/05_keychain_export_p12.png -->
<!-- 🎬 VIDEO PLACEHOLDER: temp/05_keychain_export_p12_video.webp -->

### Step 4. Base64 Encoding — 📷 NEEDS RECORDING (Terminal)
To use the certificate in GitHub Actions, it must be Base64 encoded as a single string.

```bash
# macOS (pbcopy copies it directly to clipboard)
base64 -i "certificate.p12" | pbcopy

# Linux (disable line wrapping)
base64 -w 0 "certificate.p12" > /tmp/cert.b64
```

<!-- 🖼️ IMAGE PLACEHOLDER: temp/06_terminal_base64_encode.png -->

---

## Apple ID (Developer Email) — ✅ RECORDED

> 📄 **See:** [cupertino-apple-id.md](cupertino-apple-id.md) for full step-by-step guide with screenshots and video.

---

## App-Specific Password — ✅ RECORDED

> 📄 **See:** [cupertino-apple-password.md](cupertino-apple-password.md) for full step-by-step guide with screenshots and video.

---

## Certificate Lifecycle — 📷 PARTIALLY RECORDED

> 📄 **See:** [cupertino-apple-certificate.md](cupertino-apple-certificate.md) for full step-by-step guide.

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

### Adding `APPLE_CERTIFICATE` to GitHub Secrets — ✅ RECORDED
1. Click **New repository secret**.
2. Name: `APPLE_CERTIFICATE`, Secret: paste the Base64-encoded `.p12` string.
3. Click **Add secret**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/09_github_apple_certificate_filled.png ✅ -->
<!-- Video:      temp/09_github_apple_certificate_video.webp ✅ -->

### Adding `APPLE_CERTIFICATE_PASSWORD` to GitHub Secrets — ✅ RECORDED
1. Click **New repository secret**.
2. Name: `APPLE_CERTIFICATE_PASSWORD`, Secret: the password set during `.p12` export.
3. Click **Add secret**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/10_github_apple_cert_password_filled.png ✅ -->
<!-- Video:      temp/10_github_apple_cert_password_video.webp ✅ -->

### Adding `APPLE_SIGNING_IDENTITY` to GitHub Secrets — ✅ RECORDED
1. Click **New repository secret**.
2. Name: `APPLE_SIGNING_IDENTITY`, Secret: the exact certificate Common Name from Keychain.
3. Click **Add secret**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/11_github_apple_signing_identity_filled.png ✅ -->
<!-- Video:      temp/11_github_apple_signing_identity_video.webp ✅ -->

### Adding `APPLE_ID` to GitHub Secrets — 📷 NEEDS RECORDING
1. Click **New repository secret**.
2. Name: `APPLE_ID`, Secret: your Apple Developer Account email.
3. Click **Add secret**.

<!-- 🖼️ IMAGE PLACEHOLDER: temp/10_github_apple_id.png -->

### Adding `APPLE_TEAM_ID` to GitHub Secrets — ✅ RECORDED
1. Click **New repository secret**.
2. Name: `APPLE_TEAM_ID`, Secret: your 10-character Team ID.
3. Click **Add secret**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/04_apple_developer_membership.png ✅ -->
<!-- Video:      temp/04_apple_team_id_video.webp ✅ -->

### All Secrets Configured — ✅ RECORDED
Take a final screenshot showing all required secrets listed in the repository settings.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/12_github_all_secrets_overview.png ✅ -->

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

<!-- 🖼️ IMAGE PLACEHOLDER: temp/13_github_workflow_yaml.png -->
<!-- 🎬 VIDEO PLACEHOLDER: temp/13_github_workflow_video.webp -->

---

## Troubleshooting Guide

| Error | Cause | Solution |
|-------|-------|----------|
| `failed to resolve signing identity` | Mismatch between `.p12` identity and `APPLE_SIGNING_IDENTITY`. | Open Keychain, copy the exact certificate name and update the secret. |
| `The certificate is not trusted` | Missing Apple Intermediate Certificates in CI. | Usually handled by actions, but you may need to manually install the Apple WWDR CA certificate. |
| `notarization failed` | Incorrect App-Specific Password. | Revoke the old one at appleid.apple.com and generate a new one. |
