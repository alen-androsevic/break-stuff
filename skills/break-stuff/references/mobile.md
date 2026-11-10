# Mobile driver: adb (Android), simctl and computer use (iOS Simulator)

Run every command through Bash. On Android, the REA tools (`mcp__rea__*`) cover the same ground where they are easier: `list_adb_devices`, `start_adb_app`, `stop_adb_app`, `capture_adb_screen`, `read_adb_logcat`, `inspect_adb_window`. They take a moment to load through ToolSearch.

## Android (emulator or device)

- **Open**: `adb devices`, then `adb shell monkey -p <package> -c android.intent.category.LAUNCHER 1` (or REA `start_adb_app`).
- **Snapshot / refs**: `adb exec-out uiautomator dump /dev/tty` gives the view tree with text, content-desc and bounds. Tap the centre of an element's bounds.
- **Act**: `adb shell input tap X Y`, `input text '<escaped>'` (spaces as `%s`; for emoji, RTL and other non-ASCII text, set the clipboard and paste instead, and say so), `input keyevent KEYCODE_BACK|KEYCODE_ENTER|KEYCODE_TAB`, `input swipe`.
- **Oracle checks**: `adb logcat -c` before the run, then after each action run `adb logcat -d -b crash,main *:E` (or REA `read_adb_logcat`) for `FATAL EXCEPTION`, `ANR in`, and exceptions. Read the dump's text for junk values.
- **Evidence**: `adb exec-out screencap -p > <scratchpad>/break-NN.png` (or REA `capture_adb_screen`), then look at it with Read. For timing and monkey runs, use `adb shell screenrecord`.
- **layout**: `settings put system accelerometer_rotation 0`, then `settings put system user_rotation 1` (landscape) or `0` (portrait). `settings put system font_scale 2.0`. `cmd uimode night yes|no`. Set every setting back when done.
- **navigation**: `KEYCODE_BACK` spam, `KEYCODE_HOME` then relaunch, `am force-stop <package>` then relaunch.
- **timing**: chain several `input tap` commands in one shell call for rapid taps.
- **network**: `cmd connectivity airplane-mode enable|disable` (or `svc wifi disable` and `svc data disable`), and turn it back on afterwards.
- **a11y**: in the dump, any `clickable="true"` node with empty `text` and `content-desc` has no label. Run the keyboard pass with `KEYCODE_TAB` and `KEYCODE_ENTER`.

## iOS Simulator

- **Open**: `xcrun simctl list devices booted`, then `xcrun simctl launch booted <bundle-id>`.
- **Act**: there is no tap command in `simctl`. Drive the Simulator window with computer use (load the `computer-use` skill), with screenshots and coordinates. If computer use is not available in this session, mark the touch attacks `n/a: no touch driver` and do only what `simctl` can.
- **Oracle checks**: run `xcrun simctl spawn booted log stream --predicate 'process == "<App>"' --level error` in the background and read its output after each action. Check `~/Library/Logs/DiagnosticReports/` for crash reports.
- **Evidence**: `xcrun simctl io booted screenshot <scratchpad>/break-NN.png`, and `recordVideo` for timing runs.
- **layout**: `xcrun simctl ui booted appearance dark|light`, `xcrun simctl ui booted content_size accessibility-extra-extra-extra-large` (set it back to `large`). Rotate with `cmd+←/→` on the Simulator window.
- **navigation**: `xcrun simctl terminate booted <bundle-id>` then relaunch. Send the app to the background with `cmd+shift+h`.
- **inputs**: `printf '%s' "<payload>" | xcrun simctl pbcopy booted`, then paste in the field.
- **network**: the Simulator uses the Mac's network, so stop the backend instead of going offline, and say so.

If a capability is missing, mark that attack `n/a: <what is missing>` rather than faking it.
