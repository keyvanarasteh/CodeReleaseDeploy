# Finding Your Team ID & Adding `APPLE_TEAM_ID` Secret

This document covers finding your 10-character Apple Team ID and storing it as a GitHub Actions secret.

---

## Step 1: Navigate to Apple Developer Portal

1. Open your browser and go to: `https://developer.apple.com/account`.
2. Sign in with your Apple ID if prompted.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/04_apple_developer_account.png ✅ -->

---

## Step 2: Go to Membership Details

1. On the Account page, click **Membership details** in the top navigation icons.
2. You will see your membership information including your **Team ID** — a 10-character alphanumeric string (e.g., `XXXXXXXXXX`).
3. Copy this Team ID.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/04_apple_developer_membership.png ✅ -->

---

## Step 3: Add as GitHub Secret

1. Open your repository on GitHub → **Settings**.
2. In the sidebar under "Security", expand **Secrets and variables** → **Actions**.
3. Click the green **New repository secret** button.
4. **Name:** `APPLE_TEAM_ID`
5. **Secret:** Paste your 10-character Team ID.
6. Click **Add secret**.

<!-- 🖼️ IMAGE PLACEHOLDER: temp/04_github_apple_team_id.png -->
<!-- 🎬 VIDEO: temp/04_apple_team_id_video.webp ✅ -->
