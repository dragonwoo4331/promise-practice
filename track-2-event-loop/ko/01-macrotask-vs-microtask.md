# [Track 2] Exercise 1: MacroTask vs MicroTask 순서 예측

## Difficulty
Medium

## Learning Goal
Event Loop이 다음 MacroTask(`setTimeout`, `setInterval`, I/O)를 Task Queue에서 꺼내기 전에 항상 Microtask Queue(Promise `.then`/`.catch`/`.finally`, `queueMicrotask`)를 완전히 비운다는 사실을 이해하고, 동기 코드/microtask/macrotask가 섞인 코드의 정확한 실행 순서를 추적할 수 있어야 한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/queueMicrotask
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Part 1 — Understand Before You Code

아래 질문에 코드를 작성하기 전에 `NOTES.md`에 본인의 언어로 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 거부된다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. **Why Necessary?** — JS에는 Call Stack이 하나뿐인데, 왜 단일한 큐가 아니라 Microtask Queue와 (Macro)Task Queue를 구분해야 하는가?
2. **Why Origin?** — 이전의 콜백 기반 비동기 코드가 가지고 있던 문제(Callback Hell, 일관되지 않은 순서 보장)가 어떻게 Promise를 일반 `setTimeout` 콜백보다 더 강력하고 예측 가능한 스케줄링 보장(항상 다음 macrotask보다 먼저 실행)을 갖도록 명세하게 만들었는가?
3. **When to Use?** — microtask의 순서 보장(예: 브라우저가 다시 그리기 전에 상태 업데이트를 끝내거나, 다음 타이머가 발동하기 전에 무언가를 끝내는 것)이 실제로 정확성에 영향을 미치는 실제 시나리오를 설명하라.
4. **What if Absent?** — 만약 Promise 콜백이 특별한 우선순위 없이 일반 macrotask처럼 스케줄링된다면, 여러 `.then()`을 체이닝하는 코드와 타이머가 섞였을 때 어떤 구체적인 순서 버그나 경쟁 상태(race condition)가 발생할 수 있는가?

## Part 2 — Prediction Quiz or Coding Task

다음 코드를 보라:

```js
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

queueMicrotask(() => {
  console.log("4");
});

Promise.resolve().then(() => {
  console.log("5");
}).then(() => {
  console.log("6");
});

console.log("7");
```

`ANSWER.md`에 `1`부터 `7`까지가 정확히 어떤 순서로 로그에 찍히는지 예측하고, 각 줄마다 왜 그 위치에 놓이는지 설명하라. 구체적으로: 어떤 줄이 동기적으로 실행되는지, 어떤 줄이 microtask로 큐잉되는지, 어떤 줄이 macrotask로 큐잉되는지, 그리고 체이닝된 `.then()` 호출(`5` 다음 `6`)이 왜 다른 microtask들과 그런 식으로 인터리빙되는지를 밝혀라.

전체 예측과 근거를 다 쓰기 전까지는 코드를 실행하지 마라.

## Part 3 — Requirements & Edge Cases

- [ ] 작성한 근거는 최종 순서를 말하기 전에 모든 줄을 "동기(synchronous)", "microtask", "macrotask" 중 하나로 명시적으로 분류해야 한다.
- [ ] `queueMicrotask(() => console.log("4"))`와 `Promise.resolve().then(() => console.log("3"))`이 하나는 Promise 호출처럼 보이지 않는데도 둘 다 microtask인 이유를 설명하라.
- [ ] 체인의 두 번째 `.then()`(`6`)이 첫 번째(`5`)와 같은 microtask-queue 처리 주기 안에서 실행될 수 없는 이유 — 즉 `.then()`을 체이닝하면 왜 첫 콜백 안에서 동기적으로 실행되지 않고 새로운 microtask가 스케줄링되는지 — 를 설명하라.
- [ ] 예측을 다 쓴 후 실제로 (Node나 브라우저 콘솔에서) 코드를 실행해 맞았는지 기록하라. 틀렸다면 구체적으로 어떤 가정이 잘못되었는지 설명하라.

## Submission
답을 `workspace/track-2-event-loop/01-macrotask-vs-microtask/`에 작성하라. `NOTES.md`(Part 1 답변)와 `ANSWER.md`(예상 순서 + 줄별 근거, 그리고 실제 결과와 예측이 맞았는지 여부)를 포함해야 한다.
