# [Track 7] Exercise 3: Origin, Domain & Same-Origin Policy

## Difficulty
Medium

## Learning Goal
Precisely apply the same-origin definition (scheme + host + port must all match) to determine whether two URLs share an origin, and observe a real browser-enforced CORS block firsthand.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy
- https://developer.mozilla.org/en-US/docs/Glossary/Origin
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS/Errors

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before doing the investigative task. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why does a browser need a concept of "origin" at all — what specific threat exists if a script loaded from one site could freely read data from a page/session on another site?
2. **Why Origin?** Give the historical/security reasoning for why the same-origin policy checks exactly scheme + host + port (no more, no less) — why is `http` vs `https` a different origin even on the same domain, and why is a different port a different origin even though the domain and scheme match?
3. **When to Use?** Describe a concrete production scenario where you had to reason about origins — e.g., debugging why a `fetch()` call to an API subdomain failed, deciding whether a `postMessage` handler needs an origin check, or configuring a CDN/asset domain.
4. **What if Absent?** Describe a concrete disaster that same-origin policy prevents — e.g., a malicious page silently reading a logged-in user's bank balance via `fetch` and exfiltrating it, or a malicious `iframe` reading data from the parent page's DOM.

## Part 2 — Prediction Task + Hands-On Investigation

### Part 2a — Prediction
For each pair of URLs below, state whether they are **same-origin** or **cross-origin**, and justify using the exact definition (scheme, host, port must all match):

1. `https://app.example.com` vs `https://api.example.com`
2. `https://example.com` vs `http://example.com`
3. `https://example.com:443` vs `https://example.com:8443`
4. `https://example.com` vs `https://example.com:443`
5. `https://example.com/path/one` vs `https://example.com/path/two`
6. `https://EXAMPLE.com` vs `https://example.com`
7. `https://example.com` vs `https://www.example.com`

### Part 2b — Hands-On CORS Error
Trigger a real CORS error in your own browser:

1. Open a local HTML file directly in the browser (via `file://`) or serve it from `http://localhost` with a simple static server.
2. In that page's script, call `fetch()` against a cross-origin API endpoint that does not send permissive CORS headers back to you (pick any public API that you confirm does NOT return `Access-Control-Allow-Origin` for your origin — you may need to try a couple).
3. Open DevTools console and capture the exact error message text.
4. In `ANSWER.md`, paste the exact console error text and explain, in your own words, which part of the browser stack is enforcing the block (hint: it is not the server rejecting the request outright — the response usually does arrive at the network level).

## Part 3 — Requirements & Edge Cases
- [ ] For each of the 7 pairs in Part 2a, state same-origin/cross-origin AND name which of scheme/host/port differs (if any).
- [ ] Do not confuse "same site" (eTLD+1) with "same origin" — if you use the word "domain" in your justification, be precise about whether you mean host or registrable domain.
- [ ] The hands-on CORS error must be a real, reproduced error — paste the actual console text, not a paraphrase.
- [ ] Explicitly state in `ANSWER.md` whether the failed request appears in the Network tab with a response (it typically does) — this is evidence that the server responded but the browser blocked script access to the response.
- [ ] Explain why opening a page via `file://` produces an origin of `null` (or a unique opaque origin) and how that affects CORS behavior.

## Submission
Write your answer/code in `workspace/track-7-url-and-web/03-origin-domain-same-origin-policy/`. Include `NOTES.md` and `ANSWER.md`.
