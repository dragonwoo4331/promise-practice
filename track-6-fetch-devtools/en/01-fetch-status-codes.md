# [Track 6] Exercise 1: Basic Fetch and Manual Status Code Handling

## Difficulty
Easy

## Learning Goal
Understand precisely when the `fetch()` Promise rejects versus resolves, and
correctly implement manual HTTP status checking, since `fetch()` only rejects
on network-level failure and resolves normally even for 4xx/5xx responses.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
- https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch
- https://developer.mozilla.org/en-US/docs/Web/API/Response
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

## Part 1 — Understand Before You Code

Answer in your own words in `NOTES.md`, **before** writing any code:

1. Why Necessary? — Why does the `fetch()` API require you to manually check
   `response.ok`/`response.status` instead of throwing automatically on a 404
   or 500? Describe what "network failure" means to `fetch()` versus what an
   HTTP error status code means, and why the spec treats them differently.
2. Why Origin? — Before `fetch()`, the browser's asynchronous HTTP API was
   `XMLHttpRequest`, using callback-based events (`onload`, `onerror`,
   `onreadystatechange`). Describe concretely what was awkward about
   `XMLHttpRequest`'s ergonomics — e.g. how many event listeners/state checks
   were needed to reliably detect "request finished successfully" versus
   "request finished with an error status" versus "request failed to
   connect."
3. When to Use? — Describe a concrete production scenario where failing to
   manually check `response.ok` would cause a bug (e.g. what would your UI
   show if you assumed `fetch()` throwing was the only failure signal).
4. What if Absent? — If a junior engineer writes `fetch(url).then(res =>
   res.json())` and nothing else, what concrete disaster happens when the
   server returns a 404 with a JSON error body, or a 500 with an HTML error
   page?

Shallow answers like "for async processing" will be rejected — reference the
actual runtime mechanics.

## Part 2 — Coding Task

Use `https://jsonplaceholder.typicode.com` as your test API for this entire
track (stay consistent across all three exercises).

Write an `async` function `fetchPost(id)` that:

1. Sends a `GET` request to `https://jsonplaceholder.typicode.com/posts/{id}`.
2. If the network request itself fails (e.g. DNS failure, offline), let the
   rejection propagate to the caller — do not swallow it silently.
3. If the response completes but `response.ok` is `false` (e.g. you request
   an id that doesn't exist, which this API returns as `404` for), manually
   construct and `throw` an `Error` whose message includes the HTTP status
   code and status text.
4. If the response is `ok`, parse and return the JSON body.

Then write a small driver script that calls `fetchPost(1)` (should succeed)
and `fetchPost(99999)` (should hit the manual 404 handling you wrote), logging
the outcome of each distinctly.

## Part 3 — Requirements & Edge Cases

- [ ] `fetchPost` never assumes a non-2xx response means the Promise
      rejected — it checks `response.ok` or `response.status` explicitly.
- [ ] A manually thrown error for a bad status includes the numeric status
      code in its message.
- [ ] A genuine network failure (you can simulate this by pointing the URL at
      a non-existent host, e.g. `https://this-domain-does-not-exist-xyz.test`)
      is handled distinctly from an HTTP error status in your driver script's
      logging — the two failure modes must not be confused with each other.
- [ ] The success path correctly returns the parsed JSON object for a valid
      post id.
- [ ] `NOTES.md` states explicitly, in one sentence, the exact condition
      under which `fetch()`'s returned Promise rejects.

## Submission

Write your answer/code in `workspace/track-6-fetch-devtools/01-fetch-status-codes/`.
Include `NOTES.md` and `solution.js`.
