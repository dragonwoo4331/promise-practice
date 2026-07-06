# Track 2: Event Loop & Task Queues

이 트랙은 Track 1의 런타임 기초 위에 바로 이어진다. 여기서는 동기 코드, Microtask Queue(Promise, `queueMicrotask`), (Macro)Task Queue(`setTimeout`, `setInterval`, I/O) 사이에서 Event Loop이 정확히 어떤 순서로 작업을 처리하는지, 그리고 긴 동기 작업이 어떻게 UI를 얼려버리는지, 나아가 실제 해결책(청킹, Web Worker)이 메커니즘 수준에서 무엇을 하는지를 파고든다. 여기서는 정밀함이 중요하다 — "그냥 비동기로 동작한다"는 이해로는 이 과제들을 통과할 수 없다.

## Exercises

1. [01 — MacroTask vs MicroTask 순서 예측](./01-macrotask-vs-microtask.md) — Medium. 동기 코드, microtask, macrotask가 섞인 코드의 정확한 실행 순서를 추적한다.
2. [02 — 미니 Task Queue 로거 만들기](./02-mini-task-queue-logger.md) — Medium. "microtask는 항상 다음 macrotask보다 먼저 실행된다"는 주장을 중첩된 microtask까지 포함해 계측하고 검증한다.
3. [03 — UI Freezing 시뮬레이션과 탈출구](./03-ui-freezing-escape-hatch.md) — Hard. 실제 프리징을 재현한 뒤 청킹 및/또는 Web Worker로 해결한다.

## Reference

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API

모든 결과물은 각 exercise 파일에서 지정한 대로 `workspace/track-2-event-loop/<exercise-folder>/` 아래에 제출한다.
