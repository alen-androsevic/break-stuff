## Oracles

A break is any of these:

- Crash, blank screen, frozen UI, or an unrecoverable state.
- Unhandled console error or exception (web), or a crash/ANR log line (mobile).
- Duplicate side effect: two records, two charges, two messages from one intent.
- Rendered junk: `undefined`, `null`, `NaN`, `[object Object]`, raw template tags, or escaped markup shown as HTML.
- Layout failure: overlapping, clipped or off-screen controls; horizontal page scroll; text overflowing its box.
- Lost state: input or progress gone after back, refresh, rotate or app-switch when the user would expect it kept.
- Silent failure: an action fails with no visible message, or the message gives the user no way forward.
- Validation gap: invalid input accepted, or valid input rejected.
- Accessibility failure: unreachable by keyboard, focus trap, missing label, axe-core violation (serious or critical).
