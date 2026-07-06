# [Track 3] Exercise 1: Promise의 상태(States)와 기본 생성

## Difficulty
Easy

## Learning Goal
Promise의 세 가지 상태(Pending, Fulfilled, Rejected)를 이해하고, Promise의 상태 전이가 단방향이며 단 한 번만 일어난다는 사실, 그리고 `resolve`/`reject`를 두 번 이상 호출하거나 아예 호출하지 않았을 때 실제로 어떤 결과가 발생하는지 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — 비동기 작업이 끝날 때마다 콜백이 그냥 호출되도록 두지 않고, 왜 굳이 "Pending / Fulfilled / Rejected"라는 공식적인 상태 머신이 필요한가?
2. Why Origin? — Promise가 Callback Hell / Pyramid of Doom, 그리고 inversion of control 문제(내 콜백을 남의 함수에 넘기고, 그 함수가 정확히 한 번, 올바른 인자로 콜백을 호출해줄 것이라고 믿어야만 하는 문제)에 대한 대응으로 등장한 배경을 설명하라.
3. When to Use? — 라이브러리가 이미 반환해주는 Promise를 그냥 소비하는 것이 아니라, 직접 `new Promise(...)`를 생성해야 하는 구체적인 프로덕션 시나리오를 설명하라.
4. What if Absent? — 어떤 코드 경로에서 Promise는 생성되었지만 `resolve`/`reject`가 끝내 호출되지 않는다면, 실제 애플리케이션에서 구체적으로 무엇이 깨지는가?

## Part 2 — 코딩 과제
`new Promise((resolve, reject) => { ... })`를 사용해 다음 Promise들을 생성하는 짧은 스크립트를 작성하라 (특별히 export할 필요 없는 평범한 Node 스크립트 또는 브라우저 콘솔 스크립트면 충분하다).

1. 비동기적으로(`setTimeout` 사용) 원하는 값으로 resolve되는 Promise. 결과를 로그로 남기는 `.then()`을 붙여라.
2. 비동기적으로(`setTimeout` 사용) `Error`로 reject되는 Promise. reason을 로그로 남기는 `.catch()`를 붙여라.
3. **절대 settle되지 않는** Promise — `resolve`도 `reject`도 결코 호출되지 않는다. 여기에도 `.then()`과 `.catch()`를 붙여라. 스크립트를 실행하고 무슨 일이 일어나는지(혹은 일어나지 않는지) 관찰하라.
4. executor 안에서 `resolve('first')`를 호출한 직후, 곧바로(모두 동기적으로, 같은 executor 안에서) `resolve('second')`도 호출하는 Promise. resolve된 값을 로그로 남기는 `.then()`을 붙여라.
5. (4)의 변형으로, executor가 `resolve('first')`를 호출한 뒤 `reject(new Error('second'))`를 호출하는 경우. `.then()`과 `.catch()`를 모두 붙여라.

코드만 작성하고 넘어가지 말고, `NOTES.md`에 3, 4, 5번 케이스에 대해 다음도 함께 기록하라:
- 실행 전에 어떤 결과를 예측했는가.
- 실제로 실행했을 때 무슨 일이 일어났는가.
- 명세(spec)가 왜 이렇게 동작하는지 — Promise 내부의 "이미 settle됨" 플래그를 언급하며 자신의 언어로 설명하라.

## Part 3 — 요구사항 및 엣지 케이스
- [ ] 세 가지 상태(Pending, Fulfilled, Rejected) 모두 최소 하나 이상의 Promise로 시연되었다.
- [ ] "절대 settle되지 않는" Promise가 포함되어 있고, `NOTES.md`에 영원히 멈춰 있는 Promise의 구체적인 위험 요소가 최소 하나 이상 설명되어 있다 (예: 이를 `await`하는 `async` 함수가 영원히 블록됨, 리소스 누수, 영원히 로딩 상태에 머무는 UI 등).
- [ ] "resolve 두 번 호출" 케이스가 포함되어 있고, 노트에 다음이 명확하고 정확하게, 근거와 함께 서술되어 있다: `resolve`/`reject`의 **첫 번째** 호출만 효력이 있으며, 이후의 모든 호출은 조용히 무시된다.
- [ ] "resolve 후 reject" 케이스가 포함되어 있고, "첫 번째 호출이 이긴다"는 규칙이 같은 함수 내에서뿐 아니라 `resolve`와 `reject` 사이에서도 동일하게 적용됨을 노트에서 확인한다.
- [ ] 의도적으로 처리한 케이스에서 unhandled promise rejection 경고가 콘솔에 남아있지 않다 (3번 케이스의 rejection 핸들러는 애초에 호출되지 않는 것이 정상이며 문제 없다).

## Submission
`workspace/track-3-promises/01-states-and-construction/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변 및 Part 2에서 요구한 예측/관찰 내용 포함)와 `solution.js`를 포함해야 한다.
