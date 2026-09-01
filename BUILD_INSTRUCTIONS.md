# Building the Android App (with daily notifications)

This turns your Personal Finance Manager into a real Android app using
Capacitor, with a native daily reminder notification (fires at 8:00 PM
even if the app is closed).

## Prerequisites (install once on your computer)

1. **Node.js** (v18+): https://nodejs.org
2. **Android Studio**: https://developer.android.com/studio
   - During setup, let it install the Android SDK (default options are fine).

## Steps

1. Unzip this project folder anywhere on your computer, then open a
   terminal inside it.

2. Install dependencies:
   ```
   npm install
   ```
   (This now includes `@capacitor/filesystem` and `@capacitor/share`,
   which the app needs to actually save Excel/backup files on Android —
   without them, "Export Excel" and "Backup Data" would silently do
   nothing, which is a bug in earlier versions of this project.)

3. Add the Android platform:
   ```
   npx cap add android
   ```

4. Sync your web app + plugins into the native project:
   ```
   npx cap sync
   ```
   Run this again any time you change `www/index.html` or add a plugin
   to `package.json`.

   **If you already ran `npx cap add android` before this update:**
   just re-run `npm install` then `npx cap sync` — no need to re-add
   the platform.

5. Open the project in Android Studio:
   ```
   npx cap open android
   ```

6. In Android Studio:
   - Wait for Gradle to finish syncing (bottom status bar).
   - Plug in your phone via USB (with USB debugging enabled in
     Developer Options), or use an emulator.
   - Click the green ▶ Run button to install and launch the app
     directly on your device.

   **OR**, to get a standalone `.apk` file you can share/sideload:
   - Go to `Build` → `Build Bundle(s) / APK(s)` → `Build APK(s)`.
   - Once built, click the "locate" link in the notification popup,
     or find it at:
     `android/app/build/outputs/apk/debug/app-debug.apk`
   - Copy that file to your phone and open it to install (you'll need
     to allow "install unknown apps" for whichever app you use to
     open it).

## Changing the reminder time

Open `www/index.html` and find this block near the bottom:

```js
schedule: {
  on: { hour: 20, minute: 0 }, // fires daily at 8:00 PM
  ...
}
```

Change `hour`/`minute` (24-hour format) to whatever time you'd like,
then re-run `npx cap sync` and rebuild.

## Exporting Excel / backup files on Android

Tapping "Export Excel" or "Backup Data" now writes the file with the
Filesystem plugin and opens the Android share sheet ("Save to Files",
Google Drive, WhatsApp, etc.) so you can choose where it lands — this
works reliably inside the app's WebView, unlike a plain browser
download link. Pick "Save to Files" / "Drive" and choose Downloads (or
any folder) to keep a copy on the device.

## Notes

- The first time you launch the app, Android will ask for notification
  permission — make sure to allow it.
- App icon/name can be customized later via Android Studio's image
  asset tool if you want something nicer than the placeholder icon.
- If you eventually want to publish to the Play Store, you'll need to
  build a signed release APK/AAB — Android Studio has a guided wizard
  for this under `Build` → `Generate Signed Bundle / APK`.
