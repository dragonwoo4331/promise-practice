# Track 3: Promise 심화 학습

이 트랙은 `Promise` 객체 자체를 다룬다: 내부 상태 머신, `.then()`/`.catch()`/`.finally()` 체인이 실제로 어떻게 동작하는지, 에러가 체인 전반에 걸쳐 어떻게 전파되고 (또한 어떻게 복구될 수 있는지), 그리고 레거시 콜백 기반 API를 Promise 기반 코드로 어떻게 연결하는지를 배운다. 이 트랙은 JS 런타임과 이벤트 루프(Track 1–2)를 이미 이해하고 있다고 가정한다. `async`/`await` 문법은 의도적으로 다루지 않는다 — 이는 Track 5의 주제이며, 먼저 Promise 자체의 동작 원리에 대한 탄탄한 이해를 갖추기 위함이다.

## Exercises

1. [상태(States)와 기본 생성](./01-states-and-construction.md) — Pending/Fulfilled/Rejected, `new Promise(...)`로 Promise 생성하기, `resolve`/`reject`를 두 번 호출하거나 전혀 호출하지 않을 때의 동작.
2. [.then() / .catch() / .finally() 체이닝](./02-then-catch-finally-chaining.md) — 여러 단계로 이루어진 체인 만들기와 Promise flattening 이해하기.
3. [체인 전반에 걸친 에러 전파](./03-error-propagation.md) — 던져진 에러가 가장 가까운 `.catch()`로 건너뛰는 방식, 그리고 체인이 이후 어떻게 복구될 수 있는지.
4. [콜백 기반 함수를 Promise화하기](./04-promisify-callback-api.md) — Node 스타일 `(err, data) => {}` API를 범용 Promise 반환 함수로 감싸기.

## Reference

- [MDN: Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
