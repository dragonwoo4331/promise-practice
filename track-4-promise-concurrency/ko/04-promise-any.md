# [Track 4] Exercise 4: Promise.any

## Difficulty
Hard

## Learning Goal
`Promise.any`가 입력 Promise들 중 **어느 하나라도** fulfill되는 즉시 resolve되며(그 과정에서 발생한 rejection들은 무시하며), **모든** 입력이 reject되었을 때만 reject된다는 것 — 이 경우 `.errors` 속성에 입력 순서대로 모든 개별 rejection 이유가 담긴 배열을 가진 `AggregateError`로 reject된다는 것을 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AggregateError
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — 개별 rejection들은 무시하고 오직 첫 번째 성공만을 신경 쓰는 프리미티브가 왜 유용한가 — 이것이 `Promise.race`나 `Promise.all`이 줄 수 없는 무엇을 제공하는가?
2. Why Origin? — `Promise.any`의 존재를 동기부여한 실제 분산/병렬 요청 시나리오(예: 여러 개의 중복/미러링된 API 엔드포인트나 CDN에 질의하여, 일부가 다운되어 있는 것을 감수하면서 실제로 성공적으로 응답하는 첫 번째 것에 만족하는 경우)를 예로 들어 설명하라.
3. When to Use? — `Promise.race`가 아니라 `Promise.any`가 명백히 옳은 선택인 구체적인 프로덕션 시나리오를 설명하라 (`race`가 왜 그 상황에서 틀린 선택인지 구체적으로 서술하라 — 힌트: 바로 뒤이어 성공이 오고 있더라도 `race`는 빠른 실패로 settle되어 버린다).
4. What if Absent? — `Promise.any`의 존재를 몰랐고 "첫 성공, 일부 실패는 허용"이라는 의미가 필요했다면, 어떤 더 복잡한 우회 방법을 직접 구현해야 했을 것이며, 그 과정에서 주니어 엔지니어가 저지르기 쉬운 실수는 무엇인가(예: 실수로 `Promise.race`를 대신 사용했다가 이른 실패에 발목 잡히는 경우)?

## Part 2 — 코딩 과제
(`setTimeout`을 사용하는) mock 비동기 작업을 최소 4개 만들되, 결과와 지연 시간을 다음과 같이 섞어라:

- 최소 2개는 reject된다.
- 최소 1개는 결국 fulfill된다(그 지연 시간이 일부 reject되는 작업들보다 길어도 상관없다 — 오히려 그것이 핵심이다).

1. 이를 `Promise.any`에 전달해 실행하고, 시간순으로 하나 이상의 rejection이 먼저 발생했음에도 불구하고 fulfill된 작업의 값으로 resolve됨을 확인하라.
2. 입력된 작업이 **모두** reject되는 **별도의 두 번째 시나리오**를 만들어라. 이 배치를 `Promise.any`에 전달하고 결과를 catch하여, 잡힌 에러가 `AggregateError`의 인스턴스임을 확인/관찰하라.
3. 그 `AggregateError`의 `.errors` 속성을 확인하고 로그로 남겨라 — 이것이 배열임을 확인하고, 길이가 실행한 작업의 개수와 일치함을 확인하며, 내부 이유들의 순서가 (실제로 reject된 순서가 아니라) 입력 순서와 대응됨을 확인하라.

## Part 3 — 요구사항 및 엣지 케이스
- [ ] "결국 성공하는" 시나리오에는 최종 성공보다 시간상 *먼저* settle되는 rejection이 최소 하나 포함되어 있어, `Promise.any`가 이를 무시하고 계속 기다린다는 것을 증명한다.
- [ ] "모두 실패하는" 시나리오는 별개의, 독립된 작업 배치이다 (혼합 배치를 재사용하며 우연히 맞아떨어지길 기대하지 말 것).
- [ ] `error instanceof AggregateError`(또는 동등한 검사)를 명시적으로 확인하고 그 결과를 로그로 남긴다.
- [ ] `error.errors`를 명시적으로 로그로 남기고 확인하며, 이것이 배열이고 그 길이가 reject된 입력의 개수와 같음을 확인한다.
- [ ] `NOTES.md`에 `Promise.any`가 reject되는 유일한 조건 — 모든 입력이 하나도 빠짐없이 reject되었을 때 — 이 자신의 언어로 명확히 서술되어 있다.

## Submission
`workspace/track-4-promise-concurrency/04-promise-any/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변)와 `solution.js`를 포함해야 한다.
