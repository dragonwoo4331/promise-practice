# [Track 4] Exercise 1: Promise.all

## Difficulty
Medium

## Learning Goal
`Promise.all`이 여러 Promise를 동시에 실행하고, **모두** fulfill되었을 때만 결과 배열을 입력 순서대로 resolve한다는 것, 그리고 fail-fast 방식으로 동작하여 — 첫 번째 rejection이 발생하는 즉시 그 이유로 바로 reject되며, 아직 pending 중이거나 이미 fulfill된 배치 내 다른 결과들은 버려진다는 것을 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — 여러 Promise를 순차적으로 하나씩 await하는 대신, 왜 `Promise.all`과 같은 전용 동시성(concurrency) 프리미티브가 필요한가?
2. Why Origin? — `Promise.all`은 실제 시스템이 독립적인 여러 네트워크/데이터베이스/디스크 요청을 (하나씩이 아니라) 동시에 보내야 하는 경우가 흔하기 때문에 존재한다 (예: 사용자의 프로필, 설정, 알림을 병렬로 가져오기). 실제 분산/병렬 요청 시나리오를 근거로, 순차적으로 await하는 방식이 왜 그 상황에서 받아들여질 수 없는지 설명하라.
3. When to Use? — `allSettled`, `race`, `any`가 아니라 특별히 `Promise.all`을 사용해야 하는 구체적인 프로덕션 시나리오 — 즉, 부분적인 결과 집합은 전혀 쓸모가 없고 반드시 *모든* 결과가 필요한 경우 — 를 설명하라.
4. What if Absent? — `Promise.all`의 fail-fast 동작을 모르고, 실패한 것에 대한 에러와 함께 성공한 것들의 결과도 함께 받을 수 있을 것이라고 가정했다면, 어떤 프로덕션 버그를 작성하게 되며, 실제로는 무슨 일이 일어나는가?

## Part 2 — 코딩 과제
`setTimeout`으로 지연을 흉내내며 Promise를 반환하는 mock 비동기 작업 함수들을 작성하고, 이를 `Promise.all`과 함께 사용하라.

1. 결국 모두 resolve되는(서로 다른 지연 시간을 가진) mock 작업을 최소 3개 만들어 `Promise.all`에 전달하고, 결과 배열을 로그로 남겨라. 결과 배열의 순서가 실제로 완료된 순서가 아니라 Promise를 전달한 순서와 일치함을 확인하라.
2. 3개 이상의 작업 중 하나가 (반드시 가장 먼저나 가장 나중이 아니라, 중간 어딘가의 지연 시간으로) reject되는 두 번째 변형을 만들어라. 다음을 예측한 뒤 관찰하라:
   - `.catch()`(또는 `await Promise.all(...)`을 사용한다면 `try/catch`)가 받는 값/이유는 무엇인가.
   - 성공적으로 resolve된 작업들의 결과를 어디에서든 접근할 수 있는가, 아니면 `Promise.all`의 반환값 관점에서는 사실상 "사라진" 것인가.
3. 각 mock 작업의 `.then()` 내부(또는 내부 `setTimeout`이 실행된 직후)에 로그문을 추가하여, `Promise.all`이 이미 reject된 이후에도 다른 작업들이 계속 완료까지 실행됨을 콘솔에서 관찰하라 — 즉, 빠른 reject가 다른 진행 중인 작업들을 취소시키지 않는다는 것을 확인하라.

## Part 3 — 요구사항 및 엣지 케이스
- [ ] 모두 성공하는 변형에서, 지연 시간이 제각각인(모두 동일하지 않은) 동시 mock 작업이 최소 3개 사용되었다.
- [ ] 정확히 하나의 작업이 reject되고, 그 reject가 시간순으로 가장 먼저 settle되는 것이 아닌 별도의 fail-fast 변형이 존재한다.
- [ ] 노트/출력에서 `Promise.all`의 reject가 아직 pending 중인 것들을 기다리지 않고 reject하는 Promise가 settle되는 즉시 발생함을 명확히 보여준다.
- [ ] 노트/출력에서 reject되지 않은 작업들의 fulfilled 값이 `Promise.all`이 만들어내는 rejection reason 어디에도 존재하지 않으며, 단 하나의 rejection reason만 얻게 된다는 것을 명확히 확인한다.
- [ ] `Promise.all`이 이미 reject된 뒤에도 다른 mock 작업들이 백그라운드에서 자신의 작업을 계속 완료함을 (로그를 통해) 시연한다 — `Promise.all`이 입력들을 취소하거나 중단시키지 않는다는 점을 `NOTES.md`에 명시하라.

## Submission
`workspace/track-4-promise-concurrency/01-promise-all/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변)와 `solution.js`를 포함해야 한다.
