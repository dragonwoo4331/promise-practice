# Track 1: JS Runtime Basics

이 트랙은 JavaScript 실행 모델의 기초 — Call Stack, Heap, 그리고 단일 스레드에서 비동기 동작을 가능하게 하는 Web APIs 계층 — 을 다룬다. 이후 트랙에서 Promise와 Event Loop로 넘어가기 전에 이 동작 원리를 정확히 이해해야 한다. Stack/Heap/Web API 구조를 이해하지 못한 채 "비동기 마법"을 추측으로 다루면 디버깅할 수 없는 버그를 만들게 된다.

## Exercises

1. [01 — Call Stack와 Stack Overflow](./01-call-stack.md) — Easy. 재귀, LIFO 프레임 해제, `RangeError: Maximum call stack size exceeded`.
2. [02 — Memory Heap과 참조(Reference) vs 값(Value)](./02-heap-reference-vs-value.md) — Easy. 원시 타입 vs 객체, 변경 가시성, Garbage Collection 도달 가능성(reachability).
3. [03 — Web APIs와 Single-threaded 증명](./03-web-apis-single-threaded.md) — Medium. 메인 스레드 블로킹, `setTimeout` 스케줄링, UI Freezing 관찰.

## Reference

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop

모든 결과물은 각 exercise 파일에서 지정한 대로 `workspace/track-1-runtime/<exercise-folder>/` 아래에 제출한다.
