# [Track 4] Exercise 4: Promise.any

## Difficulty
Hard

## Learning Goal
Understand that `Promise.any` resolves as soon as **any one** input Promise fulfills (ignoring rejections along the way), and only rejects if **every** input rejects — in which case it rejects with an `AggregateError` whose `.errors` property is an array of all the individual rejection reasons, in input order.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AggregateError
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why is a primitive that ignores individual rejections and only cares about the first success useful — what does it give you that `Promise.race` or `Promise.all` cannot?
2. Why Origin? — Reference a real distributed/parallel-request scenario (e.g. querying several redundant/mirrored API endpoints or CDNs and being satisfied with the first one that actually responds successfully, tolerating some of them being down) that motivated `Promise.any`'s existence.
3. When to Use? — Describe a concrete production scenario where `Promise.any` is clearly the right choice over `Promise.race` (be specific about why `race` would be wrong there — hint: `race` would settle on a fast failure even if a success is coming right behind it).
4. What if Absent? — If you didn't know `Promise.any` existed and needed "first success, tolerate some failures" semantics, what more complicated workaround would you have to hand-roll, and what mistake would a junior engineer likely make in that workaround (e.g. accidentally using `Promise.race` instead and getting bitten by an early failure)?

## Part 2 — Coding Task
Build at least 4 mock async operations (using `setTimeout`) with a mix of outcomes and delays such that:

- At least 2 reject.
- At least 1 eventually fulfills (its delay can be longer than some of the rejecting ones — that's the point).

1. Run them through `Promise.any` and confirm it resolves with the value of the fulfilling operation, even though one or more rejections happened first chronologically.
2. Build a **second, separate scenario** where **all** of the input operations reject. Run this batch through `Promise.any`, catch the result, and confirm/observe that the caught error is an instance of `AggregateError`.
3. Inspect and log the `.errors` property of that `AggregateError` — confirm it is an array, confirm its length matches the number of operations you ran, and confirm the order of the reasons inside it corresponds to the input order of your operations (not the order in which they actually rejected).

## Part 3 — Requirements & Edge Cases
- [ ] The "eventually succeeds" scenario includes at least one rejection that settles chronologically *before* the eventual success, proving `Promise.any` ignores it and keeps waiting.
- [ ] The "all reject" scenario is a separate, distinct batch of operations (do not reuse the mixed batch and expect it to happen to work).
- [ ] You explicitly check `error instanceof AggregateError` (or equivalent) and log the result of that check.
- [ ] You explicitly log and inspect `error.errors`, confirming it is an array whose length equals the number of rejecting inputs.
- [ ] `NOTES.md` states clearly, in your own words, the one condition under which `Promise.any` rejects: only when every single input has rejected.

## Submission
Write your answer/code in `workspace/track-4-promise-concurrency/04-promise-any/`. Include `NOTES.md` (Part 1 answers) and `solution.js`.
