# [Track 4] Exercise 1: Promise.all

## Difficulty
Medium

## Learning Goal
Understand that `Promise.all` runs multiple Promises concurrently and resolves with an array of results in input order only if **all** of them fulfill, but fails fast — rejecting immediately with the first rejection reason — discarding the outcomes of any still-pending or already-fulfilled promises in the batch.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why do you need a dedicated concurrency primitive like `Promise.all` instead of just awaiting several Promises one after another in sequence?
2. Why Origin? — `Promise.all` exists because real systems routinely need to issue multiple independent network/database/disk requests at once (e.g. fetching a user's profile, settings, and notifications in parallel) rather than one at a time. Explain, referencing a real distributed/parallel-request scenario, why sequential awaiting would be unacceptable there.
3. When to Use? — Describe a concrete production scenario where you'd use `Promise.all` specifically (as opposed to `allSettled`, `race`, or `any`) — i.e. a case where you genuinely need *all* results and would have no use for a partial result set.
4. What if Absent? — If you didn't know about the fail-fast behavior of `Promise.all` and assumed it would give you whatever results *did* succeed alongside an error for the one that failed, what production bug would you write, and what would actually happen instead?

## Part 2 — Coding Task
Write mock async operations — functions that return Promises and simulate latency with `setTimeout` — and use them with `Promise.all`.

1. Build at least 3 mock operations that all eventually resolve (with different delays), pass them to `Promise.all`, and log the resulting array. Confirm the order of the results array matches the order you passed the Promises in, **not** the order in which they actually finished.
2. Build a second variant where one of the 3+ operations rejects (with a delay somewhere in the middle of the pack — not necessarily first or last to fire). Predict, then observe:
   - What value/reason the `.catch()` (or `try/catch` if you use `await Promise.all(...)`) receives.
   - Whether the results from the operations that *did* successfully resolve are available to you anywhere, or whether they are effectively "lost" from the perspective of `Promise.all`'s own return value.
3. Add a log statement inside each mock operation's `.then()` (or right after its internal `setTimeout` fires) so you can observe, in the console, that all operations continued running to completion even after `Promise.all` had already rejected — i.e. rejecting fast does not cancel the other in-flight operations.

## Part 3 — Requirements & Edge Cases
- [ ] At least 3 concurrent mock operations, with staggered delays (not all the same delay), used in the all-succeed variant.
- [ ] A separate all-fail-fast variant where exactly one operation rejects and the rejection is not the first one to settle chronologically.
- [ ] Your notes/output explicitly show that `Promise.all`'s rejection happens as soon as the rejecting Promise settles, without waiting for the still-pending ones.
- [ ] Your notes/output explicitly confirm that the fulfilled values from the non-rejecting operations are not present anywhere in the rejection reason `Promise.all` produces — you only get the one rejection reason.
- [ ] Demonstrate (via logging) that the other mock operations still complete their own work in the background even though `Promise.all` has already rejected — clarify in `NOTES.md` that `Promise.all` does not cancel or abort its inputs.

## Submission
Write your answer/code in `workspace/track-4-promise-concurrency/01-promise-all/`. Include `NOTES.md` (Part 1 answers) and `solution.js`.
