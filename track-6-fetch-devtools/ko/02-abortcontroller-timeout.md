# [Track 6] Exercise 2: `AbortController`와 수동 Fetch 타임아웃

## Difficulty
Medium

## Learning Goal
`AbortController`를 사용하여 `fetch()`에 수동 요청 타임아웃을 구현하고,
그 결과로 발생하는 `AbortError`를 다른 종류의 fetch 실패(네트워크 에러,
HTTP 에러 status)와 정확히 구분합니다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/AbortController
- https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
- https://developer.mozilla.org/en-US/docs/Web/API/DOMException

## Part 1 — 코드 작성 전에 이해하기

코드를 작성하기 **전에** 아래 질문에 대한 답을 본인 언어로 `NOTES.md`에
작성하십시오.

1. Why Necessary? — `fetch()`에는 내장된 `timeout` 옵션이 없습니다.
   `.abort()`가 호출될 때 `AbortController`/`AbortSignal`이 진행 중인
   `fetch()` 호출에 실제로 무엇을 하는지 설명하고, 단순히 rejecting하는
   타이머를 사용한 `Promise.race`가 아니라 이 메커니즘이 (단순히 기다리는
   것을 멈추는 것이 아니라) 기반 네트워크 요청 자체를 *취소*하는 올바른
   방법인 이유를 설명하십시오.
2. Why Origin? — `XMLHttpRequest`는 요청 객체 자체에 내장된 `.abort()`
   메서드를 가지고 있었습니다. `fetch()`의 설계(별도의 `AbortController`
   객체를 만들고 그 `.signal`을 `fetch()` 옵션에 전달하는 방식)가 왜 다르게
   구조화되었는지, 그리고 이 분리가 어떤 문제를 해결하는지 설명하십시오
   (힌트: 하나의 controller로 여러 관련 요청을 함께 취소하는 상황을
   생각해보십시오).
3. When to Use? — 타임아웃이 없는 fetch 요청이 사용자를 멈춰있게 만드는
   구체적인 프로덕션 시나리오를 서술하십시오 (예: 느리거나 멈춘 백엔드,
   요청 도중 끊기는 모바일 네트워크).
4. What if Absent? — 어떤 코드베이스가 타임아웃을 위해 `AbortController`를
   전혀 사용하지 않았다면, 키 입력마다 fetch를 발생시키는 UI(예:
   search-as-you-type 검색창)에서 응답이 순서가 뒤바뀌어 도착하거나
   백엔드가 가끔 무한정 멈출 때 구체적으로 어떤 재앙이 발생합니까?

"비동기 처리를 위해서" 같은 얕은 답변은 반려됩니다 — 실제 런타임 동작 원리를
근거로 답하십시오.

## Part 2 — Coding Task

다음을 수행하는 `async` 함수 `fetchWithTimeout(url, timeoutMs)`를
작성하십시오.

1. `AbortController`를 생성하고 그 `signal`을 `fetch(url, { signal })`에
   전달합니다.
2. `timeoutMs` 밀리초짜리 타이머를 시작합니다. fetch가 settle되기 전에
   타이머가 먼저 발동하면 controller의 `.abort()`를 호출합니다.
3. fetch가 타임아웃 발동 전에 (성공이든 실패든) settle되면 타이머를
   clear하여 프로세스/탭에 타이머가 남아있지 않도록 합니다.
4. 타임아웃 때문에 fetch가 abort된 경우, 그것이 명확히 타임아웃임을 알 수
   있는 에러를 던지거나 전파하십시오 — 잡아낸 에러의 `name` 속성을
   확인하고(`"AbortError"`일 것입니다) 호출자가 `AbortError`의 내부 동작을
   알 필요가 없도록 더 명확한 에러(예:
   `new Error("Request timed out after {timeoutMs}ms")`)를 다시
   던지십시오.
5. fetch가 다른 이유(네트워크 실패, non-ok status — Exercise 1의 수동
   status 확인을 재사용하십시오)로 실패하면, 그 에러는 변경 없이 전파되어
   타임아웃 에러와 구분되어야 합니다.

`https://jsonplaceholder.typicode.com/posts/1`에 대해 넉넉한 타임아웃으로
테스트하십시오(성공해야 함). 그리고 직접 만든 느린 엔드포인트에 대해서도
테스트하십시오 — `https://httpbin.org/delay/5`는 5초 후에 응답을 반환하므로
`fetchWithTimeout("https://httpbin.org/delay/5", 3000)`을 호출하면
타임아웃이 발생해야 합니다.

## Part 3 — Requirements & Edge Cases

- [ ] controller의 `signal`이 실제로 `fetch()` 호출에 연결되어 있습니다 —
      단순히 생성만 하고 무시되지 않습니다.
- [ ] 타임아웃 타이머는 성공/실패 경로 모두에서 clear됩니다 (누수되는
      `setTimeout`이 없습니다).
- [ ] 타임아웃은 명확히 식별 가능한 에러를 만듭니다 (내부적으로
      `error.name === "AbortError"`로 확인한 뒤, 호출자를 위해 명확하고
      설명적인 에러로 다시 던집니다).
- [ ] 타임아웃이 아닌 네트워크 실패와 non-ok HTTP status도 여전히 처리되고,
      드라이버 스크립트의 로그에서 타임아웃 케이스와 구분됩니다.
- [ ] 빠른 엔드포인트에 넉넉한 타임아웃으로 `fetchWithTimeout`을 호출하면
      성공하고 파싱된 데이터를 반환합니다.
- [ ] `httpbin.org/delay/5`에 3초 타임아웃으로 `fetchWithTimeout`을
      호출하면 일반적인 에러가 아니라 타임아웃 전용 에러가 로그로
      남습니다.

## Submission

`workspace/track-6-fetch-devtools/02-abortcontroller-timeout/`에 답안/코드를
작성하십시오. `NOTES.md`와 `solution.js`를 포함하십시오.
