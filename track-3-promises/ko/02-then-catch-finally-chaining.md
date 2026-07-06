# [Track 3] Exercise 2: .then() / .catch() / .finally() 체이닝

## Difficulty
Easy

## Learning Goal
`.then()` 호출마다 **새로운** Promise가 반환된다는 사실, `.then()` 콜백 내부에서 일반 값을 반환하는 것과 Promise를 반환하는 것의 동작 차이(후자는 중첩되지 않고 "flatten"되어 흡수된다는 점), 그리고 결과와 무관하게 체인에 `.finally()`가 어떻게 끼어드는지 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — `.then()`이 호출될 때마다 자기 자신을 변형해서 같은 Promise 객체를 반환하는 대신, 왜 매번 완전히 새로운 Promise를 반환해야 하는가?
2. Why Origin? — 콜백 기반 비동기 호출을 서로의 안쪽에 중첩시켜, 들여쓰기가 갈수록 깊어지는 Callback Hell / Pyramid of Doom 문제를 체이닝된 `.then()` 호출이 어떻게 해결하는지 설명하라.
3. When to Use? — 서로 의존하는 비동기 단계들이 순차적으로 이어지는 구체적인 프로덕션 시나리오(예: 인증 후 프로필 조회, 이어서 권한 조회)를 설명하고, 이런 경우 `.then()` 체인이 자연스러운 선택인 이유를 서술하라.
4. What if Absent? — `.then()`에서 반환한 Promise가 flatten된다는 사실을 몰랐다면, 비동기 단계들을 조합할 때 구체적으로 어떤 실수를 저지르게 되며, 그 결과 어떤 버그가 발생하는가?

## Part 2 — 코딩 과제
초기 Promise에서 시작해, **최소 4개의 `.then()` 호출**을 포함하는 하나의 체인을 만들어라. 조건은 다음과 같다.

- 최소 하나의 `.then()` 콜백은 일반(Promise가 아닌) 값을 반환한다.
- 최소 하나의 `.then()` 콜백은 단순 값이 아니라 **새로운 Promise**(예: `new Promise(resolve => setTimeout(() => resolve(...), someDelay))`)를 반환한다.
- 체인은 무언가를 로그로 남기는 `.finally()`로 끝나고, 그 뒤에 `.finally()`가 전달받은 값을 변경하지 않는다는 것을 보여주기 위해 전달받은 값을 로그로 남기는 마지막 `.then()`을 하나 더 붙인다.

실행하기 전에 `ANSWER.md`에 다음을 작성하라:
- 로그가 출력될 정확한 순서에 대한 예측.
- 중간의 `.then()`이 반환한 Promise가 `Promise<Promise<value>>`처럼 중첩된 구조를 만들지 않는 이유에 대한 근거 — `.then()` 콜백이 thenable을 반환했을 때 Promise 내부 메커니즘이 실제로 무엇을 하는지 설명하라.

이후 실행해서 예측과 비교하고, 맞았는지 틀렸는지를 `ANSWER.md`에 기록하며, 틀렸다면 이해를 수정하라.

## Part 3 — 요구사항 및 엣지 케이스
- [ ] 체인에 (`.catch()`/`.finally()`를 제외하고) 4개 이상의 `.then()` 호출이 있다.
- [ ] 최소 하나의 `.then()`은 일반 값을 반환하고, 최소 하나는 (`setTimeout`으로 지연을 준) Promise를 반환하여, flatten 동작이 값뿐 아니라 타이밍에서도 실제로 관찰 가능하다.
- [ ] `.finally()`가 존재하며 resolve된 값을 전달받거나 변경하지 않는다 — 이를 `.finally()` 뒤에 `.then()`을 체이닝해서 증명한다.
- [ ] `ANSWER.md`에 `.then(() => somePromise)`가 왜 Promise를 감싼 Promise가 아니라 하나로 flatten된 Promise를 만드는지, 학생 본인의 언어로 명확히 서술되어 있다.
- [ ] 이 과제 전체에서 `async`/`await`는 사용하지 않는다 — `async`/`await`는 다른 트랙에서 다루며, 여기서 사용하면 순수하게 `.then()`/`.catch()`/`.finally()` 체이닝을 익힌다는 목적이 훼손된다.

## Submission
`workspace/track-3-promises/02-then-catch-finally-chaining/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변), `solution.js`, 그리고 `ANSWER.md`(예측한 순서와 근거를 먼저 작성한 뒤 실행으로 검증하고 맞았는지 기록)를 포함해야 한다.
