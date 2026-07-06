# [Track 6] Exercise 3: Network Tab and Throttling Investigation

## Difficulty
Medium

## Learning Goal
Read and interpret a real request's timing waterfall in Chrome DevTools'
Network tab, and understand the distinction between the network-level status
DevTools reports and what your JavaScript code actually observes, including
how the browser cache changes both.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview

(DevTools UI itself is not MDN-documented in the same way, but the underlying
HTTP mechanics — caching, status codes — are; use these pages to ground your
observations in the actual protocol.)

## Part 1 — Understand Before You Code

Answer in your own words in `NOTES.md`, **before** doing the investigation:

1. Why Necessary? — Why is it not enough to test your fetch code only under
   normal, fast local network conditions? Explain what categories of bugs
   (race conditions, premature UI updates, missing loading states) only
   surface under realistic latency.
2. Why Origin? — Before browser DevTools had a dedicated Network tab with
   throttling, developers debugging `XMLHttpRequest`-based code had to rely
   on manual `console.log` timestamps or external proxy tools to see request
   timing. Describe concretely what was hard to diagnose without a visual
   waterfall (e.g. distinguishing "slow DNS" from "slow server processing"
   from "slow download").
3. When to Use? — Describe a concrete production scenario where you would
   deliberately throttle your network in DevTools before shipping a feature.
4. What if Absent? — If a team never tested under throttled/slow network
   conditions, what concrete disaster would ship to real users on poor
   mobile connections (e.g. what would a fetch-dependent UI look like for 5+
   seconds)?

Shallow answers like "for async processing" will be rejected — reference the
actual runtime mechanics.

## Part 2 — Investigative Task (No Solution Code Required)

This exercise has no `solution.js`. It is a documented investigation.

1. Open Chrome DevTools, go to the **Network** tab.
2. Set throttling to **Slow 3G** (in the throttling dropdown, or Network
   Conditions panel).
3. In the DevTools **Console**, run a `fetch()` call against
   `https://jsonplaceholder.typicode.com/posts/1` (or any endpoint from your
   Exercise 1/2 work) and inspect the resulting entry in the Network tab.
4. Click into that request's **Timing** tab and read the waterfall.
5. Reload the page (with the same request firing again, e.g. from a script
   tag or by re-running your fetch) once normally, and once with "Disable
   cache" unchecked, to observe cached responses.

## Part 3 — Requirements & Edge Cases

Your `ANSWER.md` must document, with specifics (actual millisecond numbers
from your own DevTools session, not hypothetical placeholders):

- [ ] The full timing waterfall breakdown for one throttled request: DNS
      Lookup, Initial Connection (and SSL if applicable), Time to First Byte
      (Waiting/TTFB), and Content Download — with the approximate duration of
      each phase as shown in the Timing tab.
- [ ] The HTTP status DevTools shows for the request in the Network tab's
      Status column, compared against what your JavaScript `fetch()` code
      observes on `response.status`/`response.ok` for the same request —
      explain whether/when these can differ from what a naive read of the
      Network tab alone would suggest (e.g. a request that DevTools marks
      "finished" but that your code still must check manually per Exercise 1).
- [ ] At least one observed entry showing `(disk cache)` or `(memory cache)`
      in the Size column after a reload, with an explanation of what each
      means (where the response came from, whether a network request was
      sent at all, and how this affects timing shown in the waterfall for
      that entry versus the original uncached request).
- [ ] A comparison of total request time for the same endpoint under Slow 3G
      versus no throttling (two actual numbers from your own testing).

## Submission

Write your investigation in `workspace/track-6-fetch-devtools/03-network-tab-throttling/`.
Include `NOTES.md` (Part 1 answers) and `ANSWER.md` (investigation findings —
no `solution.js` needed for this exercise).
