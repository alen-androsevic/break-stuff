# break-stuff

A Claude Code skill and plugin. The skill's source is `dev/skill/`. `skills/break-stuff/` is built from it, and the plugin installs from that built copy.

- Edit the parts in `dev/skill/`, never the files in `skills/break-stuff/`. Run `skat` after every change, and commit the parts and the built files together.
- `skat --check` exits 1 when the built files are out of date.
- A directory named `<file>.md` holds that file's parts, joined in name order with one blank line between them. Number the parts in steps of ten (`10-`, `20-`), so a new part fits in between without renaming the others.
- skat lives at https://github.com/alen-androsevic/skat. If `skat` isn't on the `PATH`, install it with `cargo install --git https://github.com/alen-androsevic/skat`.

## MIG

Skin: alen (terminal output only; it uses no colour)
