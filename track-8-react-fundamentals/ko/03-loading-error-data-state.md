# [Track 8] Exercise 3: Loading / Error / Data State 패턴

## Difficulty
Medium

## Learning Goal
비동기 fetch 상태를 모델링할 때, 서로 모순될 수 있는 독립적인 boolean 플래그 대신 단일 status enum이나 discriminated union을 사용하여 loading, error, success가 상호 배타적이 되도록 하고 불가능한 상태 자체가 표현 불가능하도록 설계한다.

## Prerequisite Reading
- https://react.dev/learn/synchronizing-with-effects
- https://react.dev/learn/you-might-not-need-an-effect
- https://react.dev/learn/conditional-rendering
- https://react.dev/learn/choosing-the-state-structure

## Part 1 — Understand Before You Code
코드를 작성하기 전에 아래 질문에 대한 답을 본인의 언어로 `NOTES.md`에 작성하라. 피상적인 답변은 반려된다 — 막연한 유행어가 아니라 구체적인 동작 원리를 근거로 제시하라.

1. **Why Necessary?** fetch 상태를 세 개의 독립적인 boolean(`isLoading`, `isError`, `data`)으로 추적하는 것이 왜 충분하지 않은가? 논리적으로는 불가능해야 하지만 실제 코드에서는 도달 가능한 이 boolean들의 구체적인 조합은 무엇인가?
2. **Why Origin?** 숙련된 엔지니어들이 여러 독립적인 플래그 대신 단일 `status` 필드(예: `'idle' | 'loading' | 'success' | 'error'`)나, 해당 status에서만 유효한 필드를 담는 discriminated union을 선호하는 이유는 무엇인가? "불가능한 상태를 표현 불가능하게 만든다"는 것이 타입 시스템이나 리듀서 로직이 표현할 수 있는 것과 없는 것의 관점에서 구체적으로 무엇을 의미하는가?
3. **When to Use?** 이전의 실패한 요청에서 남은 오래된 `isError` 플래그가 `true`로 남아 있어 성공적인 새 `data` 값과 함께 렌더링되며 사용자를 혼란스럽게 만드는 구체적인 실무 시나리오를 설명하라.
4. **What if Absent?** 독립적인 boolean들을 신중한 가드 없이 사용했을 때, 어떤 정확한 사용자 행동 시퀀스(예: 실패 후 재시도)가 불가능/모순 상태를 유발하는지 구체적으로 설명하라.

## Part 2 — Coding Task
실제 또는 mock API에서 아이템 목록을 가져와서, 항상 다음 세 UI 상태 중 정확히 하나만 렌더링하는 컴포넌트를 만들어라.

- 요청이 진행 중일 때 로딩 표시(텍스트나 스피너).
- 요청이 실패했을 때 에러 메시지(잘못된 URL로 fetch하거나 rejected promise를 시뮬레이션하는 등, 의도적으로 유발하라).
- 성공적으로 로드된 데이터를 목록으로 렌더링.

State 형태를 직접 설계하라. 다음 중 하나를 선택하고 `NOTES.md`에서 그 선택을 정당화해야 한다.

- 단일 `status` 문자열 필드(`'idle' | 'loading' | 'success' | 'error'`)와 별도의 `data`, `error` 필드를 두되, 렌더링 로직은 오직 `status`만으로 분기한다.
- 단일 discriminated union State 객체(개념적으로 `{ status: 'success', data: T } | { status: 'error', error: E } | { status: 'loading' }`), `useState` 또는 `useReducer`로 저장.

피해야 할 버그를 설명하기 위한 `NOTES.md`의 일회성 예시로서가 아니라면, 세 개의 독립적인 boolean을 사용하는 naive한 버전을 실제로 구현하지 마라 — 실제 컴포넌트는 그 형태를 사용해서는 안 된다.

또한 에러 이후 fetch를 다시 트리거하는 "재시도" 버튼을 구현하고, 재시도가 성공이나 에러로 다시 도달하기 전에 loading 상태를 올바르게 거쳐 가는지 확인하라 — 재시도의 로딩 단계 동안 화면에 오래된 에러 메시지가 남아 있어서는 안 된다.

## Part 3 — Requirements & Edge Cases
- [ ] 어느 시점에도 loading/error/success UI 중 정확히 하나만 보인다 — 0개도, 2개 이상도 아니다.
- [ ] State 형태는 독립적인 boolean이 아니라 단일 status enum이나 discriminated union이다.
- [ ] Error 상태에 도달 가능하며 (의도적으로 유발하여) 시연되었다.
- [ ] 재시도 버튼은 loading 상태를 다시 보여주기 전에 이전 에러/데이터를 올바르게 지운다.
- [ ] `NOTES.md`에 Part 1의 Q1에서 식별한 불가능한 boolean 조합이 포함되어 있으며, 선택한 설계가 이를 어떻게 표현 불가능하게 만드는지 설명되어 있다.
- [ ] 빈 결과 케이스(성공적인 fetch, 아이템 0개)를 에러 케이스와 구분하여 처리한다.

## Submission
`workspace/track-8-react-fundamentals/03-loading-error-data-state/`에 작은 독립 실행형 React 앱 또는 샌드박스로 컴포넌트를 작성하라(Vite로 한 번 스캐폴딩(`npm create vite@latest`)하여 여러 예제에서 재사용하거나, 아주 작은 예제라면 script 태그로 React를 불러온 단일 HTML 파일을 사용해도 된다 — 선택은 자유이며 어떤 방식을 택했는지 NOTES.md에 기록하라). `NOTES.md`와 컴포넌트 파일들을 포함하라.
