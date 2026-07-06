# [Track 7] Exercise 2: URLSearchParams Manipulation

## Difficulty
Medium

## Learning Goal
Learn to add, update, delete, and read query string parameters — including repeated keys — using `URLSearchParams`, and understand how it handles percent-encoding automatically.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams
- https://developer.mozilla.org/en-US/docs/Web/API/URL/searchParams
- https://developer.mozilla.org/en-US/docs/Glossary/Percent-encoding

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before writing any code. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why can't query strings be reliably built or read with plain string concatenation (`url + '?foo=' + value`) once you consider special characters, multiple params, and repeated keys?
2. **Why Origin?** Why does the query string grammar require percent-encoding of characters like spaces, `&`, `=`, and `#` in the first place — what would happen to the parser if these characters appeared raw inside a key or value?
3. **When to Use?** Describe a concrete production scenario where you'd build or mutate query parameters with `URLSearchParams` (e.g., adding a `page` and `pageSize` param for pagination, updating a `sort` param when a user clicks a column header, stripping a `token` param before logging a URL).
4. **What if Absent?** Describe a concrete bug caused by manual query string construction — e.g., a search term containing `&` silently splitting into a bogus extra parameter, or a space breaking the query string instead of becoming `%20`/`+`.

## Part 2 — Coding Task
Starting from this base URL:

```
https://example.com/search?tag=js&tag=react&tag=node&sort=asc
```

Write a module that exposes functions to do the following, all using `URLSearchParams` (no manual string splitting/concatenation on the query string):

1. `addTag(url, newTag)` — append an additional `tag` value without removing the existing ones.
2. `replaceSort(url, newSortValue)` — update the single `sort` param to a new value (not append a duplicate).
3. `removeTag(url, tagToRemove)` — remove exactly one specific `tag` value, leaving the other `tag` values intact.
4. `getAllTags(url)` — return an array of every `tag` value.
5. `getFirstTag(url)` — return only the first `tag` value.
6. `buildQuery(paramsObject)` — given a plain object (values may be strings or arrays of strings for repeated keys), build a full query string, correctly percent-encoding a value that contains a space, an `&`, an `=`, and a non-ASCII character (e.g., `"café & bar"`).

For each function, log a before/after `toString()` (or full `href`) so the change is visible.

## Part 3 — Requirements & Edge Cases
- [ ] Never use `.split('&')`, `.split('=')`, `encodeURIComponent` manual concatenation, or regex to read/write the query string — `URLSearchParams` must do all parsing/encoding.
- [ ] Demonstrate the difference between `.get('tag')` (returns only the first match) and `.getAll('tag')` (returns all matches) using the sample URL above — state explicitly which one a naive implementation might wrongly assume returns everything.
- [ ] `removeTag` must not use `.delete('tag')` naively if that would delete all `tag` values when you only want to remove one — figure out the correct approach (hint: read all values, filter, then rebuild that key).
- [ ] Show that `.toString()` on a `URLSearchParams` containing `"café & bar"` produces correctly percent-encoded output, and print the raw encoded string as proof.
- [ ] Confirm whether `URLSearchParams` encodes spaces as `+` or `%20` in `.toString()` output, and state which one you observed.
- [ ] Handle a param object with an array value in `buildQuery` (e.g., `{ tag: ['a', 'b'] }` should produce `tag=a&tag=b`).

## Submission
Write your answer/code in `workspace/track-7-url-and-web/02-urlsearchparams-manipulation/`. Include `NOTES.md` and `solution.js`.
