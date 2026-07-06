# Track 7 — URL과 Web 기초

이 트랙은 URL을 다루는 웹 플랫폼의 핵심 메커니즘, 브라우저 보안 경계, 그리고 쿠키를 다룬다. 이는 학술적인 세부사항이 아니다 — 이들 중 하나라도 잘못 다루면 실제 버그(깨진 쿠리 문자열, 조용한 데이터 손실)나 실제 보안 취약점(CSRF, origin 간 데이터 유출)이 발생한다.

각 과제는 코드를 작성하거나 실습 과제를 수행하기 **전에** `NOTES.md`에 개념적 질문에 답할 것을 요구한다. 답변은 막연한 용어가 아니라 구체적인 동작 원리를 근거로 해야 한다. 코딩/조사 과제에는 절대 해답이 제공되지 않는다 — 100% 스스로 작성하거나 조사해야 한다.

## 과제 목록

1. **[01 — URL의 구조와 URL API](./01-url-anatomy-and-url-api.md)** (Easy)
   `URL` 생성자로 복잡한 URL을 파싱한다. `host` vs `hostname`, `href` vs `origin`을 구분한다.

2. **[02 — URLSearchParams 조작하기](./02-urlsearchparams-manipulation.md)** (Medium)
   `URLSearchParams`를 사용해 반복되는 쿼리 파라미터를 올바르게 추가, 수정, 삭제, 조회하며 percent-encoding을 정확히 처리한다.

3. **[03 — Origin, Domain과 Same-Origin Policy](./03-origin-domain-same-origin-policy.md)** (Medium)
   URL 쌍에 정확한 same-origin 정의를 적용하고, 자신의 브라우저에서 실제 CORS 오류를 발생시켜 분석한다.

4. **[04 — Cookie, Domain Scope와 Site vs Origin](./04-cookies-domain-scope-site-vs-origin.md)** (Hard)
   `Domain`과 `Path`가 쿠키 범위를 어떻게 결정하는지 이해하고, "site"와 "origin"을 구분하며, DevTools로 실제 쿠키를 조사한다.

## 참고 랜딩 페이지

- https://developer.mozilla.org/en-US/docs/Web/API/URL
- https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams
- https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

## 제출 규칙

각 과제의 결과물은 `workspace/track-7-url-and-web/0N-<slug>/`에 작성하며, `NOTES.md`와 `solution.js`(코딩 과제) 또는 `ANSWER.md`(조사/예측 과제)를 포함해야 한다.
