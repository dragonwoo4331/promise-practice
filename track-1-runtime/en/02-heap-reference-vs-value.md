# [Track 1] Exercise 2: Memory Heap & Reference vs Value

## Difficulty
Easy

## Learning Goal
Understand the distinction between how primitives (stored/copied by value) and objects/arrays (stored on the Heap, referenced by pointer on the Stack) behave when passed into functions or assigned to new variables, and how this affects mutation visibility outside the function.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Glossary/Primitive
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_management

## Part 1 — Understand Before You Code

Answer the following in `NOTES.md`, in your own words, before writing any code. Shallow answers like "for async processing" will be rejected — reference the actual runtime mechanics.

1. **Why Necessary?** — Given a single-threaded runtime with a Call Stack and a Heap, why does JavaScript need two different storage/copy strategies (by value vs by reference) instead of treating every variable the same way?
2. **Why Origin?** — What practical/performance limitation (e.g. cost of deep-copying large structures on every assignment, need for stack frames to stay small and fixed-size) led language designers to store objects on the Heap and only keep a reference on the Stack?
3. **When to Use?** — Describe a concrete scenario (e.g. passing a config object into a function, sharing state between two parts of a UI) where understanding reference semantics is required to avoid a bug.
4. **What if Absent?** — If JavaScript could not create shared references at all (everything was deep-copied on every assignment/function call), what concrete problems would arise (performance cost, inability to share mutable state, memory usage)?

## Part 2 — Prediction Quiz or Coding Task

Given this snippet:

```js
function updateUser(user) {
  user.age = 99;
  user = { name: "Replaced", age: 1 };
}

const person = { name: "Ari", age: 30 };
updateUser(person);
console.log(person);
```

In `ANSWER.md`, predict and explain:
- What does `console.log(person)` print, exactly?
- Why does the `user.age = 99` line affect `person`, but the `user = { name: "Replaced", age: 1 }` reassignment does not?

Then write your own small program (`solution.js`) that:
1. Creates an array of at least 3 numbers.
2. Passes it into a function that mutates it in place (e.g. via `.push()` or index assignment).
3. Passes it into a second function that reassigns the parameter to a brand-new array.
4. Logs the original array after both calls, and adds comments explaining, line by line, whether the Heap object was mutated or whether only a local Stack reference was changed.

## Part 3 — Requirements & Edge Cases

- [ ] Demonstrate at least one in-place mutation (visible outside the function) and one reassignment (not visible outside the function).
- [ ] In comments, explicitly name what lives on the Stack (the reference/pointer) versus what lives on the Heap (the actual object/array data).
- [ ] Add a note (in `NOTES.md` or as a comment) explaining reachability: once no variable/reference points to a Heap object anymore, why does the JS engine's garbage collector consider it eligible for collection?
- [ ] Cover the edge case of copying an object with `{ ...obj }` (spread) — is this a deep copy or a shallow copy? Demonstrate with a nested object whether an inner object is still shared after the spread.

## Submission
Write your answer/code in `workspace/track-1-runtime/02-heap-reference-vs-value/`. Include `NOTES.md` (Part 1 answers), `ANSWER.md` (prediction + reasoning for the quiz snippet), and `solution.js` (the coding task).
