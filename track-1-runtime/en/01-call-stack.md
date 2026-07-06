# [Track 1] Exercise 1: Call Stack & Stack Overflow

## Difficulty
Easy

## Learning Goal
Understand how the JavaScript Call Stack grows and unwinds via LIFO (Last-In-First-Out) frame management, and precisely what condition triggers a `RangeError: Maximum call stack size exceeded`.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Glossary/Call_stack
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions#recursion
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RangeError

## Part 1 — Understand Before You Code

Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. **Why Necessary?** — Why does JavaScript need a Call Stack at all, given that it executes on a single thread? What would happen to function execution order without one?
2. **Why Origin?** — What historical/technical limitation in how functions are executed (frame management, return addresses, recursion depth) led engines to implement a fixed-size stack rather than an unbounded one?
3. **When to Use?** — Describe a real scenario where an engineer must think carefully about call stack depth (e.g. deep recursion over a tree, JSON parsing, recursive DOM traversal).
4. **What if Absent?** — If there were no stack size limit at all, what concrete disaster could occur on a real machine when a recursive function has a bug (e.g. missing base case)?

## Part 2 — Prediction Quiz or Coding Task

Write a recursive function `countDown(n)` that logs `n` and then calls itself with `n - 1`, with **no base case** (or a base case that never triggers, e.g. `n !== -999999999`).

Before running it, answer in `ANSWER.md`:
- Roughly how many stack frames do you predict it will reach before crashing?
- What error message and error type do you expect to see?
- Will any of your `console.log` output actually get flushed to the console before the crash, or is it lost?

Then actually run it in Node or a browser console, observe the real output and error, and record what really happened.

## Part 3 — Requirements & Edge Cases

- [ ] The recursive function must have no working base case (so it genuinely overflows).
- [ ] Capture the exact error name and message text from your runtime.
- [ ] Note whether the observed max depth differs between Node.js and your browser's console (they use different engines/limits — Node and Chrome both use V8, but limits can still vary by call stack configuration).
- [ ] Explain, using the term "stack frame," why each recursive call adds a fixed amount of memory before the function even executes its body.
- [ ] Explain why the stack unwinds in LIFO order once a `return` (or a thrown error) occurs.

## Submission
Write your answer/code in `workspace/track-1-runtime/01-call-stack/`. Include `NOTES.md` (Part 1 answers) and `ANSWER.md` (your predicted crash point + reasoning, then the real observed result and whether you were right).
