# [Track 9] Exercise 2: Server Components vs. Client Components Boundary

## Difficulty
Medium

## Learning Goal
Understand precisely which capabilities (interactivity via event handlers, React hooks like `useState`/`useEffect`, browser-only APIs) require a component to be a Client Component via the `"use client"` directive, and that a Server Component can still be composed into a Client Component's `children` without itself becoming a Client Component.

## Prerequisite Reading
- https://nextjs.org/docs/app/building-your-application/rendering/server-components
- https://nextjs.org/docs/app/building-your-application/rendering/client-components
- https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns
- https://react.dev/reference/rsc/use-client

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why can't a Server Component use `useState` or an `onClick` handler at all, even in principle? What is actually missing at runtime on the server that makes this impossible rather than just discouraged?
2. **Why Origin?** Why did Next.js/React introduce Server Components as the default rendering mode instead of keeping everything as Client Components (the old default, effectively, in the Pages Router)? What specific cost (bundle size, waterfall requests, exposed secrets) does defaulting to Server Components reduce?
3. **When to Use?** Describe a concrete production scenario where a page should remain a Server Component overall, but one small interactive island (e.g., a "like" button, a dropdown, a copy-to-clipboard button) needs `"use client"`. Explain why marking the *entire page* `"use client"` instead would be a worse choice.
4. **What if Absent?** Concretely describe the actual build-time or runtime error message you get when you try to use `useState` or `onClick` inside a component that lacks `"use client"`, and explain what that error is telling you about where the code is (attempting to) execute.

## Part 2 — Coding Task
Build a page that is a Server Component by default (no `"use client"` at the top). Inside it, add one deliberately interactive piece — for example:

```jsx
// Inside your Server Component page — do not fix this yet.
function LikeButton() {
  const [liked, setLiked] = useState(false); // this will not work here
  return <button onClick={() => setLiked(!liked)}>{liked ? "Liked" : "Like"}</button>;
}
```

Run it and capture the actual error Next.js gives you (build error or runtime error — copy the exact message into `NOTES.md`). Do not skip this step; you must see the real failure before fixing it.

Then fix it by extracting the interactive piece into its own component file/module with `"use client"` at the top, imported into the Server Component page. Verify it now works.

Finally, demonstrate the "Server Component as children" composition pattern: create a Client Component wrapper (e.g., a collapsible panel or a modal shell that manages open/closed state) that accepts `children`, and pass a Server Component (one that does something only a server can do cheaply, e.g., read a value that would require a secret/env var, or just a plain server-rendered content block) as its `children` from the parent Server Component. Verify this works without the child needing `"use client"` itself.

## Part 3 — Requirements & Edge Cases
- [ ] Page starts as a pure Server Component with no `"use client"`.
- [ ] You reproduced and recorded the actual error before fixing anything.
- [ ] The interactive piece is isolated into its own smallest-possible Client Component module, not the whole page.
- [ ] `NOTES.md` states exactly which file needed `"use client"` and why that file specifically (not a parent, not the whole tree).
- [ ] A working demonstration of a Server Component passed as `children` into a Client Component, without the Server Component itself needing the directive.
- [ ] Explanation in `NOTES.md` of why the composition pattern (Server Component as children) is the recommended approach over converting the parent to a Client Component.

## Submission
Add a route/page for this exercise inside your existing Next.js app scaffolded in `workspace/track-9-nextjs-fundamentals/`. Include `NOTES.md` describing where you put it and why.
