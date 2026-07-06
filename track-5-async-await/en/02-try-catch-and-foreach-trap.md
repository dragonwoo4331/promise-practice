# [Track 5] Exercise 2: `try...catch` Scope and the `forEach` Trap

## Difficulty
Medium

## Learning Goal
Correctly scope `try...catch` around multiple sequential `await` calls, and
understand precisely why `await` inside a `forEach` callback does not pause
the loop — because `Array.prototype.forEach` ignores the return value of its
callback, silently discarding the Promise the `async` callback returns.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

## Part 1 — Understand Before You Code

Answer in your own words in `NOTES.md`, **before** writing any code:

1. Why Necessary? — Why does `async/await` require `try...catch` for error
   handling instead of `.catch()`? Explain what a `throw` inside an `async`
   function actually does to the Promise that function returns, and how that
   connects to why `try...catch` can intercept it.
2. Why Origin? — Before `async/await`, asynchronous error handling relied on
   `.catch()` chained onto Promises (or, further back, error-first callbacks).
   Describe concretely what was awkward about handling errors from several
   independent asynchronous steps using only `.then()`/`.catch()` — e.g. what
   happens to error handling granularity when you want step 2's failure
   handled differently from step 3's failure, using only chained `.catch()`.
3. When to Use? — Describe a concrete production scenario where you need
   fine-grained `try...catch` blocks around specific `await` calls rather
   than one `try...catch` wrapping an entire function.
4. What if Absent? — If a junior engineer wraps a loop of `await` calls in
   `.forEach()` expecting each iteration to complete before the next starts,
   what concrete disaster happens at runtime? Be specific about ordering and
   about whether errors inside that loop get caught at all.

Shallow answers like "for async processing" will be rejected — reference the
actual runtime mechanics.

## Part 2 — Coding Task

You are given a mock API:

```js
function fetchItem(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id === 3) reject(new Error(`Item ${id} not found`));
      else resolve({ id, value: id * 10 });
    }, 200);
  });
}
```

Write an `async` function `processItems(ids)` that:

1. Iterates over an array of `ids` (e.g. `[1, 2, 3, 4]`).
2. Calls `fetchItem(id)` for each id, one at a time, in order.
3. Logs each successfully fetched item as it arrives.
4. If a given id's fetch rejects, logs an error message identifying which id
   failed, but continues processing the remaining ids (a single failure must
   not abort the whole batch).
5. Returns an array of the successfully fetched items only, in the order
   fetched.

Before writing the correct version, **first deliberately write a broken
version using `ids.forEach(async (id) => { ... })` with `await` inside the
callback**, run it, and observe/log what actually happens (does the function
wait for each `fetchItem` before starting the next? do thrown errors get
caught by an outer `try...catch` wrapping the `forEach` call?). Record your
observations in `NOTES.md`, then implement the corrected version using
`for...of` with `try...catch` inside the loop body.

## Part 3 — Requirements & Edge Cases

- [ ] `NOTES.md` documents the observed (broken) behavior of the `forEach` +
      `async` callback version, including whether iteration order/timing was
      as expected and whether a wrapping `try...catch` caught rejections.
- [ ] `NOTES.md` explains, referencing `Array.prototype.forEach`'s return
      value handling, exactly why the callback's returned Promise is
      discarded and cannot be awaited by the loop.
- [ ] Final `processItems` implementation uses `for...of`, not `forEach`.
- [ ] Each `await fetchItem(id)` is wrapped so that a rejection for one id
      does not stop processing of subsequent ids.
- [ ] The final resolved array contains only the successful items, in fetch
      order, excluding the failed id.
- [ ] Calling `processItems([1, 2, 3, 4])` logs a clear error for id `3` and
      still logs/returns items `1`, `2`, and `4`.

## Submission

Write your answer/code in `workspace/track-5-async-await/02-try-catch-and-foreach-trap/`.
Include `NOTES.md` and `solution.js`.
