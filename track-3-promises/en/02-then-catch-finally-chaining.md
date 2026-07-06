# [Track 3] Exercise 2: .then() / .catch() / .finally() Chaining

## Difficulty
Easy

## Learning Goal
Understand that each `.then()` call returns a **new** Promise, that returning a plain value vs. returning a Promise from inside a `.then()` callback behaves differently (the latter is "flattened" / assimilated rather than nested), and how `.finally()` fits into a chain regardless of outcome.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why does `.then()` need to return a brand-new Promise each time, instead of mutating and returning the same Promise object it was called on?
2. Why Origin? — Explain how chained `.then()` calls solve the Callback Hell / Pyramid of Doom problem that arises when you nest callback-based async calls inside each other, each one indented further than the last.
3. When to Use? — Describe a concrete production scenario involving a sequence of dependent async steps (e.g. authenticate, then fetch profile, then fetch permissions) where a `.then()` chain is the natural fit.
4. What if Absent? — If you did not know that returning a Promise from `.then()` gets flattened, what specific mistake would you make when composing async steps, and what bug would result?

## Part 2 — Coding Task
Build a single `.then()` chain, starting from an initial Promise, that includes **at least 4 `.then()` calls**, where:

- At least one `.then()` callback returns a plain (non-Promise) value.
- At least one `.then()` callback returns a **new Promise** (e.g. `new Promise(resolve => setTimeout(() => resolve(...), someDelay))`), not just a value.
- The chain ends with a `.finally()` that logs something, followed by a final `.then()` after the `.finally()` that logs the value received (to demonstrate that `.finally()` does not change the resolved value passing through it).

Before running it, write in `ANSWER.md`:
- Your prediction of the exact sequence of logged output, in order.
- Your reasoning for why the Promise returned from the middle `.then()` does not produce a nested `Promise<Promise<value>>` structure — i.e., explain what the Promise machinery does internally when a `.then()` callback returns a thenable.

Then run it, compare against your prediction, and record whether you were right, and correct your understanding in `ANSWER.md` if you were wrong.

## Part 3 — Requirements & Edge Cases
- [ ] Chain has 4 or more `.then()` calls (not counting `.catch()`/`.finally()`).
- [ ] At least one `.then()` returns a plain value; at least one returns a Promise (constructed with a delay via `setTimeout`, so the flattening behavior is actually observable in timing, not just in value).
- [ ] A `.finally()` is present and does not receive or alter the resolved value (demonstrate this by chaining a `.then()` after it).
- [ ] `ANSWER.md` explicitly states, in the student's own words, why `.then(() => somePromise)` results in a single flattened Promise rather than a Promise wrapping a Promise.
- [ ] No use of `async`/`await` anywhere in this exercise — pure `.then()`/`.catch()`/`.finally()` chaining only, since `async`/`await` is covered in a different track and would defeat the purpose here.

## Submission
Write your answer/code in `workspace/track-3-promises/02-then-catch-finally-chaining/`. Include `NOTES.md` (Part 1 answers), `solution.js`, and `ANSWER.md` (your predicted order + reasoning, then verify by running it and note whether you were right).
