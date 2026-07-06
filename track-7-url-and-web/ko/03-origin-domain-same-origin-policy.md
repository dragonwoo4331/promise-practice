# [Track 7] Exercise 3: Origin, Domain과 Same-Origin Policy

## Difficulty
Medium

## Learning Goal
Same-Origin Policy의 정확한 정의(scheme + host + port가 모두 일치해야 함)를 적용해 두 URL이 같은 origin을 공유하는지 판단하고, 브라우저가 실제로 CORS를 차단하는 상황을 직접 관찰한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy
- https://developer.mozilla.org/en-US/docs/Glossary/Origin
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS/Errors

## Part 1 — 코드 작성 전에 이해하기
아래 질문에 실습 과제를 하기 전, `NOTES.md`에 자신의 언어로 답하라. 피상적인 답변은 반려된다 — 막연한 용어가 아니라 구체적인 동작 원리를 근거로 답하라.

1. **Why Necessary?** 브라우저는 왜 애초에 "Origin"이라는 개념이 필요한가 — 한 사이트에서 로드된 스크립트가 다른 사이트의 페이지/세션 데이터를 자유롭게 읽을 수 있다면 구체적으로 어떤 위협이 존재하는가?
2. **Why Origin?** Same-Origin Policy가 정확히 scheme + host + port(그 이상도 이하도 아님)를 검사하는 이유에 대한 역사적/보안적 근거를 서술하라 — 같은 도메인이라도 `http`와 `https`가 왜 서로 다른 origin인지, 도메인과 scheme이 같아도 포트가 다르면 왜 다른 origin인지 설명하라.
3. **When to Use?** Origin에 대해 실제로 고민해야 했던 구체적인 운영 시나리오를 서술하라 — 예를 들어 API 서브도메인으로의 `fetch()` 호출이 실패한 원인을 디버깅한 경험, `postMessage` 핸들러에 origin 검사가 필요한지 판단한 경험, CDN/자산 도메인을 구성한 경험 등.
4. **What if Absent?** Same-Origin Policy가 방지하는 구체적인 재앙적 상황을 서술하라 — 예를 들어 악성 페이지가 `fetch`로 로그인된 사용자의 은행 잔액을 조용히 읽어 외부로 유출하는 경우, 또는 악성 `iframe`이 부모 페이지의 DOM 데이터를 읽어가는 경우.

## Part 2 — 예측 과제 + 실습 조사

### Part 2a — 예측
아래 각 URL 쌍에 대해 **same-origin**인지 **cross-origin**인지 판단하고, 정확한 정의(scheme, host, port가 모두 일치해야 함)를 근거로 설명하라:

1. `https://app.example.com` vs `https://api.example.com`
2. `https://example.com` vs `http://example.com`
3. `https://example.com:443` vs `https://example.com:8443`
4. `https://example.com` vs `https://example.com:443`
5. `https://example.com/path/one` vs `https://example.com/path/two`
6. `https://EXAMPLE.com` vs `https://example.com`
7. `https://example.com` vs `https://www.example.com`

### Part 2b — 실습: CORS 오류 발생시키기
자신의 브라우저에서 실제 CORS 오류를 직접 발생시켜라:

1. 로컬 HTML 파일을 브라우저에서 직접 열거나(`file://`), 간단한 정적 서버로 `http://localhost`에서 서빙한다.
2. 그 페이지의 스크립트에서, 자신에게 관대한(permissive) CORS 헤더를 보내주지 않는 cross-origin API 엔드포인트에 `fetch()`를 호출한다 (자신의 origin에 대해 `Access-Control-Allow-Origin`을 반환하지 않는 공개 API를 확인하여 선택하라 — 몇 개 시도해봐야 할 수도 있다).
3. DevTools 콘솔을 열어 정확한 오류 메시지 텍스트를 캡처한다.
4. `ANSWER.md`에 실제 콘솔 오류 텍스트를 그대로 붙여넣고, 브라우저 스택의 어느 부분이 이 차단을 강제하는지 자신의 언어로 설명하라 (힌트: 서버가 요청을 완전히 거부하는 것이 아니다 — 응답은 보통 네트워크 레벨에서 실제로 도착한다).

## Part 3 — Requirements & Edge Cases
- [ ] Part 2a의 7개 쌍 각각에 대해 same-origin/cross-origin 여부와 scheme/host/port 중 어느 것이 다른지(있다면) 명시할 것.
- [ ] "same site"(eTLD+1)와 "same origin"을 혼동하지 말 것 — 설명에 "도메인"이라는 단어를 사용한다면 host를 의미하는지 등록 가능한 도메인(registrable domain)을 의미하는지 명확히 하라.
- [ ] 실습 CORS 오류는 실제로 재현된 오류여야 한다 — 의역이 아니라 실제 콘솔 텍스트를 붙여넣을 것.
- [ ] 실패한 요청이 Network 탭에 응답과 함께 나타나는지(대개 그렇다) `ANSWER.md`에 명시적으로 서술할 것 — 이는 서버는 응답했지만 브라우저가 스크립트의 응답 접근을 차단했다는 증거이다.
- [ ] `file://`로 페이지를 열면 origin이 `null`(또는 고유한 opaque origin)이 되는 이유와 이것이 CORS 동작에 어떤 영향을 미치는지 설명하라.

## Submission
답안/코드는 `workspace/track-7-url-and-web/03-origin-domain-same-origin-policy/`에 작성하라. `NOTES.md`와 `ANSWER.md`를 포함할 것.
