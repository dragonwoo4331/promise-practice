# Track 6 — Fetch API & DevTools Debugging

This track covers the Fetch API's actual failure semantics (it does not
reject on HTTP error statuses), request cancellation/timeouts via
`AbortController`, and reading real network behavior in Chrome DevTools —
skills that matter the moment your code leaves `localhost` and meets real
latency and real error responses.

Read the MDN track landing page first:
https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API

## Exercises

1. [01 — Basic Fetch and Manual Status Code Handling](./01-fetch-status-codes.md)
   Fetch from a real public API and manually handle 404/500 responses, since
   `fetch()` only rejects on network failure, never on HTTP error status.
2. [02 — `AbortController` and Manual Fetch Timeout](./02-abortcontroller-timeout.md)
   Implement a fetch timeout using `AbortController` and distinguish the
   resulting `AbortError` from other failure types.
3. [03 — Network Tab and Throttling Investigation](./03-network-tab-throttling.md)
   A DevTools investigation exercise: throttle to Slow 3G, read the timing
   waterfall, and document cache behavior — no solution code required.

See the repository root `README.md` for submission rules (no AI, `NOTES.md`
before code, push per track).
