# Track 3: Promises Deep Dive

This track covers the `Promise` object itself: its internal state machine, how `.then()`/`.catch()`/`.finally()` chains actually behave, how errors propagate (and can be recovered from) across a chain, and how to bridge legacy callback-based APIs into Promise-based code. This track assumes you already understand the JS runtime and event loop (Tracks 1–2). It deliberately avoids `async`/`await` syntax — that's Track 5 — so you build a solid mental model of the underlying Promise mechanics first.

## Exercises

1. [States & Basic Construction](./01-states-and-construction.md) — Pending/Fulfilled/Rejected, constructing Promises with `new Promise(...)`, and what happens when `resolve`/`reject` is called twice or never.
2. [.then() / .catch() / .finally() Chaining](./02-then-catch-finally-chaining.md) — building multi-step chains and understanding Promise flattening.
3. [Error Propagation Across a Chain](./03-error-propagation.md) — how thrown errors skip to the nearest `.catch()`, and how a chain can recover afterward.
4. [Promisify a Callback-Based Function](./04-promisify-callback-api.md) — wrapping a Node-style `(err, data) => {}` API into a generic Promise-returning function.

## Reference

- [MDN: Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
