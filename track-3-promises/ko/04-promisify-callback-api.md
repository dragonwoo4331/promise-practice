# [Track 3] Exercise 4: 콜백 기반(Node 스타일) 함수를 Promise화하기

## Difficulty
Hard

## Learning Goal
마지막 인자가 `(err, data) => {}` 형태의 콜백인 레거시 Node 스타일 API를, Promise를 반환하는 함수로 감싸는 방법을 이해하고, 이 래핑(`promisify`)을 해당 형태를 가진 어떤 함수에도 동작할 만큼 범용적으로 작성한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — `(err, data) => {}` 형태의 콜백을 받는 Node 스타일 함수를 왜 곧바로 `await`할 수 없는가? 이 형태의 구체적으로 어떤 점이 `await`/`.then()`과 호환되지 않는가?
2. Why Origin? — Node 스타일 `(err, data)` 콜백 관례 자체가 그 이전의 임시방편적인 에러 처리 방식에 대한 부분적 해결책이었지만, 이런 호출들이 여러 개 중첩되면 여전히 Callback Hell / Pyramid of Doom로 이어졌다는 점을 설명하고, 이를 Promise로 감싸는 것이 inversion of control 문제(콜백 기반 API마다 내 콜백을 올바르게 호출해줄 것이라고 일일이 신뢰해야 하는 문제)를 어떻게 해결하는지 설명하라.
3. When to Use? — 최신 `async`/`await` 기반 코드베이스 안에서 레거시 또는 서드파티 콜백 기반 API를 사용하기 위해 `promisify`가 필요한 구체적인 프로덕션 시나리오를 설명하라.
4. What if Absent? — Promise/`async` 기반 코드베이스에 감싸지 않은 콜백 기반 코드를 그대로 섞어 사용한다면, 구체적으로 어떤 버그나 구조적 문제가 나타나는가(예: 체인이 깨짐, `Promise.all`과 결합하기 어려움, 중첩이 다시 나타남)?

## Part 2 — 코딩 과제
이 과제는 두 부분으로 구성된다.

### Part 2a — 가짜 레거시 API 만들기
다음 조건을 만족하는 나만의 가짜 Node 스타일 콜백 함수 `readFileMock(path, callback)`을 작성하라:
- `setTimeout`을 사용해 비동기 작업을 흉내낸다 (실제 파일시스템을 건드리지 않는다 — 가짜다).
- "성공" 시 `callback(null, someFakeContentString)`을 호출한다 — 무엇이 성공을 결정하는지는 직접 정한다 (예: 특정 `path` 값들).
- 최소 하나의 입력(예: 가짜 데이터에 존재하지 않는 path)에 대해 "실패" 시 `callback(new Error('some message'))`을 호출한다.
- 한 번의 호출에 대해 콜백을 두 번 이상 호출하지 않는다.

### Part 2b — 범용 `promisify` 작성하기
다음 조건을 만족하는 함수 `promisify(fn)`을 작성하라:
- 마지막 매개변수가 Node 스타일 `(err, data) => {}` 콜백이고, 그 앞의 매개변수들은 임의의 개수인 어떤 함수 `fn`도 받을 수 있어야 한다 (즉, 인자 1개+콜백, 인자 2개+콜백 등 다양한 형태에서 동작해야 하며, `readFileMock`의 정확한 시그니처에 하드코딩해서는 안 된다).
- 콜백을 제외한 동일한 인자로 호출했을 때 Promise를 반환하는 **새로운 함수**를 반환한다.
- 콜백이 `callback(null, data)`로 호출되면 그 Promise는 `data`로 resolve되어야 한다.
- 콜백이 (첫 번째 인자가 truthy인) `callback(err)`로 호출되면 그 Promise는 해당 에러로 reject되어야 한다.

`readFileMock`을 대상으로, 성공하는 path와 실패하는 path 각각에 대해 Promise화된 버전을 호출하고, `.then()`/`.catch()`로 결과를 관찰하며 `promisify`를 테스트하라.

## Part 3 — 요구사항 및 엣지 케이스
- [ ] `readFileMock`은 실제로 비동기적이다 (지연이 0이더라도 `setTimeout`을 사용한다) — 콜백을 동기적으로 호출해서는 안 된다.
- [ ] `promisify`는 콜백 앞에 오는 인자의 개수가 *가변적인* 함수에서도 동작한다 (범용성을 증명하기 위해, `readFileMock`과 다른 arity를 가진 함수를 최소 하나 더 — 작은 mock을 직접 만들어서라도 — 테스트하라).
- [ ] 성공 경로: Promise화된 호출은 `callback(null, data)`에 전달된 `data` 값 그대로 resolve된다.
- [ ] 실패 경로: Promise화된 호출은 `callback(err)`에 전달된 `error` 값 그대로 reject된다.
- [ ] `promisify` 자체는 `fn` 내부에서 발생하는 동기적 throw를 삼키지 않는다 — `fn`이 콜백을 스케줄하기도 전에 동기적으로 throw한다면 무슨 일이 일어나는지, 그리고 구현이 이 경우를 처리하는지 무시하는지를 `NOTES.md`에 고려/기록하라.
- [ ] Node의 내장 `util.promisify`를 사용하지 않는다 — 이 과제의 핵심은 메커니즘을 직접 구현하는 것이다.

## Submission
`workspace/track-3-promises/04-promisify-callback-api/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변 및 동기적 throw 분석 포함)와 `solution.js`를 포함해야 한다.
