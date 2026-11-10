## iOS Simulator

- **Open**: `xcrun simctl list devices booted`, then `xcrun simctl launch booted <bundle-id>`.
- **Act**: there is no tap command in `simctl`. Drive the Simulator window with computer use (load the `computer-use` skill), with screenshots and coordinates. If computer use is not available in this session, mark the touch attacks `n/a: no touch driver` and do only what `simctl` can.
- **Oracle checks**: run `xcrun simctl spawn booted log stream --predicate 'process == "<App>"' --level error` in the background and read its output after each action. Check `~/Library/Logs/DiagnosticReports/` for crash reports.
- **Evidence**: `xcrun simctl io booted screenshot <scratchpad>/break-NN.png`, and `recordVideo` for timing runs.
- **layout**: `xcrun simctl ui booted appearance dark|light`, `xcrun simctl ui booted content_size accessibility-extra-extra-extra-large` (set it back to `large`). Rotate with `cmd+←/→` on the Simulator window.
- **navigation**: `xcrun simctl terminate booted <bundle-id>` then relaunch. Send the app to the background with `cmd+shift+h`.
- **inputs**: `printf '%s' "<payload>" | xcrun simctl pbcopy booted`, then paste in the field.
- **network**: the Simulator uses the Mac's network, so stop the backend instead of going offline, and say so.
