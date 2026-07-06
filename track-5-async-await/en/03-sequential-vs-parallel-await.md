# [Track 5] Exercise 3: Sequential vs. Parallel `await`

## Difficulty
Medium

## Learning Goal
Measure and explain the real time-cost difference between awaiting
independent asynchronous operations one at a time versus starting them
concurrently and awaiting them together with `Promise.all`.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
- https://developer.mozilla.org/en-US/docs/Web/API/Performance/now

## Part 1 — Understand Before You Code

Answer in your own words in `NOTES.md`, **before** writing any code:

1. Why Necessary? — Why does writing `await op1(); await op2(); await op3();`
   for three *independent* operations cost more wall-clock time than starting
   all three first and then awaiting them? Explain what each `await` is
   actually blocking (the async function's continuation) and why that
   matters when the three operations don't depend on each other's results.
2. Why Origin? — Before `Promise.all`, achieving concurrent execution of
   multiple asynchronous operations with plain `.then()` chains or callbacks
   required manually tracking completion counts (e.g. counting callbacks
   until all have fired). Describe concretely what made this manual
   bookkeeping error-prone.
3. When to Use? — Describe a concrete production scenario where converting
   three sequential `await` calls into a `Promise.all`-based parallel version
   would produce a measurable, user-facing improvement.
4. What if Absent? — If a developer never learned this distinction and always
   awaited independent operations sequentially, what concrete cost would this
   have on a page or endpoint that fans out to, say, 5 independent network
   calls?

Shallow answers like "for async processing" will be rejected — reference the
actual runtime mechanics.

## Part 2 — Coding Task

You are given three independent mock operations, each taking approximately
1 second:

```js
function fetchUserProfile() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ name: "Ada" }), 1000);
  });
}

function fetchUserSettings() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ theme: "dark" }), 1000);
  });
}

function fetchUserNotifications() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ unread: 3 }), 1000);
  });
}
```

1. Write an `async` function `loadDashboardSequential()` that awaits all
   three calls one after another (in any order), logs the total elapsed time
   using `performance.now()` (or `Date.now()` if running under plain Node
   without `performance` available — check which applies to your runtime),
   and returns a combined object `{ profile, settings, notifications }`.
2. Write a second `async` function `loadDashboardParallel()` that starts all
   three calls at the same time and uses `Promise.all` with a single `await`
   to wait for them together, logs the total elapsed time the same way, and
   returns the same combined shape.
3. Call both functions and log both elapsed times so they can be compared
   directly.

## Part 3 — Requirements & Edge Cases

- [ ] `loadDashboardSequential` measurably takes roughly 3x longer than
      `loadDashboardParallel` when both are run (log actual numbers, not
      estimates).
- [ ] `loadDashboardParallel` actually starts all three underlying
      `setTimeout`-based operations before any of them has resolved — explain
      in `NOTES.md` how you confirmed this (e.g. via timing, or via adding
      temporary logs at call time vs. resolve time).
- [ ] Both functions return the identical combined shape
      `{ profile, settings, notifications }` with correct values.
- [ ] Elapsed time is measured with `performance.now()`/`Date.now()` around
      the actual awaited work, not including unrelated setup code.
- [ ] `NOTES.md` explains, in terms of the underlying Promise objects, why
      `Promise.all([p1, p2, p3])` does not make the operations run "more
      parallel" than calling all three functions without awaiting each —
      i.e. what `Promise.all` is actually doing versus what starts the
      concurrent work in the first place.

## Submission

Write your answer/code in `workspace/track-5-async-await/03-sequential-vs-parallel-await/`.
Include `NOTES.md` and `solution.js`.
