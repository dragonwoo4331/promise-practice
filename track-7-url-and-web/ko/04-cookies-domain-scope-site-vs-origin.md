# [Track 7] Exercise 4: Cookie, Domain Scope와 Site vs Origin

## Difficulty
Hard

## Learning Goal
Cookie의 `Domain`과 `Path` 속성이 어떤 요청에 해당 쿠키가 첨부되는지를 어떻게 결정하는지 정확히 설명하고, `SameSite` 쿠키 의미론에서 사용되는 "site"(eTLD+1)와 "origin"을 구분한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
- https://developer.mozilla.org/en-US/docs/Web/Privacy/Guides/Same-site_and_same-origin_requests
- https://developer.mozilla.org/en-US/docs/Glossary/Origin

## Part 1 — 코드 작성 전에 이해하기
아래 질문에 조사/실습 과제를 하기 전, `NOTES.md`에 자신의 언어로 답하라. 피상적인 답변은 반려된다 — 막연한 용어가 아니라 구체적인 동작 원리를 근거로 답하라.

1. **Why Necessary?** 모든 쿠키를 모든 서버로의 모든 요청에 단순히 실어 보내는 대신, HTTP는 왜 애초에 쿠키에 범위를 지정하는 속성(`Domain`, `Path`)이 필요한가?
2. **Why Origin?** "host-only" 쿠키(`Domain` 속성 미설정)와 `Domain`으로 범위가 지정된 쿠키(예: `Domain=example.com`)의 구분에 대한 역사적 근거를 설명하라 — 사이트가 왜 의도적으로 둘 중 하나를 선택하는지, 그리고 `SameSite` 속성이 왜 전체 "origin"이 아니라 "site"(eTLD+1) 개념으로 판단하는지 설명하라.
3. **When to Use?** 쿠키 범위와 관련된 구체적인 운영 시나리오를 서술하라 — 예를 들어 `app.example.com`과 `api.example.com`에서 공유되어야 하는 로그인 세션 쿠키, 또는 CSRF를 방지하기 위해 cross-site 요청에는 절대 실려서는 안 되는 쿠키.
4. **What if Absent?** 잘못된 쿠키 범위 설정이 일으키는 구체적인 보안 재앙을 서술하라 — 예를 들어 지나치게 넓게 설정된 세션 쿠키(과도한 `Domain` 설정이나 `SameSite` 누락)가 cross-site 요청에 함께 실려 CSRF를 가능하게 하는 경우, 또는 지나치게 좁게 설정된 쿠키가 서브도메인 인증을 조용히 실패시키는 경우.

## Part 2 — 조사 과제 + 실습 조사

### Part 2a — 개념 조사
다음을 조사하고 `ANSWER.md`에 설명하라:
1. `Set-Cookie: session=abc; Domain=example.com`으로 설정된 쿠키와 `Set-Cookie: session=abc`(`Domain` 없음 — host-only 쿠키)로 설정된 쿠키의 브라우저 동작 차이. 구체적으로: 각각 `sub.example.com`으로 전송되는가? 각각 `example.com` 자체로도 전송되는가?
2. `Path` 속성이 쿠키가 첨부되는 요청 경로를 어떻게 제한하는지, 구체적인 예시(예: `Path=/admin`)와 함께 설명하라.
3. "site"(eTLD+1, 예: `example.com`은 `app.example.com`과 `api.example.com`을 모두 포함)와 "origin"(scheme + host + port)의 정확한 차이, 그리고 `SameSite=Strict`, `SameSite=Lax`, `SameSite=None`이 각각 완전한 origin 일치가 아니라 "site" 개념을 사용하는 방식.

### Part 2b — 실습: DevTools 조사
1. 현재 로그인되어 있는 실제 웹사이트에서 DevTools를 연다.
2. Application 탭(Chrome) 또는 Storage 탭(Firefox)에서 Cookies 패널을 찾는다.
3. 쿠키 하나 이상을 선택하고 정확한 `Domain`, `Path`, `SameSite`, `Secure` 속성 값을 기록한다.
4. `ANSWER.md`에서, 그 사이트의 실제 구조(예: 로그인 상태를 공유해야 하는 서브도메인이 있는지)를 이해한 바에 근거하여, 그 쿠키에 대해 왜 이 네 가지 속성이 그렇게 설정되어 있을 가능성이 높은지 설명하라.

## Part 3 — Requirements & Edge Cases
- [ ] `Domain` 속성이 없는 host-only 쿠키가 서브도메인으로 전송되는지 명확히 서술할 것 — 추측하지 말고 명세/MDN을 통해 검증할 것.
- [ ] `Domain=example.com`으로 설정된 쿠키가 `example.com` 자체에도, `sub.example.com`에도 전송되는지 명확히 서술할 것.
- [ ] `Path` 범위 지정이 쿠키를 요청의 일부 하위 집합으로 제한하는 올바른 구체적 예시를 제시할 것.
- [ ] `SameSite` 설명에서 "site"와 "origin"을 혼동하지 말 것 — `SameSite`는 전체 origin이 아니라 등록 가능한 도메인(registrable domain)을 기준으로 판단한다는 점을 명시할 것 (따라서 `https://a.example.com`과 `https://b.example.com`은 cross-origin이지만 `SameSite` 관점에서는 same-site이다).
- [ ] DevTools 조사는 실제로 검사한 실제 쿠키를 근거로 해야 한다 — 사이트 이름을 밝히거나(민감하다면 일반적으로 서술) 실제 속성 값을 보고할 것, 추정값이 아니라.
- [ ] `Secure` 속성이 요구하는 것(HTTPS 전용 전송)과, 일반 HTTP도 함께 서빙하는 사이트에서 이 속성이 없는 쿠키가 왜 위험한지 설명하라.

## Submission
답안/코드는 `workspace/track-7-url-and-web/04-cookies-domain-scope-site-vs-origin/`에 작성하라. `NOTES.md`와 `ANSWER.md`를 포함할 것.
