# [Track 2] Exercise 3: UI Freezing 시뮬레이션과 탈출구

## Difficulty
Hard

## Learning Goal
Call Stack을 오래 점유하는 동기 작업이 유발하는 실제 UI 프리징을 재현하고, `setTimeout`/`requestAnimationFrame`을 통한 청킹(chunking)이나 Web Worker로의 오프로딩이 어떻게 Event Loop에 제어권을 되돌려주어 응답성을 회복시키는지, 막연한 설명이 아니라 실제 메커니즘을 근거로 설명한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop
- https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame

## Part 1 — Understand Before You Code

아래 질문에 코드를 작성하기 전에 `NOTES.md`에 본인의 언어로 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 거부된다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. **Why Necessary?** — 긴 동기 연산이 왜 전체 UI(클릭, 스크롤, 애니메이션, 다시 그리기)를 얼려버리는가, 반면 동일한 양의 작업을 여러 개의 `setTimeout` 청크로 나누면 왜 그렇지 않은가?
2. **Why Origin?** — 하나의 Call Stack / 하나의 Event Loop 모델(렌더링, 입력 처리, 스크립트 실행을 모두 하나의 스레드가 담당)이 가진 한계가 왜 JS가 같은 스레드에서 "백그라운드로" 코드를 실행하는 방법을 얻는 대신 Web Worker라는 플랫폼 기능을 필요하게 만들었는가?
3. **When to Use?** — 엔지니어가 청킹과 Web Worker 중 하나를 선택해야 하는 구체적인 실제 시나리오(예: 클라이언트 측 이미지 필터링, 거대한 CSV/JSON 페이로드 파싱, 차트를 위한 복잡한 데이터 변환)를 설명하라.
4. **What if Absent?** — 만약 청킹도 Web Worker도 존재하지 않고 오직 끊을 수 없는 하나의 동기 블록만 가능하다면, 클라이언트에서 어느 정도 큰 데이터셋을 처리해야 하는 실제 제품의 사용성에 어떤 일이 일어나겠는가?

## Part 2 — Prediction Quiz or Coding Task

Part A — 프리징 재현: 눈에 보이는 클릭 가능한 버튼과, `setInterval`이나 `requestAnimationFrame`으로 증가하는 카운터(눈에 보이는 "심박수" 역할)가 있는 페이지 스크립트를 작성하라. 몇 초간 루프를 도는 동기 함수(예: 의미 없는 계산을 하는 중첩 루프)를 추가하라. 두 번째 버튼을 통해 이를 실행시켜라. 관찰하고 기록하라: 심박수 카운터가 멈추는가? 프리징 중에 첫 번째 버튼을 클릭할 수 있는가?

Part B — 해결책 설계(및 선택적으로 구현):
- 무거운 작업을 **청크로 나눈(chunked)** 버전으로 다시 작성하라. `setTimeout`(또는 `requestAnimationFrame`)을 사용해 루프를 작은 조각으로 나누고, 각 청크 사이에 Event Loop에 제어권을 돌려줘라. 청킹된 실행 동안 심박수 카운터가 계속 움직이고 버튼이 계속 클릭 가능한지 확인하라.
- 스트레치 목표로, 무거운 작업을 실제 **Web Worker**로 `postMessage`/`onmessage`를 사용해 옮기고, 작업 전체 기간(청크 사이의 짧은 틈만이 아니라) 동안 메인 스레드가 완전히 응답 가능한 상태로 유지되는지 확인하라.

이 과제는 정확한 콘솔 출력을 예측하는 문제가 아니라 직접 만들고 관찰하는 과제이므로, 예상 출력을 미리 밝힐 필요는 없다.

## Part 3 — Requirements & Edge Cases

- [ ] Part A는 프리징이 콘솔 타이밍으로 추측하는 게 아니라 눈으로 직접 관찰 가능하도록, 눈에 보이고 계속 갱신되는 UI 요소(심박수 카운터나 애니메이션)를 포함해야 한다.
- [ ] 무거운 작업의 시작/종료 시점에 비해 심박수가 정확히 언제 멈추고 다시 시작되는지 기록하라.
- [ ] Part B의 청킹 버전은 심박수와 버튼이 응답성을 유지한다는 것을 실제로 입증해야 한다 — 코드뿐 아니라 관찰 결과도 포함하라.
- [ ] 설계 노트에서 청킹이 왜 여전히 *같은* 메인 스레드에서 실행되는지(즉 진정한 병렬성이 아니라 협력적 양보(cooperative yielding)만 달성한다는 것) 구체적으로 설명하라 — 이를 진짜 별도의 스레드에서 실행되며 공유 메모리가 없는(메시지 전달로만 통신하는) Web Worker와 대조하라.
- [ ] Web Worker 스트레치 목표를 시도했다면, 겪었거나 조사한 Web Worker의 구체적인 한계 하나를 기록하라(예: DOM 접근 불가, 참조 공유가 아니라 structured clone 알고리즘으로 데이터가 복제되는 것).
- [ ] 30초 이상 걸리는 CPU 바운드 작업에 대해 청킹과 Web Worker 중 어느 쪽을 선택할지 명시적으로 밝히고, 이번에 관찰한 메커니즘을 근거로 정당화하라.

## Submission
결과물을 `workspace/track-2-event-loop/03-ui-freezing-escape-hatch/`에 작성하라. `NOTES.md`(Part 1 답변), 프리징 재현 코드와 청킹 수정 코드(예: `index.html` + `solution.js`), 청킹-vs-Web-Worker 메커니즘을 다루는 설계 노트(`ANSWER.md`), 그리고 시도했다면 Web Worker 스트레치 목표 파일을 포함해야 한다.
