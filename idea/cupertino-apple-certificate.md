# Certificate Lifecycle (CSR to .p12)

This document covers the manual steps required on a Mac to generate, export, and encode your Developer ID Application certificate for use in GitHub Actions.

---

## Step 1: Certificate Signing Request (CSR)

1. Open **Keychain Access** on your Mac.
2. In the menu bar, go to **Keychain Access** → **Certificate Assistant** → **Request a Certificate from a Certificate Authority...**
3. **User Email Address:** Enter your Apple Developer email.
4. **Common Name:** Enter your name (e.g., `Keyvan Arasteh`).
5. **Request is:** Select **Saved to disk**.
6. Click **Continue** and save the `.certSigningRequest` file to your desktop.

<!-- 🖼️ PLACEHOLDER: 04_keychain_csr_request.png -->
<!-- 🎬 PLACEHOLDER: 04_keychain_csr_request_video.webp -->

---

## Step 2: Download the Developer ID Certificate

1. Log in to the [Apple Developer Portal](https://developer.apple.com/account/resources/certificates/list).
2. Click the **+ (Plus)** button to create a new certificate.
3. Select **Developer ID Application** (under Software).
4. Upload the `.certSigningRequest` file you generated in Step 1.
5. Click **Continue**, then **Download** the resulting `.cer` file.
6. Double-click the `.cer` file to install it into your Keychain.

---

## Step 3: Export to .p12

1. Open **Keychain Access** and go to the **My Certificates** tab.
2. Find your new certificate: `Developer ID Application: Your Name (TEAMID)`.
3. Right-click the certificate and select **Export "Developer ID Application..."**.
4. **File Format:** Ensure **Personal Information Exchange (.p12)** is selected.
5. Save it as `certificate.p12`.
6. **Password:** Enter a strong password. **Save this password** — it will be your `APPLE_CERTIFICATE_PASSWORD` secret.

<!-- 🖼️ PLACEHOLDER: 05_keychain_export_p12.png -->
<!-- 🎬 PLACEHOLDER: 05_keychain_export_p12_video.webp -->

---

## Step 4: Base64 Encoding

To use the certificate in GitHub Secrets, you must encode it as a Base64 string.

### On macOS (Terminal):
```bash
base64 -i "certificate.p12" | pbcopy
```
*The encoded string is now in your clipboard.*

### On Linux (Terminal):
```bash
base64 -w 0 "certificate.p12" > cert_base64.txt
```

<!-- 🖼️ PLACEHOLDER: 06_terminal_base64_encode.png -->
