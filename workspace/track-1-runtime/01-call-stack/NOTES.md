## Part 1 — Understand Before You Code

1. **Why Necessary?** — JavaScript가 단일 스레드(Single-threaded)로 동작함에도 불구하고 Call Stack이 필요한 이유는 무엇인가? Call Stack이 없다면 함수 실행 순서는 어떻게 될까?
   Q. callstack이 없으면 주소를 저장을 못해 A함수안에 B함수가 있을경우 B함수가 끝나면 callstack안에 A함수가 저장되있지않아 그대로 모든동작이 끝난다. 순서는 다시 A함수로 못돌아오는 일방통행순으로 실행된다

2. **Why Origin?** — 함수 실행 방식(프레임 관리, 반환 주소, 재귀 깊이)에 존재했던 어떤 기술적 한계 때문에 엔진들이 무제한이 아닌 고정 크기의 스택을 구현하게 되었는가?
   Q. 함수 실행 방식이 가진 '반환 주소를 위한 연속성 필요, 프레임 크기 예측 불가, 컨텍스트 보존 비용'이라는 근본적 한계 때문에 엔진은 고정 크기 스택을 강제할 수밖에 없었고 이 좁은 감옥에서 탈출하기 위해 개발자들은 인라인, 비동기, 가상 스레드가 생겼다.

3. **When to Use?** — 엔지니어가 Call Stack의 깊이를 신중히 고려해야 하는 실제 시나리오(예: 트리 구조에 대한 깊은 재귀, JSON 파싱, 재귀적 DOM 순회)를 설명하라.
   Q. 트리구조: 조직도, 파일시스템탐색- 노드의 개수만큼 스택 프레임이 일려로 쌓인다, 노드가 2만개가 되도 스택오버플로우가 발생해 서버가다운되거나 클라이언트가 튕긴다.

4. **What if Absent?** — 만약 스택 크기 제한이 전혀 없다면, 베이스 케이스가 누락된 버그가 있는 재귀 함수가 실행될 때 실제 컴퓨터에서 어떤 구체적인 재앙이 발생할 수 있는가?
   Q. 단순히 해당 프로그램의 오작동을 넘어 OS 커널 레벨의 시스템크래쉬로 이어진다
   제한장치가 없어 메모리는 다른영역도 침범하여 데이터덮어쓰기가 시작되 시스템전체의 데이터가 오염된다
   이후 한계를 초과하면 디스크스래싱상태에 빠져 컴퓨터전체가 프리징상태가 되어 블루스크린 발생하게된다

## Part 2 — Prediction Quiz or Coding Task

`n`을 로그로 남기고 `n - 1`로 자기 자신을 호출하는 재귀 함수 `countDown(n)`을 작성하되, **베이스 케이스를 두지 마라** (또는 절대 도달하지 않는 조건, 예: `n !== -999999999`).

실행하기 전에 `ANSWER.md`에 답하라:

- 대략 몇 개의 스택 프레임에 도달한 뒤 충돌할 것이라고 예상하는가?
- 어떤 에러 메시지와 에러 타입이 나타날 것으로 예상하는가?
- 충돌 전에 작성한 `console.log` 출력이 실제로 콘솔에 찍힐까, 아니면 유실될까?

이제 실제로 Node나 브라우저 콘솔에서 실행해 실제 출력과 에러를 관찰하고, 실제로 무슨 일이 일어났는지 기록하라.

const countDown = (n) => {
console.log(n);
countDown(n - 1);
};
countDown(10);

node.ver
-10338
node:internal/util/inspect:1887
function formatPrimitive(fn, value, ctx) {
^

RangeError: Maximum call stack size exceeded
at formatPrimitive (node:internal/util/inspect:1887:25)
at formatValue (node:internal/util/inspect:852:12)
at inspect (node:internal/util/inspect:409:10)
at formatWithOptionsInternal (node:internal/util/inspect:2579:40)
at formatWithOptions (node:internal/util/inspect:2441:10)
at console.value (node:internal/console/constructor:336:14)
at console.log (node:internal/console/constructor:384:61)
at countDown (C:\Users\IPC226\Desktop\IPC\Quest\inhouse-code-challenges\kim\ex\Quiz\test.js:111:11)
at countDown (C:\Users\IPC226\Desktop\IPC\Quest\inhouse-code-challenges\kim\ex\Quiz\test.js:112:3)
at countDown (C:\Users\IPC226\Desktop\IPC\Quest\inhouse-code-challenges\kim\ex\Quiz\test.js:112:3)

Node.js v22.20.0

IPC226@SB-ELCH-226 MINGW64 ~/Desktop/IPC/Quest/inhouse-code-challenges/kim/ex/Quiz (main)

Chrome.ver
-17354
VM44:1 Uncaught RangeError: Maximum call stack size exceeded
at countDown (<anonymous>:1:19)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)
at countDown (<anonymous>:3:3)

## Part 3 — Requirements & Edge Cases

- [o] 재귀 함수는 실제로 오버플로우가 발생하도록 동작하는 베이스 케이스가 없어야 한다.
- [o] 런타임에서 발생한 정확한 에러 이름과 메시지 텍스트를 기록하라.
- [o] Node.js와 브라우저 콘솔에서 관찰된 최대 깊이가 다른지 확인하라 (Node와 Chrome 모두 V8을 사용하지만, Call Stack 설정에 따라 한도가 다를 수 있다).
- [o] "stack frame"이라는 용어를 사용하여, 각 재귀 호출이 함수 본문을 실행하기도 전에 왜 고정된 양의 메모리를 추가로 소비하는지 설명하라.
  ㄴ함수를 실행할려면 돌아갈주소와 매개변수를 담을 스택프레임 이 필요함으로 함수 본문을 실행하기도 전에 고정된 메모리가 추가로 소비된다.
- [o] `return`(또는 에러 발생) 이후 스택이 왜 LIFO 순서로 풀리는지 설명하라.
  ㄴlifo가 아니면 A(B(C()))라는 함수를 실행했을때 안의 B,C함수의 실행이 필요가없어서 버려진다.
  그래서 A-B-C-C-B-A순이 된다

## Submission

답과 코드를 `workspace/track-1-runtime/01-call-stack/`에 작성하라. `NOTES.md`(Part 1 답변)와 `ANSWER.md`(예상한 충돌 시점 및 근거, 그리고 실제 관찰 결과와 예측이 맞았는지 여부)를 포함해야 한다.
