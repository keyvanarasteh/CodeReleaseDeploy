# Microsoft Ecosystem (Windows)

## Overview
This document covers building, signing, and distributing applications for Windows.

## Authenticode Signing
1. **Certificates**: Obtaining an EV (Extended Validation) or Standard Code Signing Certificate.
2. **SignTool**: Using `signtool.exe` to sign `.exe`, `.msi`, and `.dll` files.
3. **Hardware Tokens**: Managing Cloud HSMs or physical USB tokens for EV signing.

## Packaging
- **MSIX / APPX**: Creating modern app packages.
- **NSIS / Inno Setup**: Creating classic executable installers.

## Windows Store
- **Partner Center**: Registering the application.
- **Automated Submission**: Pushing updates via the Microsoft Store API.
