# Troubleshooting

Common issues and how to fix them.

---

## "Unable to connect to Torque plugin service"

- Torque Pro is not running: open it, connect to OBD2 adapter, keep it open
- OBD2 adapter not paired: re-pair via phone Bluetooth settings
- OBD2 adapter not plugged in: check the diagnostic port under the steering wheel
- Car ignition off: turn ignition to ON position (engine can be off for pairing)

## AA Torque doesn't appear in Android Auto

- Developer Mode not enabled: tap "About Android Auto" 10 times
- Unknown Sources not checked: go to Developer Settings and check "Unknown Sources"
- Not installed via KingInstaller: reinstall using KingInstaller
- App disappeared after update: Android updates can reset permissions, re-enable Unknown Sources (see the main [README](../README.md#installation-guide))

## Pixel Phone: App won't install

1. Update Google Package Installer from APKMirror
2. Install via KingInstaller (not directly)
3. If still failing, try the "Force Update" option in AA Torque Settings
4. Check [GitHub Discussions](https://github.com/agronick/aa-torque/discussions) for latest Pixel-specific workarounds

## No data showing on car display

1. Check that Torque Pro shows data on your phone (open Torque, look at gauges)
2. Make sure OBD2 adapter is connected (green indicator in Torque)
3. Restart both Torque Pro and AA Torque
4. Check if your car's ECU supports standard OBD2 PIDs

## Frequent disconnections

- Use a high-quality USB cable (data + charge, not charge-only)
- Keep your phone cool (heat can cause throttling)
- Disable battery optimization for AA Torque (Settings > Apps > AA Torque > Battery > Unrestricted)
- Close other apps consuming CPU

## Gauge shows "Connecting to Torque..."

- Torque Pro is not connected to the OBD2 adapter yet
- Wait for the green indicator in Torque Pro
- Make sure the car engine is running

## App crashes or freezes

1. Open AA Torque Settings > Menu > "Copy logs to clipboard"
2. Paste the logs into a [GitHub Issue](https://github.com/agronick/aa-torque/issues) or [Discussion](https://github.com/agronick/aa-torque/discussions)
3. Try clearing the app data: Settings > Apps > AA Torque > Storage > Clear Data
4. Reconfigure your dashboards
