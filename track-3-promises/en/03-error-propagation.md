# [Track 3] Exercise 3: Error Propagation Across a Chain

## Difficulty
Medium

## Learning Goal
Understand that a thrown error (or a rejected Promise) inside a `.then()` chain skips forward to the nearest `.catch()`, bypassing any intermediate `.then()` handlers, and that a `.catch()` itself returns a new (fulfilled, by default) Promise — meaning the chain can "recover" and continue afterward unless the `.catch()` handler explicitly rethrows.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why does a single `.catch()` at the end of a chain need to be able to intercept an error thrown from *any* earlier point in the chain, rather than requiring a `try/catch`-equivalent around every individual step?
2. Why Origin? — Explain how this "skip to the nearest handler" behavior addresses a specific pain point of Callback Hell, where every individual callback in a nested pyramid had to check for its own `err` argument and handle it (or forget to).
3. When to Use? — Describe a concrete production scenario with a multi-step chain (e.g. validate input, then call an API, then persist to a database) where centralizing error handling in one trailing `.catch()` is preferable to handling errors at every step.
4. What if Absent? — If a chain had no `.catch()` at all and one of the `.then()` steps threw, what would actually happen at runtime (in Node.js and in a browser), and what disaster could that cause in production (e.g. silent failures, crashed processes)?

## Part 2 — Coding Task
Build a `.then()` chain with the following exact structure and behavior:

1. An initial Promise that resolves.
2. At least two `.then()` calls that each transform the value normally.
3. A third `.then()` that deliberately `throw`s an `Error` partway through the chain.
4. One or more additional `.then()` calls positioned **after** the throwing step but **before** the `.catch()` — these must never execute, and you must prove/observe that they don't.
5. A single trailing `.catch()` that catches the error.
6. A `.then()` positioned **after** that `.catch()`, which logs whatever value it receives.

Once that variant works and you've recorded your findings, write a **second variant** in the same file: change the `.catch()` handler so that it **rethrows** the error (or throws a new one) instead of swallowing it, and add a second, final `.catch()` after that to catch the rethrown error. Predict, then verify, whether the `.then()` that follows the first `.catch()` (before the second `.catch()`) executes in this variant.

In `NOTES.md`, explicitly answer: does execution "recover" and continue normally after a `.catch()` handles an error (assuming it doesn't rethrow)? What is the resolved value of the Promise returned by a `.catch()` that doesn't rethrow, if the handler doesn't explicitly return anything?

## Part 3 — Requirements & Edge Cases
- [ ] The `.then()` calls positioned between the throw and the `.catch()` are proven not to execute (e.g. via a log statement that never appears in your output).
- [ ] The trailing `.then()` after the (non-rethrowing) `.catch()` executes and logs a value, demonstrating recovery.
- [ ] A second variant exists where the `.catch()` rethrows, and a subsequent `.then()` (if any, between the two catches) is proven not to execute, while the final `.catch()` does catch the rethrown error.
- [ ] `NOTES.md` states precisely what value flows into the `.then()` after a non-rethrowing `.catch()` when the catch handler has no explicit `return`.
- [ ] Errors are constructed as actual `Error` objects (e.g. `new Error('message')`), not raw strings or other primitives — explain in one sentence why using `Error` objects specifically matters (hint: stack traces).

## Submission
Write your answer/code in `workspace/track-3-promises/03-error-propagation/`. Include `NOTES.md` (Part 1 answers plus the recovery/rethrow analysis) and `solution.js`.
