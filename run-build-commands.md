# Ionic Angular + Capacitor Android Setup

## 1. Prerequisites

Installed software:

- Node.js and npm
- Ionic CLI
- Android Studio
- Android SDK
- Android SDK Build-Tools
- Android SDK Platform-Tools
- Android SDK Command-line Tools
- Android Studio bundled JDK

Android environment variable:

```text
ANDROID_HOME=C:\Users\YOUR_USERNAME\AppData\Local\Android\Sdk
```

Add the following to Windows PATH:

```text
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\cmdline-tools\latest\bin
```

Verify Android SDK installation:

```bash
adb --version
```

---

## 2. Open Ionic Project in VS Code

Open the terminal in VS Code and navigate to the Ionic project root (the folder containing package.json).

```bash
cd path\to\myApp
```

Check Capacitor installation:

```bash
npx cap --version
```

Check installed Capacitor packages:

```bash
npm ls @capacitor/core @capacitor/cli @capacitor/android
```

---

## 3. Install Capacitor (Only If Not Already Installed)

Install Capacitor Core and CLI:

```bash
npm install @capacitor/core @capacitor/cli
```

Install the Android platform package matching the same Capacitor major version.

Example for Capacitor 8:

```bash
npm install @capacitor/android@8
```

Note: Keep @capacitor/core, @capacitor/cli and @capacitor/android on compatible versions.

---

## 4. Add Android Platform

Generate the native Android project:

```bash
npx cap add android
```

This creates the `android` folder inside the Ionic project.

The Android folder contains the native Android application project, including Gradle configuration, manifests, resources and native code.

Run this command only once when initially adding Android.

---

## 5. Build Ionic Application

Compile the Ionic Angular application:

```bash
ionic build
```

This generates the production web application files in the configured web output directory (normally `www`).

---

## 6. Synchronize Ionic with Android

Execute:

```bash
npx cap sync android
```

This performs two operations:

- Copies the latest web application files into the Android project.
- Updates native Capacitor plugins and Android dependencies.

Run this whenever you install or update Capacitor plugins, or when you need to synchronize web assets and native dependencies.

---

## 7. Open Android Studio

Execute:

```bash
npx cap open android
```
 
This opens the generated Android project in Android Studio.

Wait for Gradle synchronization to finish.

You can now run the application using an Android emulator or a connected physical Android device.

---

## 8. Normal Development Workflow

Whenever you modify your Ionic Angular application:

```bash
ionic build
npx cap sync android
npx cap open android
```

You can omit `npx cap open android` if Android Studio is already open.

After synchronization, click Run in Android Studio to launch the updated application.

---

# Android Application Configuration

## 9. Application ID / Package Name

File:

```text
capacitor.config.ts
```

Example:

```typescript
const config: CapacitorConfig = {
  appId: 'com.company.myapp',
  appName: 'My Application',
  webDir: 'www'
};

export default config;
```

`appId` represents the application's unique package identifier.

Example:

```text
com.company.myapp
```

This should match the Android application's `applicationId`.

## 10. Android Application ID

File:

```text
android/app/build.gradle
```

Find:

```gradle
defaultConfig {
    applicationId "com.company.myapp"
}
```

This is the Android application ID used for installation and Google Play publishing.

Keep it consistent with `appId` in capacitor.config.ts.

## 11. Version Code and Version Name

File:

```text
android/app/build.gradle
```

Find:

```gradle
defaultConfig {
    versionCode 1
    versionName "1.0"
}
```

Explanation:

- `versionCode`: Internal integer version used by Android and Google Play. Increment it for every Play Store release.
- `versionName`: User-visible application version.

Example:

```gradle
versionCode 2
versionName "1.1"
```

## 12. Minimum SDK, Target SDK and Compile SDK

File:

```text
android/variables.gradle
```

Find:

```gradle
ext {
    minSdkVersion = 23
    compileSdkVersion = 36
    targetSdkVersion = 36
}
```

Explanation:

- `minSdkVersion`: Minimum Android version supported by your application.
- `compileSdkVersion`: Android SDK version used to compile the application.
- `targetSdkVersion`: Android version whose behavior and compatibility requirements your application targets.

Actual values depend on the installed Capacitor and Android Gradle Plugin versions. Check plugin requirements before changing them.

## 13. Application Name

File:

```text
capacitor.config.ts
```

Find:

```typescript
appName: 'My Application'
```

This is the application name configured through Capacitor.

The generated Android application label can also be found in:

```text
android/app/src/main/res/values/strings.xml
```

Look for the `app_name` string.

---

# Important Notes

1. Do not run `npx cap add android` repeatedly. It is an initial setup command.
2. Run `ionic build` before synchronizing web assets.
3. Run `npx cap sync android` after installing or updating native plugins.
4. Keep the three Capacitor packages on compatible versions.
5. Keep the Android folder in Git/version control.
6. Do not casually change the application ID after publishing to Google Play.
7. Increment versionCode for every new Play Store release.
8. Native Android configuration is maintained in the Android folder, not in config.xml like Cordova.

