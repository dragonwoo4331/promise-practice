# Track 5 — Async/Await 문법적 설탕(Syntactic Sugar)

`async/await`는 별도의 동시성 메커니즘이 아니라 Promise 위에 얹힌
문법입니다. 이 트랙은 `async` 함수가 실제로 무엇을 반환하는지, `await`가
왜 현재 함수의 실행 컨텍스트만 일시 정지시키는지, `try...catch`가 reject된
Promise와 어떻게 상호작용하는지, 그리고 순차 `await`와 병렬 `await`의 실제
시간 비용 차이를 정확히 이해하는 데 필요한 사고 구조를 훈련합니다.

먼저 MDN 트랙 랜딩 페이지를 읽으십시오:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function

## Exercises

1. [01 — `.then()` 체인을 `async/await`로 변환하기](./01-then-to-async-await.md)
   체이닝된 `.then()` mock API 시퀀스를 `async/await`로 재작성하고,
   `await`가 왜 async 함수만 정지시키고 Call Stack 전체를 정지시키지 않는지
   설명합니다.
2. [02 — `try...catch` 스코프와 `forEach` 함정](./02-try-catch-and-foreach-trap.md)
   여러 순차적인 `await` 호출의 reject를 올바르게 처리하고,
   `Array.prototype.forEach` 안의 `await`가 왜 기대한 대로 동작하지 않는지
   직접 발견합니다.
3. [03 — 순차 `await` vs. 병렬 `await`](./03-sequential-vs-parallel-await.md)
   세 개의 독립적인 작업을 하나씩 await하는 것과 `Promise.all`로 동시에
   실행하는 것의 실제 시간 비용을 측정합니다.

제출 규칙(AI 사용 금지, 코드 작성 전 `NOTES.md`, 트랙 단위 push)은 저장소
루트의 `README.md`를 참고하십시오.
