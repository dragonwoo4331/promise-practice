# [Track 4] Exercise 3: Promise.allSettled

## Difficulty
Medium

## Learning Goal
`Promise.allSettled`가 (결과와 무관하게) 모든 입력 Promise가 settle될 때까지 기다린 뒤 `{ status, value }` 또는 `{ status, reason }` 형태의 결과 객체 배열로 resolve된다는 것, 그리고 입력 중 하나가 reject되었다고 해서 그 자체가 reject되는 일은 결코 없다는 것을 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — (`Promise.all`과 달리) 절대 통째로 실패하지 않는 프리미티브가 왜 유용한가 — "좋든 나쁘든 모든 결과를 달라"는 요구는 fail-fast 방식으로는 해결할 수 없는 어떤 문제를 해결하는가?
2. Why Origin? — 모든 요청 각각의 결과를 — 어떤 것이 실패했고 왜 실패했는지까지 포함해서 — 알아야 하고, 첫 번째 실패에서 전체를 중단시켜서는 안 되는 실제 분산/병렬 요청 시나리오(예: 동일한 배치 작업을 10개의 워커에 보내는 경우, 또는 대시보드를 위해 여러 서드파티 API에 핑을 보내는 경우)를 예로 들어 설명하라.
3. When to Use? — `Promise.all`보다 `Promise.allSettled`가 명백히 더 나은 선택인 구체적인 프로덕션 시나리오를 설명하라.
4. What if Absent? — `Promise.allSettled`가 존재하지 않았다면(혹은 그 존재를 몰랐다면), 어떤 우회 방법(예: 모든 Promise를 자체 then/catch로 감싸 결과를 정규화하는 방법)을 강제로 써야 했을 것이며, 이를 일관되게 적용하는 것을 잊었을 때의 고통/재앙은 무엇인가 — 예를 들어 `Promise.all`에서는 배치 내 단 하나의 unhandled rejection이 호출 코드 전체를 무너뜨릴 수 있다.

## Part 2 — 코딩 과제
(`setTimeout`을 사용하는) mock 비동기 작업을 최소 5개 만들되, 결과가 실제로 섞이도록 하라: 최소 2개는 resolve되고, 최소 2개는 reject되며, 지연 시간도 서로 엇갈리게 한다.

1. 이 작업들 전체를 `Promise.allSettled`에 전달해 실행하고, 결과 배열의 정확한 형태(`{status: 'fulfilled', value: ...}` / `{status: 'rejected', reason: ...}`)를 확인하기 위해 그대로 로그로 남겨라.
2. (서드파티 라이브러리를 사용하지 않고) 이 결과 배열을 처리하는 로직을 직접 작성하여, fulfilled 값만 모은 배열과 rejection 이유만 모은 배열, 두 개로 분리하라. 둘 다 각각 로그로 남겨라.
3. 이 분리 로직을 확장하여 간단한 집계 통계도 함께 보고하라: 전체 개수, 성공 개수, 실패 개수.

## Part 3 — 요구사항 및 엣지 케이스
- [ ] 배치에 최소 2개의 fulfilled와 최소 2개의 rejected 작업이 포함되어 있으며, 지연 시간이 엇갈려 있어 모두 입력 순서대로 settle되지는 않는다.
- [ ] `Promise.allSettled`의 원본 출력을 로그로 남기고, 분리 로직을 작성하기 전에 그 형태(`status` + `value`/`reason` 키)를 `NOTES.md`의 예측과 대조하여 확인했다.
- [ ] 분리 로직은 `value`/`reason`의 존재 여부만으로 판단하지 않고 (이는 취약하므로) `status === 'fulfilled'`와 `status === 'rejected'`를 정확히 구분한다.
- [ ] 집계 개수(전체/성공/실패)는 이미 알고 있는 입력값을 하드코딩한 것이 아니라 결과 배열 자체로부터 정확히 계산된다.
- [ ] `Promise.allSettled` 자체는 입력 중 얼마나 많은 것이 reject되든 절대 reject되지 않는다는 것을 명시적으로 확인하고 기록하라 — 전체 호출을 `.catch()`로 감싸고 그것이 결코 실행되지 않음을 증명하여 시연하라.

## Submission
`workspace/track-4-promise-concurrency/03-promise-allsettled/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변)와 `solution.js`를 포함해야 한다.
