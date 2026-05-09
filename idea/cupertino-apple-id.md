# Apple ID (Developer Email) & GitHub Secret

This document covers identifying your Apple Developer Account email and storing it as a GitHub Actions secret (`APPLE_ID`).

---

## Step 1: Identify your Apple ID

1. Open your browser and go to: `https://developer.apple.com/account`.
2. Your Apple ID (email) is typically displayed in the top right corner or under the **Account Summary**.
3. For the `APPLE_ID` secret, you must use the email address associated with your Apple Developer Program membership.

---

## Step 2: Store as GitHub Secret (`APPLE_ID`)

1. Open your repository on GitHub → **Settings**.
2. In the sidebar under "Security", expand **Secrets and variables** → **Actions**.
3. Click the green **New repository secret** button.
4. **Name:** `APPLE_ID`
5. **Secret:** Paste your Apple Developer email address (e.g., `keyvan.araste@gmail.com`).
6. Click **Add secret**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/05_github_apple_id_filled.png ✅ -->
<!-- Screenshot: temp/05_github_apple_id_saved.png ✅ -->
<!-- Video:      temp/05_apple_id_video.webp ✅ -->

---

## Final Secrets Overview

Once `APPLE_ID`, `APPLE_PASSWORD`, and `APPLE_TEAM_ID` are added, your secrets list should look similar to this:

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/05_github_secrets_existing.png ✅ -->
