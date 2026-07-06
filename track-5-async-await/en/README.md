# Track 5 — Async/Await Syntactic Sugar

`async/await` is syntax built on top of Promises, not a separate concurrency
mechanism. This track drills the mental model needed to use it correctly:
what an `async` function actually returns, why `await` only suspends the
current function's execution context, how `try...catch` interacts with
rejected Promises, and the difference in wall-clock cost between sequential
and parallel `await`.

Read the MDN track landing page first:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function

## Exercises

1. [01 — Converting a `.then()` Chain to `async/await`](./01-then-to-async-await.md)
   Rewrite a chained `.then()` mock API sequence as `async/await` and explain
   why `await` pauses only the async function, not the whole Call Stack.
2. [02 — `try...catch` Scope and the `forEach` Trap](./02-try-catch-and-foreach-trap.md)
   Handle rejections from multiple sequential `await` calls correctly, and
   discover firsthand why `await` inside `Array.prototype.forEach` does not
   work as expected.
3. [03 — Sequential vs. Parallel `await`](./03-sequential-vs-parallel-await.md)
   Measure the real time cost of awaiting three independent operations one at
   a time versus running them concurrently with `Promise.all`.

See the repository root `README.md` for submission rules (no AI, `NOTES.md`
before code, push per track).
