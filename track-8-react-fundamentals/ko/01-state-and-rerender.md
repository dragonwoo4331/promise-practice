# [Track 8] Exercise 1: State와 Re-render 멘탈 모델

## Difficulty
Easy

## Learning Goal
`useState` setter 호출이 해당 컴포넌트(와 그 자식들)의 Re-render를 예약한다는 것, 같은 이벤트 핸들러 내에서 트리거된 업데이트들이 하나의 Re-render로 배치(batch)된다는 것, 그리고 이전 값 기반 업데이터 형태(`setX(x => x + 1)`)와 직접 값 형태(`setX(x + 1)`)를 연속으로 여러 번 호출했을 때 오래된(stale) 클로저 State를 참조하여 서로 다른 결과가 나온다는 것을 이해한다.

## Prerequisite Reading
- https://react.dev/learn/state-a-components-memory
- https://react.dev/learn/render-and-commit
- https://react.dev/learn/queueing-a-series-of-state-updates
- https://react.dev/learn/state-as-a-snapshot

## Part 1 — Understand Before You Code
코드를 작성하기 전에 아래 질문에 대한 답을 본인의 언어로 `NOTES.md`에 작성하라. 피상적인 답변은 반려된다 — 막연한 유행어가 아니라 구체적인 동작 원리를 근거로 제시하라.

1. **Why Necessary?** 시간에 따라 변하는 데이터를 담기 위해 컴포넌트가 일반 `let` 변수를 쓰는 대신 왜 특별한 `useState` 훅이 필요한가? `let` 변수를 대신 사용하면 구체적으로 무엇이 실패하는가?
2. **Why Origin?** State setter를 호출해도 왜 변수가 즉시 업데이트되지 않는가(즉, 왜 State는 렌더링이 진행되는 동안 "스냅샷"으로 취급되는가)? 이 스냅샷 모델은 값을 즉시 변경(mutate)하는 방식과 비교했을 때 어떤 문제를 해결하는가?
3. **When to Use?** 직접 값 형태 대신 업데이터 함수 형태(`setX(prev => ...)`)를 의도적으로 사용해야 하는 구체적인 실무 시나리오를 설명하고, 그 시나리오에서 직접 값 형태가 왜 버그를 유발하는지 설명하라.
4. **What if Absent?** 개발자가 하나의 이벤트 핸들러 안에서 `setCount(count + 1)`을 두 번 연속 호출하며 카운트가 2만큼 증가하기를 기대했을 때, UI에서 구체적으로 무엇이 눈에 띄게 잘못되는지 설명하라.

## Part 2 — Coding Task
최소 두 개의 독립적인 `useState` 값을 가진 컴포넌트를 만들어라(예: 클릭 카운터와 불리언 플래그 토글). 컴포넌트 본문(effect 안이 아니고, 핸들러 안도 아닌)에 렌더링될 때마다 출력되는 `console.log`를 추가하되, 레이블과 두 State 값의 현재 값을 함께 출력하라.

그다음 아래 두 버튼을 구현하고 비교하라.

```jsx
// Button A (버그 트랩 — 왜 잘못됐는지 직접 증명해야 함)
function handleTripleIncrementDirect() {
  setCount(count + 1);
  setCount(count + 1);
  setCount(count + 1);
}

// Button B (올바른 패턴 — 직접 구현해야 함)
function handleTripleIncrementUpdater() {
  // 업데이터 형태를 사용한 구현
}
```

각 함수를 각각의 버튼에 연결하라. 다음을 관찰하고 `NOTES.md`에 기록하라.

- 각 버튼을 클릭할 때 본문 레벨 `console.log`가 클릭당 몇 번 실행되는가.
- Button A와 Button B를 각각 빠르게 3번 클릭했을 때 최종 count 값이 무엇이며, 왜 서로 다른가.
- 한 버튼을 클릭하는 것이 다른 State 변수의 UI를 Re-render하게 만드는지, 그리고 왜 그런지 또는 그렇지 않은지.

단순히 읽고 넘어가지 말고, 직접 버튼을 클릭하여 실제 콘솔 출력을 확인한 뒤, 사전에 예측한 내용과 실제 관찰한 내용을 모두 기록하라.

## Part 3 — Requirements & Edge Cases
- [ ] 같은 컴포넌트 안에 최소 두 개의 독립적인 `useState` 값이 있다.
- [ ] 본문 레벨 `console.log`가 존재한다(클릭할 때뿐 아니라 렌더링될 때마다 실행).
- [ ] 직접 값 형태(`setCount(count + 1)`)를 한 핸들러에서 3번 호출하는 Button A로 버그를 시연한다.
- [ ] 업데이터 형태를 사용하여 정확히 3만큼 증가하는 Button B가 있다.
- [ ] `NOTES.md`에 (코드 실행 전에 작성한) 예측과 (실행 후에 작성한) 실제 관찰이 모두 있으며, 둘 사이의 불일치를 명시적으로 설명한다.
- [ ] React가 Button A의 세 `setCount` 호출을 하나의 Re-render로 배치(batch)하면서도 값은 왜 1만 증가하는지에 대한 설명이 `NOTES.md`에 있다.

## Submission
`workspace/track-8-react-fundamentals/01-state-and-rerender/`에 작은 독립 실행형 React 앱 또는 샌드박스로 컴포넌트를 작성하라(Vite로 한 번 스캐폴딩(`npm create vite@latest`)하여 여러 예제에서 재사용하거나, 아주 작은 예제라면 script 태그로 React를 불러온 단일 HTML 파일을 사용해도 된다 — 선택은 자유이며 어떤 방식을 택했는지 NOTES.md에 기록하라). `NOTES.md`와 컴포넌트 파일들을 포함하라.
