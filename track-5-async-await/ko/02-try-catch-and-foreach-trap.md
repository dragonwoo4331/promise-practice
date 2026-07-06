# [Track 5] Exercise 2: `try...catch` 스코프와 `forEach` 함정

## Difficulty
Medium

## Learning Goal
여러 개의 순차적인 `await` 호출을 올바르게 `try...catch`로 감싸는 방법을
익히고, `forEach` 콜백 안의 `await`가 루프를 일시정지시키지 못하는 정확한
이유를 이해합니다 — `Array.prototype.forEach`는 콜백의 반환값을 무시하기
때문에 `async` 콜백이 반환하는 Promise가 조용히 버려집니다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

## Part 1 — 코드 작성 전에 이해하기

코드를 작성하기 **전에** 아래 질문에 대한 답을 본인 언어로 `NOTES.md`에
작성하십시오.

1. Why Necessary? — `async/await`는 왜 `.catch()` 대신 `try...catch`로 에러를
   처리해야 합니까? `async` 함수 안에서의 `throw`가 그 함수가 반환하는
   Promise에 실제로 무슨 일을 하는지 설명하고, 그것이 왜 `try...catch`로
   가로챌 수 있는지와 연결지어 설명하십시오.
2. Why Origin? — `async/await` 이전에는 비동기 에러 처리를 Promise에 체이닝된
   `.catch()`(또는 그 이전에는 error-first 콜백)에 의존했습니다. 여러
   개의 독립적인 비동기 단계에서 발생하는 에러를 오직 `.then()`/`.catch()`
   만으로 처리할 때 구체적으로 무엇이 번거로웠는지 설명하십시오 — 예를 들어
   2단계의 실패를 3단계의 실패와 다르게 처리하고 싶을 때, 체이닝된
   `.catch()`만으로는 에러 처리의 세분화(granularity)에 어떤 문제가
   생기는지 설명하십시오.
3. When to Use? — 함수 전체를 감싸는 하나의 `try...catch`가 아니라, 특정
   `await` 호출 주위에 세분화된 `try...catch` 블록이 필요한 구체적인
   프로덕션 시나리오를 서술하십시오.
4. What if Absent? — 주니어 개발자가 `await` 호출 루프를 `.forEach()`로
   감싸면서 각 반복이 다음 반복 전에 완료되기를 기대한다면, 런타임에서
   구체적으로 어떤 재앙이 발생합니까? 순서에 대해, 그리고 그 루프 안의
   에러가 애초에 잡히는지 여부에 대해 구체적으로 답하십시오.

"비동기 처리를 위해서" 같은 얕은 답변은 반려됩니다 — 실제 런타임 동작 원리를
근거로 답하십시오.

## Part 2 — Coding Task

다음과 같은 mock API가 주어집니다.

```js
function fetchItem(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id === 3) reject(new Error(`Item ${id} not found`));
      else resolve({ id, value: id * 10 });
    }, 200);
  });
}
```

다음을 수행하는 `async` 함수 `processItems(ids)`를 작성하십시오.

1. `ids` 배열(예: `[1, 2, 3, 4]`)을 순회합니다.
2. 각 id에 대해 순서대로, 하나씩 `fetchItem(id)`를 호출합니다.
3. 성공적으로 가져온 각 item을 도착하는 즉시 로그로 남깁니다.
4. 특정 id의 fetch가 reject되면 어떤 id가 실패했는지 식별하는 에러 메시지를
   로그로 남기되, 나머지 id 처리는 계속 진행합니다 (하나의 실패가 전체
   배치를 중단시켜서는 안 됩니다).
5. 성공적으로 가져온 item들만 가져온 순서대로 배열에 담아 반환합니다.

올바른 버전을 작성하기 **전에**, 먼저 `ids.forEach(async (id) => { ... })`
형태로 콜백 안에 `await`를 넣은 깨진 버전을 의도적으로 작성하고 실행하여
실제로 무슨 일이 일어나는지 관찰/로그로 남기십시오 (함수가 다음 id로
넘어가기 전에 각 `fetchItem`을 기다립니까? `forEach` 호출을 감싸는 외부
`try...catch`가 reject를 잡아냅니까?). 관찰 결과를 `NOTES.md`에 기록한 뒤,
`for...of`와 루프 본문 안의 `try...catch`를 사용한 올바른 버전을
구현하십시오.

## Part 3 — Requirements & Edge Cases

- [ ] `NOTES.md`에 `forEach` + `async` 콜백 버전의 관찰된 (깨진) 동작이
      문서화되어 있습니다 — 반복 순서/타이밍이 예상대로였는지, 그리고
      감싸는 `try...catch`가 reject를 잡아냈는지 포함합니다.
- [ ] `NOTES.md`는 `Array.prototype.forEach`의 반환값 처리 방식을 참조하여
      콜백이 반환한 Promise가 왜 버려지고 루프가 그것을 await할 수 없는지
      정확히 설명합니다.
- [ ] 최종 `processItems` 구현은 `forEach`가 아니라 `for...of`를 사용합니다.
- [ ] 각 `await fetchItem(id)`는 하나의 id에 대한 reject가 나머지 id 처리를
      멈추지 않도록 감싸져 있습니다.
- [ ] 최종적으로 resolve되는 배열은 실패한 id를 제외하고, fetch된 순서대로
      성공한 item들만 포함합니다.
- [ ] `processItems([1, 2, 3, 4])`를 호출하면 id `3`에 대한 명확한 에러가
      로그로 남고, item `1`, `2`, `4`는 여전히 로그/반환됩니다.

## Submission

`workspace/track-5-async-await/02-try-catch-and-foreach-trap/`에 답안/코드를
작성하십시오. `NOTES.md`와 `solution.js`를 포함하십시오.
