# [Track 7] Exercise 4: Cookies, Domain Scope & Site vs Origin

## Difficulty
Hard

## Learning Goal
Explain precisely how a cookie's `Domain` and `Path` attributes determine which requests it is attached to, and distinguish "site" (eTLD+1) from "origin" as used in `SameSite` cookie semantics.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
- https://developer.mozilla.org/en-US/docs/Web/Privacy/Guides/Same-site_and_same-origin_requests
- https://developer.mozilla.org/en-US/docs/Glossary/Origin

## Part 1 — Understand Before You Code
Answer these in your own words in `NOTES.md` before doing the research/investigation. Shallow answers will be rejected — reference concrete mechanics, not vague buzzwords.

1. **Why Necessary?** Why does HTTP need cookies to carry scoping attributes (`Domain`, `Path`) at all, instead of every cookie simply being sent with every request to every server?
2. **Why Origin?** Explain the historical reasoning behind the distinction between a "host-only" cookie (no `Domain` attribute set) and a `Domain`-scoped cookie (e.g., `Domain=example.com`) — why would a site deliberately choose one over the other, and why does the `SameSite` attribute reason about "site" (eTLD+1) rather than full "origin"?
3. **When to Use?** Describe a concrete production scenario involving cookie scope — e.g., a login session cookie that must be shared across `app.example.com` and `api.example.com`, or a cookie that must NOT be sent on cross-site requests to prevent CSRF.
4. **What if Absent?** Describe a concrete security disaster caused by incorrect cookie scoping — e.g., a session cookie set too broadly (`Domain=.com` style overreach, or missing `SameSite`) being sent along with a cross-site request, enabling CSRF, or a cookie scoped too narrowly silently failing to authenticate a subdomain.

## Part 2 — Research Task + Hands-On Investigation

### Part 2a — Conceptual Research
Research and explain, in `ANSWER.md`:
1. The difference in browser behavior between a cookie set with `Set-Cookie: session=abc; Domain=example.com` versus `Set-Cookie: session=abc` (no `Domain` — host-only cookie). Specifically: does each version get sent to `sub.example.com`? Does each get sent to `example.com` itself?
2. How the `Path` attribute restricts which request paths a cookie is attached to, with a concrete example (e.g., `Path=/admin`).
3. The precise difference between "site" (eTLD+1, e.g., `example.com` covers `app.example.com` and `api.example.com`) and "origin" (scheme + host + port), and how `SameSite=Strict`, `SameSite=Lax`, and `SameSite=None` each use the "site" concept rather than full origin matching.

### Part 2b — Hands-On DevTools Investigation
1. Open DevTools on any real website you are currently logged into.
2. Go to the Application (Chrome) or Storage (Firefox) tab and find the Cookies panel.
3. Pick at least one cookie and record its exact `Domain`, `Path`, `SameSite`, and `Secure` attribute values.
4. In `ANSWER.md`, explain — using the site's actual architecture as you understand it (e.g., does it have subdomains that need to share login state?) — why each of those four attributes is plausibly set the way it is for that specific cookie.

## Part 3 — Requirements & Edge Cases
- [ ] Clearly state whether a host-only cookie (no `Domain` attribute) is sent to subdomains — do not guess, verify against the spec/MDN.
- [ ] Clearly state whether a cookie with `Domain=example.com` is sent to `example.com` itself as well as `sub.example.com`.
- [ ] Give a correct concrete example of `Path` scoping restricting a cookie to a subset of requests.
- [ ] Do not conflate "site" and "origin" in your `SameSite` explanation — state explicitly that `SameSite` cares about registrable domain, not full origin (so `https://a.example.com` and `https://b.example.com` are same-site for `SameSite` purposes even though they are cross-origin).
- [ ] The DevTools investigation must reference a real cookie you actually inspected — name the site (or describe it generically if sensitive) and report the actual attribute values, not assumed ones.
- [ ] Explain what `Secure` requires (HTTPS-only transmission) and why a cookie without it is a risk on a site that also serves plain HTTP.

## Submission
Write your answer/code in `workspace/track-7-url-and-web/04-cookies-domain-scope-site-vs-origin/`. Include `NOTES.md` and `ANSWER.md`.
