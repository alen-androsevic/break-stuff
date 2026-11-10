# break-stuff

A Claude Code skill that drives a running web page or mobile app as a hostile user and reports what breaks: crashes, console errors, duplicate side effects, junk on screen, layout failures, lost state, silent failures, validation gaps and accessibility failures. It ends with a report sorted by severity, with repro steps, evidence and counts of what held.

## Install

From the [alen-androsevic marketplace](https://github.com/alen-androsevic/skills):

```bash
claude plugin marketplace add alen-androsevic/skills
claude plugin install break-stuff@alen-androsevic
```

Or without plugins, as a plain skill:

```bash
git clone https://github.com/alen-androsevic/break-stuff ~/code/break-stuff
ln -s ~/code/break-stuff/skills/break-stuff ~/.claude/skills/break-stuff
```

Then ask Claude Code to break or stress test a page, for example "break the checkout form on localhost:3000". It asks for the target if you leave it out, and for a cost cap before it attacks anything that starts paid or outward work (model calls, emails, payments).

## What's in it

- `skills/break-stuff/SKILL.md`: the run (scope, driver, map the surface, attack, report) and the oracles that decide what counts as a break.
- `references/attacks.md`: the attack menu by category: inputs, timing, navigation, layout, network, a11y, monkey.
- `references/web.md`: the web driver, Claude in Chrome. It covers a dialog and print guard, stubbing paid transports, an iframe harness for widths and zoom (Chrome won't size a window below about 500 px, and the computer tool refuses page-zoom keys), axe-core, and seeded monkey runs played inside the page.
- `references/mobile.md`: Android through `adb` (or REA's adb tools), and the iOS Simulator through `xcrun simctl` plus computer use for taps.
- `references/report.md`: the report template and severity levels.

## Working on the skill

`skills/break-stuff/` is built from the parts in `dev/skill/` by [skat](https://github.com/alen-androsevic/skat); don't edit it by hand. Each directory whose name ends in `.md` is one file, made of its parts in name order, so `dev/skill/references/web.md/10-dialog-guard.md` is the dialog-guard section of `references/web.md`.

```bash
cargo install --git https://github.com/alen-androsevic/skat
skat            # build skills/break-stuff/ from dev/skill/
skat --check    # exit 1 if skills/break-stuff/ is out of date
```

Commit the parts and the built files together.

## Where it comes from

A fork of [`skills/.experimental/break-the-ui`](https://github.com/Jeroen-G/skills/tree/main/skills/.experimental/break-the-ui) by Jeroen-G, at commit `340d20f` (2026-11-09), under the MIT licence in `LICENSE`. That version drives pages with the `agent-browser` and `agent-device` CLIs. This fork:

- replaces both drivers with Claude's own tools, and rewrites `web.md` and `mobile.md` for them;
- drops `alert()` payloads and installs a dialog and print guard, because a real browser dialog blocks the Chrome extension;
- asks for a cost cap before attacking submits that start paid or outward work, and runs repeat attacks against a stub of that transport;
- plays monkey runs inside the page from a seeded PRNG, so the same seed gives the same run.
