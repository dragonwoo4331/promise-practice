# [Track 8] Exercise 1: State & the Re-render Mental Model

## Difficulty
Easy

## Learning Goal
Understand that calling a `useState` setter schedules a re-render of that component (and its children), that updates triggered within the same event handler are batched into a single re-render, and that using the previous-value updater form (`setX(x => x + 1)`) versus the direct form (`setX(x + 1)`) produces different results when called multiple times against stale closure state.

## Prerequisite Reading
- https://react.dev/learn/state-a-components-memory
- https://react.dev/learn/render-and-commit
- https://react.dev/learn/queueing-a-series-of-state-updates
- https://react.dev/learn/state-as-a-snapshot

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why does React need a special `useState` hook instead of just letting a component use a plain local variable to hold data that changes over time? What specifically fails if you use a plain `let` variable instead?
2. **Why Origin?** Why does calling a state setter not update the variable immediately (i.e., why is state treated as a "snapshot" for the duration of a render)? What problem does this snapshot model solve compared to mutating a value in place?
3. **When to Use?** Describe a concrete production scenario where you would deliberately use the updater function form (`setX(prev => ...)`) instead of the direct form, and explain why the direct form would produce a bug in that scenario.
4. **What if Absent?** Concretely describe what visibly breaks in the UI if a developer calls `setCount(count + 1)` twice in a row inside a single event handler, expecting the count to increase by 2.

## Part 2 — Coding Task
Build a component with at least two independent `useState` values (for example, a click counter and a toggle for a boolean flag). Add a `console.log` statement in the component body (not inside an effect, not inside a handler) that prints on every render, including a label and the current values of both state variables.

Then implement and compare two buttons:

```jsx
// Button A (buggy trap — you must demonstrate why this is wrong)
function handleTripleIncrementDirect() {
  setCount(count + 1);
  setCount(count + 1);
  setCount(count + 1);
}

// Button B (correct pattern — you must implement this yourself)
function handleTripleIncrementUpdater() {
  // your implementation using the updater form
}
```

Wire each function to its own button. Observe and record in `NOTES.md`:
- How many times the body `console.log` fires per click for each button.
- What the final count value is after 3 rapid clicks on Button A vs. Button B, and why they differ.
- Whether clicking one button ever causes the *other* state variable's UI to re-render, and why or why not.

Do not just read about this — click the buttons yourself, read the actual console output, and write down what you observed versus what you predicted beforehand.

## Part 3 — Requirements & Edge Cases
- [ ] At least two independent `useState` values in the same component.
- [ ] A body-level `console.log` (fires on every render, not just on click).
- [ ] Button A using the direct form (`setCount(count + 1)`) called 3 times in one handler — demonstrate the bug.
- [ ] Button B using the updater form, correctly incrementing by 3.
- [ ] `NOTES.md` contains your prediction (written before running the code) and your actual observation (written after), and explicitly reconciles any mismatch.
- [ ] Explanation in `NOTES.md` of why React batches the three `setCount` calls in Button A into a single re-render, yet the value only increases by 1.

## Submission
Write your component in `workspace/track-8-react-fundamentals/01-state-and-rerender/` as a small standalone React app or sandbox (you may scaffold once with Vite: `npm create vite@latest` and reuse it across exercises, or a single HTML file with React via script tags for tiny ones — your choice, note which in NOTES.md). Include `NOTES.md` and your component file(s).
