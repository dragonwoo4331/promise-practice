# [Track 2] Exercise 2: 미니 Task Queue 로거 만들기

## Difficulty
Medium

## Learning Goal
"현재 큐잉된 모든 microtask는 다음 macrotask가 시작되기 전에 실행된다"는 주장을, 추측이 아니라 직접 계측(instrumentation)을 통해 검증한다 — microtask를 비우는 도중(draining)에 새로 스케줄링된 microtask 역시 다음 macrotask 전에 실행된다는 미묘한 지점까지 포함해서 (즉 Microtask Queue는 "한 번 실행"되는 게 아니라 완전히 빌 때까지 처리된다).

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/setTimeout
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then

## Part 1 — Understand Before You Code

아래 질문에 코드를 작성하기 전에 `NOTES.md`에 본인의 언어로 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 거부된다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. **Why Necessary?** — 순서에 대한 문서상의 주장을 그냥 "믿는" 것으로 충분하지 않은 이유는 무엇인가 — 엔지니어가 실제 환경에서 Event Loop 동작을 직접 계측하고 검증할 수 있어야 하는 이유는 무엇인가?
2. **Why Origin?** — 어떤 역사적 혼란이나 버그 유형(예: 개발자들이 `setTimeout(fn, 0)`이 "즉시" 실행된다고 가정하거나, `.then()` 체인이 동기적으로 실행된다고 가정하는 것)이 주니어 엔지니어에게 이런 검증 연습을 필요하게 만드는가?
3. **When to Use?** — 상태 업데이트가 예약된 타이머 콜백보다 먼저 반영되는지 나중에 반영되는지를 정확히 판단해야 하는 프로덕션 시나리오(예: 디바운싱, UI 업데이트 배치 처리, `requestAnimationFrame`과의 조율)를 설명하라.
4. **What if Absent?** — 만약 개발자가 `setTimeout(fn, 0)`이 대기 중인 promise 콜백보다 먼저 실행된다고 잘못 가정한다면, 실제 코드에서 어떤 구체적인 버그(예: 오래된 상태를 읽는 것, UI 업데이트의 경쟁 상태)가 발생할 수 있는가?

## Part 2 — Prediction Quiz or Coding Task

`[sync]`, `[microtask]`, `[macrotask]` 태그를 붙여 모든 로그 줄에 어떤 큐 유형에서 나온 것인지 한눈에 보이도록 계측된 작은 스크립트를 작성하라.

스크립트는 다음 조건을 포함해야 한다:
- 서로 다른 지연 시간(예: `0`과 `10`)을 가진 최소 2개의 `setTimeout` 호출(macrotask).
- 최소 2개의 별도 `.then()` 체인, 그중 최소 하나는 3개 이상 연결(예: `.then().then().then()`).
- `.then()` 콜백 중 최소 하나는 그 안에서 *새로운* microtask를 스케줄링해야 한다(예: `queueMicrotask`를 호출하거나 또 다른 resolved Promise를 반환) — 이것은 "비우는 도중 새로 큐잉된 microtask"가 여전히 다음 macrotask보다 먼저 실행되는지를 테스트하는 핵심 케이스다.

아무것도 실행하기 전에, 예상되는 전체 출력(정확한 순서와 태그 포함)을 `ANSWER.md`에 작성하고 각 줄에 대한 근거를 함께 적어라.

## Part 3 — Requirements & Edge Cases

- [ ] 모든 로그 줄은 어떤 큐 유형에서 생성되었는지를 반영하는 `[sync]`, `[microtask]`, `[macrotask]` 태그가 붙어 있어야 한다.
- [ ] 특정 엣지 케이스를 포함하라: 자기 자신 내부에서 또 다른 microtask를 스케줄링하는 microtask — 그 중첩된 microtask가 첫 번째 macrotask가 실행되기 전에 여전히 실행되는지 예측하고 검증하라.
- [ ] "모든 microtask는 항상 다음 macrotask보다 먼저 실행된다"는 주장을 명시적으로 서술한 뒤 테스트하라. 당신의 실험은 이 주장을 확인해 주는가, 아니면 복잡하게 만드는가? 두 개의 `setTimeout(fn, 0)` 호출이 모두 대기 중이라면, 모든 microtask가 *둘 다*보다 먼저 실행되는가, 아니면 첫 번째보다만 먼저 실행되는가?
- [ ] 실행 후 `ANSWER.md`에 짧은 결론을 작성하라: 관찰 결과를 바탕으로 그 주장이 완전히 참인지, 조건부로 참인지, 거짓인지. 실제 태그가 붙은 출력으로 근거를 제시하라.

## Submission
답과 코드를 `workspace/track-2-event-loop/02-mini-task-queue-logger/`에 작성하라. `NOTES.md`(Part 1 답변), `solution.js`(계측된 스크립트), `ANSWER.md`(예상 출력, 실제 출력, 순서 주장에 대한 결론)를 포함해야 한다.
