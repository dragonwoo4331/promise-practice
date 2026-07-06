# [Track 8] Exercise 3: Loading / Error / Data State Pattern

## Difficulty
Medium

## Learning Goal
Model asynchronous fetch state so that loading, error, and success are mutually exclusive and impossible states are unrepresentable, using either a single discriminated union / status enum or carefully guarded conditionals, instead of independent boolean flags that can contradict each other.

## Prerequisite Reading
- https://react.dev/learn/synchronizing-with-effects
- https://react.dev/learn/you-might-not-need-an-effect
- https://react.dev/learn/conditional-rendering
- https://react.dev/learn/choosing-the-state-structure

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why is it not sufficient to track fetch state with three independent booleans (`isLoading`, `isError`, `data`)? What specific combination of these booleans represents a state that should be logically impossible but is actually reachable in code?
2. **Why Origin?** Why do experienced engineers prefer a single `status` field (e.g., `'idle' | 'loading' | 'success' | 'error'`) or a discriminated union carrying only the fields valid for that status, over multiple independent flags? What does "making impossible states unrepresentable" mean concretely in terms of what the type system or the reducer logic can and cannot express?
3. **When to Use?** Describe a concrete production scenario where a stale `isError` flag (left `true` from a previous failed request) silently persists and renders alongside a successful new `data` value, confusing the user.
4. **What if Absent?** Concretely describe the exact sequence of user actions (e.g., retry after failure) that would trigger the impossible/contradictory state if you used independent booleans without careful guards.

## Part 2 — Coding Task
Build a component that fetches a list of items from a real or mock API and renders exactly one of three UI states at any given time:
- Loading indicator (text or spinner) while the request is in flight.
- Error message if the request fails (trigger this deliberately, e.g., by fetching a bad URL or simulating a rejected promise).
- The successfully loaded data, rendered as a list.

Design your own state shape. You must choose and justify in `NOTES.md` one of:
- A single `status` string field (`'idle' | 'loading' | 'success' | 'error'`) plus separate `data` and `error` fields, with rendering logic that switches on `status` alone.
- A single discriminated union state object, e.g., conceptually `{ status: 'success', data: T } | { status: 'error', error: E } | { status: 'loading' }`, stored via `useState` or `useReducer`.

Do not implement the naive three-independent-booleans version except as a throwaway example in `NOTES.md` to illustrate the bug you are avoiding — your actual component must not use that shape.

Also implement a "retry" button that re-triggers the fetch after an error, and verify it correctly transitions back through loading before landing on success or error again — no stale error message left on screen during the retry's loading phase.

## Part 3 — Requirements & Edge Cases
- [ ] Exactly one of loading/error/success UI is visible at any time — never zero, never more than one.
- [ ] State shape is a single status enum or discriminated union, not independent booleans.
- [ ] Error state is reachable and demonstrated (deliberately triggered).
- [ ] Retry button correctly clears the previous error/data before showing the loading state again.
- [ ] `NOTES.md` includes the illegal boolean combination you identified in Part 1, Q1, and explains how your chosen design makes it unrepresentable.
- [ ] Handles the empty-result case (successful fetch, zero items) distinctly from the error case.

## Submission
Write your component in `workspace/track-8-react-fundamentals/03-loading-error-data-state/` as a small standalone React app or sandbox (you may scaffold once with Vite: `npm create vite@latest` and reuse it across exercises, or a single HTML file with React via script tags for tiny ones — your choice, note which in NOTES.md). Include `NOTES.md` and your component file(s).
