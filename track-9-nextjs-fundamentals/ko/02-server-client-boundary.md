# [Track 9] Exercise 2: Server Component와 Client Component의 경계

## Difficulty
Medium

## Learning Goal
어떤 기능들(이벤트 핸들러를 통한 인터랙티비티, `useState`/`useEffect` 같은 React 훅, 브라우저 전용 API)이 `"use client"` 디렉티브를 통해 Client Component가 되어야만 사용 가능한지 정확히 이해하고, Server Component가 Client Component 자신이 되지 않고도 그 Client Component의 `children`으로 조합될 수 있다는 것을 이해한다.

## Prerequisite Reading
- https://nextjs.org/docs/app/building-your-application/rendering/server-components
- https://nextjs.org/docs/app/building-your-application/rendering/client-components
- https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns
- https://react.dev/reference/rsc/use-client

## Part 1 — Understand Before You Code
코드를 작성하기 전에 아래 질문에 대한 답을 본인의 언어로 `NOTES.md`에 작성하라. 피상적인 답변은 반려된다 — 막연한 유행어가 아니라 구체적인 동작 원리를 근거로 제시하라.

1. **Why Necessary?** Server Component는 왜 원칙적으로도 `useState`나 `onClick` 핸들러를 전혀 사용할 수 없는가? 이것을 단지 권장하지 않는 정도가 아니라 원천적으로 불가능하게 만드는, 서버 런타임에 실제로 빠져 있는 것은 무엇인가?
2. **Why Origin?** Next.js/React는 왜 (Pages Router에서 사실상 기본값이었던) 모든 것을 Client Component로 유지하는 대신 Server Component를 기본 렌더링 모드로 도입했는가? Server Component를 기본값으로 하는 것은 (번들 크기, 워터폴 요청, 노출된 시크릿 중) 어떤 구체적인 비용을 줄이는가?
3. **When to Use?** 페이지 전체는 Server Component로 유지되어야 하지만 작은 인터랙티브 아일랜드 하나(예: "좋아요" 버튼, 드롭다운, 클립보드 복사 버튼)에만 `"use client"`가 필요한 구체적인 실무 시나리오를 설명하라. 페이지 전체를 `"use client"`로 표시하는 것이 왜 더 나쁜 선택인지 설명하라.
4. **What if Absent?** `"use client"`가 없는 컴포넌트 안에서 `useState`나 `onClick`을 사용하려고 할 때 실제로 발생하는 빌드 타임 또는 런타임 에러 메시지를 구체적으로 설명하고, 그 에러가 코드가 (실행을 시도하는) 위치에 대해 무엇을 말해주는지 설명하라.

## Part 2 — Coding Task
기본적으로 Server Component인 페이지를 만들어라(최상단에 `"use client"` 없음). 그 안에 의도적으로 인터랙티브한 부분 하나를 추가하라. 예:

```jsx
// Server Component 페이지 안 — 아직 고치지 마라.
function LikeButton() {
  const [liked, setLiked] = useState(false); // 여기서는 동작하지 않는다
  return <button onClick={() => setLiked(!liked)}>{liked ? "Liked" : "Like"}</button>;
}
```

실행해서 실제로 Next.js가 내는 에러를 캡처하라(빌드 에러든 런타임 에러든 — 정확한 메시지를 `NOTES.md`에 복사하라). 이 단계를 건너뛰지 마라; 고치기 전에 실제 실패를 반드시 확인해야 한다.

그다음 인터랙티브한 부분을 최상단에 `"use client"`가 있는 별도의 컴포넌트 파일/모듈로 추출하여 Server Component 페이지에서 import함으로써 고쳐라. 이제 동작하는지 확인하라.

마지막으로 "children으로서의 Server Component" 조합 패턴을 시연하라. `children`을 받는 Client Component 래퍼(예: 열림/닫힘 State를 관리하는 접을 수 있는 패널이나 모달 셸)를 만들고, 서버만이 저렴하게 할 수 있는 작업을 수행하는 Server Component(예: 시크릿/환경 변수가 필요한 값을 읽거나, 단순한 서버 렌더링 콘텐츠 블록)를 부모 Server Component에서 그 `children`으로 전달하라. 이 자식이 `"use client"` 없이도 동작하는지 확인하라.

## Part 3 — Requirements & Edge Cases
- [ ] 페이지는 `"use client"`가 없는 순수 Server Component로 시작한다.
- [ ] 무언가를 고치기 전에 실제 에러를 재현하고 기록했다.
- [ ] 인터랙티브한 부분이 페이지 전체가 아니라 가능한 가장 작은 별도의 Client Component 모듈로 격리되어 있다.
- [ ] `NOTES.md`에 정확히 어떤 파일에 `"use client"`가 필요했는지, 그리고 왜 (부모가 아니라, 전체 트리가 아니라) 그 파일이었는지가 명시되어 있다.
- [ ] Server Component가 `"use client"` 없이 Client Component의 `children`으로 전달되어 동작하는 시연이 있다.
- [ ] 부모를 Client Component로 바꾸는 것보다 조합 패턴(children으로서의 Server Component)이 권장되는 이유가 `NOTES.md`에 설명되어 있다.

## Submission
`workspace/track-9-nextjs-fundamentals/`에 스캐폴딩한 기존 Next.js 앱 안에 이 예제를 위한 라우트/페이지를 추가하라. `NOTES.md`에 어디에 배치했는지와 그 이유를 설명하라.
