# AA Torque - Honda Elevate (2025) Guide 🇮🇳🚗

Everything you need to know about using AA Torque with your 2025 Honda Elevate in India.

---

## Table of Contents

1. [Honda Elevate & Android Auto Compatibility](#honda-elevate--android-auto-compatibility)
2. [What You Need (India Shopping List)](#what-you-need-india-shopping-list)
3. [Recommended OBD2 Adapters for India](#recommended-obd2-adapters-for-india)
4. [Setup Guide for Honda Elevate](#setup-guide-for-honda-elevate)
5. [Recommended PIDs for Honda Elevate](#recommended-pids-for-honda-elevate)
6. [Theme & Layout Recommendations](#theme--layout-recommendations)
7. [India-Specific Considerations](#india-specific-considerations)
8. [Troubleshooting in Indian Conditions](#troubleshooting-in-indian-conditions)

---

## Honda Elevate & Android Auto Compatibility

### Does the Honda Elevate Support Android Auto?

The 2025 Honda Elevate in India comes with Honda's **Connect infotainment system**. Android Auto support depends on your trim level:

| Trim | Android Auto | Wireless AA |
|------|-------------|-------------|
| V | Check with dealer | Likely no |
| VX | Check with dealer | Likely no |
| ZX | Yes | Yes (on some units) |
| ZX CVT | Yes | Yes |

**Important:** Honda India may have updated Android Auto support for 2025 models. Check with your dealer or the Honda Connect app.

### Head Unit Specifications

- **Screen Size:** 8-inch (most trims) or 10.25-inch (ZX)
- **Resolution:** Likely 800×480 or 1280×720
- **Touch:** Capacitive touch
- **Rotary Dial:** Not available on most trims (use swipe gestures)
- **USB Port:** USB-A port for wired Android Auto connection

### Connection Method

AA Torque works via **wired Android Auto** (USB cable from phone to car's USB port). Wireless Android Auto dongles like AAWireless can also work.

---

## What You Need (India Shopping List)

### Essential Items

| Item | Price Range (₹) | Where to Buy |
|------|----------------|--------------|
| Torque Pro app | ₹200 (Play Store) | Google Play Store |
| OBD2 Bluetooth adapter | ₹800 - ₹3,000 | Amazon.in, Flipkart |
| AA Torque APK | Free | GitHub releases |
| KingInstaller APK | Free | GitHub releases |
| USB cable (USB-A to USB-C) | ₹200 - ₹500 | Amazon.in |

### Total Estimated Cost: ₹1,200 - ₹4,000

### Recommended OBD2 Adapters

**Budget Options (India):**

1. **OBDII ELM327 Bluetooth** (₹800 - ₹1,200)
   - Available on Amazon.in
   - Works but may have slower refresh rates
   - Search: "ELM327 Bluetooth OBD2"

2. **Vgate iCar2 Bluetooth** (₹1,500 - ₹2,500)
   - Better quality and faster connection
   - Available on Amazon.in
   - Recommended for daily use

**Premium Options:**

3. **OBDLink MX+** (₹5,000 - ₹8,000)
   - Professional grade, very fast
   - Best for serious monitoring
   - Available on Amazon.in (may ship from abroad)

**What to Look For:**
- ✅ ELM327 chipset (or compatible)
- ✅ Bluetooth 2.0+ (for older phones) or Bluetooth 4.0+ (BLE)
- ✅ Low power consumption
- ✅ Compatible with Torque Pro

---

## Recommended OBD2 Adapters for India

### Budget Picks (Amazon.in)

| Adapter | Price | Rating | Notes |
|---------|-------|--------|-------|
| OBDII ELM327 | ₹800 | 3.5/5 | Basic, works for most PIDs |
| OBDII Mini | ₹600 | 3/5 | Compact, may have connectivity issues |
| Vgate iCar2 | ₹1,800 | 4.5/5 | Best value, reliable |

### How to Pair

1. Plug OBD2 adapter into the diagnostic port (under steering wheel, left side)
2. Turn ignition ON (engine can be off for pairing)
3. On your phone: Settings → Bluetooth → Pair with adapter
4. Default PIN: `1234` or `0000`
5. Open Torque Pro → Settings → OBD2 Adapter → Bluetooth → Select your adapter

---

## Setup Guide for Honda Elevate

### Step 1: Install Required Apps

```bash
# On your phone:
1. Install Torque Pro from Play Store
2. Download AA Torque APK from GitHub
3. Download KingInstaller APK from GitHub
```

### Step 2: Install AA Torque via KingInstaller

1. Install KingInstaller
2. Open KingInstaller
3. Select "Install from storage"
4. Navigate to the AA Torque APK
5. Install it
6. KingInstaller spoofs the installer as Play Store

### Step 3: Enable Android Auto Developer Mode

1. **Disconnect** phone from car
2. Open Android Auto app on phone
3. Tap **About Android Auto** 10 times
4. Tap 3-dot menu → **Developer Settings**
5. Check **Unknown Sources**
6. Now AA Torque will appear in Android Auto

### Step 4: Configure Torque Pro

1. Pair OBD2 adapter via Bluetooth
2. Open Torque Pro
3. Go to Settings → OBD2 Adapter Settings
4. Select your Bluetooth adapter
5. Start the car engine
6. Wait for Torque to connect to ECU (green indicator)
7. **Important:** Torque must be running and connected for AA Torque to work

### Step 5: Configure AA Torque

1. Open **AA Torque Settings** on your phone
2. Grant all permissions (location, phone, etc.)
3. Select theme, font, background
4. Configure dashboards:
   - Tap each dashboard to configure gauges and displays
   - Select PIDs from Torque's list
   - Set min/max values, units, icons

### Step 6: Connect to Car

1. Connect phone to car via USB cable
2. Android Auto should launch automatically
3. Navigate to AA Torque in the menu
4. Data should start appearing within 5-10 seconds

---

## Recommended PIDs for Honda Elevate

The Honda Elevate uses a **1.5L i-VTEC petrol engine**. Here are the most useful PIDs:

### Essential PIDs

| PID | Description | Min | Max | Unit |
|-----|-------------|-----|-----|------|
| `torque_0c,0` | Engine RPM | 0 | 8000 | rpm |
| `torque_0d,0` | Vehicle Speed | 0 | 200 | km/h |
| `torque_05,0` | Coolant Temperature | -40 | 215 | °C |
| `torque_11,0` | Throttle Position | 0 | 100 | % |
| `torque_04,0` | Engine Load | 0 | 100 | % |

### Useful PIDs

| PID | Description | Min | Max | Unit |
|-----|-------------|-----|-----|------|
| `torque_0f,0` | Intake Air Temperature | -40 | 215 | °C |
| `torque_0e,0` | Timing Advance | -64 | 63.5 | ° |
| `torque_14,0` | O2 Sensor Voltage | 0 | 1.275 | V |
| `torque_2f,0` | Fuel Tank Level | 0 | 100 | % |
| `torque_42,0` | Control Module Voltage | 0 | 65.535 | V |

### Honda-Specific Custom PIDs

Some Honda-specific data requires Torque Pro's custom PID editor:

```python
# Coolant Temperature (alternative formula)
Mode: 01
PID: 05
Formula: (A-40)
Min: -40
Max: 215
Unit: °C

# Intake Manifold Absolute Pressure
Mode: 01
PID: 0B
Formula: A
Min: 0
Max: 255
Unit: kPa
```

### Dashboard Layout Recommendation for Elevate

**Screen 1: Driving**
- Left: RPM (`torque_0c,0`)
- Center: Speed (`torque_0d,0`) — High visibility mode
- Right: Throttle (`torque_11,0`)
- Display 1: Coolant Temp
- Display 2: Fuel Level

**Screen 2: Efficiency**
- Left: Engine Load (`torque_04,0`)
- Center: Intake Air Temp (`torque_0f,0`)
- Right: Timing Advance (`torque_0e,0`)
- Display 1: Control Module Voltage
- Display 2: O2 Sensor Voltage

---

## Theme & Layout Recommendations

### Best Themes for Honda Elevate

Since Honda doesn't have a VAG-style virtual cockpit, here are good generic options:

1. **Electro Vehicle** — Clean, modern, works well on any car
2. **Dark** — Minimal, easy on eyes at night
3. **Minimalistic** — Clean look without brand styling
4. **BMW** — Sporty look that works for any car

### Best Fonts for Readability

1. **7 Segment Display** — Classic dashboard look, very readable
2. **Electro Vehicle** — Modern, clean
3. **Frutiger** — Professional, easy to read

### Layout Tips for 8" Screen

- Use **3 gauges** with **4 displays**
- Enable **center gauge large** for better visibility
- Set **gauge opacity to 80-90%** for better background visibility
- Use **high visibility mode** for the speed gauge

### Night Driving Tips

- Use **dark backgrounds** (black, dark grid)
- Reduce **gauge opacity** to 70-80%
- Enable **blur** on album art if using media background
- Use **dim needle colors** (avoid bright white)

---

## India-Specific Considerations

### Temperature & Weather

- **Indian summers (40°C+):** OBD2 adapters may overheat. Keep the car AC on.
- **Monsoon season:** Moisture can affect Bluetooth connections. Ensure adapter is dry.
- **Dust:** Keep the OBD2 port clean. Use a port cover when not in use.

### Power Supply

- Use a **good quality USB cable** (data + charge)
- Avoid cheap cables that cause connection drops
- Keep your phone charged — AA Torque uses battery
- Consider a **USB Y-cable** if your car's USB port doesn't provide enough power

### Network Considerations

- AA Torque works **offline** — no internet needed
- Only needed for: initial install, update checks, crash reports
- Crash reports go to email (ACRA) — can be disabled if on limited data

### Hindi/Regional Language

- The app interface is in **English only** (for car display)
- Settings app supports multiple languages
- PID labels can be set to any language

### Insurance & Warranty

- **OBD2 adapters are safe** — they only read data, don't modify anything
- **No warranty voiding** — standard OBD2 diagnostic port
- **Insurance:** Using an OBD2 reader doesn't affect insurance

### Theft Prevention

- Remove OBD2 adapter when parking in unfamiliar areas
- Some adapters have low power draw — remove if parking for days
- The adapter itself is small and can be hidden

---

## Troubleshooting in Indian Conditions

### "Unable to connect to Torque plugin service"

**Common causes in India:**
1. Cheap ELM327 adapter — try a better quality one
2. Phone Bluetooth issues — restart Bluetooth
3. Torque Pro not running — open Torque first
4. Car ignition not ON — turn ignition to ON position

### Frequent Disconnections

1. Check USB cable quality
2. Try a different USB port in the car
3. Disable battery optimization for AA Torque:
   - Settings → Apps → AA Torque → Battery → Unrestricted
4. Keep phone cool — Indian heat can cause throttling

### "No data" or Stuck Values

1. Check if Torque Pro shows data on phone
2. Restart Torque Pro
3. Restart AA Torque
4. Check OBD2 adapter is properly plugged in
5. Some Honda ECUs take longer to respond — wait 30 seconds

### App Not Appearing in Android Auto

1. Ensure Developer Mode is enabled
2. Check Unknown Sources is enabled
3. Force stop Android Auto and reopen
4. Reinstall via KingInstaller

### Slow Refresh Rate

1. Reduce number of PIDs being monitored
2. Use faster OBD2 adapter (Vgate iCar2 or better)
3. Don't run Torque Pro in background on phone
4. Close other apps consuming CPU

### Honda ECU Compatibility

The Honda Elevate uses **Honda's proprietary ECU**. Most standard OBD2 PIDs work, but some Honda-specific data may require:
- Custom PIDs in Torque Pro
- Extended OBD2 mode (Mode 22 for Honda)

If certain PIDs show 0 or error:
1. Check if the PID is supported by your ECU
2. Try Torque Pro's built-in Honda PID list
3. Search online forums for Honda Elevate specific PIDs

---

## Quick Start Checklist

- [ ] Torque Pro installed and working
- [ ] OBD2 adapter paired and connected
- [ ] AA Torque installed via KingInstaller
- [ ] Android Auto developer mode enabled
- [ ] Unknown Sources checked
- [ ] AA Torque permissions granted
- [ ] Dashboard configured with preferred PIDs
- [ ] Theme and font selected
- [ ] Phone connected to car via USB
- [ ] Data flowing on car display

---

## Useful Resources

- **AA Torque GitHub:** https://github.com/agronick/aa-torque
- **AA Torque Discussions:** https://github.com/agronick/aa-torque/discussions
- **Torque Pro:** https://torque-bhp.com/
- **KingInstaller:** https://github.com/fcaronte/KingInstaller
- **Honda Elevate Owners Forum:** Search for "Honda Elevate OBD2" on Team-BHP

---

*Last updated: June 2026*
*Vehicle: 2025 Honda Elevate (India)*
