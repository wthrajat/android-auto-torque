# AA Torque - Complete Codebase Analysis

A file-by-file analysis of every source file in the project, explaining what it does and how it connects to other components.

---

## Table of Contents

1. [Source Files Map](#source-files-map)
2. [Java Source Files](#java-source-files)
3. [Kotlin Source Files](#kotlin-source-files)
4. [Resource Files](#resource-files)
5. [Configuration Files](#configuration-files)
6. [External Dependencies](#external-dependencies)

---

## Source Files Map

```
com.aatorque.stats          → Dashboard & car display components
com.aatorque.prefs          → Settings & preferences system
com.aatorque.utils          → Utility/helper classes
com.aatorque.ui.theme       → Compose theme (alarm editor)
org.prowl.torque.remote     → Torque Pro IPC interface
```

---

## Java Source Files

### `CarService.java` (7 lines)
**Purpose:** Android Auto entry point service.

Extends `CarActivityService`. When Android Auto launches the app, it calls this service which returns `MainCarActivity`. This is the bridge between Android Auto's framework and the app's UI.

**Registered in:** `AndroidManifest.xml` with intent filters for `CATEGORY_PROJECTION` and `CATEGORY_PROJECTION_OEM`.

### `ITorqueService.java` (~4000+ lines, auto-generated)
**Purpose:** AIDL-generated IPC interface for communicating with Torque Pro.

This file is **auto-generated** from `ITorqueService.aidl` and should never be manually edited. It contains:
- `Stub` class — Server-side implementation base
- `Proxy` class — Client-side proxy for IPC calls
- `Default` class — Default no-op implementation

**Key methods used by AA Torque:**
- `listAllPIDs()` → Gets all available sensor IDs
- `getPIDInformation(String[])` → Gets sensor metadata (name, unit, min, max)
- `getPIDValuesAsDouble(String[])` → Gets current sensor values
- `isConnectedToECU()` → Checks if car is connected
- `setDebugTestMode(boolean)` → Enables simulated data for development

### `ListMenuAdapter.java` (96 lines)
**Purpose:** Android Auto menu adapter for the car display's menu system.

Implements `MenuAdapter` from the Android Auto SDK. Manages menu items and handles click callbacks via the `MenuCallbacks` interface. Used by `MainCarActivity` for the menu with Dashboard, Credits, and Stopwatch options.

---

## Kotlin Source Files

### Stats Package (`com.aatorque.stats`)

#### `App.kt` (47 lines)
**Purpose:** Application class, initialized when the app starts.

- Plants a `CacheLogTree` for Timber logging
- Fixes Android 14 permission issues with car SDK files
- Initializes ACRA crash reporting (email reports)

#### `MainCarActivity.kt` (168 lines)
**Purpose:** Main activity shown on the car's Android Auto display.

- Extends `CarActivity` from the Android Auto SDK
- Manages fragment switching (Dashboard, Credits, Stopwatch)
- Handles rotary dial input (scroll wheel)
- Handles keyboard events (DPAD_CENTER)
- Observes theme changes and recreates activity when theme changes
- Uses `LocalBroadcastManager` to forward key events to fragments

**Key constants:**
- `MENU_DASHBOARD`, `MENU_CREDITS`, `MENU_STOPWATCH` — Menu item names
- `FRAGMENT_CAR`, `FRAGMENT_CREDITS`, `FRAGMENT_STOPWATCH` — Fragment tags

#### `CarFragment.kt` (15 lines)
**Purpose:** Abstract base class for all fragments shown in the car display.

Defines `title` property and `setupStatusBar()` abstract method. All car-facing fragments extend this.

#### `AlbumArt.kt` (103 lines)
**Purpose:** Abstract class that provides album art detection from media sessions.

- Listens to `MediaSessionManager` for active media sessions
- Extracts album art bitmaps from `MediaMetadata`
- Uses `MutableSharedFlow` to emit metadata changes
- Subclasses implement `onMediaChanged()` to handle artwork

**Extended by:** `DashboardFragment`

#### `DashboardFragment.kt` (340 lines)
**Purpose:** The main dashboard fragment — the most important UI component.

**Responsibilities:**
- Hosts 3 gauge fragments (`TorqueGauge` × 3)
- Hosts 4 text display fragments (`TorqueDisplay` × 4)
- Hosts 1 chart fragment (`TorqueChart`)
- Manages screen switching (up to 10 dashboards)
- Observes all user preferences via DataStore
- Manages album art background with blur/darken effects
- Handles swipe gestures and rotary input for screen navigation
- Manages gauge opacity and center gauge scaling

**Data flow:**
```
DataStore.data → collect → update gauges/displays/chart
                         → update background
                         → update font
                         → update opacity
                         → update connection status
```

#### `TorqueGauge.kt` (252 lines)
**Purpose:** Individual gauge fragment displaying a speedometer-style dial.

**Components:**
- `TorqueSpeedometer` — Main gauge with needle
- `RaySpeedometer` — High-visibility ray mode
- `SpeedView` — Min/max indicator overlay

**Features:**
- Custom needle/indicator via theme attributes
- Tick marks with labels
- Min/max value tracking and display
- Alarm color overlays
- Icon display

#### `TorqueSpeedometer.kt` (104 lines)
**Purpose:** Custom speedometer widget extending `ImageSpeedometer`.

- Draws custom icons in the center
- Renders alarm arc overlays with animated transitions
- Handles icon positioning relative to speed text

#### `TorqueDisplay.kt` (73 lines)
**Purpose:** Text-based display fragment for showing a single PID value.

- Shows value, unit, icon, and label
- Supports alarm color changes
- Used for secondary data points (4 per dashboard)

#### `TorqueData.kt` (124 lines)
**Purpose:** Data model for a single PID/displayable item.

**Key properties:**
- `pid` — The PID identifier (e.g., "0c,0")
- `lastData` — Current value (triggers formatting on set)
- `lastDataStr` — Formatted string for display
- `minValue`, `maxValue` — Historical min/max
- `expression` — EvalEx expression for custom formulas
- `currentAlarm` — LiveData for alarm state

**Key features:**
- Applies custom EvalEx expressions for unit conversion
- Formats numbers with appropriate precision
- Tracks min/max history
- Evaluates alarm conditions (GT, LT, EQ, etc.)

#### `TorqueRefresher.kt` (120 lines)
**Purpose:** Manages periodic data polling from Torque Pro.

**Key components:**
- `ScheduledThreadPoolExecutor` with 7 threads
- `REFRESH_INTERVAL = 300ms` — Polling rate
- `ConnectStatus` enum — Connection state machine
- `cache` — Per-screen TorqueData cache

**Flow:**
1. `makeExecutors()` — Schedules refresh tasks for all active PIDs
2. `doRefresh()` — Calls `ITorqueService.getPIDValuesAsDouble()` and updates `TorqueData`
3. `stopExecutors()` — Cancels all scheduled tasks

#### `TorqueChart.kt` (147 lines)
**Purpose:** Real-time line chart fragment using GraphView.

- Plots up to 3 PID values over time
- 22-second time window
- Color-coded lines with legend
- Scaled values (0-100%) for comparison
- X-axis shows MM:SS timestamps

#### `TorqueService.kt` (88 lines)
**Purpose:** IPC bridge to Torque Pro for the car display.

- Binds to `org.prowl.torque.remote.TorqueService`
- Manages connection lifecycle
- `runIfConnected()` — Safely execute Torque API calls
- `addConnectCallback()` — Register connection listeners

#### `TorqueServiceWrapper.kt` (117 lines)
**Purpose:** IPC bridge to Torque Pro for the settings app.

- Similar to `TorqueService.kt` but used in settings
- Loads PID information (names, units, ranges)
- Used by `SettingsDashboard` and `SettingsPIDFragment`

#### `ViewAdapter.kt` (126 lines)
**Purpose:** Data Binding adapters for custom view attributes.

Custom `@BindingAdapter` functions:
- `setConstraintTopToBottomOf()` — Dynamic constraint changes
- `setBackground()` — Sets dial background from theme
- `wholeNumbers()` — Toggles integer/decimal display
- `setMinMax()` — Sets gauge min/max speed
- `bitmapOrResource()` — Sets image from bitmap or resource
- `reversed()` — Reverses child order in LinearLayout

#### `CreditsFragment.kt` (32 lines)
**Purpose:** Credits/about screen with links to GitHub, PayPal, and translation.

#### `NotiService.kt` (12 lines)
**Purpose:** Notification listener service for detecting media sessions.

Required for album art detection. Checks if notification access is enabled.

#### `CacheLogTree.kt` (44 lines)
**Purpose:** Custom Timber tree that caches logs for clipboard export.

Keeps last 1,000 log entries. Used by "Copy logs to clipboard" feature.

#### `SizedImageIndicator.kt` (27 lines)
**Purpose:** Custom speedometer indicator using a Drawable.

Draws the themed needle image at the correct size.

#### `NoninteractiveGraphView.kt` (13 lines)
**Purpose:** GraphView subclass that ignores touch events.

Prevents accidental zooming/panning on the car display.

#### `LegendAdatper.kt` (50 lines)
**Purpose:** RecyclerView adapter for chart legend items.

Shows color, label, and current value for each charted PID.

### Prefs Package (`com.aatorque.prefs`)

#### `SettingsActivity.kt` (259 lines)
**Purpose:** Main settings activity shown on the phone.

**Features:**
- Hosts `SettingsFragment` and navigation to sub-screens
- Menu with Preview, Export/Import, Copy Logs, Credits, Force Update
- Auto-update checker (GitHub releases API)
- APK download and install via DownloadManager
- Fragment lifecycle callbacks for action bar updates
- Permission handling for `CAR_VENDOR_EXTENSION`

#### `SettingsFragment.kt` (186 lines)
**Purpose:** Main settings screen with appearance options.

**Preferences:**
- Theme selector (ImageListPreference)
- Font selector (ImageListPreference)
- Background selector (ImageListPreference)
- Center gauge toggle
- Min/Max below gauge toggle
- Number of dashboards
- Media background options (blur, darken, opacity)
- Dynamic dashboard list

#### `SettingsViewModel.kt` (28 lines)
**Purpose:** ViewModel for sharing state between settings fragments.

- `selectedFont` → `typefaceLiveData` (FontRes → Typeface)
- `chartVisible` — Whether chart mode is active
- `minMaxBelow` — Whether min/max is below gauge

#### `SettingsDashboard.kt` (120 lines)
**Purpose:** Per-dashboard settings screen.

- Binds to Torque to load PID list
- Shows 3 gauge + 4 display configuration entries
- Allows setting dashboard title

#### `SettingsPIDFragment.kt` (292 lines)
**Purpose:** Per-PID configuration screen.

**Configurable options:**
- Enable/disable
- PID selection (from Torque's PID list)
- Show label vs icon
- Label text
- Icon selection
- Min/max values
- Unit text
- Custom expression (EvalEx)
- Whole numbers toggle
- Tick marks toggle
- Min/max display mode
- High visibility mode
- Chart color

#### `PrefStore.kt` (65 lines)
**Purpose:** DataStore setup and default values.

- Defines `DEFAULT_SETTINGS` as Protobuf text format
- Creates `UserPreferenceSerializer` for DataStore
- Default dashboard: RPM, Speed, Throttle with EV theme

#### `ColorPreference.kt` (40 lines)
**Purpose:** Color picker preference for chart colors.

Uses `ColorPickerView` in an AlertDialog.

#### `ImageListPreference.kt` (99 lines)
**Purpose:** Custom list preference with icon previews.

Shows images next to each option in the selection dialog.

#### `FormulaPreference.kt` (95 lines)
**Purpose:** Custom expression editor with preset formulas.

Provides a spinner with preset formulas (F→C, PSI→BAR, etc.) and a text field for custom expressions. Links to EvalEx documentation.

#### `AlarmFragment.kt` (263 lines)
**Purpose:** Jetpack Compose-based alarm editor.

Allows adding color-coded alarm conditions (e.g., "if RPM > 6000, turn red"). Uses `mutableStateListOf` for reorderable alarm list.

#### `DashboardPreviewFragment.kt` (57 lines)
**Purpose:** Full-screen preview of the dashboard in portrait mode.

Hides system bars and forces portrait orientation.

### Utils Package (`com.aatorque.utils`)

#### `AnimatedLine.kt` (57 lines)
**Purpose:** Custom view that draws an animated rectangle.

Used for visual effects (alarm indicators). Animates width from 0 to full.

#### `CountdownLatch.kt` (38 lines)
**Purpose:** Coroutine-based countdown latch.

Similar to Java's `CountDownLatch` but suspend-function based. Used to wait for view initialization before data binding.

#### `AwareObserver.kt` (17 lines)
**Purpose:** Lifecycle-aware LiveData observer base class.

Handles observer binding/unbinding automatically.

#### `OpenCloseAnimator.kt` (30 lines)
**Purpose:** ValueAnimator that supports open/close transitions.

Animates between 0 and 1, supporting reverse animation for smooth transitions.

### UI Theme Package (`com.aatorque.ui.theme`)

#### `Color.kt` (57 lines)
**Purpose:** Material Design 3 color definitions for light and dark themes.

Used by the alarm editor's Compose UI.

#### `Theme.kt` (66 lines)
**Purpose:** Material Design 3 theme composable.

Defines `AppTheme` composable with light and dark color schemes.

---

## Resource Files

### Layouts (`res/layout/`)

| File | Purpose |
|------|---------|
| `activity_car_main.xml` | Single FrameLayout container for car fragments |
| `activity_settings.xml` | Settings activity with toolbar and fragment container |
| `fragment_dashboard.xml` | Main dashboard with gauges, displays, chart, background |
| `fragment_gauge.xml` | Individual gauge with speedometer, ray, min/max |
| `fragment_display.xml` | Text display with icon, label, and value |
| `fragment_chart.xml` | Line chart with legend RecyclerView |
| `fragment_credits.xml` | Credits screen with links |
| `fragment_stopwatch.xml` | Stopwatch (not fully implemented) |
| `fragment_dashboard_preview.xml` | Full-screen dashboard preview |
| `chart_legend.xml` | Legend item for chart |
| `icon_list_row.xml` | Row for icon selection dialogs |
| `script_preference.xml` | Custom expression editor dialog |

### Values (`res/values/`)

| File | Purpose |
|------|---------|
| `strings.xml` | All user-facing strings (English) |
| `colors.xml` | Color definitions (needle colors, graph colors, UI colors) |
| `styles.xml` | All theme styles (25+ themes) |
| `arrays.xml` | Theme names, font names, background names, icons |
| `attrs.xml` | Custom theme attributes (themedNeedle, themedDialBackground, etc.) |
| `dimens.xml` | Dimension values |
| `ids.xml` | View IDs |
| `bools.xml` | Boolean resources |
| `user_scripts.xml` | Preset EvalEx formulas |

### XML (`res/xml/`)

| File | Purpose |
|------|---------|
| `settings.xml` | Main settings preference screen |
| `display_setting.xml` | Per-dashboard settings |
| `pid_setting.xml` | Per-PID settings |
| `backup_rules.xml` | Backup rules |
| `automotive_app_desc.xml` | Android Auto app descriptor |

### Fonts (`res/font/`)

11 custom fonts matching car brand aesthetics:
- `digital.ttf` — 7-segment display
- `vwtextcarui_regular.ttf` — Volkswagen CarUI
- `vwthesis_mib_regular.ttf` — Volkswagen Thesis
- `vw_digit_reg.otf` — VW Digit
- `seat_metastyle_monodigit_regular.ttf` — Seat
- `auditypedisplayhigh.ttf` — Audi Virtual Cockpit
- `frutiger.otf` — Frutiger
- `skoda.ttf` — Skoda
- `larabie.ttf` — Larabie
- `unitedsans.otf` — Ford (United Sans)
- `ev.otf` — Electro Vehicle

---

## Configuration Files

### `build.gradle` (app)
- `compileSdk 34`, `minSdkVersion 28`, `targetSdkVersion 34`
- Kotlin 1.9.20, Java 19
- Protobuf plugin for generating `UserPreference` class
- Data Binding and View Binding enabled
- Compose enabled (for alarm editor)
- R8 minification for release builds

### `build.gradle` (project)
- Kotlin 1.9.20
- Gradle 8.2.2
- Android Gradle Plugin 8.2.2

### `version.properties`
```properties
majorVersion=2
minorVersion=0
patchVersion=30
```

### `settings.gradle`
```groovy
include ':app', ':speedviewlib'
project(':speedviewlib').projectDir = new File(rootDir, 'lib/speedviewlib/speedviewlib')
```

### `gradle.properties`
- AndroidX enabled
- Jetifier enabled (for legacy dependencies)
- R8 full mode enabled
- Parallel builds enabled
- 1536MB JVM heap

---

## External Dependencies

### Binary Dependencies
- `lib/aauto.aar` — Android Auto SDK (not available on Maven)
- `lib/speedviewlib/` — Speedometer/gauge library (git submodule)

### Key Libraries
| Library | Version | Purpose |
|---------|---------|---------|
| `speedviewlib` | custom | Gauge/speedometer widgets |
| `graphview` | 4.2.2 | Line charts |
| `EvalEx` | 3.0.5 | Math expression evaluation |
| `Timber` | 5.0.1 | Logging |
| `ACRA` | 5.11.3 | Crash reporting |
| `Protobuf Java Util` | 3.25.1 | Protocol Buffers |
| `DataStore` | 1.1.1 | Preferences storage |
| `Compose BOM` | 2023.10.01 | Jetpack Compose |
| `Material3` | — | Material Design 3 |
| `ColorPicker` | 3.1.0 | Color picker dialog |
| `rotate-layout` | 3.0.0 | Layout rotation |
| `compose-reorderable` | 0.9.6 | Reorderable lists |
| `listenablefuture` | 9999.0 | Guava conflict resolution |
