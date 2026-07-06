# [Track 1] Exercise 3: Web APIs와 Single-threaded 증명

## Difficulty
Medium

## Learning Goal
JavaScript의 메인 스레드가 단일 스레드(Single-threaded)임을 실험적으로 증명하고, `setTimeout`이 지정된 지연 시간에 정확히 실행되는 것을 보장하지 않는다는 것 — 지정된 지연 시간보다 일찍 큐에 들어가지 않는 것만 보장하며, Call Stack이 비워지고 블로킹 `while` 루프를 포함한 모든 동기 코드가 끝날 때까지 기다려야 한다는 것을 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/setTimeout
- https://developer.mozilla.org/en-US/docs/Glossary/Main_thread
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop

## Part 1 — Understand Before You Code

아래 질문에 코드를 작성하기 전에 `NOTES.md`에 본인의 언어로 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 거부된다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. **Why Necessary?** — JS가 단일 스레드에서 동작한다는 전제하에, 타이머 스케줄링(`setTimeout`)이 왜 JS 엔진 자체가 아니라 (Call Stack이 아닌) 브라우저가 제공하는 Web APIs에 의해 처리되어야 하는가?
2. **Why Origin?** — JS 실행 스레드와 브라우저의 타이머/렌더링 메커니즘을 분리하게 만든 역사적 문제(UI Freezing, 응답 없는 페이지, "이 페이지의 스크립트가 응답하지 않습니다" 브라우저 대화상자)는 무엇이었는가?
3. **When to Use?** — 개발자가 실수로 긴 동기 루프(예: 거대한 배열 정렬, 무거운 동기 `JSON.parse`, 최적화되지 않은 이미지 처리 루프)로 메인 스레드를 블로킹하는 실제 프로덕션 시나리오와 사용자에게 보이는 결과를 설명하라.
4. **What if Absent?** — 만약 브라우저에 Web APIs / Task Queue를 위한 별도의 메커니즘이 전혀 없고 모든 것이 콜백 큐 없이 엄격하게 하나의 스택에서 동기적으로 실행된다면, `setTimeout`, 클릭 핸들러, 네트워크 요청은 어떻게 되겠는가?

## Part 2 — Prediction Quiz or Coding Task

다음을 이 순서대로 수행하는 스크립트를 (Node가 아니라 실제 브라우저 탭에서) 작성하라:

```js
console.log("start");

setTimeout(() => {
  console.log("timeout fired");
}, 0);

const end = Date.now() + 3000; // 약 3초간 블로킹
while (Date.now() < end) {
  // 의도적으로 메인 스레드를 바쁘게 블로킹
}

console.log("end of synchronous block");
```

실행하기 전에 `ANSWER.md`에 답하라:
- `"start"`, `"timeout fired"`, `"end of synchronous block"`은 어떤 순서로 로그에 찍히는가?
- `setTimeout`의 지연 시간은 `0`이다. 타이머가 "만료"되는 즉시 콜백이 실행될까, 아니면 지연될까? 지연된다면 대략 얼마나, 그리고 왜 그런가?
- `while` 루프가 실행되는 동안 페이지의 버튼을 클릭할 수 있는가? 왜 그런가, 혹은 왜 안 되는가?

이제 페이지에 눈에 보이는 버튼이 있는(예: `<button onclick="...">Click me</button>`가 포함된 단순한 HTML 파일) 실제 브라우저 탭에서 실행해 보라. 루프가 실행되는 동안 버튼을 클릭해 보라. 관찰한 내용을 기록하라.

## Part 3 — Requirements & Edge Cases

- [ ] UI Freezing을 관찰할 수 있도록 실제 브라우저(Node만이 아니라)에서 실행하라 — 블로킹 루프가 실행되는 동안 버튼 클릭, 스크롤, 타이핑을 시도해 본다.
- [ ] 타임아웃을 예약한 시점과 콜백이 실제로 실행된 시점 사이의 실제 시간 지연(wall-clock delay)을 기록하고, 요청한 `0`ms와 비교하라.
- [ ] "Web APIs", "Task Queue"(macrotask queue), "Call Stack"이라는 용어를 사용해, `while` 루프와 `console.log("end of synchronous block")` 줄이 끝나기 전까지 타임아웃 콜백이 왜 실행될 수 없는지 정확히 설명하라.
- [ ] 프리징이 일어나는 동안 다른 이벤트에는 어떤 일이 일어나는지 기록하라(예: 프리징 도중 등록된 클릭이 이후 즉시 처리되는가, 아니면 유실/큐잉되는가?).
- [ ] 이 실험이 JS가 단일 스레드임을 증명하는지, 아니면 Web API 계층이 JS 스레드와 분리되어 있다는 것만을 증명하는지 명시적으로 밝히고 그 이유를 설명하라.

## Submission
답과 코드를 `workspace/track-1-runtime/03-web-apis-single-threaded/`에 작성하라. `NOTES.md`(Part 1 답변), `ANSWER.md`(예측과 근거), `solution.js`(또는 `index.html` + 스크립트), 그리고 브라우저에서 실제로 관찰한 내용을 담은 짧은 관찰 기록을 포함해야 한다.
