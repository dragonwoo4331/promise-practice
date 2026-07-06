# [Track 4] Exercise 2: Promise.race

## Difficulty
Medium

## Learning Goal
Understand that `Promise.race` settles — as either fulfilled or rejected — as soon as **any** one of its input Promises settles, regardless of whether that first-to-settle Promise succeeded or failed. `Promise.race` does not mean "first to succeed."

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why would you need a primitive that settles on the first-to-finish Promise, instead of always waiting for a specific one or for all of them?
2. Why Origin? — Reference a real distributed/parallel-request scenario (e.g. implementing a request timeout by racing a real network call against a `setTimeout`-based timer Promise, or querying redundant mirrored servers and taking whichever responds first) that motivated `Promise.race`'s existence.
3. When to Use? — Describe a concrete production scenario where `Promise.race` is the right tool (be specific — e.g. timeout patterns).
4. What if Absent? — A junior engineer assumes `Promise.race` always gives you the fastest *successful* result, similar to "first one to finish wins, as long as it's good." What bug would this misconception cause in a timeout-pattern implementation, and what would actually happen if the fast operation happens to fail first?

## Part 2 — Coding Task
Build the following mock async operations, each simulating latency with `setTimeout`:

1. A "slow success": resolves with a value after a longer delay.
2. A "fast failure": rejects with an `Error` after a short delay.
3. Race these two with `Promise.race([slowSuccess(), fastFailure()])`. Predict, then observe, the outcome — is it fulfilled or rejected? What value/reason does it carry?

Then build the mirrored case:

4. A "fast success": resolves with a value after a short delay.
5. A "slow failure": rejects with an `Error` after a longer delay.
6. Race these two. Predict, then observe, the outcome.

Finally, write a **third scenario** demonstrating a realistic use case: race a mock "network request" Promise (resolves after a random or fixed delay) against a "timeout" Promise (rejects after a fixed delay via `setTimeout`), and handle both outcomes (successful response vs. timeout) distinctly in your `.then()`/`.catch()`.

## Part 3 — Requirements & Edge Cases
- [ ] Case 1 (slow success vs. fast failure) is implemented, and `ANSWER.md`/`NOTES.md` explicitly states the outcome was a **rejection**, not a fulfillment — and explains why, in terms of settling order, not "success."
- [ ] Case 2 (fast success vs. slow failure) is implemented, and the outcome (fulfillment) is explained the same way — first to settle, coincidentally the successful one this time.
- [ ] Your notes explicitly call out the common junior misconception: "`Promise.race` returns the first successful result" is **false** — it returns whichever Promise settles first, success or failure.
- [ ] The timeout-pattern scenario is implemented and correctly distinguishes, in your handling code, between "got a real response" and "timed out" as two different resolution paths.
- [ ] All mock Promises that lose the race are still allowed to run to completion in the background (do not attempt cancellation) — note in `NOTES.md` that `Promise.race` does not cancel the losing Promises either.

## Submission
Write your answer/code in `workspace/track-4-promise-concurrency/02-promise-race/`. Include `NOTES.md` (Part 1 answers) and `ANSWER.md` (your predicted order + reasoning for each of the two races, then verify by running it and note whether you were right), plus `solution.js`.
