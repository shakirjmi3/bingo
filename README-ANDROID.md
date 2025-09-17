# Bingo Android App

This wraps your existing `index.html` in an Android WebView so it can be installed and uploaded to Google Play.

## Build prerequisites
- Android Studio Ladybug+ or command line tools
- JDK 17
- Android SDK Platform 34, Build-Tools 34.x

## Project structure
- `android/` – Gradle project
- `android/app/src/main/assets/` – Contains `index.html`, `styles.css`, `app.js`

## Run debug build (ADB-connected device/emulator)
```
cd android
./gradlew assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```
On Windows use `gradlew.bat`.

## Release signing
Create a keystore (one-time):
```
keytool -genkey -v -keystore bingo-release.keystore -alias bingo -keyalg RSA -keysize 2048 -validity 36500
```
Create `android/keystore.properties` (not committed):
```
storeFile=bingo-release.keystore
storePassword=YOUR_STORE_PASSWORD
keyAlias=bingo
keyPassword=YOUR_KEY_PASSWORD
```
Place the `.keystore` file in `android/` or set an absolute path in `storeFile`.

## Build release APK / App Bundle
```
cd android
./gradlew assembleRelease   # APK
./gradlew bundleRelease     # AAB for Play Store
```
Outputs:
- APK: `android/app/build/outputs/apk/release/app-release.apk`
- AAB: `android/app/build/outputs/bundle/release/app-release.aab`

## Play Console upload checklist
- Prepare app listing (name, short/long description, screenshots, icon, feature graphic)
- Content rating, target audience, data safety
- Upload signed `app-release.aab`
- Set `applicationId` uniquely (`com.shakir.bingo` currently)
- Set versioning in `android/app/build.gradle` (`versionCode`, `versionName`)

## Customization
- App icon: replace files under `android/app/src/main/res/mipmap-*` and `ic_launcher_foreground.xml`
- App name: edit `android/app/src/main/res/values/strings.xml`
- Package name: update `namespace`/`applicationId` and Kotlin package paths

## Notes
- WebView loads `file:///android_asset/index.html` offline.
- `speechSynthesis` and WebAudio work on modern WebView; device policies may vary.

