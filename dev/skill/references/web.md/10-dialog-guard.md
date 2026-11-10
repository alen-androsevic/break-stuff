## Before the first attack, and after every navigation or refresh

Install the dialog guard with `javascript_tool`. A real `alert`/`confirm`/`prompt` blocks the extension, and you have to dismiss it by hand:

```js
for (const k of ['alert', 'confirm', 'prompt']) window[k] = (...a) => { console.log(`[break-stuff] ${k}:`, ...a); return k === 'confirm' ? false : null; };
window.onbeforeunload = null;
```

A page reload drops the guard, so reinstall it. If a dialog still appears, tell the user to dismiss it and mark the action `break` (the oracle is an unexpected dialog). Stub `window.print` the same way: the print dialog blocks the extension too.
