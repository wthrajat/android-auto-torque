# AA Torque - Modding & Customization Guide

This guide covers everything you need to know to modify the app, add features, change themes, and release your own build.

---

## Table of Contents

1. [What You Can Customize](#what-you-can-customize)
2. [Making Your First Change](#making-your-first-change)
3. [Adding a New Theme](#adding-a-new-theme)
4. [Adding a New Font](#adding-a-new-font)
5. [Modifying the Dashboard Layout](#modifying-the-dashboard-layout)
6. [Adding New PIDs or Features](#adding-new-pids-or-features)
7. [Changing the App Name/Icon](#changing-the-app-nameicon)
8. [Modifying Default Settings](#modifying-default-settings)
9. [Adding New Strings (Localization)](#adding-new-strings-localization)
10. [Compiling & Recompiling](#compiling--recompiling)
11. [Releasing Your Build](#releasing-your-build)
12. [Common Modifications](#common-modifications)

---

## What You Can Customize

| Area | Difficulty | Files Involved |
|------|-----------|----------------|
| Theme (colors, needle, dials) | Easy | `styles.xml`, `drawable/`, `arrays.xml` |
| Font | Easy | `res/font/`, `DashboardFragment.kt` |
| Background images | Easy | `res/drawable/`, `arrays.xml` |
| App name/icon | Easy | `strings.xml`, `mipmap/` |
| Default PID layout | Easy | `PrefStore.kt` (DEFAULT_SETTINGS) |
| Gauge layout | Medium | `fragment_dashboard.xml`, `ViewAdapter.kt` |
| Add new features | Medium-Hard | Various Kotlin/Java files |
| Add new UI components | Hard | New fragments, layouts, bindings |
| Modify data flow | Hard | `TorqueRefresher.kt`, `TorqueData.kt` |

---

## Making Your First Change

### Step 1: Fork and Clone

```bash
# Fork on GitHub, then clone
git clone --recurse-submodules https://github.com/YOUR_USERNAME/aa-torque.git
cd aa-torque
```

### Step 2: Change the App Name

Edit `app/src/main/res/values/strings.xml`:

```xml
<!-- Change from -->
<string name="app_name">AA Torque</string>

<!-- To -->
<string name="app_name">My Custom Torque</string>
```

### Step 3: Build and Test

```bash
./gradlew assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

---

## Adding a New Theme

### Step 1: Create Drawable Assets

You need these PNG images (800×400px recommended):

```
res/drawable/
├── needle_mytheme.png          # Needle at 12 o'clock position
├── dial_background_mytheme.png # Dial with red warning zone
├── dial_background_empty_mytheme.png  # Dial without warning zone
├── dial_background_blank_mytheme.png  # Plain dial
├── sw_background_mytheme.png   # Stopwatch background
└── background_incar_mytheme.png # App wallpaper (800×400px)
```

### Step 2: Add Theme Style

In `app/src/main/res/values/styles.xml`, add:

```xml
<style name="AppTheme.MyTheme" parent="AppTheme">
    <item name="themedDialBackground">@drawable/dial_background_mytheme</item>
    <item name="themedEmptyDialBackground">@drawable/dial_background_empty_mytheme</item>
    <item name="themedBlankDialBackground">@drawable/dial_background_blank_mytheme</item>
    <item name="themedCarBackground">@drawable/background_incar_mytheme</item>
    <item name="themedNeedle">@drawable/needle_mytheme</item>
    <item name="themedStopWatchBackground">@drawable/sw_background_mytheme</item>
    <item name="themedNeedleColor">@color/mytheme_needle_color</item>
    <item name="sv_withIndicatorLight">true</item>
    <item name="sv_indicatorLightColor">#44FFFFFF</item>
    <item name="sv_indicator">HalfLineIndicator</item>
    <item name="sv_indicatorColor">#FFFFFFFF</item>
    <item name="sv_indicatorWidth">4dp</item>
    <item name="sv_tickPadding">20dp</item>
    <item name="sv_tickRotation">false</item>
</style>
```

### Step 3: Register the Theme

In `app/src/main/res/values/arrays.xml`, add entries to three arrays:

```xml
<!-- Add to Themes array -->
<string-array name="Themes">
    <!-- existing themes... -->
    <item>MyTheme</item>
</string-array>

<!-- Add thumbnail -->
<array name="ThemesThumbs">
    <!-- existing thumbs... -->
    <item>@drawable/ic_theme_mytheme</item>
</array>

<!-- Add style reference -->
<array name="ThemeList">
    <!-- existing styles... -->
    <item>@style/AppTheme.MyTheme</item>
</array>
```

**Important:** All three arrays must have entries in the **same order** and the **same count**.

### Step 4: Add Thumbnail

Create a 100×100px thumbnail image:
```
res/drawable/ic_theme_mytheme.png
```

---

## Adding a New Font

### Step 1: Add Font File

Place your `.ttf` or `.otf` font in:
```
app/src/main/res/font/myfont.ttf
```

### Step 2: Register in Arrays

In `arrays.xml`:

```xml
<string-array name="fontEntries">
    <!-- existing fonts... -->
    <item>My Custom Font</item>
</string-array>

<string-array name="fontValues">
    <!-- existing values... -->
    <item>myfont</item>
</string-array>

<array name="FontThumbs">
    <!-- existing thumbs... -->
    <item>@drawable/ic_font_myfont</item>
</array>
```

### Step 3: Add Font Handler

In `DashboardFragment.kt`, add to `setupTypeface()`:

```kotlin
fun setupTypeface(selectedFont: String) {
    val font = when (selectedFont) {
        "segments" -> R.font.digital
        // ... existing fonts ...
        "myfont" -> R.font.myfont  // ADD THIS
        else -> R.font.digital
    }
    settingsViewModel.setFont(font)
}
```

---

## Modifying the Dashboard Layout

The dashboard layout is in `app/src/main/res/layout/fragment_dashboard.xml`.

### Key Layout Structure

```
ConstraintLayout (root)
├── ImageView (background)
├── ConstraintLayout (headers)
│   ├── TextView (status)
│   ├── TextView (title)
│   ├── ImageButton (chart toggle)
│   ├── ImageButton (prev)
│   └── ImageButton (next)
└── RelativeLayout (content wrapper)
    └── ConstraintLayout (dashContent)
        ├── TorqueChart (chart, hidden when gauges shown)
        ├── TorqueGauge × 3 (left, center, right)
        ├── TorqueDisplay × 4 (text displays)
        └── Guidelines (positioning)
```

### Adjusting Gauge Sizes

The center gauge can be larger via the `largeCenter` boolean:

```xml
app:scaleX="@{largeCenter ? 1.1f : 1f}"
app:scaleY="@{largeCenter ? 1.1f : 1f}"
```

Guidelines control the 1/3 splits:
```xml
<Guideline
    app:guidelinePercent="@{1f/3f}" />  <!-- Left/Center split -->
<Guideline
    app:guidelinePercent="@{(2f/3f)}" /> <!-- Center/Right split -->
```

### Adding More Gauges

To add a 4th gauge, you would need to:

1. Add a new `FragmentContainerView` in `fragment_dashboard.xml`
2. Update `DashboardFragment.kt` to manage the new gauge
3. Update `PrefStore.kt` default settings
4. Update the `Screen` protobuf message (if needed)

---

## Adding New PIDs or Features

### Understanding PIDs

PIDs (Parameter IDs) are identifiers for vehicle sensors. They come from Torque Pro. AA Torque doesn't read them directly: it asks Torque Pro for the values.

### Common PIDs

| PID | Description |
|-----|-------------|
| `torque_0c,0` | Engine RPM |
| `torque_0d,0` | Vehicle Speed |
| `torque_05,0` | Coolant Temperature |
| `torque_0f,0` | Intake Air Temperature |
| `torque_11,0` | Throttle Position |
| `torque_04,0` | Engine Load |
| `torque_42,0` | Control Module Voltage |
| `torque_2f,0` | Fuel Tank Level |

### Adding a New Display Element

In `PrefStore.kt`, modify `DEFAULT_SETTINGS`:

```kotlin
const val DEFAULT_SETTINGS = """
screens {
  gauges {
    pid: "torque_0c,0"
    showLabel: true
    label: "RPM"
    icon: "ic_cylinder"
    maxValue: 10000
    unit: "rpm"
    wholeNumbers: true
    ticksActive: true
    chartColor: -12734743
    disabled: false
  }
  gauges {
    pid: "torque_0d,0"
    showLabel: true
    label: "Speed"
    icon: "ic_barometer"
    maxValue: 160
    unit: "km/h"
    highVisActive: true
    ticksActive: true
    chartColor: -5314243
    disabled: false
  }
  gauges {
    pid: "torque_11,0"
    showLabel: true
    label: "Throttle"
    icon: "ic_throttle"
    maxValue: 100
    unit: "%"
    ticksActive: true
    chartColor: -1476547
    disabled: false
  }
  displays {}
  displays {}
  displays {}
  displays {}
}
selectedTheme: "Electro Vehicle"
selectedFont: "ev"
selectedBackground: "background_incar_ev"
centerGaugeLarge: true
"""
```

### Adding a Custom Expression

In the settings, users can add EvalEx expressions. The variable `a` represents the current value.

Examples:
- `a * 1.8 + 32`: Convert Celsius to Fahrenheit
- `a * 14.5038`: Convert BAR to PSI
- `a > 100 ? 100 : a`: Clamp to 0-100

---

## Changing the App Name/Icon

### App Name

Edit `app/src/main/res/values/strings.xml`:
```xml
<string name="app_name">Your App Name</string>
```

### App Icon

Replace the launcher icons in:
```
app/src/main/res/mipmap-hdpi/     → 72×72px
app/src/main/res/mipmap-mdpi/     → 48×48px
app/src/main/res/mipmap-xhdpi/    → 96×96px
app/src/main/res/mipmap-xxhdpi/   → 144×144px
app/src/main/res/mipmap-xxxhdpi/  → 192×192px
```

Or use Android Studio: **Right-click res → New → Image Asset**

### Package Name

To change the package name (application ID):

1. Edit `app/build.gradle`:
```groovy
applicationId "com.yourname.yourapp"
```

2. Update `AndroidManifest.xml` queries if needed
3. Update any hardcoded package references

**Warning:** Changing the package name breaks compatibility with existing data.

---

## Modifying Default Settings

The default dashboard configuration is in `PrefStore.kt` as `DEFAULT_SETTINGS`.

### Adding More Dashboards

```kotlin
const val DEFAULT_SETTINGS = """
screens {
  // Dashboard 1
  gauges { ... }
  gauges { ... }
  gauges { ... }
  displays {}
  displays {}
  displays {}
  displays {}
}
screens {
  // Dashboard 2
  gauges { pid: "torque_05,0" label: "Coolant" ... }
  gauges { pid: "torque_0f,0" label: "Intake Temp" ... }
  gauges { pid: "torque_04,0" label: "Engine Load" ... }
  displays {}
  displays {}
  displays {}
  displays {}
}
"""
```

### Changing Default Theme

```kotlin
selectedTheme: "VW"
selectedFont: "vw"
selectedBackground: "background_incar_vw3"
```

---

## Adding New Strings (Localization)

### Adding a New String

1. Add to `app/src/main/res/values/strings.xml`:
```xml
<string name="my_new_string">Hello World</string>
```

2. Add translations to other locale folders:
```xml
<!-- values-de/strings.xml -->
<string name="my_new_string">Hallo Welt</string>

<!-- values-fr/strings.xml -->
<string name="my_new_string">Bonjour le monde</string>
```

### Available Locales

The app currently supports:
- English (default)
- Czech (`values-cs`)
- German (`values-de`)
- Greek (`values-el`)
- Spanish (`values-es`)
- French (`values-fr`)
- Canadian French (`values-fr-rCA`)
- Italian (`values-it`)
- Dutch (`values-nl`)
- Polish (`values-pl`)
- Portuguese (`values-pt`)
- Brazilian Portuguese (`values-pt-rBR`)
- Russian (`values-ru`)
- Slovenian (`values-sl`)
- Ukrainian (`values-uk`)
- Hong Kong Chinese (`values-zh-rHK`)

---

## Compiling & Recompiling

### First-Time Setup

```bash
# Ensure Java 19 is installed
brew install --cask zulu@19
export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-19.jdk/Contents/Home

# Clone with submodules
git clone --recurse-submodules https://github.com/agronick/aa-torque.git
cd aa-torque

# Create local.properties
echo "sdk.dir=$HOME/Library/Android/sdk" > local.properties
```

### Build Commands

```bash
# Debug build (fast, for testing)
./gradlew assembleDebug

# Release build (minified, for distribution)
./gradlew assembleRelease

# Clean and rebuild
./gradlew clean assembleDebug

# Build with lint check
./gradlew lintRelease
```

### Build Output Locations

```
app/build/outputs/apk/debug/app-debug.apk          → Debug APK
app/build/outputs/apk/release/app-release.apk       → Release APK
app/build/outputs/apk/release_debug/                → Minified debug
```

### Viewing Build Logs

```bash
# Verbose output
./gradlew assembleDebug --info

# Very verbose
./gradlew assembleDebug --debug
```

---

## Releasing Your Build

### Version Numbering

Edit `version.properties`:
```properties
majorVersion=2
minorVersion=0
patchVersion=31
```

Version code = `major * 1000 + minor * 100 + patch * 10`

### Signing for Release

```bash
# Create a keystore
keytool -genkey -v \
  -keystore release-key.jks \
  -keyalg RSA -keysize 2048 \
  -validity 10000 \
  -alias my-key

# Build signed release
./gradlew assembleRelease \
  -Pandroid.injected.signing.store.file=/path/to/release-key.jks \
  -Pandroid.injected.signing.key.alias=my-key \
  -Pandroid.injected.signing.store.password=YOUR_PASSWORD \
  -Pandroid.injected.signing.key.password=YOUR_PASSWORD
```

### Distributing

1. Rename the APK: `mv app/build/outputs/apk/release/app-release.apk aa-torque-v2.0.31.apk`
2. Share via GitHub Releases, direct download, or file sharing
3. Users install via KingInstaller or Force Update method

---

## Common Modifications

### Change Update Interval

In `TorqueRefresher.kt`:
```kotlin
const val REFRESH_INTERVAL = 300L  // milliseconds
```

Faster = more responsive but higher CPU/battery usage.

### Change Max Screens

In `SettingsFragment.kt`, the validation is:
```kotlin
if (intVal in 1..10) {  // Change 10 to your max
```

### Change Default PID Labels

In `PrefStore.kt`, modify the `DEFAULT_SETTINGS` string.

### Add a New Custom Attribute

1. Define in `res/values/attrs.xml`:
```xml
<attr name="myCustomAttr" format="reference" />
```

2. Use in theme style:
```xml
<item name="myCustomAttr">@drawable/my_drawable</item>
```

3. Access in code:
```kotlin
val typedArray = context.theme.obtainStyledAttributes(intArrayOf(R.attr.myCustomAttr))
val drawable = typedArray.getDrawable(0)
typedArray.recycle()
```

### Enable Test Mode (Simulated Data)

The debug build has `SIMULATE_METRICS = true`, which makes Torque return simulated data when not connected to an ECU. This is useful for development.

---

## Tips for Non-Android Developers

1. **XML Layouts** are like HTML: they define the visual structure
2. **Data Binding** lets you bind Kotlin/Java variables directly to XML elements
3. **Fragments** are like sub-pages within an Activity
4. **Protobuf** is a compact binary format for storing structured data
5. **DataStore** is like SharedPreferences but modern and async
6. **AIDL** enables communication between different Android apps
7. **Gradle** is the build system (like npm for JavaScript)
8. **R8/ProGuard** shrinks and obfuscates code for release builds
