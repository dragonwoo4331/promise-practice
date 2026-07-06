# [Track 8] Exercise 2: useEffect Dependency Array, Cleanup, and the Async Race Condition

## Difficulty
Medium

## Learning Goal
Correctly scope a `useEffect` dependency array so a data fetch re-runs only when its input changes, and implement an ignore-flag or `AbortController` cleanup pattern that prevents an out-of-order (race condition) response from overwriting a newer one; understand precisely why the effect callback itself cannot be declared `async`.

## Prerequisite Reading
- https://react.dev/reference/react/useEffect
- https://react.dev/learn/synchronizing-with-effects
- https://react.dev/learn/you-might-not-need-an-effect
- https://react.dev/learn/lifecycle-of-reactive-effects
- https://react.dev/learn/removing-effect-dependencies

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why does synchronizing a component with an external system (like a network request keyed off a prop) require `useEffect` rather than just calling `fetch` directly in the component body during render?
2. **Why Origin?** Why did React's design require the dependency array to be exhaustive (reflecting every reactive value read inside the effect), and what class of bugs does an incomplete dependency array cause that lifecycle methods like `componentDidUpdate` made easy to introduce accidentally?
3. **When to Use?** Describe a concrete production scenario (e.g., a search-as-you-type field) where failing to guard against out-of-order responses causes a visible, user-facing bug. Be specific about the sequence of network events that triggers it.
4. **What if Absent?** Concretely describe what happens on screen if the cleanup/ignore-flag logic is missing and a user types a fast query, then a slower one, and the requests resolve out of order.

## Part 2 — Coding Task
Build a component that takes a search query (from a controlled input's state) and fetches results from a mock or real API (you may use `https://jsonplaceholder.typicode.com/` or any public API) whenever the query changes.

Requirements to implement yourself:
- The effect's dependency array must cause a re-fetch only when the query actually changes — not on every render.
- The effect callback must NOT be `async` directly. Explain in `NOTES.md` why this is illegal/dangerous:

```jsx
// Why is this signature a problem?
useEffect(async () => {
  const res = await fetch(url);
  // ...
}, [query]);
```

- Implement a race-condition guard using either a boolean ignore-flag set in the cleanup function, or an `AbortController` whose `.abort()` is called in the cleanup function, so that if the query changes before a previous fetch resolves, the stale response is discarded and never applied to state.
- To actually prove the race condition guard works, artificially delay one of the responses (e.g., add a random or fixed `setTimeout` delay before resolving, or query terms that you know return slower) and log timestamps showing a slow, earlier request's response arriving after a fast, later request's response — then show that the stale one is correctly ignored.

## Part 3 — Requirements & Edge Cases
- [ ] Dependency array includes exactly the reactive values read in the effect (the query), no more, no less.
- [ ] Effect callback is a synchronous function that defines and calls an inner async function, or uses `.then()` — not `async () => {}` itself.
- [ ] Cleanup function returned from the effect that either sets an ignore flag or calls `AbortController.abort()`.
- [ ] Demonstrated proof (via console logs with timestamps) that a stale, out-of-order response does NOT overwrite newer state.
- [ ] `NOTES.md` explains why an async effect callback's implicit Promise return would conflict with React's use of the effect's return value for cleanup.
- [ ] Handles the case where the query is empty (define and document the behavior yourself — e.g., skip fetching).

## Submission
Add a route/page for this exercise inside `workspace/track-8-react-fundamentals/02-useeffect-deps-cleanup/`. Include `NOTES.md` and your component file(s).
