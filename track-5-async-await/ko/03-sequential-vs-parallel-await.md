# [Track 5] Exercise 3: 순차 `await` vs. 병렬 `await`

## Difficulty
Medium

## Learning Goal
독립적인 비동기 작업들을 하나씩 순서대로 await하는 것과, 동시에 시작한 뒤
`Promise.all`로 함께 await하는 것 사이의 실제 시간 비용 차이를 측정하고
설명합니다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
- https://developer.mozilla.org/en-US/docs/Web/API/Performance/now

## Part 1 — 코드 작성 전에 이해하기

코드를 작성하기 **전에** 아래 질문에 대한 답을 본인 언어로 `NOTES.md`에
작성하십시오.

1. Why Necessary? — 서로 *독립적인* 세 작업에 대해
   `await op1(); await op2(); await op3();`를 작성하는 것이 왜 세 작업을
   먼저 모두 시작한 뒤 함께 await하는 것보다 더 많은 실제 시간(wall-clock
   time)을 소비합니까? 각 `await`가 실제로 무엇을(async 함수의 이어지는
   실행을) 막고 있는지, 그리고 세 작업이 서로의 결과에 의존하지 않을 때 그것이
   왜 중요한지 설명하십시오.
2. Why Origin? — `Promise.all`이 없던 시절, 순수 `.then()` 체인이나
   콜백만으로 여러 비동기 작업을 동시에 실행하려면 완료 횟수를 수동으로
   추적해야 했습니다(예: 모든 콜백이 실행될 때까지 카운트하기). 이런 수동
   방식의 집계가 왜 오류에 취약했는지 구체적으로 설명하십시오.
3. When to Use? — 세 개의 순차적인 `await` 호출을 `Promise.all` 기반의 병렬
   버전으로 전환하는 것이 측정 가능하고 사용자에게 체감되는 개선을 만들어낼
   구체적인 프로덕션 시나리오를 서술하십시오.
4. What if Absent? — 어떤 개발자가 이 차이를 전혀 배우지 못해서 독립적인
   작업들을 항상 순차적으로 await한다면, 5개의 독립적인 네트워크 호출로
   fan-out하는 페이지나 엔드포인트에 어떤 구체적인 비용이 발생합니까?

"비동기 처리를 위해서" 같은 얕은 답변은 반려됩니다 — 실제 런타임 동작 원리를
근거로 답하십시오.

## Part 2 — Coding Task

각각 약 1초가 걸리는 세 개의 독립적인 mock 작업이 주어집니다.

```js
function fetchUserProfile() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ name: "Ada" }), 1000);
  });
}

function fetchUserSettings() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ theme: "dark" }), 1000);
  });
}

function fetchUserNotifications() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ unread: 3 }), 1000);
  });
}
```

1. 세 호출을 (어떤 순서로든) 하나씩 순서대로 await하는 `async` 함수
   `loadDashboardSequential()`을 작성하십시오. `performance.now()`(순수
   Node 환경에서 `performance`를 사용할 수 없다면 `Date.now()` — 본인
   런타임에 무엇이 적용되는지 확인하십시오)를 사용해 총 경과 시간을
   로그로 남기고, 결합된 객체 `{ profile, settings, notifications }`를
   반환하십시오.
2. 세 호출을 동시에 시작하고 `Promise.all`과 단 하나의 `await`로 함께
   기다리는 두 번째 `async` 함수 `loadDashboardParallel()`을 작성하십시오.
   동일한 방식으로 총 경과 시간을 로그로 남기고 동일한 형태를 반환하십시오.
3. 두 함수를 모두 호출하여 두 경과 시간을 직접 비교할 수 있도록 로그로
   남기십시오.

## Part 3 — Requirements & Edge Cases

- [ ] 둘 다 실행했을 때 `loadDashboardSequential`이 `loadDashboardParallel`
      보다 측정 가능할 정도로 약 3배 더 오래 걸립니다 (추정치가 아니라 실제
      숫자를 로그로 남기십시오).
- [ ] `loadDashboardParallel`이 세 개의 기반 `setTimeout` 기반 작업 중
      어느 하나도 resolve되기 전에 실제로 모두 시작한다는 것을 확인할 수
      있습니다 — `NOTES.md`에 이를 어떻게 확인했는지 설명하십시오 (예: 타이밍
      측정, 또는 호출 시점과 resolve 시점에 임시 로그를 추가하는 방법 등).
- [ ] 두 함수 모두 동일한 결합 형태 `{ profile, settings, notifications }`를
      올바른 값과 함께 반환합니다.
- [ ] 경과 시간은 관련 없는 설정 코드가 아니라 실제로 await되는 작업 주위에서
      `performance.now()`/`Date.now()`로 측정됩니다.
- [ ] `NOTES.md`는 기반 Promise 객체의 관점에서, `Promise.all([p1, p2, p3])`
      자체가 각 함수를 await 없이 모두 호출하는 것보다 작업을 "더 병렬로"
      만들어주는 것이 아니라는 점을 설명합니다 — 즉 `Promise.all`이 실제로
      하는 일과, 애초에 동시 작업을 시작시키는 것이 무엇인지 구분하여
      설명하십시오.

## Submission

`workspace/track-5-async-await/03-sequential-vs-parallel-await/`에 답안/코드를
작성하십시오. `NOTES.md`와 `solution.js`를 포함하십시오.
