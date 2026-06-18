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
- Fill `bubblewrap-config.json` with your real domain & package name and commit it?
- Create a GitHub Action that runs Bubblewrap and builds an unsigned AAB for internal testing? (You’ll still need to provide signing credentials securely.)
