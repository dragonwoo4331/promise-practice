# Track 4: Promise 동시성(Concurrency) 메서드

이 트랙은 `Promise`의 네 가지 정적 동시성 결합자(combinator) — `Promise.all`, `Promise.race`, `Promise.allSettled`, `Promise.any` — 를 다룬다. 이들은 모두 여러 Promise를 동시에 실행하지만, resolve/reject되는 규칙이 각각 다르다. 이 규칙들을 (대충이 아니라) 정확히 이해하는 것이 프로덕션에서 중요하다 — 잘못된 결합자를 선택하면 부하 상황에서 조용히 잘못된 실패 동작을 만들어낸다. 이 트랙은 Track 3(Promise 심화 학습)을 완료했다고 가정한다.

## Exercises

1. [Promise.all](./01-promise-all.md) — fail-fast 동시성; 하나의 rejection이 다른 모든 결과를 잃게 만든다.
2. [Promise.race](./02-promise-race.md) — 성공이든 실패든, 가장 먼저 끝나는 것으로 settle된다.
3. [Promise.allSettled](./03-promise-allsettled.md) — 모든 Promise를 기다리며, 전체 결과 배열을 반환한다.
4. [Promise.any](./04-promise-any.md) — 첫 성공이 이긴다; 모든 것이 실패했을 때만 `AggregateError`로 실패한다.

## Stretch Goal

시니어 엔지니어라면 `all`, `race`, `allSettled`, `any`의 차이를 각각 한 문장으로, 망설임 없이 설명할 수 있어야 한다. 네 개의 실습을 모두 마쳤다면, 기억을 더듬어 이 네 문장을 직접 작성해보고 실제로 관찰한 내용과 대조해보라 — 여기에는 의도적으로 답을 제공하지 않는다.

## Reference

- [MDN: Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
- [MDN: Promise.race](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)
- [MDN: Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
- [MDN: Promise.any](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)
- [MDN: AggregateError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)
