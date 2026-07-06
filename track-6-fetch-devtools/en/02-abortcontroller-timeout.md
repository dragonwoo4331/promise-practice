# [Track 6] Exercise 2: `AbortController` and Manual Fetch Timeout

## Difficulty
Medium

## Learning Goal
Implement a manual request timeout for `fetch()` using `AbortController`, and
correctly distinguish the resulting `AbortError` from other kinds of fetch
failures (network errors, HTTP error statuses).

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/AbortController
- https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
- https://developer.mozilla.org/en-US/docs/Web/API/DOMException

## Part 1 — Understand Before You Code

Answer in your own words in `NOTES.md`, **before** writing any code:

1. Why Necessary? — `fetch()` has no built-in `timeout` option. Explain what
   `AbortController`/`AbortSignal` actually do to an in-flight `fetch()` call
   when `.abort()` is invoked, and why this mechanism, rather than something
   like `Promise.race` with a rejecting timer alone, is the correct way to
   *cancel* the underlying network request (not just stop waiting for it).
2. Why Origin? — `XMLHttpRequest` had a built-in `.abort()` method directly on
   the request object. Describe concretely why `fetch()`'s design (a
   separate `AbortController` object whose `.signal` you pass into the
   `fetch()` options) is structured differently, and what problem that
   separation solves (hint: think about aborting multiple related requests
   with one controller).
3. When to Use? — Describe a concrete production scenario where a fetch
   request without a timeout would leave a user stuck (e.g. a slow or hung
   backend, a mobile network dropping mid-request).
4. What if Absent? — If a codebase never used `AbortController` for
   timeouts, what concrete disaster happens to a UI that fires a fetch
   on every keystroke (e.g. a search-as-you-type box) when responses arrive
   out of order or the backend occasionally hangs indefinitely?

Shallow answers like "for async processing" will be rejected — reference the
actual runtime mechanics.

## Part 2 — Coding Task

Write an `async` function `fetchWithTimeout(url, timeoutMs)` that:

1. Creates an `AbortController` and passes its `signal` into `fetch(url,
   { signal })`.
2. Starts a timer for `timeoutMs` milliseconds; if the timer fires before the
   fetch settles, calls `.abort()` on the controller.
3. Clears the timer if the fetch settles (successfully or not) before the
   timeout fires, so the process/tab isn't left with a dangling timer.
4. If the fetch was aborted due to timeout, throws (or lets propagate) an
   error that is distinguishably a timeout — inspect the caught error's
   `name` property (it will be `"AbortError"`) and re-throw a clearer error,
   e.g. `new Error("Request timed out after {timeoutMs}ms")`, so callers
   don't have to know about `AbortError` internals.
5. If the fetch fails for any other reason (network failure, non-ok status —
   reuse your manual status check from Exercise 1), that error propagates
   unchanged, distinguishable from the timeout error.

Test it against `https://jsonplaceholder.typicode.com/posts/1` with a
generous timeout (should succeed), and against a slow endpoint you construct
yourself — `https://httpbin.org/delay/5` returns after 5 seconds, so calling
`fetchWithTimeout("https://httpbin.org/delay/5", 3000)` should time out.

## Part 3 — Requirements & Edge Cases

- [ ] The controller's `signal` is actually wired into the `fetch()` call —
      not just created and ignored.
- [ ] The timeout timer is cleared on both success and failure paths (no
      leaked `setTimeout`).
- [ ] A timeout produces a distinctly identifiable error (checked via
      `error.name === "AbortError"` internally, then re-thrown as a clear,
      descriptive error for the caller).
- [ ] A non-timeout network failure and a non-ok HTTP status are each still
      handled and distinguishable from the timeout case in your driver
      script's logging.
- [ ] Calling `fetchWithTimeout` against a fast endpoint with a generous
      timeout succeeds and returns parsed data.
- [ ] Calling `fetchWithTimeout` against `httpbin.org/delay/5` with a 3 second
      timeout logs the timeout-specific error, not a generic one.

## Submission

Write your answer/code in `workspace/track-6-fetch-devtools/02-abortcontroller-timeout/`.
Include `NOTES.md` and `solution.js`.
