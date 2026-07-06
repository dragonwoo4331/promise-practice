# [Track 5] Exercise 1: Converting a `.then()` Chain to `async/await`

## Difficulty
Easy

## Learning Goal
Understand that `async/await` is syntactic sugar over Promises — it does not
introduce a new concurrency model — and be able to convert a `.then()` chain
into an equivalent `async/await` form while preserving execution order and
error propagation.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — Understand Before You Code

Answer in your own words in `NOTES.md`, **before** writing any code:

1. Why Necessary? — What specific problem does `async/await` solve that plain
   `.then()` chaining does not? Be precise about what changes at the syntax
   level versus what changes (if anything) at the runtime/scheduling level.
2. Why Origin? — Long `.then()` chains have a well-known readability problem
   once you add branching, nested error handling, or intermediate variables
   that need to be reused across steps. Describe concretely what goes wrong
   with a `.then()` chain in those situations (not just "it's ugly" — explain
   the actual structural cause, e.g. what happens to variable scope across
   chained `.then()` calls, or how nesting grows when a later step depends on
   two earlier results).
3. When to Use? — Describe a concrete production scenario where converting a
   `.then()` chain to `async/await` is worth doing.
4. What if Absent? — If a codebase never adopted `async/await` and stuck with
   `.then()` chains everywhere, what concrete disaster or maintenance cost
   would show up in a large, long-lived codebase?

Shallow answers like "for async processing" will be rejected — reference the
actual runtime mechanics.

## Part 2 — Coding Task

Convert the following `.then()` chain into an equivalent `async/await`
function. The mock API calls are provided — do not change their behavior,
only the calling code.

```js
function getUser(id) {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ id, name: "Ada" }), 300);
  });
}

function getOrders(userId) {
  return new Promise((resolve) => {
    setTimeout(() => resolve([{ id: 1, total: 42 }]), 300);
  });
}

function getInvoice(orderId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (orderId === 1) resolve({ orderId, amount: 42, paid: true });
      else reject(new Error("Invoice not found"));
    }, 300);
  });
}

function loadUserInvoice(userId) {
  return getUser(userId)
    .then((user) => getOrders(user.id))
    .then((orders) => getInvoice(orders[0].id))
    .then((invoice) => {
      console.log("Invoice:", invoice);
      return invoice;
    })
    .catch((err) => {
      console.error("Failed:", err.message);
      throw err;
    });
}
```

Rewrite `loadUserInvoice` as an `async` function that produces the exact same
observable behavior (same resolved value on success, same thrown/logged error
on failure).

In `NOTES.md`, explain — using the specific example above — why `await
getOrders(user.id)` pauses only the execution of `loadUserInvoice`'s function
body, not the entire JavaScript Call Stack or the browser/Node thread. Explain
what the engine is actually doing with the rest of `loadUserInvoice`'s
suspended work while the `Promise` returned by `getOrders` is pending, and
what would still be able to run on the Call Stack during that time (e.g. a
`setTimeout` callback registered elsewhere, a click handler, etc.). This is a
common junior misconception — treat it as a serious conceptual checkpoint,
not a formality.

## Part 3 — Requirements & Edge Cases

- [ ] `loadUserInvoice` is declared with the `async` keyword and uses `await`
      for each dependent call — no `.then()` remains in your version.
- [ ] A rejected `getInvoice` call is caught with `try...catch` and produces
      the same console output and re-thrown error as the original.
- [ ] The success path logs the invoice exactly as the original did.
- [ ] Calling `loadUserInvoice(1).catch(...)` from outside your function still
      works — i.e. your `async` function still returns a `Promise` that can be
      chained.
- [ ] `NOTES.md` explicitly addresses what remains on the Call Stack while an
      `await` is pending, with reference to this exact example.

## Submission

Write your answer/code in `workspace/track-5-async-await/01-then-to-async-await/`.
Include `NOTES.md` and `solution.js`.
