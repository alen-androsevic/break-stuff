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
