# Track 2: Event Loop & Task Queues

This track builds directly on Track 1's runtime fundamentals. Here you drill into exactly how the Event Loop orders work between synchronous code, the Microtask Queue (Promises, `queueMicrotask`), and the (Macro)Task Queue (`setTimeout`, `setInterval`, I/O), and how a long synchronous task can freeze the UI — plus what real fixes (chunking, Web Workers) actually do at the mechanism level. Precision matters here: "it just works async" is not an acceptable understanding for any of these exercises.

## Exercises

1. [01 — MacroTask vs MicroTask Ordering Prediction](./01-macrotask-vs-microtask.md) — Medium. Trace exact execution order across sync code, microtasks, and macrotasks.
2. [02 — Build a Mini Task Queue Logger](./02-mini-task-queue-logger.md) — Medium. Instrument and verify the "microtasks always run before the next macrotask" claim, including nested microtasks.
3. [03 — UI Freezing Simulation & Escape Hatch](./03-ui-freezing-escape-hatch.md) — Hard. Reproduce a real freeze, then fix it via chunking and/or a Web Worker.

## Reference

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API

Submit all work under `workspace/track-2-event-loop/<exercise-folder>/` as specified in each exercise file.
