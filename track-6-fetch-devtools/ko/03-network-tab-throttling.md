# [Track 6] Exercise 3: Network 탭과 Throttling 조사

## Difficulty
Medium

## Learning Goal
Chrome DevTools의 Network 탭에서 실제 요청의 타이밍 워터폴(waterfall)을
읽고 해석하며, DevTools가 보여주는 네트워크 수준의 status와 여러분의
JavaScript 코드가 실제로 관찰하는 것 사이의 차이, 그리고 브라우저 캐시가
이 둘을 어떻게 바꾸는지 이해합니다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview

(DevTools UI 자체는 MDN에서 동일한 방식으로 문서화되어 있지 않지만, 캐싱과
status code 같은 기반 HTTP 동작 원리는 문서화되어 있습니다. 이 페이지들을
사용하여 여러분의 관찰을 실제 프로토콜에 근거하게 하십시오.)

## Part 1 — 조사 전에 이해하기

조사를 시작하기 **전에** 아래 질문에 대한 답을 본인 언어로 `NOTES.md`에
작성하십시오.

1. Why Necessary? — 왜 정상적이고 빠른 로컬 네트워크 조건에서만 fetch
   코드를 테스트하는 것으로는 충분하지 않습니까? 실제 지연 시간에서만
   드러나는 버그의 종류(race condition, 성급한 UI 업데이트, 누락된 로딩
   상태 등)를 설명하십시오.
2. Why Origin? — 브라우저 DevTools에 throttling 기능이 있는 전용 Network
   탭이 생기기 전, `XMLHttpRequest` 기반 코드를 디버깅하던 개발자들은
   수동 `console.log` 타임스탬프나 외부 프록시 도구에 의존해야 했습니다.
   시각적인 워터폴 없이는 무엇을 진단하기 어려웠는지 구체적으로 설명하십시오
   (예: "느린 DNS"와 "느린 서버 처리"와 "느린 다운로드"를 구분하는 문제).
3. When to Use? — 기능을 배포하기 전에 DevTools에서 의도적으로 네트워크를
   throttle하는 구체적인 프로덕션 시나리오를 서술하십시오.
4. What if Absent? — 어떤 팀이 throttle된/느린 네트워크 조건에서 전혀
   테스트하지 않았다면, 열악한 모바일 연결을 사용하는 실제 사용자에게
   구체적으로 어떤 재앙이 배포되겠습니까 (예: fetch에 의존하는 UI가 5초
   이상 동안 어떻게 보일지)?

"비동기 처리를 위해서" 같은 얕은 답변은 반려됩니다 — 실제 런타임 동작 원리를
근거로 답하십시오.

## Part 2 — 조사 과제 (Solution Code 불필요)

이 exercise에는 `solution.js`가 없습니다. 문서화된 조사입니다.

1. Chrome DevTools를 열고 **Network** 탭으로 이동하십시오.
2. Throttling을 **Slow 3G**로 설정하십시오 (throttling 드롭다운 또는
   Network Conditions 패널에서).
3. DevTools **Console**에서 `https://jsonplaceholder.typicode.com/posts/1`
   (또는 Exercise 1/2에서 작업한 다른 엔드포인트)에 대해 `fetch()` 호출을
   실행하고 Network 탭에 나타나는 항목을 확인하십시오.
4. 해당 요청의 **Timing** 탭을 클릭하여 워터폴을 읽으십시오.
5. 페이지를 (동일한 요청이 다시 발생하도록, 예: script 태그나 fetch를
   다시 실행하는 방식으로) 한 번은 일반적으로, 한 번은 "Disable cache"를
   체크 해제한 상태로 리로드하여 캐시된 응답을 관찰하십시오.

## Part 3 — Requirements & Edge Cases

`ANSWER.md`에는 다음이 (가상의 값이 아니라 본인의 실제 DevTools 세션에서
얻은 실제 밀리초 수치와 함께) 구체적으로 문서화되어야 합니다.

- [ ] Throttle된 요청 하나에 대한 전체 타이밍 워터폴 분석: DNS Lookup,
      Initial Connection(해당 시 SSL 포함), Time to First Byte
      (Waiting/TTFB), Content Download — Timing 탭에 표시된 각 단계의
      대략적인 소요 시간과 함께.
- [ ] Network 탭의 Status 컬럼에서 DevTools가 보여주는 HTTP status와,
      동일한 요청에 대해 여러분의 JavaScript `fetch()` 코드가
      `response.status`/`response.ok`로 관찰하는 것을 비교하십시오 —
      이 둘이 언제/어떻게 다를 수 있는지 설명하십시오 (예: DevTools는
      "finished"로 표시하지만 Exercise 1에 따라 여러분의 코드는 여전히
      수동으로 확인해야 하는 요청).
- [ ] 리로드 후 Size 컬럼에 `(disk cache)` 또는 `(memory cache)`가 표시된
      항목을 최소 하나 관찰하고, 각각이 무엇을 의미하는지 설명하십시오
      (응답이 어디서 왔는지, 네트워크 요청이 애초에 전송되었는지 여부, 그리고
      원본 캐시되지 않은 요청과 비교했을 때 해당 항목의 워터폴에 표시되는
      타이밍에 이것이 어떤 영향을 미치는지).
- [ ] Slow 3G와 throttling 없음 조건에서 동일한 엔드포인트에 대한 총 요청
      시간 비교 (본인이 직접 테스트한 실제 숫자 두 개).

## Submission

`workspace/track-6-fetch-devtools/03-network-tab-throttling/`에 조사
내용을 작성하십시오. `NOTES.md`(Part 1 답변)와 `ANSWER.md`(조사 결과 —
이 exercise는 `solution.js`가 필요 없습니다)를 포함하십시오.
