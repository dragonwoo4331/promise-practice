# Track 1: JS Runtime Basics

This track covers the foundational execution model of JavaScript: the Call Stack, the Heap, and the Web APIs layer that makes asynchronous behavior possible on a single thread. You must understand these mechanics precisely before moving on to Promises and the Event Loop in later tracks — guessing at "async magic" without understanding the stack/heap/Web-API split will produce bugs you cannot debug.

## Exercises

1. [01 — Call Stack & Stack Overflow](./01-call-stack.md) — Easy. Recursion, LIFO frame unwinding, and `RangeError: Maximum call stack size exceeded`.
2. [02 — Memory Heap & Reference vs Value](./02-heap-reference-vs-value.md) — Easy. Primitives vs objects, mutation visibility, and garbage collection reachability.
3. [03 — Web APIs & Proof of Single-Threaded Execution](./03-web-apis-single-threaded.md) — Medium. Blocking the main thread, `setTimeout` scheduling, and observing UI freezing.

## Reference

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop

Submit all work under `workspace/track-1-runtime/<exercise-folder>/` as specified in each exercise file.
