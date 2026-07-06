# Track 7 — URL & Web Fundamentals

This track covers the core web platform mechanisms for working with URLs, browser security boundaries, and cookies. These are not academic details — mishandling any of them causes real bugs (broken query strings, silent data loss) or real security vulnerabilities (CSRF, data leakage across origins).

Each exercise requires you to answer conceptual questions in `NOTES.md` **before** writing any code or doing the hands-on task. Answers must reference concrete mechanics, not buzzwords. Coding/investigation tasks never come with a solution — you write or investigate 100% of it yourself.

## Exercises

1. **[01 — URL Anatomy & the URL API](./01-url-anatomy-and-url-api.md)** (Easy)
   Parse a complex URL with the `URL` constructor; distinguish `host` vs `hostname`, `href` vs `origin`.

2. **[02 — URLSearchParams Manipulation](./02-urlsearchparams-manipulation.md)** (Medium)
   Add, update, delete, and read repeated query parameters correctly using `URLSearchParams`, with proper percent-encoding.

3. **[03 — Origin, Domain & Same-Origin Policy](./03-origin-domain-same-origin-policy.md)** (Medium)
   Apply the exact same-origin definition to URL pairs, then trigger and analyze a real CORS error in your own browser.

4. **[04 — Cookies, Domain Scope & Site vs Origin](./04-cookies-domain-scope-site-vs-origin.md)** (Hard)
   Understand how `Domain` and `Path` scope cookies, distinguish "site" from "origin", and inspect real cookies via DevTools.

## Reference Landing Pages

- https://developer.mozilla.org/en-US/docs/Web/API/URL
- https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams
- https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

## Submission Convention

Each exercise's work goes in `workspace/track-7-url-and-web/0N-<slug>/`, containing `NOTES.md` and either `solution.js` (coding tasks) or `ANSWER.md` (investigation/prediction tasks).
