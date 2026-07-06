# [Track 2] Exercise 3: UI Freezing Simulation & Escape Hatch

## Difficulty
Hard

## Learning Goal
Reproduce a real UI freeze caused by a long-running synchronous task occupying the Call Stack, then explain — with reference to actual mechanisms, not vague hand-waving — how chunking work via `setTimeout`/`requestAnimationFrame` or offloading it to a Web Worker restores responsiveness by yielding control back to the Event Loop.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame

## Part 1 — Understand Before You Code

Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. **Why Necessary?** — Why does a long synchronous computation freeze the entire UI (clicks, scrolling, animations, repaints) in a way that an equivalent amount of work spread across many `setTimeout`-scheduled chunks does not?
2. **Why Origin?** — What limitation of the single Call Stack / single Event Loop model (one thread doing rendering, input handling, and script execution) made Web Workers necessary as a platform feature, rather than JS simply getting a way to run code "in the background" on the same thread?
3. **When to Use?** — Describe a concrete real-world scenario (e.g. client-side image filtering, parsing a huge CSV/JSON payload, complex data transformation for a chart) where an engineer must choose between chunking and a Web Worker.
4. **What if Absent?** — If neither chunking nor Web Workers existed, and the only option was a single unbreakable synchronous block, what would happen to a real product's usability when it needs to process a moderately large dataset on the client?

## Part 2 — Prediction Quiz or Coding Task

Part A — Reproduce the freeze: write a script with a page containing a visible, clickable button and a counter that increments via `setInterval` or `requestAnimationFrame` (so you have a visible "heartbeat"). Add a synchronous function that loops for several seconds doing busy work (e.g. a nested loop doing meaningless math). Trigger it via a second button. Observe and record: does the heartbeat counter pause? Can you click the first button during the freeze?

Part B — Design (and optionally implement) the fix:
- Rewrite the busy work as a **chunked** version using `setTimeout` (or `requestAnimationFrame`) to break the loop into smaller pieces, yielding control back to the Event Loop between chunks. Verify the heartbeat counter keeps ticking and the button stays clickable during the chunked run.
- As a stretch goal, move the busy work into a real **Web Worker** using `postMessage`/`onmessage`, and verify the main thread stays fully responsive throughout, even for the *entire* duration of the work (not just in small windows between chunks).

You do not need to reveal exact expected console output for this exercise — this is a build-and-observe task, not a pure prediction quiz.

## Part 3 — Requirements & Edge Cases

- [ ] Part A must include a visible, continuously-updating UI element (heartbeat counter or animation) so freezing is observable, not just inferred from console timing.
- [ ] Record precisely when the heartbeat stops and resumes relative to the busy-work start/end.
- [ ] The chunked version in Part B must demonstrably keep the heartbeat and button responsive — include your observation, not just the code.
- [ ] In your design note, explain specifically why chunking still runs on the *same* main thread (i.e. it does not achieve true parallelism, only cooperative yielding) — contrast this with a Web Worker, which runs on a genuinely separate thread with no shared memory (communication only via message passing).
- [ ] If you attempt the Web Worker stretch goal, document one concrete limitation of Web Workers you encountered or researched (e.g. no DOM access, data cloned via structured clone algorithm rather than shared by reference).
- [ ] State explicitly which approach (chunking vs Web Worker) you would choose for a CPU-bound task that takes 30+ seconds, and justify it using the mechanics you've now observed.

## Submission
Write your work in `workspace/track-2-event-loop/03-ui-freezing-escape-hatch/`. Include `NOTES.md` (Part 1 answers), your freeze-reproduction code and chunked-fix code (e.g. `index.html` + `solution.js`), a written design note (`ANSWER.md`) covering the chunking-vs-Web-Worker mechanism, and — if attempted — your Web Worker stretch-goal files.
