# [Track 9] Exercise 3: Data Fetching & searchParams in Server Components

## Difficulty
Hard

## Learning Goal
Read a query string filter via a Server Component's `searchParams` prop (no client-side `URLSearchParams` involved), filter data server-side accordingly, and understand Next.js's `fetch` caching semantics (`force-cache` default, `no-store`, and time-based `revalidate`) by empirically testing whether a fetch actually re-executes on reload.

## Prerequisite Reading
- https://nextjs.org/docs/app/api-reference/file-conventions/page
- https://nextjs.org/docs/app/api-reference/functions/fetch
- https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating
- https://nextjs.org/docs/app/building-your-application/caching

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why can a Server Component read `searchParams` directly as a prop instead of needing `URLSearchParams` or a client-side hook the way a Client Component would (contrast with how you handled query strings in Track 7)? What is different about where and when a Server Component executes that makes this possible?
2. **Why Origin?** Why does Next.js's extended `fetch` default to aggressive caching (`force-cache`) for Server Component data fetching instead of always fetching fresh data on every request? What performance/cost problem does this default solve, and what correctness problem can it silently introduce if a developer doesn't know about it?
3. **When to Use?** Describe a concrete production scenario where `no-store` is the correct choice (data must always be fresh) and a separate concrete scenario where a `revalidate` interval (e.g., every 60 seconds) is the correct choice (data can be slightly stale for performance). Justify each choice by the nature of the data.
4. **What if Absent?** Concretely describe the disaster if a developer fetches user-specific or rapidly changing data (e.g., account balance, live inventory count) using the default caching behavior without realizing it, and what a user would see as a result.

## Part 2 — Coding Task
Build a page at a route like `/products` that reads a `category` filter from the query string (e.g., `/products?category=shoes`) via the Server Component's `searchParams` prop — do not use `useSearchParams` from `next/navigation` (that is a Client Component hook and is out of scope here) and do not parse `window.location` on the client.

Fetch or define a mock dataset of products (a hardcoded array is acceptable, or fetch from a public API) and filter it server-side according to the `category` value from `searchParams`. Render the filtered list. Handle the case where `category` is absent (show all products) and where it matches nothing (show an explicit empty state, not a blank page).

Then run a controlled caching experiment:
- Make one fetch call in this page (or a related one) using `{ cache: 'force-cache' }` (or the default, which is equivalent) and log a timestamp (e.g., `console.log(Date.now(), 'force-cache fetch ran')`) every time the fetch actually executes.
- Make a second fetch call using `{ cache: 'no-store' }`, similarly logging a timestamp.
- Reload the page multiple times (hard refresh and normal navigation) and observe, from your terminal logs (these run server-side), whether each fetch's timestamp changes on every reload or stays constant.
- Additionally test `{ next: { revalidate: <N> } }` with a short N (e.g., 10 seconds): reload once immediately (expect the same cached timestamp) and once after waiting past N seconds (expect a new timestamp).

Record your actual observed timestamps for all three modes in `NOTES.md`, not just your prediction.

## Part 3 — Requirements & Edge Cases
- [ ] `searchParams` is read directly as a Server Component prop — no client-side query string parsing.
- [ ] Filtering happens server-side based on the `category` value.
- [ ] Absent `category` shows all items; a `category` matching zero items shows an explicit empty state.
- [ ] At least two of `force-cache` (or default), `no-store`, and `revalidate` are actually tested, not just described.
- [ ] `NOTES.md` contains real logged timestamps demonstrating whether each fetch re-ran on reload, with your interpretation of why.
- [ ] Explanation in `NOTES.md` of which caching mode you would use for this specific product-listing data in a real production catalog, and why.

## Submission
Add a route/page for this exercise inside your existing Next.js app scaffolded in `workspace/track-9-nextjs-fundamentals/`. Include `NOTES.md` describing where you put it and why.
