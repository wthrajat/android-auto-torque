![CI Status](https://github.com/agronick/aa-torque/actions/workflows/ci.yml/badge.svg?branch=master)
![downloads](https://img.shields.io/github/downloads/agronick/aa-torque/total.svg)

# AA Torque — Torque Plugin for Android Auto

Display real-time vehicle data from [Torque Pro](https://play.google.com/store/apps/details?id=org.prowl.torque) on your car's Android Auto screen. Customizable gauges, charts, themes, and dashboards.

> **This is NOT a standalone app.** It requires [Torque Pro](https://play.google.com/store/apps/details?id=org.prowl.torque) and a Bluetooth OBD2 adapter to function.

## Table of Contents

- [Video Walkthrough](#video-walkthrough)
- [Features](#features)
- [What You Need](#what-you-need)
  - [Hardware Shopping List](#hardware-shopping-list)
  - [Recommended OBD2 Adapters](#recommended-obd2-bluetooth-adapters)
  - [Software You Need](#software-you-need-to-install)
- [Installation Guide](#installation-guide)
  - [Step 1: Pair OBD2 Adapter](#step-1-buy-and-pair-your-obd2-adapter)
  - [Step 2: Install Torque Pro](#step-2-install-torque-pro)
  - [Step 3: Download AA Torque](#step-3-download-aa-torque)
  - [Step 4: Install KingInstaller (Pixel)](#step-4-install-kinginstaller-required-for-pixel-phones)
  - [Step 5: Install AA Torque](#step-5-install-aa-torque-via-kinginstaller)
  - [Step 6: Enable Developer Mode](#step-6-enable-android-auto-developer-mode)
  - [Step 7: Configure](#step-7-configure-aa-torque)
  - [Step 8: Use in Car](#step-8-use-in-your-car)
- [Updating the App](#updating-the-app)
- [Optimization Tips](#optimization-tips)
- [Troubleshooting](#troubleshooting)
- [Custom Expressions](#custom-expressions)
- [Building from Source](#building-from-source)
- [Documentation](#documentation)
- [Screenshots](#screenshots)
- [Help & Community](#help--community)
- [Contributing](#contributing)
- [Donate](#donate)
- [Credits](#credits)

## Video Walkthrough

[![Watch on YouTube](https://github.com/agronick/aa-torque/assets/2042303/b735fc1c-8732-4237-8a8d-a540a0d1f778)](https://www.youtube.com/watch?v=gYJJ9M9a0m8)

## Features

- Display any PID from Torque Pro including custom PIDs
- **25+ built-in themes** (VW, Audi, Skoda, Seat, BMW, Ford, Aston Martin, EV, and more)
- **11 custom fonts** matching car brand aesthetics
- **23 background wallpapers** including album art support
- Up to **10 customizable dashboards** with swipe/rotary navigation
- **3 gauge styles**: needle, high-visibility ray, or min/max indicator
- **4 text displays** per dashboard for secondary data
- **Real-time line chart** with up to 3 simultaneous traces
- **Rotary dial support** for cars with scroll wheels
- **Custom expressions** using [EvalEx](https://ezylang.github.io/EvalEx/references/functions.html) for unit conversion (e.g., °F→°C, PSI→BAR)
- **Alarm system** — change gauge colors based on thresholds (shift lights, temperature warnings)
- **Album art background** — use now-playing album art as your dashboard wallpaper
- **Backup & restore** — export/import your configuration as a `.pb` file
- **Auto-update** — check for new releases from within the app

---

# What You Need

## Hardware Shopping List

You need **3 things** to use AA Torque:

| # | Item | Price (India ₹) | Price (US $) | Where to Buy |
|---|------|----------------|-------------|--------------|
| 1 | **OBD2 Bluetooth Adapter** | ₹800 – ₹5,000 | $15 – $120 | Amazon, Flipkart |
| 2 | **Torque Pro App** | ₹200 (one-time) | $4.99 (one-time) | Google Play Store |
| 3 | **USB Cable** (for wired Android Auto) | ₹200 – ₹500 | $5 – $15 | Amazon, local store |

**Optional but recommended:**

| Item | Price | Purpose |
|------|-------|---------|
| AAWireless Dongle | ₹8,000 – ₹12,000 | Wireless Android Auto |
| KingInstaller App | Free | Required for Pixel phones (see below) |

### Total Estimated Cost: ₹1,200 – ₹6,000 ($25 – $140)

---

## Recommended OBD2 Bluetooth Adapters

The OBD2 adapter plugs into your car's diagnostic port (usually under the steering wheel) and sends vehicle sensor data to your phone via Bluetooth.

### 🏆 Best Overall: OBDLink MX+ (~₹5,000 / $100)

- Fastest data refresh rates
- Supports all Honda protocols
- Sleep mode (won't drain car battery)
- Works with Torque Pro, Car Scanner, and all major apps
- [Amazon India](https://www.amazon.in/s?k=OBDLink+MX%2B) | [Amazon US](https://www.amazon.com/s?k=OBDLink+MX%2B)

### 💰 Best Value: Vgate iCar2 (~₹1,800 / $35)

- Reliable Bluetooth connection
- Good enough for most users
- Fast shipping on Amazon India
- [Amazon India](https://www.amazon.in/s?k=Vgate+iCar2)

### 💵 Budget Option: ELM327 Bluetooth (~₹800 / $15)

- Works for basic PIDs (RPM, Speed, Coolant Temp)
- May have slower refresh rates
- Quality varies — buy from reputable sellers
- [Amazon India](https://www.amazon.in/s?k=ELM327+Bluetooth+OBD2)

### What to Avoid

- ❌ **WiFi-only adapters** — Drain phone battery, unreliable
- ❌ **Very cheap clones (< ₹500)** — Often fail to connect or drop data
- ❌ **ELM327 with version < 1.5** — May not support all protocols

### OBD2 Port Location on Honda Elevate

The OBD2 diagnostic port on the 2025 Honda Elevate is located **under the dashboard on the driver's side**, near the steering column. Look for a **16-pin trapezoidal connector**. You may need to remove a small cover panel.

> 📖 **Honda Elevate owner?** See our detailed [Honda Elevate Guide](docs/HONDA_ELEVATE.md) for vehicle-specific setup tips, recommended PIDs, and India-specific advice.

---

## Software You Need to Install

### On Your Phone

| # | App | Price | Purpose | Link |
|---|-----|-------|---------|------|
| 1 | **Torque Pro** | ₹200 / $4.99 | OBD2 reader app (reads car data) | [Google Play Store](https://play.google.com/store/apps/details?id=org.prowl.torque&hl=en_US&gl=US) |
| 2 | **AA Torque** | Free | Android Auto plugin (displays data on car screen) | [GitHub Releases](https://github.com/agronick/aa-torque/releases) |
| 3 | **KingInstaller** | Free | Spoofs Play Store installer (required for Pixel) | [GitHub Releases](https://github.com/fcaronte/KingInstaller/releases) |

### On Your Computer (only if building from source)

| # | Software | Version | Purpose | Link |
|---|----------|---------|---------|------|
| 1 | **Android Studio** | Latest | IDE for Android development | [developer.android.com](https://developer.android.com/studio) |
| 2 | **Java JDK** | 19 (Zulu) | Build tool | `brew install --cask zulu@19` (macOS) |
| 3 | **Git** | Latest | Version control | Pre-installed on macOS |

---

# Installation Guide

> 📹 **Prefer video?** Watch the [video installation guide](https://www.youtube.com/watch?v=DDxPrPzxZ3k).

## Step 1: Buy and Pair Your OBD2 Adapter

1. Plug the OBD2 adapter into your car's diagnostic port
2. Turn the ignition to **ON** (engine can be off)
3. On your phone, go to **Settings → Bluetooth**
4. Find the adapter (usually named `OBDII` or `Vlink`) and pair with it
5. Default PIN is usually `1234` or `0000`
6. Don't open Torque Pro yet — just complete the Bluetooth pairing

## Step 2: Install Torque Pro

1. Open the **Google Play Store**
2. Search for **"Torque Pro"** by Ian Hawkins
3. Purchase and install it (~₹200 / $4.99)
4. Open Torque Pro
5. Go to **Settings → OBD2 Adapter Settings → Bluetooth**
6. Select your paired adapter
7. Start your car engine
8. Wait for the **green indicator** in Torque Pro (means connected to ECU)
9. **Keep Torque Pro running** — it must be active in the background for AA Torque to work

## Step 3: Download AA Torque

1. Open your phone's browser
2. Go to [github.com/agronick/aa-torque/releases](https://github.com/agronick/aa-torque/releases)
3. Download the latest **`aa-torque.apk`** file
4. Don't install it yet — we'll install via KingInstaller in the next step

## Step 4: Install KingInstaller (Required for Pixel Phones)

Google Pixel phones require an extra step to make Android Auto recognize third-party apps.

### Why KingInstaller?

Android Auto normally only shows apps installed from the Play Store. KingInstaller "spoofs" the installation source so your phone thinks AA Torque came from the Play Store.

1. Go to [github.com/fcaronte/KingInstaller/releases](https://github.com/fcaronte/KingInstaller/releases)
2. Download the latest **`KingInstaller.apk`**
3. Install it using your phone's normal package installer
4. Open KingInstaller
5. Grant it the requested permissions

### ⚠️ Pixel-Specific: Update Google Package Installer First

On recent Pixel phones (Android 14/15), KingInstaller may fail without updating the Google Package Installer first.

**How to fix:**

1. Download the latest Google Package Installer from [APKMirror](https://www.apkmirror.com/apk/google-inc/package-installer-com-google-android-packageinstaller/)
2. Alternatively, extract the APK from your phone using a file manager like [MiXplorer](https://mixplorer.com/)
3. Install the updated Package Installer
4. **Now** use KingInstaller to install AA Torque

> **Note from the KingInstaller README:**
> "For this phone you need to reinstall/update the current Google PackageInstaller, you can download the same version from ApkMirror or similar site, or you can get the apk directly from your phone using a file manager with that feature (I use MiXplorer) then install the apk as normal and now you can use KingInstaller to install the app and will work!"

### ⚠️ Android 14/15 Warning

Google frequently patches the loopholes that KingInstaller uses. If an update breaks the installation:

1. Check [GitHub Discussions](https://github.com/agronick/aa-torque/discussions) for the latest workaround
2. Try re-enabling Unknown Sources in Android Auto Developer Settings
3. If you have an AAWireless dongle, enabling developer mode in AAWireless can work around the issue

## Step 5: Install AA Torque via KingInstaller

1. Open **KingInstaller**
2. Tap **"Install from storage"**
3. Navigate to and select the **`aa-torque.apk`** you downloaded
4. Tap **Install**
5. When prompted, allow installation from this source
6. **How to verify it worked:** Open Android Auto on your phone — you should see "AA Torque" listed in the available apps. If not, re-run KingInstaller.

## Step 6: Enable Android Auto Developer Mode

1. **Disconnect** your phone from the car (unplug USB)
2. Open the **Android Auto** app on your phone
3. Scroll down and tap **"About Android Auto"** header **10 times** rapidly
4. You'll see a toast message: **"You are now a developer!"**
5. Tap the **3-dot menu** (top right) → **"Developer Settings"**
6. Scroll down and **check "Unknown Sources"**
7. Go back to the main Android Auto screen
8. You should now see **AA Torque** in the list of available apps

> **Picture guide:** [How to Enable Developer Mode on Android Auto](https://www.howtogeek.com/271132/how-to-enable-developer-settings-on-android-auto/)

## Step 7: Configure AA Torque

1. Open the **AA Torque Settings** app on your phone
2. **Grant ALL permissions** it requests — the app needs these to function:
   - **Location** — Required by Android Auto framework
   - **Phone** — Required for media session detection (album art)
   - **Notifications** — Required for media background feature
3. Select your preferred:
   - **Theme** (try "Electro Vehicle" or "Dark" for a clean look)
   - **Font** (try "7 Segment Display" for classic dashboard look)
   - **Background** (try "Black" or "Electro Vehicle")
4. Configure your dashboards:
   - Tap each dashboard to configure **3 gauges** and **4 text displays**
   - Select which PIDs (sensors) to display
   - Set min/max values and units

## Step 8: Use in Your Car

1. Connect your phone to the car via **USB cable**
2. Android Auto should launch automatically on the car screen
3. Tap the **dashboard/clock icon** in the bottom-right corner of Android Auto
4. Select **"AA Torque"**
5. Data should start appearing within **5-10 seconds**
6. **Swipe left/right** to switch between dashboards
7. **Swipe up/down** to toggle chart view
8. **D-pad center** (steering wheel button) also toggles chart view

5. **Force Update** (do this once): Tap the **menu (3 dots)** → **"Force Update"** — this downloads the APK from GitHub releases and spoofs the Play Store installer, making your phone think the app came from the official store. This only needs to be done once.

> **⚠️ Important:** Torque Pro must be running and connected to the OBD2 adapter for AA Torque to display data.

---

# Updating the App

When a new version is released, AA Torque checks automatically and shows a notification. To update:

### Automatic Update (Recommended)

1. Open **AA Torque Settings**
2. If a new version is available, a **"Download"** button will appear at the bottom
3. Tap **Download** — the APK downloads in the background
4. When prompted, tap **OK** to install
5. If the install fails, use KingInstaller to install the new APK manually

### Manual Update

1. Go to [github.com/agronick/aa-torque/releases](https://github.com/agronick/aa-torque/releases)
2. Download the latest `aa-torque.apk`
3. Open **KingInstaller** → Install from storage → select the new APK
4. The update preserves your settings and dashboards

### After a Phone Restart or OS Update

- You may need to **re-enable Unknown Sources** in Android Auto Developer Settings
- Open Torque Pro first to re-establish the OBD2 connection
- Then connect to the car

---

# Optimization Tips

For the best experience, especially in Indian summer heat:

### Disable Battery Optimization

Both Torque Pro and AA Torque need to run continuously. Your phone may kill them to save battery.

**For AA Torque:**
1. Go to **Settings → Apps → AA Torque → Battery**
2. Select **"Unrestricted"**

**For Torque Pro:**
1. Go to **Settings → Apps → Torque Pro → Battery**
2. Select **"Unrestricted"**

### Keep Your Phone Cool

- Use a **phone mount with ventilation** (not a closed holder)
- Avoid direct sunlight on the phone
- Turn on car AC before connecting
- Close other apps running in the background

### Use a Quality USB Cable

- Use a **data + charge cable** (not charge-only)
- Short cables (1m) are more reliable than long ones
- Replace frayed or damaged cables

---

# Troubleshooting

## "Unable to connect to Torque plugin service"

- **Torque Pro is not running** → Open Torque Pro, connect to OBD2 adapter, keep it open
- **OBD2 adapter not paired** → Re-pair via phone Bluetooth settings
- **OBD2 adapter not plugged in** → Check the diagnostic port under the steering wheel
- **Car ignition off** → Turn ignition to ON position (engine can be off for pairing)

## AA Torque doesn't appear in Android Auto

- **Developer Mode not enabled** → Tap "About Android Auto" 10 times
- **Unknown Sources not checked** → Developer Settings → check "Unknown Sources"
- **Not installed via KingInstaller** → Reinstall using KingInstaller
- **App disappeared after update** → Android updates can reset permissions — re-enable Unknown Sources (see [Step 6](#step-6-enable-android-auto-developer-mode))

## Pixel Phone: App won't install

1. Update Google Package Installer from APKMirror (see [Step 4](#step-4-install-kinginstaller-required-for-pixel-phones))
2. Install via KingInstaller (not directly)
3. If still failing, try the **"Force Update"** option in AA Torque Settings
4. Check [GitHub Discussions](https://github.com/agronick/aa-torque/discussions) for latest Pixel-specific workarounds

## No data showing on car display

1. Check Torque Pro shows data on your phone (open Torque, look at gauges)
2. Make sure OBD2 adapter is connected (green indicator in Torque)
3. Restart both Torque Pro and AA Torque
4. Check if your car's ECU supports standard OBD2 PIDs

## Frequent disconnections

- Use a **high-quality USB cable** (data + charge, not charge-only)
- Keep your phone **cool** (Indian heat can cause throttling)
- Disable battery optimization for AA Torque (see [Optimization Tips](#optimization-tips))
- Close other apps consuming CPU

## Gauge shows "Connecting to Torque..."

- Torque Pro is not connected to the OBD2 adapter yet
- Wait for the green indicator in Torque Pro
- Make sure the car engine is running

## App crashes or freezes

1. Open **AA Torque Settings** → Menu → **Copy logs to clipboard**
2. Paste the logs into a [GitHub Issue](https://github.com/agronick/aa-torque/issues) or [Discussion](https://github.com/agronick/aa-torque/discussions)
3. Try clearing the app data: **Settings → Apps → AA Torque → Storage → Clear Data**
4. Reconfigure your dashboards

---

# Privacy & Data Safety

AA Torque is privacy-friendly:

- **No analytics or tracking** — The app does not collect or send any usage data
- **No internet required** — Works fully offline after installation
- **Crash reports** — If the app crashes, ACRA offers to email a crash report to the developer. This is opt-in and only happens when you tap "Send"
- **Update checks** — The app checks GitHub releases for new versions (once per app launch). No personal data is sent
- **No account required** — No sign-up, no login, no cloud sync

---

# Custom Expressions

AA Torque supports [EvalEx](https://ezylang.github.io/EvalEx) expressions for unit conversion. Use `a` as the variable for the current value.

### Common Expressions

| Conversion | Expression |
|-----------|------------|
| °C to °F | `a * 1.8 + 32` |
| °F to °C | `(a - 32) / 1.8` |
| BAR to PSI | `a * 14.5038` |
| PSI to BAR | `a * 0.0689476` |
| kPa to PSI | `a * 0.145038` |
| km/h to MPH | `a * 0.621371` |
| MPH to km/h | `a * 1.60934` |
| L/100km to MPG | `235.215 / a` |
| Nm to ft-lb | `a * 0.737562` |
| CC/min to gal/hr | `a * 0.0158503` |
| Round to integer | `round(a)` |

### Using Custom Expressions

1. Open **AA Torque Settings**
2. Tap a dashboard → tap a gauge or display
3. Enable **"Run unit conversion"**
4. Enter your expression in the **"Unit conversion"** field
5. Use `a` to represent the current value

**Full documentation:** [EvalEx Functions Reference](https://ezylang.github.io/EvalEx/references/functions.html)

---

# Building from Source

See the detailed **[Build Guide](docs/BUILD_GUIDE.md)** for complete macOS/Windows/Linux instructions.

**Quick start (macOS):**

> Windows/Linux users: See the [full Build Guide](docs/BUILD_GUIDE.md) for platform-specific instructions.

```bash
# Install Java 19
brew install --cask zulu@19
export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-19.jdk/Contents/Home

# Clone with submodules
git clone --recurse-submodules https://github.com/agronick/aa-torque.git
cd aa-torque

# Set up SDK path
echo "sdk.dir=$HOME/Library/Android/sdk" > local.properties

# Build debug APK
./gradlew assembleDebug

# Install on connected phone
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

---

# Documentation

Detailed documentation is available in the [`docs/`](docs/) folder:

| Document | Description |
|----------|-------------|
| [Build Guide](docs/BUILD_GUIDE.md) | Complete build instructions for macOS, Windows, Linux |
| [Architecture](docs/ARCHITECTURE.md) | How the app works internally (data flow, components) |
| [Codebase Analysis](docs/CODEBASE_ANALYSIS.md) | File-by-file analysis of every source file |
| [Modding Guide](docs/MODDING_GUIDE.md) | How to add themes, fonts, PIDs, and release your own build |
| [Honda Elevate Guide](docs/HONDA_ELEVATE.md) | Honda Elevate-specific setup and PID recommendations |

---

# Screenshots

<img src="https://github.com/agronick/aa-torque/assets/2042303/e31b0598-25ec-4003-bcee-11e5e108d3a4" width="100%" alt="Dashboard with 3 gauges and text displays" />

<img src="https://github.com/agronick/aa-torque/assets/2042303/bf40ed97-adf3-4923-bbc5-63a618899173" width="100%" alt="Album art background with ray instead of needle" />

*Album art background with high-visibility ray instead of needle*

<img src="https://github.com/agronick/aa-torque/assets/2042303/a99571f9-0bb3-43ec-89ce-938e41506e97" width="100%" alt="Chart view showing real-time data" />

*Real-time line chart with color-coded traces*

<img src="https://github.com/agronick/aa-torque/assets/2042303/229315c8-ad3b-42e6-86e6-e7fe7abb16a8" width="100%" alt="Dashboard with different theme" />

<img src="https://github.com/agronick/aa-torque/assets/2042303/698c666b-5e3c-4611-80a5-8e767b04186a" width="100%" alt="Dashboard with multiple displays" />

---

# Help & Community

- **Questions & Support:** [GitHub Discussions](https://github.com/agronick/aa-torque/discussions)
- **Bug Reports:** [GitHub Issues](https://github.com/agronick/aa-torque/issues)
- **Translations:** [Help translate the app on POEditor](https://poeditor.com/join/project/yttme0y1VZ)
- **Show off your setup:** Post on forums and social media!

# Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to contribute themes, code, or translations.

# Donate

This app takes hundreds of hours of work. If you enjoy it:

- [PayPal](https://www.paypal.me/kagronick)
- [GitHub Sponsors](https://github.com/agronick)

# Credits

Based on [Chillout's Performance Monitor](https://github.com/jilleb/mqb-pm), which was based on Martoreto's aa-stats.

Icon credits:
- Horse by Peleg Red — [Noun Project](https://thenounproject.com/browse/icons/term/horse/) (CC BY 3.0)
- Engine Cylinder by Thuy Nguyen — [Noun Project](https://thenounproject.com/browse/icons/term/engine-cylinder/) (CC BY 3.0)
- Transmission by Gustyne Pissesa Ardhaneswari — [Noun Project](https://thenounproject.com/browse/icons/term/transmission/) (CC BY 3.0)
