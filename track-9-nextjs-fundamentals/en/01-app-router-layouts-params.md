# [Track 9] Exercise 1: App Router Basics — Routes, Nested Layouts, Params

## Difficulty
Easy

## Learning Goal
Understand how the App Router maps the file system to URL routes, how a dynamic segment (`[id]`) exposes route parameters via the `params` prop, and how a shared `layout` preserves component state across navigations between its child routes instead of remounting on every navigation the way a full page swap would.

## Prerequisite Reading
- https://nextjs.org/docs/app/building-your-application/routing
- https://nextjs.org/docs/app/building-your-application/routing/defining-routes
- https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes
- https://nextjs.org/docs/app/api-reference/file-conventions/layout
- https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why does the App Router use a nested-folder-based convention (`layout.tsx`, `page.tsx`, `[id]/page.tsx`) instead of a single central route configuration file (as some other frameworks/routers do)? What does this convention make trivially easy that a central config makes awkward?
2. **Why Origin?** Why did Next.js introduce nested layouts as a first-class concept (as opposed to the older Pages Router, where a custom `_app.js` was the only shared wrapper)? What specific UX/performance problem does layout nesting solve when navigating between sibling routes?
3. **When to Use?** Describe a concrete production scenario where a shared layout must preserve component state across navigation (e.g., a persistent audio player, an open sidebar, a scroll position, an in-progress form draft in a side panel) and explain what would visibly break if that layout were instead implemented as a full page component duplicated on every route.
4. **What if Absent?** Concretely describe what happens on screen (state loss, layout flicker, refetching, remount cost) if you use a full page component instead of a shared layout for two routes that should share persistent UI.

## Part 2 — Coding Task
Create at least 3 routes in your scaffolded Next.js app:
- A static route, e.g. `/about`.
- A dynamic route, e.g. `/products/[id]`.
- A second route (static or dynamic) that shares a layout with one of the above, e.g. `/products` and `/products/[id]` both wrapped by `app/products/layout.tsx`.

In the dynamic route's `page` component, read and log the `params` prop server-side (e.g., `console.log(params)` — check your terminal, not the browser console, since this runs on the server) and render the resolved `id` value on the page.

In the shared layout, add some client-side interactive state (e.g., a counter or an expanded/collapsed toggle in a sidebar — this will require `"use client"` on that specific piece; you do not need to fully understand the Server/Client boundary yet, that is Exercise 2, but you need enough of it here to prove persistence). Navigate between the two routes under that layout using `<Link>` and verify — by observing whether the counter/toggle resets — whether the layout remounts or persists.

Then, as a contrast, build a case where navigating to a route *outside* the shared layout's subtree causes that state to reset, and explain why in `NOTES.md`.

## Part 3 — Requirements & Edge Cases
- [ ] At least 3 routes exist, including one dynamic route using a `[param]` segment.
- [ ] A `layout.tsx`/`layout.js` wraps at least two of the routes.
- [ ] The dynamic route's `params` is read and logged (server-side terminal log), and the value is rendered in the UI.
- [ ] Demonstrated proof (describe your observation in `NOTES.md`) that navigating between the two layout-wrapped routes preserves the layout's interactive state.
- [ ] Demonstrated proof that navigating outside the shared layout's subtree resets that state.
- [ ] Handles an invalid/unexpected dynamic segment value (define and document what your page does — e.g., a not-found state) yourself.

## Submission
Add a route/page for this exercise inside your existing Next.js app scaffolded in `workspace/track-9-nextjs-fundamentals/`. Include `NOTES.md` describing where you put it and why.
