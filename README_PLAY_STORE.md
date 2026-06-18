Publishing the PWA as an Android app (TWA) — Quick Guide

Overview
--------
This repo ships a Progressive Web App. To publish on Google Play, the recommended flow is to package it as a Trusted Web Activity (TWA) using Bubblewrap, then build and upload an Android App Bundle (.aab).

Files added here to help:
- `/.well-known/assetlinks.json` — placeholder Digital Asset Links file (update with your package name and SHA256 fingerprint).
- `bubblewrap-config.json` — example config used by `bubblewrap init` (replace `host` and `packageId`).

Steps
-----
1. Choose a package name (reverse-domain), e.g. `com.yourorg.gymcompanion` and set `packageId` in `bubblewrap-config.json`.
2. Host `/.well-known/assetlinks.json` on your domain with the real SHA256 fingerprint of the signing key.
3. Install Bubblewrap:

```bash
npm install -g @bubblewrap/cli
```

4. Initialize the TWA project (example):

```bash
bubblewrap init --manifest=https://your-domain/manifest.json --manifestIsValid
```

Or use the included config:

```bash
bubblewrap init --config=bubblewrap-config.json
```

5. Build and sign the release AAB:

```bash
# generate keystore (if you don't have one)
keytool -genkeypair -v -keystore key.jks -alias gymcomp -keyalg RSA -keysize 2048 -validity 10000

# build
./gradlew bundleRelease

# find AAB at app/build/outputs/bundle/release/app-release.aab
```

6. Upload the `.aab` to Google Play Console, fill store listing, content rating, privacy policy, target audience, and roll out the release.

Notes & Tips
--------------
- The `assetlinks.json` must be served from `https://<your-domain>/.well-known/assetlinks.json` and the package name and SHA256 fingerprint must match the signing key used for the AAB.
- Use internal testing track in Play Console first to validate behavior.
- If you prefer a GUI, PWABuilder can generate a TWA project as well.

Want me to:
- Fill `bubblewrap-config.json` with your real domain & package name and commit it? (Done — see `bubblewrap-config.json`)
- Create a GitHub Action that runs Bubblewrap and builds an unsigned AAB for internal testing? (You’ll still need to provide signing credentials securely.)

Suggested Play Console fields (ready-to-copy)
------------------------------------------------
- App name: Gym Companion
- Package name: com.ambijat.gymcompanion
- Default language: English (United Kingdom) — en-GB
- App or game: App
- Free or paid: Free
- Category: Health & Fitness
- Contact details: email: YOUR_DEVELOPER_EMAIL@example.com, phone optional, website: https://ambijat.github.io/gym-companion/
- Privacy policy URL: https://ambijat.github.io/gym-companion/privacy.html

Store listing text suggestions
-----------------------------
- Short description (max ~80 chars): L4/L5 safe workout companion — guided sessions, timers, logs
- Full description (detailed): Gym Companion is a lightweight progressive web app tailored for safe L4/L5 back-friendly gym sessions. It provides guided workouts, pacing timers, technique cues, progress logging, and short form demos to keep your training effective and spine-safe. Use it on the go or install from the Play Store for an app-like experience.

Assets you'll need
-------------------
- High-res icon: 512x512 PNG (transparent allowed)
- Feature graphic: 1024x500 JPEG/PNG (required for Play listing)
- Screenshots: vertical phone screenshots (1080x1920 or similar) — add 3–6 showing the hero, timer, and session list
- Promo video (optional): YouTube link

Digital Asset Links / Signing note
--------------------------------
- `/.well-known/assetlinks.json` has been created in this repo and is published at:
	`https://ambijat.github.io/gym-companion/.well-known/assetlinks.json`
- Replace `REPLACE_WITH_YOUR_SHA256_FINGERPRINT` with the SHA256 fingerprint of the signing key that will sign the app users install. If you use Google Play App Signing, use the *app signing* certificate fingerprint (you can fetch it from Play Console after enabling app signing).

Get SHA256 from a keystore (example):
```bash
# generate keystore (if needed)
keytool -genkeypair -v -keystore key.jks -alias gymcomp -keyalg RSA -keysize 2048 -validity 10000

# list fingerprint
keytool -list -v -keystore key.jks -alias gymcomp | grep SHA256
```

Hosting and verification
------------------------
- Ensure `/.well-known/assetlinks.json` is reachable at `https://ambijat.github.io/gym-companion/.well-known/assetlinks.json` before uploading the AAB.
- After you upload your first release to Play and enable Play App Signing, verify the *app signing certificate* fingerprint in Play Console and update `assetlinks.json` if necessary.

Next steps I can take for you
----------------------------
- Replace the `REPLACE_WITH_YOUR_SHA256_FINGERPRINT` value if you provide the keystore (do **not** share private keys in chat; tell me the fingerprint instead).
- Scaffold a GitHub Action to run Bubblewrap and build an unsigned AAB for internal testing (it will require storing signing secrets in GitHub Actions secrets).
- Create a basic `privacy.html` page in the repo and commit it so you have a privacy policy URL to link in Play Console. (I can draft a concise GDPR-friendly privacy policy template.)

Tell me which of the above you'd like me to do next.
