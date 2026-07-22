## Part 2 — Prediction Quiz or Coding Task

- 대략 몇 개의 스택 프레임에 도달한 뒤 충돌할 것이라고 예상하는가? 7000
- 어떤 에러 메시지와 에러 타입이 나타날 것으로 예상하는가? 멕시멈에러/ 스택프레임이 최대에 도달했습니다
- 충돌 전에 작성한 `console.log` 출력이 실제로 콘솔에 찍힐까, 아니면 유실될까? 찍힌다

RangeError: Maximum call stack size exceeded

node.ver Chrome.ver
-10338 , -17354

스택프라임: 7000 / 10000 / 17000
콜스택은 단순 호출횟수가아니라 할당된 메모리 크기에 결정된다 - 스택프레임 1개당 차지하는 메모리가 매우 작았다
