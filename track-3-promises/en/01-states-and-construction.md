# [Track 3] Exercise 1: Promise States & Basic Construction

## Difficulty
Easy

## Learning Goal
Understand the three states of a Promise (Pending, Fulfilled, Rejected), the fact that a Promise's state transition is a one-way, one-time event, and why calling `resolve`/`reject` more than once — or never — has real consequences.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why does JavaScript need a formal "Pending / Fulfilled / Rejected" state machine at all, instead of just letting a callback fire whenever the async operation finishes?
2. Why Origin? — Explain how Promises emerged as a response to Callback Hell / the Pyramid of Doom and the inversion-of-control problem (where you hand your callback to someone else's function and must trust them to call it correctly, exactly once, with the right arguments).
3. When to Use? — Describe a concrete production scenario where you would construct a `new Promise(...)` directly (as opposed to just consuming a Promise already returned by a library, e.g. `fetch`).
4. What if Absent? — What concretely breaks in a real application if a Promise is created but `resolve`/`reject` is never called under some code path?

## Part 2 — Coding Task
Write a small script (not a function you need to export anywhere fancy — a plain Node/browser-console script is fine) that constructs the following Promises using `new Promise((resolve, reject) => { ... })`:

1. A Promise that resolves asynchronously (use `setTimeout`) with a value of your choice. Attach a `.then()` to log the result.
2. A Promise that rejects asynchronously (use `setTimeout`) with an `Error`. Attach a `.catch()` to log the reason.
3. A Promise that **never settles** — neither `resolve` nor `reject` is ever called. Attach a `.then()` and `.catch()` to it anyway. Run the script and observe what happens (or doesn't happen).
4. A Promise whose executor calls `resolve('first')` and then, immediately after, also calls `resolve('second')` (both synchronously, in the same executor). Attach a `.then()` that logs the resolved value.
5. A variant of (4) where the executor calls `resolve('first')` and then `reject(new Error('second'))`. Attach both `.then()` and `.catch()`.

Do not just write the code and move on — in `NOTES.md`, also record, for cases 3, 4, and 5:
- What you predicted would happen before running it.
- What actually happened when you ran it.
- Why the spec behaves this way (in your own words, referencing the internal "already settled" flag a Promise holds).

## Part 3 — Requirements & Edge Cases
- [ ] All three states (Pending, Fulfilled, Rejected) are demonstrated by at least one Promise each.
- [ ] The "never settles" Promise is included, and `NOTES.md` explains at least one concrete danger of a Promise that hangs forever (e.g. `await`ing it blocks the calling `async` function forever; resource leaks; a UI stuck in a loading state).
- [ ] The "double resolve" case is included and your notes state explicitly, correctly, and with reasoning: only the **first** call to `resolve`/`reject` has any effect; every subsequent call is silently ignored.
- [ ] The "resolve then reject" case is included and your notes confirm the same "first call wins" rule applies across `resolve` and `reject`, not just within calls to the same function.
- [ ] No unhandled promise rejection warnings are left in your console output for cases you intended to handle (case 3's rejection handler will simply never fire — that's expected and fine).

## Submission
Write your answer/code in `workspace/track-3-promises/01-states-and-construction/`. Include `NOTES.md` (Part 1 answers, plus the predictions/observations required in Part 2) and `solution.js`.
