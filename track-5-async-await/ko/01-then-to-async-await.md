# [Track 5] Exercise 1: `.then()` 체인을 `async/await`로 변환하기

## Difficulty
Easy

## Learning Goal
`async/await`가 새로운 동시성 모델이 아니라 Promise 위에 얹힌 문법적 설탕
(syntactic sugar)이라는 점을 이해하고, `.then()` 체인을 실행 순서와 에러
전파를 그대로 유지하면서 동등한 `async/await` 형태로 변환할 수 있어야
합니다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — 코드 작성 전에 이해하기

코드를 작성하기 **전에** 아래 질문에 대한 답을 본인 언어로 `NOTES.md`에
작성하십시오.

1. Why Necessary? — `async/await`가 순수 `.then()` 체이닝이 해결하지 못하는
   어떤 구체적인 문제를 해결합니까? 문법 수준에서 무엇이 달라지는지, 그리고
   런타임/스케줄링 수준에서는 (있다면) 무엇이 달라지는지 정확히 구분해서
   설명하십시오.
2. Why Origin? — 긴 `.then()` 체인은 분기 처리, 중첩된 에러 처리, 또는 여러
   단계에 걸쳐 재사용해야 하는 중간 변수가 추가되는 순간 가독성 문제가
   생긴다는 것이 잘 알려져 있습니다. 그런 상황에서 `.then()` 체인에 구체적으로
   무엇이 잘못되는지 설명하십시오 ("지저분해서"가 아니라, 예를 들어 체이닝된
   `.then()` 호출들 사이에서 변수 스코프에 어떤 일이 일어나는지, 또는 나중
   단계가 앞선 두 결과에 의존할 때 중첩이 어떻게 늘어나는지 등 실제 구조적
   원인을 설명하십시오).
3. When to Use? — `.then()` 체인을 `async/await`로 전환하는 것이 가치 있는
   구체적인 프로덕션 시나리오를 서술하십시오.
4. What if Absent? — 만약 어떤 코드베이스가 `async/await`를 전혀 도입하지
   않고 모든 곳에서 `.then()` 체인을 계속 사용했다면, 크고 오래된
   코드베이스에서 어떤 구체적인 재앙이나 유지보수 비용이 발생하겠습니까?

"비동기 처리를 위해서" 같은 얕은 답변은 반려됩니다 — 실제 런타임 동작 원리를
근거로 답하십시오.

## Part 2 — Coding Task

다음 `.then()` 체인을 동등한 `async/await` 함수로 변환하십시오. Mock API
호출은 그대로 제공되며, 동작을 바꾸지 말고 호출하는 코드만 바꾸십시오.

```js
function getUser(id) {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ id, name: "Ada" }), 300);
  });
}

function getOrders(userId) {
  return new Promise((resolve) => {
    setTimeout(() => resolve([{ id: 1, total: 42 }]), 300);
  });
}

function getInvoice(orderId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (orderId === 1) resolve({ orderId, amount: 42, paid: true });
      else reject(new Error("Invoice not found"));
    }, 300);
  });
}

function loadUserInvoice(userId) {
  return getUser(userId)
    .then((user) => getOrders(user.id))
    .then((orders) => getInvoice(orders[0].id))
    .then((invoice) => {
      console.log("Invoice:", invoice);
      return invoice;
    })
    .catch((err) => {
      console.error("Failed:", err.message);
      throw err;
    });
}
```

`loadUserInvoice`를 정확히 동일한 관찰 가능한 동작(성공 시 동일한 resolve
값, 실패 시 동일한 throw/로그)을 내는 `async` 함수로 다시 작성하십시오.

`NOTES.md`에서, 위 예시를 구체적으로 사용하여 `await getOrders(user.id)`가
왜 `loadUserInvoice`의 함수 실행 컨텍스트만 일시 정지시키고 JavaScript
Call Stack 전체나 브라우저/Node 스레드 전체를 정지시키지 않는지 설명하십시오.
`getOrders`가 반환한 Promise가 대기(pending) 상태인 동안 엔진이
`loadUserInvoice`의 정지된 나머지 작업을 어떻게 처리하고 있는지, 그리고 그
시간 동안 Call Stack에서 여전히 실행될 수 있는 다른 작업(예: 다른 곳에
등록된 `setTimeout` 콜백, 클릭 핸들러 등)이 무엇인지 설명하십시오. 이는
주니어 개발자들이 흔히 오해하는 부분이므로 형식적인 절차가 아니라 진지한
개념 체크포인트로 다루십시오.

## Part 3 — Requirements & Edge Cases

- [ ] `loadUserInvoice`는 `async` 키워드로 선언되고 의존적인 각 호출마다
      `await`를 사용하며, 버전에 `.then()`이 남아 있지 않습니다.
- [ ] `getInvoice`가 reject되는 경우 `try...catch`로 잡아내고, 원본과 동일한
      console 출력과 재던짐(re-throw)을 수행합니다.
- [ ] 성공 경로는 원본과 정확히 동일하게 invoice를 로그로 남깁니다.
- [ ] 외부에서 `loadUserInvoice(1).catch(...)`를 호출해도 여전히 동작합니다
      — 즉, `async` 함수가 여전히 체이닝 가능한 `Promise`를 반환합니다.
- [ ] `NOTES.md`는 이 정확한 예시를 참조하여 `await`가 대기 중인 동안
      Call Stack에 무엇이 남아있는지 명시적으로 다룹니다.

## Submission

`workspace/track-5-async-await/01-then-to-async-await/`에 답안/코드를
작성하십시오. `NOTES.md`와 `solution.js`를 포함하십시오.
