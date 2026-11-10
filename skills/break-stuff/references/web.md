# Web driver: Claude in Chrome

Load all the tools in one ToolSearch call: `tabs_context_mcp`, `tabs_create_mcp`, `tabs_close_mcp`, `navigate`, `computer`, `read_page`, `get_page_text`, `find`, `form_input`, `file_upload`, `javascript_tool`, `read_console_messages`, `read_network_requests`, `resize_window`, `gif_creator` (all `mcp__claude-in-chrome__*`). Start with `tabs_context_mcp`, then work in a new tab. If the extension does not answer after 2–3 tries, stop and ask the user.

This file maps attacks to those tools.

## Before the first attack, and after every navigation or refresh

Install the dialog guard with `javascript_tool`. A real `alert`/`confirm`/`prompt` blocks the extension, and you have to dismiss it by hand:

```js
for (const k of ['alert', 'confirm', 'prompt']) window[k] = (...a) => { console.log(`[break-stuff] ${k}:`, ...a); return k === 'confirm' ? false : null; };
window.onbeforeunload = null;
```

A page reload drops the guard, so reinstall it. If a dialog still appears, tell the user to dismiss it and mark the action `break` (the oracle is an unexpected dialog). Stub `window.print` the same way: the print dialog blocks the extension too.

## Actions that cost money or reach other people

If a submit starts paid or outward work (model calls, emails, payments, webhooks), agree a cap with the user before attacking, and run the repeat attacks (double-click, key repeat, monkey) against a stub of that transport, so they count what would have been sent instead of sending it. For example, replace `window.EventSource` or `fetch` for that one endpoint with an object that logs its URL, and let everything else through. Say in the report which attacks ran against the stub. Delete whatever the capped real runs wrote, once you have kept copies as evidence.

## Mapping

- **Oracle checks**: after every action, run `read_console_messages` (filter with `pattern` to errors and `[break-stuff]`), then `read_page` or `get_page_text` to scan the rendered text for junk values. Use a `computer` screenshot when layout matters.
- **Duplicate side effects**: `read_network_requests` after timing attacks. Two POST/PUT requests from one intent is a break, even if the UI shows one result.
- **Evidence**: a `computer` screenshot on every break. Record timing and monkey runs with `gif_creator` (take extra frames before and after) and give each file a name such as `break-stuff-timing-submit.gif`.
- **inputs**: use `computer` `type` on a clicked field for real keystrokes, and `form_input` only to set long values quickly (and say which you used). For a **real paste**, put the text on the macOS clipboard with `printf '%s' "<payload>" | pbcopy` in Bash, click the field, and press `cmd+v` with `computer` `key`. Add `\n` to the payload to test a trailing newline. For files, use `file_upload` with files you generated in the scratchpad (0 bytes, 50 MB, `.exe` renamed to `.png`).
- **timing**: rapid clicks with repeated `computer` `left_click` on the same ref and no wait in between, or for a true 10× burst use `javascript_tool`: `for (let i=0;i<10;i++) el.click()`, and report which you used. For two tabs, use `tabs_create_mcp` on the same URL.
- **navigation**: use `navigate` with `back`/`forward`. To refresh, run `location.reload()` with `javascript_tool`: `navigate` to the same URL does not reload when only the hash matches. For deep links, `navigate` to a later step or to `/thing/does-not-exist-999`.
- **layout**: Chrome will not size a window below about 500 px or beyond the screen, and `computer` refuses page-zoom keys (`cmd+=`). So test widths in an iframe harness: open a second tab on any URL of the same origin (a JSON endpoint will do), and replace its document with `<iframe src="/the/page" style="flex:none;width:320px;height:760px;border:0">` (`flex: none`, or a flex body shrinks it). For widths wider than the screen, add `transform:scale(0.55);transform-origin:0 0`. 200% zoom lays out like an iframe of half the window's `innerWidth` × `innerHeight`. Read `innerWidth` inside the frame to confirm its size, and say in the report that widths ran in an iframe. Check for horizontal scroll with `document.documentElement.scrollWidth > innerWidth` and find the offending elements by scanning for any `getBoundingClientRect().right > innerWidth`. For dark mode, use the page's own theme switch if it has one. Otherwise ask before toggling the macOS appearance (`osascript -e 'tell app "System Events" to tell appearance preferences to set dark mode to not dark mode'`), and set it back afterwards.
- **network**: on localhost, stopping the backend (and starting it again) is the real test. When stopping it is not possible, use `javascript_tool` to make requests fail inside the page (wrap `fetch` and `XMLHttpRequest.prototype.send` to reject or delay, and close open `WebSocket`s), and write "simulated in page" in the report. There is no real offline or 3G throttle, so do not claim one.
- **a11y**: run the keyboard-only pass with `computer` `key` (`Tab`, `shift+Tab`, `Return`, `space`, `Escape`) and a screenshot after each step to see focus. `read_page` gives each element's accessible name. For axe, inject it with `javascript_tool`: `await new Promise((r,e)=>{const s=document.createElement('script');s.src='https://cdn.jsdelivr.net/npm/axe-core@4.10.2/axe.min.js';s.onload=r;s.onerror=e;document.head.append(s)}); (await axe.run()).violations.filter(v=>['serious','critical'].includes(v.impact)).map(v=>({id:v.id,impact:v.impact,n:v.nodes.length,target:v.nodes[0].target}))`. If CSP blocks the script, mark axe `n/a: CSP blocks injection`.
- **monkey**: 200 round trips through the extension are slow and drop easily, so play the seeded list inside the page. Run the same PRNG in one `javascript_tool` call, check that it gives the same action counts as the saved list, and run the loop without awaiting it, writing each step and its oracle flags to `window.__mlog`. Then read `__mlog` in a later call. Keep clicks inside the surface (skip other pages' buttons that start paid or destructive work), reinstall the guard after any navigation, and say in the report that the clicks and keys were synthetic.
- **clicking**: prefer a `ref` from `find` or `read_page` over coordinates. A screenshot taken after a resize or in another tab has a different frame, and a coordinate click then lands somewhere else without an error.

If a capability is missing, mark that attack `n/a: <what is missing>` rather than faking it.
