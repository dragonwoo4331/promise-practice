# [Track 6] Exercise 1: 기본 Fetch와 수동 Status Code 처리

## Difficulty
Easy

## Learning Goal
`fetch()` Promise가 정확히 언제 reject되고 언제 resolve되는지 이해하고,
수동 HTTP status 확인을 올바르게 구현합니다 — `fetch()`는 네트워크 수준의
실패에서만 reject되며, 4xx/5xx 응답에서도 정상적으로 resolve됩니다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
- https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch
- https://developer.mozilla.org/en-US/docs/Web/API/Response
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

## Part 1 — 코드 작성 전에 이해하기

코드를 작성하기 **전에** 아래 질문에 대한 답을 본인 언어로 `NOTES.md`에
작성하십시오.

1. Why Necessary? — `fetch()` API는 왜 404나 500에서 자동으로 throw하지
   않고 `response.ok`/`response.status`를 수동으로 확인하도록 요구합니까?
   `fetch()`에게 "네트워크 실패"가 의미하는 것과 HTTP 에러 status code가
   의미하는 것이 어떻게 다른지, 그리고 스펙이 왜 이 둘을 다르게 취급하는지
   설명하십시오.
2. Why Origin? — `fetch()` 이전, 브라우저의 비동기 HTTP API는
   `XMLHttpRequest`였고 `onload`, `onerror`, `onreadystatechange` 같은
   콜백 기반 이벤트를 사용했습니다. `XMLHttpRequest`의 사용성(ergonomics)이
   구체적으로 무엇이 번거로웠는지 설명하십시오 — 예를 들어 "요청이 성공적으로
   끝났다"와 "요청이 에러 status로 끝났다"와 "요청이 연결에 실패했다"를
   신뢰성 있게 구분하기 위해 몇 개의 이벤트 리스너/상태 확인이 필요했는지
   설명하십시오.
3. When to Use? — `response.ok`를 수동으로 확인하지 않으면 버그가 발생하는
   구체적인 프로덕션 시나리오를 서술하십시오 (예: `fetch()`의 throw만이
   유일한 실패 신호라고 가정했다면 UI가 무엇을 보여주게 될지).
4. What if Absent? — 주니어 개발자가 `fetch(url).then(res => res.json())`
   만 작성하고 그 외에는 아무것도 하지 않는다면, 서버가 JSON 에러 바디를
   포함한 404를 반환하거나 HTML 에러 페이지를 포함한 500을 반환할 때
   구체적으로 어떤 재앙이 발생합니까?

"비동기 처리를 위해서" 같은 얕은 답변은 반려됩니다 — 실제 런타임 동작 원리를
근거로 답하십시오.

## Part 2 — Coding Task

이 트랙 전체에서 `https://jsonplaceholder.typicode.com`을 테스트 API로
사용하십시오 (세 exercise 모두 일관되게 사용).

다음을 수행하는 `async` 함수 `fetchPost(id)`를 작성하십시오.

1. `https://jsonplaceholder.typicode.com/posts/{id}`에 `GET` 요청을
   보냅니다.
2. 네트워크 요청 자체가 실패하면(예: DNS 실패, 오프라인) 그 reject를 호출자
   에게 그대로 전파합니다 — 조용히 삼키지 마십시오.
3. 응답은 완료되었지만 `response.ok`가 `false`인 경우(예: 존재하지 않는
   id를 요청하면 이 API는 `404`를 반환합니다), HTTP status code와 status
   text를 포함한 메시지를 가진 `Error`를 수동으로 만들어 `throw`하십시오.
4. 응답이 `ok`라면 JSON 바디를 파싱하여 반환하십시오.

그런 다음 `fetchPost(1)`(성공해야 함)과 `fetchPost(99999)`(작성한 수동
404 처리에 걸려야 함)을 호출하는 작은 드라이버 스크립트를 작성하고, 각
결과를 구분하여 로그로 남기십시오.

## Part 3 — Requirements & Edge Cases

- [ ] `fetchPost`는 non-2xx 응답이 곧 Promise reject를 의미한다고 절대
      가정하지 않고, `response.ok` 또는 `response.status`를 명시적으로
      확인합니다.
- [ ] 잘못된 status에 대해 수동으로 던진 에러의 메시지에 숫자 status
      code가 포함되어 있습니다.
- [ ] 실제 네트워크 실패(존재하지 않는 호스트, 예:
      `https://this-domain-does-not-exist-xyz.test`로 URL을 지정하여
      시뮬레이션 가능)는 드라이버 스크립트의 로그에서 HTTP 에러 status와
      구분되어 처리됩니다 — 두 실패 모드가 서로 혼동되어서는 안 됩니다.
- [ ] 성공 경로는 유효한 post id에 대해 파싱된 JSON 객체를 올바르게
      반환합니다.
- [ ] `NOTES.md`는 `fetch()`가 반환하는 Promise가 정확히 어떤 조건에서
      reject되는지 한 문장으로 명시적으로 서술합니다.

## Submission

`workspace/track-6-fetch-devtools/01-fetch-status-codes/`에 답안/코드를
작성하십시오. `NOTES.md`와 `solution.js`를 포함하십시오.
