# [Track 1] Exercise 3: Web APIs & Proof of Single-Threaded Execution

## Difficulty
Medium

## Learning Goal
Prove experimentally that JavaScript's main thread is single-threaded and that `setTimeout` does not guarantee execution at the specified delay — it only guarantees the callback is queued no earlier than that delay, and it must wait for the Call Stack to be empty and for any synchronous code (including a blocking `while` loop) to finish first.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/setTimeout
- https://developer.mozilla.org/en-US/docs/Glossary/Main_thread
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop

## Part 1 — Understand Before You Code

Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. **Why Necessary?** — Why must timer scheduling (`setTimeout`) be handled outside of the JS engine itself (i.e. by Web APIs provided by the browser, not by the Call Stack) given that JS runs on a single thread?
2. **Why Origin?** — What historical problem (UI Freezing, unresponsive pages, the "a script on this page is busy" browser dialog) motivated the separation between the JS execution thread and the browser's timer/rendering mechanisms?
3. **When to Use?** — Describe a real production scenario where a developer accidentally blocks the main thread with a long synchronous loop (e.g. sorting a huge array, a heavy synchronous JSON.parse, an unoptimized image-processing loop) and the user-visible consequence.
4. **What if Absent?** — If the browser had no separate mechanism for Web APIs / Task Queue and everything ran strictly synchronously on one stack with no callback queue at all, what would happen to `setTimeout`, click handlers, and network requests?

## Part 2 — Prediction Quiz or Coding Task

Write a script (to run in a real browser tab, not just Node) that does the following, in this order:

```js
console.log("start");

setTimeout(() => {
  console.log("timeout fired");
}, 0);

const end = Date.now() + 3000; // block for ~3 seconds
while (Date.now() < end) {
  // intentionally busy-blocking the main thread
}

console.log("end of synchronous block");
```

Before running it, answer in `ANSWER.md`:
- In what order will `"start"`, `"timeout fired"`, and `"end of synchronous block"` be logged?
- The `setTimeout` delay is `0`. Will the callback fire immediately when the timer "expires," or will it be delayed? If delayed, by roughly how long, and why?
- While the `while` loop is running, can you click a button on the page? Why or why not?

Then actually run it in a real browser tab with a visible button on the page (e.g. a plain HTML file with a `<button onclick="...">Click me</button>`). Try clicking the button while the loop runs. Record what you observed.

## Part 3 — Requirements & Edge Cases

- [ ] Run this in an actual browser (not just Node) so you can observe UI freezing — attempting to click a button, scroll, or type during the blocking loop.
- [ ] Record the actual wall-clock delay between scheduling the timeout and its callback firing, and compare it to the requested `0`ms.
- [ ] Explain, using the terms "Web APIs," "Task Queue" (macrotask queue), and "Call Stack," exactly why the timeout callback cannot run until after the `while` loop and the `console.log("end of synchronous block")` line complete.
- [ ] Note what happens to other events during the freeze (e.g. does a click registered during the freeze get handled immediately after, or is it lost/queued?).
- [ ] State explicitly whether this experiment proves JS is single-threaded, or only proves that the Web API layer is separate from the JS thread — justify your answer.

## Submission
Write your answer/code in `workspace/track-1-runtime/03-web-apis-single-threaded/`. Include `NOTES.md` (Part 1 answers), `ANSWER.md` (your prediction + reasoning), and `solution.js` (or an `index.html` + script) plus a short observation log of what you actually saw in the browser.
