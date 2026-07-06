# [Track 2] Exercise 2: Build a Mini Task Queue Logger

## Difficulty
Medium

## Learning Goal
Verify, through direct instrumentation rather than assumption, the claim that "all currently-queued microtasks run before the next macrotask begins" — including the subtlety that microtasks *scheduled during* microtask draining are also run before the next macrotask (the Microtask Queue is drained to empty, not just "run once").

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/setTimeout
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then

## Part 1 — Understand Before You Code

Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. **Why Necessary?** — Why is it not enough to just "trust" documentation claims about ordering — why should an engineer be able to instrument and verify Event Loop behavior directly in a real environment?
2. **Why Origin?** — What historical confusion or class of bugs (e.g. developers assuming `setTimeout(fn, 0)` runs "immediately," or assuming `.then()` chains run synchronously) makes this kind of verification exercise necessary for junior engineers?
3. **When to Use?** — Describe a production scenario where you would need to reason precisely about whether a state update lands before or after a scheduled timer callback (e.g. debouncing, batching UI updates, coordinating with `requestAnimationFrame`).
4. **What if Absent?** — If a developer incorrectly assumes `setTimeout(fn, 0)` runs before pending promise callbacks, what concrete bug could this cause in real code (e.g. reading stale state, a race condition in a UI update)?

## Part 2 — Prediction Quiz or Coding Task

Build a small instrumented script that logs tagged output such as `[sync]`, `[microtask]`, and `[macrotask]` prefixes for every logged line, so the queue type is visible at a glance.

Your script must construct a case with:
- At least 2 `setTimeout` calls (macrotasks), with different delays (e.g. `0` and `10`).
- At least 2 separate `.then()` chains, where at least one chain has 3+ links (e.g. `.then().then().then()`).
- At least one `.then()` callback that itself schedules a *new* microtask (e.g. calls `queueMicrotask` or returns another resolved Promise) — this is the key case for testing whether "newly queued microtasks during draining" still run before the next macrotask.

Before running anything, write your full predicted output (in exact order, with tags) in `ANSWER.md`, along with your reasoning for each line.

## Part 3 — Requirements & Edge Cases

- [ ] Every logged line must be tagged with `[sync]`, `[microtask]`, or `[macrotask]` reflecting which queue type produced it.
- [ ] Include the specific edge case: a microtask that schedules another microtask from inside itself — predict and then verify whether that nested microtask still runs before the first macrotask fires.
- [ ] Explicitly state and then test the claim: "all microtasks always run before the next macrotask." Does your experiment confirm or complicate this claim? If two `setTimeout(fn, 0)` calls are both pending, do all microtasks run before *both* of them, or only before the first?
- [ ] After running, write a short verdict in `ANSWER.md`: was the claim fully true, true with caveats, or false based on your observation? Justify with your actual tagged output.

## Submission
Write your answer/code in `workspace/track-2-event-loop/02-mini-task-queue-logger/`. Include `NOTES.md` (Part 1 answers), `solution.js` (the instrumented script), and `ANSWER.md` (your predicted output, then actual output, then your verdict on the ordering claim).
