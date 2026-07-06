# [Track 3] Exercise 4: Promisify a Callback-Based (Node-Style) Function

## Difficulty
Hard

## Learning Goal
Understand how to wrap a legacy Node-style callback API — a function whose last argument is a callback shaped `(err, data) => {}` — into a function that returns a Promise, and write this wrapping (`promisify`) generically enough to work for any function of that shape.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — Understand Before You Code
Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. Why Necessary? — Why can't you just `await` a function that takes a Node-style `(err, data) => {}` callback directly? What specifically about its shape makes it incompatible with `await`/`.then()`?
2. Why Origin? — Explain how the Node-style `(err, data)` callback convention itself was a partial solution to earlier ad hoc error handling, but still led to Callback Hell / Pyramid of Doom when multiple such calls were nested — and how wrapping them in Promises addresses the inversion-of-control problem (you no longer have to trust every callback-based API to call your callback correctly).
3. When to Use? — Describe a concrete production scenario where you'd need to `promisify` a legacy or third-party callback-based API to use it inside a modern `async`/`await` codebase.
4. What if Absent? — If you mixed unpromisified callback-based code directly into an otherwise Promise-based/`async` codebase without wrapping it, what specific bugs or structural problems would show up (e.g. broken chains, difficulty combining with `Promise.all`, reintroduced nesting)?

## Part 2 — Coding Task
This is a two-part coding task.

### Part 2a — Build the mock legacy API
Write your own fake Node-style callback function, `readFileMock(path, callback)`, that:
- Simulates asynchronous work using `setTimeout` (do not make it actually touch the filesystem — it's a mock).
- Calls `callback(null, someFakeContentString)` on "success" — you decide what determines success (e.g. certain `path` values).
- Calls `callback(new Error('some message'))` on "failure" for at least one input case (e.g. a path that doesn't exist in your mock data).
- Never calls the callback more than once for a single invocation.

### Part 2b — Write a generic `promisify`
Write a function `promisify(fn)` that:
- Accepts any function `fn` whose *last* parameter is a Node-style `(err, data) => {}` callback, and whose preceding parameters are arbitrary arguments (i.e. it must work for functions with 1 argument + callback, 2 arguments + callback, etc. — do not hardcode it to `readFileMock`'s exact signature).
- Returns a **new function** that, when called with the same arguments (minus the callback), returns a Promise.
- That Promise must resolve with `data` when the callback is invoked as `callback(null, data)`.
- That Promise must reject with the error when the callback is invoked as `callback(err)` (with a truthy first argument).

Test your `promisify` against `readFileMock`, calling the promisified version with both a path that succeeds and a path that fails, using `.then()`/`.catch()` to observe the outcome.

## Part 3 — Requirements & Edge Cases
- [ ] `readFileMock` is genuinely asynchronous (uses `setTimeout`, even with a delay of 0) — it must not call its callback synchronously.
- [ ] `promisify` works with functions that take a *variable* number of leading arguments before the callback (test with at least one function of a different arity than `readFileMock`, even a second small mock you write, to prove generality).
- [ ] The success path: promisified call resolves with exactly the `data` value passed to `callback(null, data)`.
- [ ] The failure path: promisified call rejects with exactly the `error` value passed to `callback(err)`.
- [ ] `promisify` itself does not swallow synchronous throws from within `fn` — consider (and note in `NOTES.md`) what happens if `fn` throws synchronously before ever scheduling the callback, and whether your implementation handles or ignores that case.
- [ ] No use of Node's built-in `util.promisify` — the entire point of this exercise is to implement the mechanism yourself.

## Submission
Write your answer/code in `workspace/track-3-promises/04-promisify-callback-api/`. Include `NOTES.md` (Part 1 answers plus the synchronous-throw analysis) and `solution.js`.
