# [Track 4] Exercise 3: Promise.allSettled

## Difficulty
Medium

## Learning Goal
Understand that `Promise.allSettled` waits for every input Promise to settle (regardless of outcome) and resolves with an array of `{ status, value }` or `{ status, reason }` result objects — never itself rejecting because one of the inputs rejected.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why is a primitive that never fails outright (unlike `Promise.all`) useful — what problem does "give me every outcome, good or bad" solve that fail-fast semantics cannot?
2. Why Origin? — Reference a real distributed/parallel-request scenario (e.g. sending the same batch job to 10 workers, or pinging multiple third-party APIs for a dashboard) where you need to know the outcome of every single request, including which ones failed and why, rather than aborting on the first failure.
3. When to Use? — Describe a concrete production scenario where `Promise.allSettled` is clearly the right choice over `Promise.all`.
4. What if Absent? — Before `Promise.allSettled` existed (or if you didn't know about it), what workaround would you be forced into (e.g. wrapping every Promise in your own then/catch to normalize outcomes), and what's the disaster/pain if you forget to do that consistently — e.g. one unhandled rejection in the batch takes down the calling code with `Promise.all`?

## Part 2 — Coding Task
Build at least 5 mock async operations (using `setTimeout`) with a genuine mix of outcomes: at least 2 that resolve, and at least 2 that reject, with staggered delays.

1. Run them all through `Promise.allSettled`, and log the raw resulting array to see its exact shape (`{status: 'fulfilled', value: ...}` / `{status: 'rejected', reason: ...}`).
2. Write your own logic (do not use any third-party library) that processes this results array and separates it into two arrays: one of just the fulfilled values, one of just the rejection reasons. Log both separately.
3. Extend your separation logic to also report simple aggregate stats: total count, success count, failure count.

## Part 3 — Requirements & Edge Cases
- [ ] At least 2 fulfilled and at least 2 rejected operations included in the batch, with delays staggered so they don't all settle in input order.
- [ ] The raw `Promise.allSettled` output is logged and its shape (`status` + `value`/`reason` keys) is confirmed against your prediction in `NOTES.md` before you write the separation logic.
- [ ] Your separation logic correctly distinguishes `status === 'fulfilled'` vs `status === 'rejected'` — do not rely on the presence/absence of `value`/`reason` alone, since that's fragile.
- [ ] Aggregate counts (total/success/failure) are correct and computed from the results array itself, not hardcoded from what you already know you put in.
- [ ] Confirm and note explicitly: `Promise.allSettled` itself never rejects, no matter how many of its inputs reject — demonstrate this by wrapping the whole call in a `.catch()` that you prove never fires.

## Submission
Write your answer/code in `workspace/track-4-promise-concurrency/03-promise-allsettled/`. Include `NOTES.md` (Part 1 answers) and `solution.js`.
