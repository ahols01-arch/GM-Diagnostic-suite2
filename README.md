# GM Powertrain Diagnostics — Android starter project

**Version:** 0.1.0 (source project; not yet compiled or vehicle-tested)

A native Android Java app starter for Bluetooth Classic ELM327 adapters. It includes paired-device selection, serial Bluetooth connection, basic ELM initialization, standard OBD-II service requests, live-data reads, basic DTC decoding, freeze-frame requests, readiness status, and a confirmation before clearing codes.

## Current functions

- Connect to a previously paired Bluetooth Classic adapter using the Serial Port Profile (SPP).
- Initialize ELM327 (`ATZ`, echo/line/header settings, automatic protocol selection).
- Read adapter identification and supported PID response.
- Read stored (Mode 03), pending (Mode 07), and permanent (Mode 0A) DTCs.
- Read selected standard live PIDs: RPM, coolant temperature, speed, throttle, calculated load, fuel trims, intake temperature, MAP, and ignition advance.
- Request freeze-frame values and readiness status.
- Clear codes with an explicit confirmation.

## Important limits

This is an initial source implementation, not a replacement for all of GM TIS2000. Generic ELM327 adapters generally cannot provide the complete GM-enhanced data, all-module scanning, bidirectional controls, security access, SPS/module programming, or special functions of an OEM-capable interface. Vehicle year, ECU protocol, and adapter clone quality affect results. The 1997 GMC may use GM-specific communications for some enhanced diagnostics; standard OBD-II support is not the same as full GM powertrain access.

The response parser is intentionally basic and should be tested against the exact adapter. Do not use while driving. Save codes/freeze-frame before clearing them. Do not use this starter for safety-critical repair decisions without confirming data with a suitable scan tool and service information.

## Build in Android Studio

1. Install Android Studio on a computer (or use a trusted cloud Android build environment).
2. Open this folder as an existing Gradle project.
3. Allow Gradle to download the Android Gradle Plugin and dependencies; install Android SDK Platform 35 when prompted.
4. Build > Build APK(s). The debug APK normally appears at `app/build/outputs/apk/debug/app-debug.apk`.
5. Install the APK on Android. Pair the ELM327 in Android Bluetooth settings first; grant Nearby devices/Bluetooth permission when asked.

No Gradle wrapper is included in this initial package. Android SDK and Gradle are not present in the authoring environment, so an APK could not be compiled or validated here. The source should be treated as a starter implementation requiring compilation and bench/vehicle testing before use.

## Bluetooth setup

- Many low-cost ELM327 devices use Bluetooth Classic and appear as OBDII, OBD-II, or similar. Pair them in Android settings first.
- Some adapters are Bluetooth Low Energy (BLE) or Wi-Fi rather than Bluetooth Classic; this app's SPP connection will not work with those without a separate transport implementation.
- Common pairing PINs are sometimes printed on the adapter or packaging; use the manufacturer's documentation rather than guessing.

## Next development steps

1. Test against the exact adapter and 1997 GMC.
2. Improve protocol detection and robust ISO-TP / multi-frame parsing.
3. Add live refresh graphs, session logs, export/share, vehicle profiles, and PID support checks.
4. Add GM enhanced protocols only with verified hardware and documented protocol support.
5. Add actuator tests or programming only where the ECU, interface, security model, and legal access permit them.

## Build APK using GitHub Actions (can be initiated from a phone)

This project includes `.github/workflows/android.yml` so a cloud runner can build the debug APK without installing Android Studio locally:

1. Create a new repository on GitHub and upload the **contents** of this project folder (not just the ZIP file).
2. Open the repository's **Actions** tab, choose **Build debug APK**, then tap **Run workflow**. On the first run, GitHub may ask you to enable Actions.
3. When the run finishes successfully, open that workflow run and download the artifact named `gm-powertrain-diagnostics-debug-apk`.
4. Extract the downloaded artifact on the phone and install `app-debug.apk`. Android may ask you to allow installation from the browser or file manager you used.

The workflow has been included but was not executed in this environment. If it fails, the build log will identify the specific dependency or source issue to fix.
