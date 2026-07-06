# [Track 2] Exercise 1: MacroTask vs MicroTask Ordering Prediction

## Difficulty
Medium

## Learning Goal
Understand that the Event Loop always fully drains the Microtask Queue (Promise `.then`/`.catch`/`.finally`, `queueMicrotask`) before pulling the next MacroTask (`setTimeout`, `setInterval`, I/O) off the Task Queue, and be able to trace exact execution order across synchronous code, microtasks, and macrotasks.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/queueMicrotask
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — Understand Before You Code

Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. **Why Necessary?** — Given that JS has one Call Stack, why must there be a distinction between a Microtask Queue and a (Macro)Task Queue at all, instead of a single undifferentiated queue?
2. **Why Origin?** — What problem with earlier callback-based async code (Callback Hell, inconsistent ordering guarantees) led to Promises being specified with a stronger, more predictable scheduling guarantee (always-before-next-macrotask) than plain `setTimeout` callbacks?
3. **When to Use?** — Describe a real scenario where the ordering guarantee of microtasks (e.g. ensuring a state update finishes before the browser repaints, or before the next timer fires) actually matters for correctness.
4. **What if Absent?** — If Promise callbacks were scheduled as regular macrotasks with no special priority, what concrete ordering bugs or race conditions could appear in code that chains multiple `.then()` calls alongside timers?

## Part 2 — Prediction Quiz or Coding Task

Given this snippet:

```js
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

queueMicrotask(() => {
  console.log("4");
});

Promise.resolve().then(() => {
  console.log("5");
}).then(() => {
  console.log("6");
});

console.log("7");
```

In `ANSWER.md`, predict the exact order in which `1` through `7` are logged, and explain — line by line — why each one lands where it does. Reference specifically: which lines run synchronously, which are queued as microtasks, which are queued as macrotasks, and why chained `.then()` calls (`5` then `6`) are interleaved with other microtasks the way they are.

Do not run the code until after you've written your full prediction and reasoning.

## Part 3 — Requirements & Edge Cases

- [ ] Your written reasoning must explicitly classify every line as "synchronous," "microtask," or "macrotask" before stating the final order.
- [ ] Explain why `queueMicrotask(() => console.log("4"))` and `Promise.resolve().then(() => console.log("3"))` are both microtasks even though one doesn't look like a Promise call.
- [ ] Explain why the second `.then()` in the chain (`6`) cannot run in the same microtask-queue pass as the first (`5`) — i.e. why chaining `.then()` schedules a new microtask rather than running synchronously inside the first callback.
- [ ] After writing your prediction, actually run the snippet (Node or browser console) and record whether you were correct. If you were wrong, explain specifically which assumption was incorrect.

## Submission
Write your answer in `workspace/track-2-event-loop/01-macrotask-vs-microtask/`. Include `NOTES.md` (Part 1 answers) and `ANSWER.md` (your predicted order + line-by-line reasoning, then the actual result and whether you were right).
