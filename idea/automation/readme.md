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

### Example: Multi-Architecture Build Matrix (Tauri/Rust)

A robust release pipeline uses a strategy matrix to spawn multiple OS runners. Notice how we use `fail-fast: false` so that if one platform fails, the others continue building.

```yaml
strategy:
  fail-fast: false
  matrix:
    include:
      - platform: 'macos-latest'
        args: '--target aarch64-apple-darwin'
        label: '🍎 macOS (Apple Silicon)'
      - platform: 'macos-latest'
        args: '--target x86_64-apple-darwin'
        label: '🍎 macOS (Intel)'
      - platform: 'ubuntu-22.04'
        args: ''
        label: '🐧 Linux (x86_64)'
      - platform: 'windows-latest'
        args: ''
        label: '🪟 Windows (x86_64)'
runs-on: ${{ matrix.platform }}
```

### Example: Multi-Architecture Linux Snaps (QEMU)

For Linux packages like Snaps, you can use QEMU to cross-compile for ARM architectures on standard x86_64 runners:

```yaml
jobs:
  build:
    strategy:
      matrix:
        architecture: [amd64, arm64]
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up QEMU
        if: matrix.architecture == 'arm64'
        uses: docker/setup-qemu-action@v3
        
      - name: Build Snap
        uses: snapcore/action-build@v1
        id: snapBuild
        with:
          build-info: true
          snapcraft-args: --target-arch=${{ matrix.architecture }}
```

## CI/CD Reporting
Integrating Telegram directly into the failure/success steps of your workflows allows for real-time monitoring of release builds without checking GitHub.

### Example: Detailed Failure Reporting to Telegram

Using the `gh run view` command, you can automatically extract the failed step name and the last 5 lines of error logs, posting them directly to a Telegram chat.

```yaml
- name: 📡 Telegram — Platform Build Failed
  if: failure()
  shell: bash
  env:
    GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  run: |
    TAG="${GITHUB_REF_NAME}"
    
    # Get failed step names from this job
    FAILED_STEPS=$(gh api "repos/${{ github.repository }}/actions/runs/${{ github.run_id }}/jobs" \
      --jq '[.jobs[] | select(.status == "completed" and .conclusion == "failure") | .steps[] | select(.conclusion == "failure") | .name] | join(", ")' 2>/dev/null || echo "")
    
    # Get last lines from failed logs (head -c 300 to limit length)
    ERROR_LOG=$(gh run view ${{ github.run_id }} --log-failed 2>/dev/null | grep -v "^$" | tail -5 | head -c 300 | sed 's/&/\&amp;/g; s/</\&lt;/g; s/>/\&gt;/g' || echo "")
    
    # Build message
    MSG="❌ <b>${{ matrix.label }} Build FAILED</b>%0A%0A📦 Tag: <code>${TAG}</code>"
    if [ -n "$FAILED_STEPS" ]; then
      MSG="${MSG}%0A💥 Step: <code>${FAILED_STEPS}</code>"
    fi
    if [ -n "$ERROR_LOG" ]; then
      MSG="${MSG}%0A%0A<pre>${ERROR_LOG}</pre>"
    fi
    MSG="${MSG}%0A%0A🔗 <a href='https://github.com/${{ github.repository }}/actions/runs/${{ github.run_id }}'>View Full Logs</a>"
    
    curl -s -X POST "https://api.telegram.org/bot${{ secrets.DEBUGGER_BOT_TOKEN }}/sendMessage" \
      -d chat_id="${{ secrets.DEBUGGER_ADMIN_ID }}" \
      -d text="${MSG}" \
      -d parse_mode="HTML" \
      -d disable_web_page_preview="true"
```
