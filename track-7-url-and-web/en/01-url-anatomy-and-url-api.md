# [Track 7] Exercise 1: URL Anatomy & the URL API

## Difficulty
Easy

## Learning Goal
Understand how the `URL` interface parses a URL string into its structural components, and precisely distinguish `host` vs `hostname` and `href` vs `origin`.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/URL
- https://developer.mozilla.org/en-US/docs/Web/API/URL/URL
- https://developer.mozilla.org/en-US/docs/Web/URI

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why does the platform provide a dedicated `URL` object instead of leaving developers to parse URLs with `split('/')`, `split('?')`, and regular expressions?
2. **Why Origin?** URLs have a long, messy grammar (scheme, userinfo, host, port, path, query, fragment) defined by a formal spec (RFC 3986 / WHATWG URL Standard). Why did browsers standardize a strict parsing algorithm instead of each site/library inventing its own?
3. **When to Use?** Describe a concrete production scenario where you would use the `URL` constructor to pull apart an incoming URL (e.g., inspecting a redirect target, validating a webhook callback URL, logging the pathname a request hit).
4. **What if Absent?** Describe a concrete bug that manual string-splitting parsing causes — for example, a URL with a `?` inside a hash fragment, or a path containing an `@` inside userinfo, breaking a naive `split('@')` or `split('?')` approach.

## Part 2 — Coding Task
Given this URL string:

```
https://user:pass@sub.example.com:8443/path/to/page?foo=1&bar=two#section-2
```

Write a script that:

1. Constructs a `URL` object from this string.
2. Logs each of the following properties on separate lines, clearly labeled: `protocol`, `username`, `password`, `hostname`, `host`, `port`, `pathname`, `search`, `searchParams` (as an iterable representation), `hash`, `origin`, `href`.
3. Logs a short explanation (as a comment or console output) of what `host` contains that `hostname` does not, using the actual values you printed as evidence.
4. Logs a short explanation of what `href` contains that `origin` does not, using the actual values you printed as evidence.
5. Additionally, construct a second `URL` using the two-argument form (`new URL(relativePath, base)`) to resolve `"../images/logo.png"` against `"https://example.com/store/products/item42"`. Log the resulting `href` and explain in a comment why the result looks the way it does (relative path resolution rules).

## Part 3 — Requirements & Edge Cases
- [ ] Use the `URL` constructor — no manual string splitting, regex parsing, or `indexOf` gymnastics on the raw URL string.
- [ ] Correctly identify that `host` includes the port (`sub.example.com:8443`) while `hostname` does not (`sub.example.com`).
- [ ] Correctly identify that `origin` never includes credentials, path, query, or hash — only scheme + host (+ port if non-default).
- [ ] Handle the case where the port is the default for the scheme (e.g., `https://example.com:443/`) — check and state what `port` and `host` return in that case.
- [ ] Demonstrate the two-argument `URL(relative, base)` resolution correctly, including a case that goes up one directory level (`../`).
- [ ] Do not swallow errors — if you test a malformed URL string, show what error `URL` throws (`TypeError`) and how you would guard against it (e.g., `try/catch`).

## Submission
Write your answer/code in `workspace/track-7-url-and-web/01-url-anatomy-and-url-api/`. Include `NOTES.md` and `solution.js`.
