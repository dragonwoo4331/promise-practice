# [Track 1] Exercise 1: Call Stack와 Stack Overflow

## Difficulty
Easy

## Learning Goal
JavaScript의 Call Stack이 LIFO(Last-In-First-Out) 방식으로 프레임을 쌓고 해제하는 원리를 이해하고, 정확히 어떤 조건에서 `RangeError: Maximum call stack size exceeded`가 발생하는지 설명할 수 있어야 한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Glossary/Call_stack
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions#recursion
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RangeError

## Part 1 — Understand Before You Code

아래 질문에 코드를 작성하기 전에 `NOTES.md`에 본인의 언어로 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 거부된다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. **Why Necessary?** — JavaScript가 단일 스레드(Single-threaded)로 동작함에도 불구하고 Call Stack이 필요한 이유는 무엇인가? Call Stack이 없다면 함수 실행 순서는 어떻게 될까?
2. **Why Origin?** — 함수 실행 방식(프레임 관리, 반환 주소, 재귀 깊이)에 존재했던 어떤 기술적 한계 때문에 엔진들이 무제한이 아닌 고정 크기의 스택을 구현하게 되었는가?
3. **When to Use?** — 엔지니어가 Call Stack의 깊이를 신중히 고려해야 하는 실제 시나리오(예: 트리 구조에 대한 깊은 재귀, JSON 파싱, 재귀적 DOM 순회)를 설명하라.
4. **What if Absent?** — 만약 스택 크기 제한이 전혀 없다면, 베이스 케이스가 누락된 버그가 있는 재귀 함수가 실행될 때 실제 컴퓨터에서 어떤 구체적인 재앙이 발생할 수 있는가?

## Part 2 — Prediction Quiz or Coding Task

`n`을 로그로 남기고 `n - 1`로 자기 자신을 호출하는 재귀 함수 `countDown(n)`을 작성하되, **베이스 케이스를 두지 마라** (또는 절대 도달하지 않는 조건, 예: `n !== -999999999`).

실행하기 전에 `ANSWER.md`에 답하라:
- 대략 몇 개의 스택 프레임에 도달한 뒤 충돌할 것이라고 예상하는가?
- 어떤 에러 메시지와 에러 타입이 나타날 것으로 예상하는가?
- 충돌 전에 작성한 `console.log` 출력이 실제로 콘솔에 찍힐까, 아니면 유실될까?

이제 실제로 Node나 브라우저 콘솔에서 실행해 실제 출력과 에러를 관찰하고, 실제로 무슨 일이 일어났는지 기록하라.

## Part 3 — Requirements & Edge Cases

- [ ] 재귀 함수는 실제로 오버플로우가 발생하도록 동작하는 베이스 케이스가 없어야 한다.
- [ ] 런타임에서 발생한 정확한 에러 이름과 메시지 텍스트를 기록하라.
- [ ] Node.js와 브라우저 콘솔에서 관찰된 최대 깊이가 다른지 확인하라 (Node와 Chrome 모두 V8을 사용하지만, Call Stack 설정에 따라 한도가 다를 수 있다).
- [ ] "stack frame"이라는 용어를 사용하여, 각 재귀 호출이 함수 본문을 실행하기도 전에 왜 고정된 양의 메모리를 추가로 소비하는지 설명하라.
- [ ] `return`(또는 에러 발생) 이후 스택이 왜 LIFO 순서로 풀리는지 설명하라.

## Submission
답과 코드를 `workspace/track-1-runtime/01-call-stack/`에 작성하라. `NOTES.md`(Part 1 답변)와 `ANSWER.md`(예상한 충돌 시점 및 근거, 그리고 실제 관찰 결과와 예측이 맞았는지 여부)를 포함해야 한다.
