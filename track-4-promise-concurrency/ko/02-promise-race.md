# [Track 4] Exercise 2: Promise.race

## Difficulty
Medium

## Learning Goal
`Promise.race`가 입력된 Promise들 중 **어느 것이든** 가장 먼저 settle되는 즉시 — 그것이 성공했든 실패했든 상관없이 — fulfilled 또는 rejected 상태로 settle된다는 것을 이해한다. `Promise.race`는 "가장 먼저 성공하는 것"을 의미하지 않는다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — 특정 하나의 Promise나 모든 Promise를 항상 기다리는 대신, 왜 가장 먼저 끝나는 것으로 settle되는 프리미티브가 필요한가?
2. Why Origin? — `Promise.race`의 존재를 동기부여한 실제 분산/병렬 요청 시나리오(예: 실제 네트워크 호출과 `setTimeout` 기반 타이머 Promise를 경쟁시켜 요청 타임아웃을 구현하는 경우, 또는 여러 미러링된 서버에 질의하여 먼저 응답하는 것을 사용하는 경우)를 예로 들어 설명하라.
3. When to Use? — `Promise.race`가 적합한 구체적인 프로덕션 시나리오를 설명하라 (구체적으로 — 예: 타임아웃 패턴).
4. What if Absent? — 어떤 주니어 엔지니어가 `Promise.race`는 항상 가장 빠른 *성공적인* 결과를 준다고, "가장 먼저 끝나는 것이 이기며, 그것이 좋은 결과이기만 하면"이라고 가정했다고 하자. 이 오해는 타임아웃 패턴 구현에서 어떤 버그를 일으키며, 빠른 작업이 먼저 실패해버리는 경우 실제로 무슨 일이 일어나는가?

## Part 2 — 코딩 과제
`setTimeout`으로 지연을 흉내내는 다음 mock 비동기 작업들을 만들어라:

1. "느린 성공(slow success)": 긴 지연 후 값으로 resolve.
2. "빠른 실패(fast failure)": 짧은 지연 후 `Error`로 reject.
3. `Promise.race([slowSuccess(), fastFailure()])`로 이 둘을 경쟁시켜라. 결과를 예측한 뒤 관찰하라 — fulfilled인가, rejected인가? 어떤 값/이유를 갖는가?

이어서 대칭되는 케이스를 만들어라:

4. "빠른 성공(fast success)": 짧은 지연 후 값으로 resolve.
5. "느린 실패(slow failure)": 긴 지연 후 `Error`로 reject.
6. 이 둘을 경쟁시켜라. 결과를 예측한 뒤 관찰하라.

마지막으로, 실제 사용 사례를 보여주는 **세 번째 시나리오**를 작성하라: mock "네트워크 요청" Promise(무작위 또는 고정 지연 후 resolve)를 "타임아웃" Promise(`setTimeout`을 통해 고정 지연 후 reject)와 경쟁시키고, `.then()`/`.catch()`에서 두 결과(정상 응답 vs. 타임아웃)를 서로 다르게 처리하라.

## Part 3 — 요구사항 및 엣지 케이스
- [ ] 케이스 1(느린 성공 vs. 빠른 실패)이 구현되어 있고, `ANSWER.md`/`NOTES.md`에 결과가 fulfillment가 아니라 **rejection**이었음이 명확히 서술되어 있으며, "성공 여부"가 아니라 settle 순서를 근거로 그 이유를 설명한다.
- [ ] 케이스 2(빠른 성공 vs. 느린 실패)가 구현되어 있고, 결과(fulfillment)도 같은 방식으로 설명된다 — 이번에는 우연히 성공한 것이 먼저 settle되었을 뿐이다.
- [ ] 노트에 흔한 주니어의 오해를 명확히 짚는다: "`Promise.race`는 가장 먼저 성공하는 결과를 반환한다"는 **거짓**이다 — 성공이든 실패든 가장 먼저 settle되는 것을 반환한다.
- [ ] 타임아웃 패턴 시나리오가 구현되어 있고, 처리 코드에서 "실제 응답을 받은 경우"와 "타임아웃된 경우"를 서로 다른 두 개의 경로로 올바르게 구분한다.
- [ ] race에서 진 mock Promise들도 백그라운드에서 계속 실행되도록 둔다(취소를 시도하지 않는다) — `Promise.race` 역시 진 쪽의 Promise를 취소하지 않는다는 것을 `NOTES.md`에 기록하라.

## Submission
`workspace/track-4-promise-concurrency/02-promise-race/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변)와 `ANSWER.md`(두 경쟁 각각에 대한 예측 순서와 근거를 먼저 작성한 뒤 실행으로 검증하고 맞았는지 기록), 그리고 `solution.js`를 포함해야 한다.
