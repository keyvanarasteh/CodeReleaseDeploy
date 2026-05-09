# Release Automation & GitHub Workflows

This folder is dedicated to automating the build and deployment processes. Below is the Table of Contents for the release automation strategies.

## Table of Contents

1. [GitHub Actions Overview](#github-actions-overview)
2. [Secrets Management](#secrets-management)
3. [Multi-Platform Release Pipelines](#multi-platform-release-pipelines)
4. [CI/CD Reporting (Telegram)](#cicd-reporting)

---

## GitHub Actions Overview
GitHub Actions is used as the primary CI/CD runner to orchestrate matrix builds across Linux, macOS, and Windows. 

## Secrets Management
When dealing with deployment, you must organize your secrets in **Repository Secrets** (`Settings → Secrets and variables → Actions`). 

### Common Secrets Used
| Secret Name | Purpose | Ecosystem |
|-------------|---------|-----------|
| `APPLE_CERTIFICATE` | Base64-encoded `.p12` developer certificate | macOS/iOS |
| `APPLE_CERTIFICATE_PASSWORD` | Password for the `.p12` | macOS/iOS |
| `APPLE_SIGNING_IDENTITY` | Exact certificate Common Name (e.g., `Developer ID Application: Your Name (TEAMID)`) | macOS/iOS |
| `APPLE_ID` | Apple ID email | macOS/iOS |
| `APPLE_PASSWORD` | App-Specific Password for Xcode/altool | macOS/iOS |
| `APPLE_TEAM_ID` | 10-character Apple Developer Team ID | macOS/iOS |
| `SNAPCRAFT_STORE_CREDENTIALS` | Macaroon token for Snap Store publish | Linux |
| `DEBUGGER_BOT_TOKEN` | Telegram bot token for CI reporting | General |
| `DEBUGGER_ADMIN_ID` | Telegram Chat ID to receive logs | General |

## Multi-Platform Release Pipelines
Automated pipelines should leverage **Matrix Strategies** to concurrently build for `amd64` and `arm64` across different operating systems.

## CI/CD Reporting
Integrating Telegram directly into the failure/success steps of your workflows allows for real-time monitoring of release builds without checking GitHub.
