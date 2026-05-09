# App-Specific Password Generation & GitHub Secret

This document covers generating an App-Specific Password from your Apple Account and storing it as a GitHub Actions secret (`APPLE_PASSWORD`).

---

## Step 1: Navigate to Apple Account Management

1. Open your browser and go to: `https://account.apple.com/account/manage`.
2. Sign in with your Apple ID if prompted.

<!-- 🖼️ IMAGE PLACEHOLDER: temp/02_apple_id_click_app_specific.png -->

---

## Step 2: Access App-Specific Passwords

1. Scroll down to the **Sign-In and Security** section.
2. Click on **App-Specific Passwords**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/02_apple_id_click_app_specific.png ✅ -->

---

## Step 3: Generate a New Password

1. Click the **Generate an app-specific password** or the **+ (Plus)** button.
2. Enter an identifiable name for the password (e.g., `WebQ CICD`).
3. Click **Create**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/02_apple_id_click_plus.png ✅ -->

---

## Step 4: Copy the Generated Password

1. Apple will display a unique 16-character password (e.g., `xxxx-xxxx-xxxx-xxxx`).
2. **Copy this password immediately** — you cannot view it again once you click "Done".
3. Click **Done**.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/02_apple_id_password_generated.png ✅ -->
<!-- Video:      temp/02_apple_id_video.webp ✅ -->

---

## Step 5: Store as GitHub Secret (`APPLE_PASSWORD`)

1. Open your repository on GitHub → **Settings**.
2. In the sidebar under "Security", expand **Secrets and variables** → **Actions**.
3. Click the green **New repository secret** button.
4. **Name:** `APPLE_PASSWORD`
5. **Secret:** Paste the 16-character App-Specific password you just copied.
6. Click **Add secret**.

> ⚠️ **Best Practice:** Generate a unique App-Specific Password for each repository/project rather than sharing one across all pipelines.

<!-- 🖼️ ASSETS -->
<!-- Screenshot: temp/03_github_apple_password_click_new.png ✅ -->
<!-- Screenshot: temp/03_github_apple_password_filled.png ✅ -->
<!-- Video:      temp/03_github_apple_password_video.webp ✅ -->
