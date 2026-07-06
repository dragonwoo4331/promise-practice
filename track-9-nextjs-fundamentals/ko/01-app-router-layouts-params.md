# [Track 9] Exercise 1: App Router 기초 — 라우트, 중첩 레이아웃, Params

## Difficulty
Easy

## Learning Goal
App Router가 파일 시스템을 URL 라우트에 어떻게 매핑하는지, 동적 세그먼트(`[id]`)가 `params` prop을 통해 라우트 파라미터를 어떻게 노출하는지, 그리고 공유 `layout`이 전체 페이지가 교체되는 것과 달리 자식 라우트 간 내비게이션에서 매번 리마운트되지 않고 어떻게 컴포넌트 State를 유지하는지 이해한다.

## Prerequisite Reading
- https://nextjs.org/docs/app/building-your-application/routing
- https://nextjs.org/docs/app/building-your-application/routing/defining-routes
- https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes
- https://nextjs.org/docs/app/api-reference/file-conventions/layout
- https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts

## Part 1 — Understand Before You Code
코드를 작성하기 전에 아래 질문에 대한 답을 본인의 언어로 `NOTES.md`에 작성하라. 피상적인 답변은 반려된다 — 막연한 유행어가 아니라 구체적인 동작 원리를 근거로 제시하라.

1. **Why Necessary?** App Router는 왜 단일 중앙 라우트 설정 파일(다른 프레임워크/라우터가 사용하는 방식) 대신 중첩 폴더 기반 컨벤션(`layout.tsx`, `page.tsx`, `[id]/page.tsx`)을 사용하는가? 이 컨벤션은 중앙 설정 파일이 어렵게 만드는 무엇을 쉽게 만드는가?
2. **Why Origin?** Next.js는 왜 중첩 레이아웃을 1급 개념으로 도입했는가(예전 Pages Router에서는 커스텀 `_app.js`가 유일한 공유 래퍼였던 것과 대조하여)? 레이아웃 중첩은 형제 라우트 간 내비게이션에서 어떤 구체적인 UX/성능 문제를 해결하는가?
3. **When to Use?** 공유 레이아웃이 내비게이션 전반에 걸쳐 컴포넌트 State를 유지해야 하는 구체적인 실무 시나리오(예: 지속되는 오디오 플레이어, 열려 있는 사이드바, 스크롤 위치, 사이드 패널의 작성 중인 폼 초안)를 설명하고, 그 레이아웃이 각 라우트에 중복 구현된 전체 페이지 컴포넌트로 대체된다면 무엇이 눈에 띄게 깨지는지 설명하라.
4. **What if Absent?** 지속적인 공유 UI를 가져야 할 두 라우트에 공유 레이아웃 대신 전체 페이지 컴포넌트를 사용하면 화면에서 무슨 일이 일어나는지(State 손실, 레이아웃 깜빡임, 재요청, 리마운트 비용) 구체적으로 설명하라.

## Part 2 — Coding Task
스캐폴딩한 Next.js 앱에 최소 3개의 라우트를 만들어라.

- 정적 라우트, 예: `/about`.
- 동적 라우트, 예: `/products/[id]`.
- 위 라우트 중 하나와 레이아웃을 공유하는 두 번째 라우트(정적 또는 동적), 예: `/products`와 `/products/[id]`를 모두 `app/products/layout.tsx`로 감싸기.

동적 라우트의 `page` 컴포넌트에서 `params` prop을 서버 사이드에서 읽고 로그를 남겨라(`console.log(params)` — 이것은 서버에서 실행되므로 브라우저 콘솔이 아니라 터미널을 확인하라). 그리고 resolve된 `id` 값을 페이지에 렌더링하라.

공유 레이아웃에 클라이언트 사이드 인터랙티브 State를 추가하라(예: 사이드바의 카운터나 펼침/접힘 토글 — 해당 부분에만 `"use client"`가 필요하다; Server/Client 경계를 완전히 이해할 필요는 없다, 그것은 Exercise 2의 내용이지만, 여기서는 지속성을 증명할 정도로는 필요하다). 해당 레이아웃 아래에서 `<Link>`를 사용해 두 라우트 사이를 내비게이션하고, 카운터/토글이 리셋되는지 관찰하여 레이아웃이 리마운트되는지 유지되는지 확인하라.

그다음 대조군으로, 공유 레이아웃의 서브트리 밖에 있는 라우트로 이동했을 때 그 State가 리셋되는 경우를 만들고, 왜 그런지 `NOTES.md`에 설명하라.

## Part 3 — Requirements & Edge Cases
- [ ] `[param]` 세그먼트를 사용하는 동적 라우트를 포함하여 최소 3개의 라우트가 존재한다.
- [ ] `layout.tsx`/`layout.js`가 최소 두 개의 라우트를 감싼다.
- [ ] 동적 라우트의 `params`를 읽고 로그로 남기며(서버 사이드 터미널 로그), 값이 UI에 렌더링된다.
- [ ] 레이아웃으로 감싸인 두 라우트 사이를 내비게이션하면 레이아웃의 인터랙티브 State가 유지된다는 것을 증명했다(`NOTES.md`에 관찰 내용 기술).
- [ ] 공유 레이아웃의 서브트리 밖으로 이동하면 해당 State가 리셋된다는 것을 증명했다.
- [ ] 유효하지 않거나 예상치 못한 동적 세그먼트 값을 처리한다(페이지가 무엇을 하는지 직접 정의하고 문서화하라 — 예: not-found 상태).

## Submission
`workspace/track-9-nextjs-fundamentals/`에 스캐폴딩한 기존 Next.js 앱 안에 이 예제를 위한 라우트/페이지를 추가하라. `NOTES.md`에 어디에 배치했는지와 그 이유를 설명하라.
