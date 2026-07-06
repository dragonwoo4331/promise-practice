# [Track 7] Exercise 1: URL의 구조와 URL API

## Difficulty
Easy

## Learning Goal
`URL` 인터페이스가 URL 문자열을 구조적 구성 요소로 어떻게 파싱하는지 이해하고, `host`와 `hostname`, `href`와 `origin`의 차이를 정확히 구분한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/URL
- https://developer.mozilla.org/en-US/docs/Web/API/URL/URL
- https://developer.mozilla.org/en-US/docs/Web/URI

## Part 1 — 코드 작성 전에 이해하기
아래 질문에 코드를 작성하기 전, `NOTES.md`에 자신의 언어로 답하라. 피상적인 답변은 반려된다 — 막연한 용어가 아니라 구체적인 동작 원리를 근거로 답하라.

1. **Why Necessary?** 왜 플랫폼은 개발자가 `split('/')`, `split('?')`, 정규식으로 URL을 직접 파싱하게 두지 않고 전용 `URL` 객체를 제공하는가?
2. **Why Origin?** URL은 형식 명세(RFC 3986 / WHATWG URL Standard)로 정의된 길고 복잡한 문법(scheme, userinfo, host, port, path, query, fragment)을 가진다. 왜 각 사이트나 라이브러리가 자체 파싱 방식을 만드는 대신 브라우저는 엄격한 파싱 알고리즘을 표준화했는가?
3. **When to Use?** 실제 운영 환경에서 `URL` 생성자를 사용해 들어오는 URL을 분해해야 하는 구체적인 시나리오를 서술하라 (예: 리다이렉트 대상 검사, 웹훅 콜백 URL 검증, 요청이 도달한 pathname 로깅 등).
4. **What if Absent?** 문자열을 수동으로 분리하는 파싱 방식이 일으키는 구체적인 버그를 서술하라 — 예를 들어 해시 프래그먼트 안에 `?`가 포함된 URL, 또는 userinfo 안에 `@`가 포함된 경로가 단순한 `split('@')`나 `split('?')` 방식을 어떻게 망가뜨리는지.

## Part 2 — 코딩 과제
다음 URL 문자열이 주어졌을 때:

```
https://user:pass@sub.example.com:8443/path/to/page?foo=1&bar=two#section-2
```

다음을 수행하는 스크립트를 작성하라.

1. 이 문자열로부터 `URL` 객체를 생성한다.
2. 다음 속성들을 각각 별도의 줄에 명확한 레이블과 함께 출력한다: `protocol`, `username`, `password`, `hostname`, `host`, `port`, `pathname`, `search`, `searchParams` (반복 가능한 형태로), `hash`, `origin`, `href`.
3. 출력한 실제 값을 근거로, `host`에는 있지만 `hostname`에는 없는 내용이 무엇인지 간단히 설명한다 (주석 또는 콘솔 출력으로).
4. 출력한 실제 값을 근거로, `href`에는 있지만 `origin`에는 없는 내용이 무엇인지 간단히 설명한다.
5. 추가로, 두 인자를 받는 형태(`new URL(relativePath, base)`)를 사용해 `"https://example.com/store/products/item42"`를 기준으로 `"../images/logo.png"`를 해석(resolve)하는 두 번째 `URL`을 생성한다. 결과 `href`를 출력하고, 왜 그런 결과가 나오는지(상대 경로 해석 규칙) 주석으로 설명하라.

## Part 3 — Requirements & Edge Cases
- [ ] `URL` 생성자를 사용할 것 — 원본 URL 문자열에 대한 수동 문자열 분리, 정규식 파싱, `indexOf` 조작을 사용하지 말 것.
- [ ] `host`는 포트를 포함하고(`sub.example.com:8443`), `hostname`은 포트를 포함하지 않음(`sub.example.com`)을 정확히 확인할 것.
- [ ] `origin`은 인증 정보, 경로, 쿼리, 해시를 절대 포함하지 않으며 scheme + host(+ 기본값이 아닌 포트)만 포함함을 정확히 확인할 것.
- [ ] 포트가 해당 scheme의 기본 포트인 경우(예: `https://example.com:443/`)를 처리하라 — 이 경우 `port`와 `host`가 각각 무엇을 반환하는지 확인하고 서술하라.
- [ ] 두 인자를 받는 `URL(relative, base)` 해석을 올바르게 시연하되, 상위 디렉터리로 올라가는(`../`) 경우를 포함할 것.
- [ ] 오류를 숨기지 말 것 — 잘못된 형식의 URL 문자열을 테스트한다면, `URL`이 던지는 오류(`TypeError`)가 무엇인지 보여주고 이를 어떻게 방어할지(예: `try/catch`) 서술하라.

## Submission
답안/코드는 `workspace/track-7-url-and-web/01-url-anatomy-and-url-api/`에 작성하라. `NOTES.md`와 `solution.js`를 포함할 것.
