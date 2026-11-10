## inputs
Apply to every text, number, date, file and search field.
- Empty; whitespace only; leading/trailing spaces.
- 5,000+ characters; one unbroken 300-character word.
- Emoji, combined emoji (👨‍👩‍👧), RTL text (مرحبا), zero-width space, Zalgo text.
- Markup and injection strings: `<script>document.title='XSS-1'</script>`, `"><img src=x onerror="document.title='XSS-2'">`, `<b>bold?</b>`, `'; DROP TABLE users;--`, `{{7*7}}`, `${7*7}`. Never use `alert()`/`confirm()`/`prompt()` payloads: a real dialog blocks the browser driver. A changed `document.title`, bold text or `49` on screen is the break.
- Numbers: 0, -1, 1e309, 0.1+0.2 style decimals, leading zeros, a comma decimal (`1,5`).
- Dates: Feb 29 on a non-leap year, year 0001 and 9999, end before start.
- Paste instead of type; paste with a trailing newline.
- Files: 0 bytes, very large, wrong extension, a renamed executable.
