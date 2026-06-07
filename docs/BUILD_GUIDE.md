# AA Torque - Build Instructions (macOS + Homebrew)

This guide walks you through building the AA Torque Android Auto app from source on macOS.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Install Dependencies via Homebrew](#install-dependencies-via-homebrew)
3. [Clone the Repository](#clone-the-repository)
4. [Open in Android Studio](#open-in-android-studio)
5. [Command-Line Build](#command-line-build)
6. [Signing the APK](#signing-the-apk)
7. [Installing on Your Phone](#installing-on-your-phone)
8. [Troubleshooting](#troubleshooting)

---

## Prerequisites

| Tool | Version Required |
|------|-----------------|
| macOS | 12 (Monterey) or later |
| Java (JDK) | 19 (Zulu recommended) |
| Android SDK | API 34 (compileSdk) |
| Android Build Tools | 34.x |
| Gradle | 8.2 (bundled via wrapper) |

---

## Install Dependencies via Homebrew

```bash
# Install Homebrew if you don't have it
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Java 19 (Zulu distribution - what CI uses)
brew install --cask zulu@19

# Set JAVA_HOME (add to ~/.zshrc or ~/.bash_profile)
export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-19.jdk/Contents/Home
export PATH="$JAVA_HOME/bin:$PATH"

# Verify Java installation
java -version
# Should show: openjdk version "19.0.x"
```

### Install Android Studio

```bash
brew install --cask android-studio
```

Open Android Studio and let it complete the initial setup wizard. This installs:
- Android SDK (API 34)
- Android SDK Build-Tools 34
- Android Emulator
- SDK Platform-Tools

Alternatively, install the Android SDK via command line:

```bash
# If you prefer CLI-only setup
brew install --cask android-commandlinetools

# Accept licenses
yes | sdkmanager --licenses

# Install required SDK components
sdkmanager "platforms;android-34" "build-tools;34.0.0" "platform-tools"
```

---

## Clone the Repository

```bash
git clone --recurse-submodules https://github.com/wthrajat/android-auto-torque.git
cd android-auto-torque

# If you forgot --recurse-submodules:
git submodule update --init --recursive
```

**Important:** The project uses a git submodule for the speedviewlib dependency (`lib/speedviewlib`). The `--recurse-submodules` flag ensures it's fetched automatically.

The project also requires a `local.properties` file pointing to your Android SDK:

```bash
# Auto-generate local.properties (Android Studio does this for you)
echo "sdk.dir=$HOME/Library/Android/sdk" > local.properties
```

---

## Open in Android Studio

1. Launch Android Studio
2. Select **File → Open**
3. Navigate to the `aa-torque` folder and click **Open**
4. Wait for Gradle sync to complete (first time takes 5-10 minutes)
5. If prompted about Gradle wrapper, click **OK**
6. If prompted about SDK, install the required SDK (API 34)

### Android Studio Setup Notes

- **JDK 19** must be selected: File → Project Structure → SDK Location → Gradle JDK → zulu-19
- The project uses **Kotlin** + **Java** + **Jetpack Compose** + **Data Binding** + **Protobuf**

---

## Command-Line Build

### Quick Build (Debug APK)

```bash
# From the project root
chmod +x gradlew
./gradlew assembleDebug

# Output: app/build/outputs/apk/debug/app-debug.apk
```

### Release Build (Unsigned)

```bash
./gradlew assembleRelease

# Output: app/build/outputs/apk/release/app-release.apk
```

### Lint Check

```bash
./gradlew lintRelease
```

### Build with Version Bump

The version is controlled by `version.properties`:

```properties
majorVersion=2
minorVersion=0
patchVersion=30
```

To bump the version, edit `version.properties`:

```bash
# Example: bump patch version from 30 to 31
sed -i '' 's/patchVersion=30/patchVersion=31/' version.properties
```

The version code is calculated as: `majorVersion * 1000 + minorVersion * 100 + patchVersion * 10`

---

## Signing the APK

For debug builds, the debug keystore is used automatically. For release builds:

```bash
# Build release APK with debug signing
./gradlew assembleRelease \
  -Pandroid.injected.signing.store.file=$HOME/.android/debug.keystore \
  -Pandroid.injected.signing.key.alias=androiddebugkey \
  -Pandroid.injected.signing.store.password=android \
  -Pandroid.injected.signing.key.password=android
```

### Creating Your Own Keystore

```bash
keytool -genkey -v \
  -keystore my-release-key.jks \
  -keyalg RSA -keysize 2048 \
  -validity 10000 \
  -alias aa-torque
```

### Building a Signed Release APK

```bash
./gradlew assembleRelease \
  -Pandroid.injected.signing.store.file=/path/to/my-release-key.jks \
  -Pandroid.injected.signing.key.alias=aa-torque \
  -Pandroid.injected.signing.store.password=YOUR_PASSWORD \
  -Pandroid.injected.signing.key.password=YOUR_PASSWORD
```

---

## Installing on Your Phone

### Step 1: Enable USB Debugging on Your Phone

1. Go to **Settings → About Phone**
2. Tap **Build Number** 7 times to enable Developer Options
3. Go to **Settings → Developer Options**
4. Enable **USB Debugging**

### Step 2: Connect and Install

```bash
# Connect phone via USB and verify it's detected
adb devices

# Install the debug APK
adb install -r app/build/outputs/apk/debug/app-debug.apk

# OR install release APK
adb install -r app/build/outputs/apk/release/app-release.apk
```

### Step 3: Enable Android Auto Developer Mode

1. Open **Android Auto** on your phone (while NOT connected to car)
2. Tap **About Android Auto** header 10 times until you see "You're a developer!"
3. Tap the 3-dot menu → **Developer Settings**
4. Scroll down and check **Unknown Sources**
5. This allows apps from outside the Play Store to run on Android Auto

### Step 4: Install via KingInstaller (Recommended)

AA Torque needs to appear as if installed from Play Store. Use [KingInstaller](https://github.com/fcaronte/KingInstaller):

1. Download KingInstaller APK
2. Install it
3. Use KingInstaller to install the AA Torque APK
4. This spoofs the installer as `com.android.vending` (Play Store)

### Step 5: Force Update (Alternative)

In AA Torque Settings app:
1. Open the menu (3 dots)
2. Select **Force Update**
3. This attempts to make the phone think the app came from Play Store

---

## Troubleshooting

### "Unable to connect to Torque plugin service"

- Make sure **Torque Pro** is installed on your phone
- Make sure an OBD2 Bluetooth adapter is paired
- Open Torque Pro first, let it connect to the car
- Then open AA Torque

### Gradle Build Fails

```bash
# Clean and rebuild
./gradlew clean
./gradlew assembleDebug

# If Java version issues:
./gradlew assembleDebug -Dorg.gradle.java.home=$JAVA_HOME
```

### "SDK not found"

Make sure `local.properties` exists with:
```properties
sdk.dir=/Users/YOUR_USERNAME/Library/Android/sdk
```

### Submodule Issues

```bash
git submodule update --init --recursive
```

### ProGuard/R8 Issues

The release build uses R8 minification. If you see crashes in release but not debug, check `app/proguard-rules.pro`.

---

## Build Variants

| Variant | Description |
|---------|-------------|
| `debug` | Debug build, no minification, simulated metrics enabled |
| `release_debug` | Minified debug build with simulated metrics |
| `release` | Full release build with minification |

Switch variants in Android Studio via **Build → Select Build Variant**.

---

## Quick Reference Commands

```bash
# Build debug APK
./gradlew assembleDebug

# Build release APK
./gradlew assembleRelease

# Clean build
./gradlew clean assembleDebug

# Run lint
./gradlew lintRelease

# Install on connected device
adb install -r app/build/outputs/apk/debug/app-debug.apk

# View logs from the app
adb logcat -s AATORQUE
```
