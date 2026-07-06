# [Track 8] Exercise 2: useEffect 의존성 배열, Cleanup, 그리고 비동기 경쟁 조건

## Difficulty
Medium

## Learning Goal
데이터를 가져오는 `useEffect`가 오직 입력값이 바뀔 때만 다시 실행되도록 의존성 배열의 범위를 올바르게 지정하고, 더 오래된(out-of-order) 응답이 더 최신 응답을 덮어쓰는 경쟁 조건(race condition)을 막기 위해 ignore-flag 또는 `AbortController` cleanup 패턴을 구현하며, effect 콜백 자체를 왜 `async`로 선언할 수 없는지 정확히 이해한다.

## Prerequisite Reading
- https://react.dev/reference/react/useEffect
- https://react.dev/learn/synchronizing-with-effects
- https://react.dev/learn/you-might-not-need-an-effect
- https://react.dev/learn/lifecycle-of-reactive-effects
- https://react.dev/learn/removing-effect-dependencies

## Part 1 — Understand Before You Code
코드를 작성하기 전에 아래 질문에 대한 답을 본인의 언어로 `NOTES.md`에 작성하라. 피상적인 답변은 반려된다 — 막연한 유행어가 아니라 구체적인 동작 원리를 근거로 제시하라.

1. **Why Necessary?** prop에 따라 달라지는 네트워크 요청처럼 외부 시스템과 컴포넌트를 동기화하는 작업이 왜 렌더링 중에 컴포넌트 본문에서 직접 `fetch`를 호출하는 대신 `useEffect`를 필요로 하는가?
2. **Why Origin?** React 설계에서 의존성 배열이 (effect 안에서 읽는 모든 반응형 값을 반영하도록) 완전(exhaustive)해야 했던 이유는 무엇인가? 불완전한 의존성 배열은 어떤 종류의 버그를 유발하며, `componentDidUpdate` 같은 생명주기 메서드는 이러한 버그를 실수로 만들기 얼마나 쉬웠는가?
3. **When to Use?** out-of-order 응답에 대한 방어가 없을 때 눈에 보이는 사용자 대면 버그가 발생하는 구체적인 실무 시나리오(예: 실시간 검색창)를 설명하라. 이 버그를 유발하는 네트워크 이벤트의 순서를 구체적으로 명시하라.
4. **What if Absent?** cleanup/ignore-flag 로직이 없는 상태에서 사용자가 빠른 검색어를 입력한 다음 더 느린 검색어를 입력했고 응답이 순서가 뒤바뀌어 도착했을 때, 화면에서 정확히 무슨 일이 일어나는지 설명하라.

## Part 2 — Coding Task
검색어(컨트롤드 입력의 State)를 받아, 검색어가 바뀔 때마다 mock API 또는 실제 API(`https://jsonplaceholder.typicode.com/` 또는 다른 공개 API 사용 가능)에서 결과를 가져오는 컴포넌트를 만들어라.

직접 구현해야 할 요구사항:
- 의존성 배열은 검색어가 실제로 바뀔 때만 재요청이 일어나도록 해야 한다 — 매 렌더링마다 재요청되어서는 안 된다.
- effect 콜백 자체는 `async`로 선언되어서는 안 된다. 왜 이 시그니처가 문제인지 `NOTES.md`에 설명하라.

```jsx
// 이 시그니처는 왜 문제인가?
useEffect(async () => {
  const res = await fetch(url);
  // ...
}, [query]);
```

- cleanup 함수 안에서 설정하는 boolean ignore-flag 방식이나, cleanup 함수에서 `.abort()`를 호출하는 `AbortController` 방식 중 하나를 사용하여 경쟁 조건 방어 로직을 구현하라. 이전 요청이 끝나기 전에 검색어가 바뀌면, 오래된 응답은 폐기되어 State에 절대 반영되지 않아야 한다.
- 경쟁 조건 방어 로직이 실제로 동작함을 증명하기 위해, 응답 중 하나를 인위적으로 지연시켜라(예: 응답을 resolve하기 전에 임의의 또는 고정된 `setTimeout` 지연을 추가하거나, 응답이 느리다고 알려진 검색어 사용). 더 느리고 먼저 보낸 요청의 응답이 더 빠르고 나중에 보낸 요청의 응답보다 늦게 도착하는 것을 타임스탬프로 로그를 남겨 보여주고, 오래된 응답이 올바르게 무시됨을 보여라.

## Part 3 — Requirements & Edge Cases
- [ ] 의존성 배열은 effect 안에서 읽는 반응형 값(검색어)만 정확히 포함하며 그 이상도 이하도 아니다.
- [ ] effect 콜백은 동기 함수이며, 내부에서 별도의 async 함수를 정의해 호출하거나 `.then()`을 사용한다 — `async () => {}` 자체가 아니다.
- [ ] effect에서 반환된 cleanup 함수가 ignore flag를 설정하거나 `AbortController.abort()`를 호출한다.
- [ ] 오래된 out-of-order 응답이 더 최신 State를 덮어쓰지 않는다는 것을 (타임스탬프가 포함된 콘솔 로그로) 증명했다.
- [ ] `NOTES.md`에 async effect 콜백의 암묵적인 Promise 반환이 React가 effect의 반환값을 cleanup 용도로 사용하는 것과 왜 충돌하는지 설명되어 있다.
- [ ] 검색어가 비어 있는 경우를 처리한다(동작을 스스로 정의하고 문서화하라 — 예: fetch를 건너뛴다).

## Submission
`workspace/track-8-react-fundamentals/02-useeffect-deps-cleanup/` 안에 이 예제를 위한 라우트/페이지를 추가하라. `NOTES.md`와 컴포넌트 파일들을 포함하라.
