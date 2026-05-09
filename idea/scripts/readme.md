# GitHub Automation Scripts

This directory contains utility scripts to automate GitHub Actions workflows, specifically focusing on secret management and workflow monitoring from the command line using the GitHub CLI (`gh`).

## Table of Contents

1. [Pushing Secrets for Release Automation](#pushing-secrets-for-release-automation)
2. [Checking Actions Result Status](#checking-actions-result-status)

---

## 1. Pushing Secrets for Release Automation

When setting up a new repository for release automation, manually entering all the required secrets (Apple certificates, Android Keystores, Telegram tokens, etc.) can be tedious and error-prone. 

You can use the `gh` CLI to push these secrets in bulk.

### Example Script: `push_secrets.sh`

Create a script to automatically read your local `.env` or keystore files and push them to the repository as GitHub Actions Secrets.

```bash
#!/bin/bash
# push_secrets.sh
# Requires GitHub CLI (gh) to be authenticated: `gh auth login`

REPO="your-username/your-repo-name"

echo "Pushing Apple Signing Secrets..."
# Base64 encode the p12 (without line wraps) and push it
base64 -w 0 certs/apple_dev.p12 > /tmp/apple_cert.b64
gh secret set APPLE_CERTIFICATE < /tmp/apple_cert.b64 -R $REPO
rm /tmp/apple_cert.b64

# Push plain text secrets
gh secret set APPLE_CERTIFICATE_PASSWORD --body "your-p12-password" -R $REPO
gh secret set APPLE_SIGNING_IDENTITY --body "Developer ID Application: Keyvan Arasteh (YOUR_TEAM_ID)" -R $REPO
gh secret set APPLE_ID --body "your-apple-id@example.com" -R $REPO
gh secret set APPLE_PASSWORD --body "YOUR_APP_SPECIFIC_PASSWORD" -R $REPO
gh secret set APPLE_TEAM_ID --body "YOUR_TEAM_ID" -R $REPO

echo "Pushing Telegram Reporting Secrets..."
gh secret set DEBUGGER_BOT_TOKEN --body "YOUR_BOT_TOKEN" -R $REPO
gh secret set DEBUGGER_ADMIN_ID --body "YOUR_ADMIN_ID" -R $REPO

echo "✅ All secrets pushed successfully to $REPO"
```

## 2. Checking Actions Result Status

Instead of opening the browser to check if your macOS build signed correctly or if the Linux snap built successfully, you can monitor the workflow runs directly from your terminal.

### Example Script: `check_actions.sh`

```bash
#!/bin/bash
# check_actions.sh

echo "📊 Recent GitHub Actions Runs:"
# List the last 5 workflow runs with their status and ID
gh run list --limit 5

# Prompt the user if they want to watch a specific run
read -p "Enter a Run ID to watch live (or press Enter to exit): " RUN_ID

if [ ! -z "$RUN_ID" ]; then
    echo "👀 Watching Run ID: $RUN_ID"
    # Watch the workflow run live in the terminal
    gh run watch $RUN_ID
fi
```

### Useful `gh` commands for actions:
- `gh run list`: See the status of recent builds.
- `gh run view [run-id] --log`: Print the entire log of a specific run to the terminal (great for grepping errors).
- `gh run watch [run-id]`: Wait for a workflow to finish, updating the status automatically.
