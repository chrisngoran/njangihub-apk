# NjangiHub — Android App (Capacitor project)

This is a ready-to-build Capacitor Android project wrapping your Njangi Credit
Union web app (`www/index.html`) as a native Android app, targeting the
Google Play Store.

App ID: `com.techbridgehub.njangi`
App Name: NjangiHub

## Fastest path: build the APK in the cloud with GitHub Actions (no install needed)

This project includes a ready-made workflow at
`.github/workflows/build-apk.yml` that builds a debug APK automatically —
no Android Studio, no local setup.

1. Create a new repo on GitHub (public or private, doesn't matter) —
   e.g. `njangihub-android`.
2. Push this project to it:
   ```
   cd njangi-app
   git init
   git add .
   git commit -m "Initial NjangiHub Android project"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/njangihub-android.git
   git push -u origin main
   ```
3. On GitHub, go to your repo → **Actions** tab. The "Build Android APK"
   workflow will already be running (it triggers automatically on push).
   Wait ~2–3 minutes for it to finish (green check ✅).
4. Click into the finished run → scroll to **Artifacts** → download
   **NjangiHub-debug-apk**. Unzip it — that's your `.apk` file.
5. Send that `.apk` to any Android phone (WhatsApp, email, USB, Google
   Drive) and install it directly — this works for testing right away,
   no Play Store needed.

If you ever change `Njangi_Credit_Union_App2.html`, just replace
`njangi-app/www/index.html` with the new version, commit, and push again —
Actions rebuilds automatically.

**Note:** this produces a *debug* APK — perfect for testing and sharing
with your njangi members directly. For the actual **Google Play Store**
submission you need a *signed release* build (see below) — I can extend
this workflow to produce that too once you're ready, using a signing
keystore stored as a GitHub secret.

## Alternative: build locally with Android Studio (10–15 minutes)

1. **Install Android Studio** (if you don't have it): https://developer.android.com/studio
2. **Unzip this project** anywhere on your computer.
3. Open Android Studio → **Open** → select the `android` folder inside this
   project (not the root folder — the `android` subfolder).
4. Let Android Studio sync (it will auto-download the Gradle wrapper and SDK
   components on first open — this needs a normal internet connection).
5. Once synced:
   - **Test on your phone**: connect your Android phone via USB (enable
     Developer Options → USB Debugging), then click the green ▶ Run button.
   - **Build a debug APK to share/test**: `Build → Build Bundle(s)/APK(s) → Build APK(s)`.
     The APK will appear in `android/app/build/outputs/apk/debug/`.
   - **Build for Google Play Store**: `Build → Generate Signed Bundle / APK`
     → choose **Android App Bundle (.aab)** → create a new signing key
     (keep this keystore file safe — you'll need the *same* one for every
     future update) → build in `release` mode. Upload the resulting `.aab`
     to Google Play Console.

## Updating the app later

Whenever you change `Njangi_Credit_Union_App2.html`, just:
```
cp Njangi_Credit_Union_App2.html njangi-app/www/index.html
cd njangi-app
npx cap sync android
```
Then rebuild (push to GitHub for Actions, or rebuild in Android Studio).

## Before submitting to Play Store, you'll still need:

- **App icon & splash screen** — currently using Capacitor's default icon.
  Easiest way: use https://icon.kitchen or Android Studio's built-in
  **Image Asset Studio** (right-click `res` folder → New → Image Asset).
- **A Google Play Developer account** ($25 one-time fee).
- **Privacy policy URL** — required by Play Store since the app syncs data
  to Supabase (a hosted URL with a short privacy statement is enough).
- **Screenshots + store listing text** (Play Console will prompt you).
- **Signing keystore** — created during "Generate Signed Bundle" above, or
  via the GitHub Actions release workflow once set up.
  Back this up somewhere safe; losing it means you can never update the
  app again under the same listing.

## Notes on this app specifically

- The app is currently a single self-contained HTML file (`www/index.html`)
  using `localStorage` + Supabase cloud sync — this works fine inside a
  Capacitor WebView with no changes needed.
- Internet permission is already added to `AndroidManifest.xml` (required
  for the Supabase sync to work).
- If you later want offline-first behavior (app opens instantly with no
  network, syncs when available), that's already partially true since data
  is cached in localStorage — no extra work needed for MVP.

