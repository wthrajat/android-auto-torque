![CI Status](https://github.com/wthrajat/android-auto-torque/actions/workflows/ci.yml/badge.svg?branch=master)
![downloads](https://img.shields.io/github/downloads/wthrajat/android-auto-torque/total.svg)

# AA Torque

Display real-time vehicle data from [Torque Pro](https://play.google.com/store/apps/details?id=org.prowl.torque) on your car's Android Auto screen. Customizable gauges, charts, themes, and dashboards.

**This is NOT a standalone app.** You need [Torque Pro](https://play.google.com/store/apps/details?id=org.prowl.torque) and a Bluetooth OBD2 adapter.

## Table of Contents

- [Features](#features)
- [What You Need](#what-you-need)
- [Installation Guide](#installation-guide)
- [Updating](#updating)
- [Optimization Tips](#optimization-tips)
- [Troubleshooting](#troubleshooting)
- [Custom Expressions](#custom-expressions)
- [Building from Source](#building-from-source)
- [Documentation](#documentation)
- [Screenshots](#screenshots)

## Video

[![Watch on YouTube](https://github.com/wthrajat/android-auto-torque/assets/2042303/b735fc1c-8732-4237-8a8d-a540a0d1f778)](https://www.youtube.com/watch?v=gYJJ9M9a0m8)

## Features

- Display any PID from Torque Pro, including custom PIDs
- 25+ built-in themes (VW, Audi, Skoda, Seat, BMW, Ford, Aston Martin, EV, and more)
- 11 custom fonts matching car brand aesthetics
- 23 background wallpapers with album art support
- Up to 10 dashboards with swipe/rotary navigation
- 3 gauge styles: needle, high-visibility ray, or min/max indicator
- 4 text displays per dashboard for secondary data
- Real-time line chart with up to 3 simultaneous traces
- Rotary dial support for cars with scroll wheels
- Custom expressions via [EvalEx](https://ezylang.github.io/EvalEx/references/functions.html) for unit conversion (e.g., F to C, PSI to BAR)
- Alarm system to change gauge colors based on thresholds
- Album art background from your music player
- Backup and restore as a `.pb` file
- Built-in auto-update from GitHub releases

---

## What You Need

| Item | Price (India) | Price (US) | Where to Buy |
|------|--------------|------------|--------------|
| OBD2 Bluetooth Adapter | Rs. 800 - Rs. 5,000 | $15 - $120 | Amazon, Flipkart |
| Torque Pro App | Rs. 200 (one-time) | $4.99 (one-time) | Google Play Store |

Optional: [KingInstaller](https://github.com/fcaronte/KingInstaller) (free, required for Pixel phones)

See the [OBD2 Adapter Guide](docs/OBD2_ADAPTERS.md) for what to buy.

---

## Installation Guide

> Prefer video? Watch the [video installation guide](https://www.youtube.com/watch?v=DDxPrPzxZ3k).

### Step 1: Pair Your OBD2 Adapter

1. Plug the OBD2 adapter into your car's diagnostic port (see [OBD2 Adapter Guide](docs/OBD2_ADAPTERS.md) for port locations)
2. Turn the ignition to ON (engine can be off)
3. Go to Settings > Bluetooth on your phone
4. Pair with the adapter (usually named `OBDII` or `Vlink`, PIN `1234` or `0000`)
5. Don't open Torque Pro yet

### Step 2: Install Torque Pro

1. Install [Torque Pro](https://play.google.com/store/apps/details?id=org.prowl.torque) from the Play Store
2. Open it, go to Settings > OBD2 Adapter Settings > Bluetooth
3. Select your paired adapter
4. Start the car engine and wait for the green indicator (connected to ECU)
5. Keep Torque Pro running in the background

### Step 3: Download AA Torque

1. Go to [github.com/wthrajat/android-auto-torque/releases](https://github.com/wthrajat/android-auto-torque/releases)
2. Download the latest `aa-torque.apk`
3. Don't install it yet

### Step 4: Install KingInstaller (Pixel Phones Only)

Google Pixel phones need this to make Android Auto recognize third-party apps. It spoofs the Play Store installer so your phone thinks AA Torque came from the official store.

1. Download [KingInstaller](https://github.com/fcaronte/KingInstaller/releases)
2. Install it normally
3. Open it and grant permissions

**Pixel Android 14/15:** You may need to update Google Package Installer first. Get it from [APKMirror](https://www.apkmirror.com/apk/google-inc/package-installer-com-google-android-packageinstaller/) or extract it from your phone using a file manager like [MiXplorer](https://mixplorer.com/).

Google frequently patches the loopholes KingInstaller uses. Check [GitHub Discussions](https://github.com/wthrajat/android-auto-torque/discussions) if something breaks.

### Step 5: Install AA Torque

1. Open KingInstaller > "Install from storage"
2. Select the `aa-torque.apk` you downloaded
3. Allow installation when prompted
4. Verify: open Android Auto on your phone, you should see "AA Torque" in the available apps

### Step 6: Enable Android Auto Developer Mode

1. Disconnect your phone from the car
2. Open the Android Auto app
3. Tap "About Android Auto" 10 times rapidly
4. Tap the 3-dot menu > Developer Settings
5. Check "Unknown Sources"
6. Go back, AA Torque should now appear in the app list

### Step 7: Configure

1. Open AA Torque Settings on your phone
2. Grant all permissions (location, phone, notifications)
3. Pick a theme, font, and background ("Electro Vehicle" or "Dark" are good starting points)
4. Configure your dashboards: set up gauges and text displays, select PIDs, set min/max values

### Step 8: Use in Your Car

1. Connect your phone to the car via USB
2. Android Auto launches automatically
3. Tap the dashboard icon, select "AA Torque"
4. Data appears within 5-10 seconds
5. Swipe left/right to switch dashboards, swipe up/down for chart view

**First time only:** tap the 3-dot menu > "Force Update". This spoofs the Play Store installer so your phone trusts the app.

> Torque Pro must be running and connected to the OBD2 adapter for AA Torque to display data.

---

## Updating

AA Torque checks for updates automatically. When a new version is available, tap "Download" in the settings app. If the install fails, download the APK manually from [GitHub Releases](https://github.com/wthrajat/android-auto-torque/releases) and install via KingInstaller.

After a phone restart or OS update, you may need to re-enable Unknown Sources in Android Auto Developer Settings.

---

## Optimization Tips

### Disable Battery Optimization

Both Torque Pro and AA Torque need to run continuously. Your phone may kill them to save battery.

- AA Torque: Settings > Apps > AA Torque > Battery > Unrestricted
- Torque Pro: Settings > Apps > Torque Pro > Battery > Unrestricted

### Keep Your Phone Cool

- Use a ventilated phone mount (not a closed holder)
- Avoid direct sunlight on the phone
- Turn on car AC before connecting
- Close background apps

### USB Cable

Use a data + charge cable (not charge-only). Short cables (1m) are more reliable than long ones.

---

## Troubleshooting

See the [Troubleshooting Guide](docs/TROUBLESHOOTING.md) for common issues and fixes.

---

## Custom Expressions

AA Torque supports [EvalEx](https://ezylang.github.io/EvalEx) expressions for unit conversion. Use `a` as the variable for the current value.

| Conversion | Expression |
|-----------|------------|
| C to F | `a * 1.8 + 32` |
| F to C | `(a - 32) / 1.8` |
| BAR to PSI | `a * 14.5038` |
| PSI to BAR | `a * 0.0689476` |
| kPa to PSI | `a * 0.145038` |
| km/h to MPH | `a * 0.621371` |
| MPH to km/h | `a * 1.60934` |
| L/100km to MPG | `235.215 / a` |
| Nm to ft-lb | `a * 0.737562` |
| Round to integer | `round(a)` |

To use: open AA Torque Settings > tap a dashboard > tap a gauge or display > enable "Run unit conversion" > enter your expression.

Full docs: [EvalEx Functions Reference](https://ezylang.github.io/EvalEx/references/functions.html)

---

## Building from Source

See the [Build Guide](docs/BUILD_GUIDE.md) for complete instructions.

Quick start (macOS):

```bash
brew install --cask zulu@19
export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-19.jdk/Contents/Home
git clone --recurse-submodules https://github.com/wthrajat/android-auto-torque.git
cd android-auto-torque
echo "sdk.dir=$HOME/Library/Android/sdk" > local.properties
./gradlew assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

---

## Documentation

| Document | Description |
|----------|-------------|
| [Build Guide](docs/BUILD_GUIDE.md) | Build instructions for macOS, Windows, Linux |
| [Architecture](docs/ARCHITECTURE.md) | How the app works internally |
| [Codebase Analysis](docs/CODEBASE_ANALYSIS.md) | File-by-file analysis of every source file |
| [Modding Guide](docs/MODDING_GUIDE.md) | How to add themes, fonts, PIDs |
| [Honda Elevate Guide](docs/HONDA_ELEVATE.md) | Honda Elevate-specific setup and PIDs |
| [OBD2 Adapter Guide](docs/OBD2_ADAPTERS.md) | What adapter to buy and how to pair it |
| [Troubleshooting](docs/TROUBLESHOOTING.md) | Common issues and fixes |

---

## Screenshots

<img src="https://github.com/wthrajat/android-auto-torque/assets/2042303/e31b0598-25ec-4003-bcee-11e5e108d3a4" width="100%" alt="Dashboard with 3 gauges and text displays" />

<img src="https://github.com/wthrajat/android-auto-torque/assets/2042303/bf40ed97-adf3-4923-bbc5-63a618899173" width="100%" alt="Album art background with ray instead of needle" />

<img src="https://github.com/wthrajat/android-auto-torque/assets/2042303/a99571f9-0bb3-43ec-89ce-938e41506e97" width="100%" alt="Chart view showing real-time data" />

<img src="https://github.com/wthrajat/android-auto-torque/assets/2042303/229315c8-ad3b-42e6-86e6-e7fe7abb16a8" width="100%" alt="Dashboard with different theme" />

<img src="https://github.com/wthrajat/android-auto-torque/assets/2042303/698c666b-5e3c-4611-80a5-8e767b04186a" width="100%" alt="Dashboard with multiple displays" />

---

Contributing: [CONTRIBUTING.md](CONTRIBUTING.md)

---

## Fork Notice

This project is a fork of [AA Torque](https://github.com/agronick/aa-torque) by Kyle Agronick, licensed under the [GNU General Public License v3](LICENSE.md).

Kyle Agronick is the original author and creator of AA Torque. The original project is based on [Chillout's Performance Monitor](https://github.com/jilleb/mqb-pm), which was based on Martoreto's aa-stats.

Under GPL v3, anyone who redistributes this software must:
- Keep the original license and copyright notices
- Distribute under the same GPL v3 license
- Make the source code available to recipients
- Mark modified files clearly

Icon credits:
- Horse by Peleg Red; [Noun Project](https://thenounproject.com/browse/icons/term/horse/) (CC BY 3.0)
- Engine Cylinder by Thuy Nguyen; [Noun Project](https://thenounproject.com/browse/icons/term/engine-cylinder/) (CC BY 3.0)
- Transmission by Gustyne Pissesa Ardhaneswari; [Noun Project](https://thenounproject.com/browse/icons/term/transmission/) (CC BY 3.0)