# [Track 9] Exercise 3: Server Component에서의 데이터 페칭과 searchParams

## Difficulty
Hard

## Learning Goal
Server Component의 `searchParams` prop을 통해 쿼리 문자열 필터를 읽고(클라이언트 사이드 `URLSearchParams` 없이), 그에 따라 데이터를 서버 사이드에서 필터링하며, `force-cache` 기본값, `no-store`, 시간 기반 `revalidate` 등 Next.js `fetch` 캐싱 시맨틱을 fetch가 리로드 시 실제로 재실행되는지 실증적으로 테스트하여 이해한다.

## Prerequisite Reading
- https://nextjs.org/docs/app/api-reference/file-conventions/page
- https://nextjs.org/docs/app/api-reference/functions/fetch
- https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating
- https://nextjs.org/docs/app/building-your-application/caching

## Part 1 — Understand Before You Code
코드를 작성하기 전에 아래 질문에 대한 답을 본인의 언어로 `NOTES.md`에 작성하라. 피상적인 답변은 반려된다 — 막연한 유행어가 아니라 구체적인 동작 원리를 근거로 제시하라.

1. **Why Necessary?** Server Component는 왜 Client Component가 필요로 하는 `URLSearchParams`나 클라이언트 사이드 훅 없이 `searchParams`를 prop으로 직접 읽을 수 있는가(Track 7에서 쿼리 문자열을 다뤘던 방식과 대조하여)? Server Component가 실행되는 위치와 시점의 어떤 차이가 이것을 가능하게 하는가?
2. **Why Origin?** Next.js의 확장된 `fetch`는 왜 Server Component 데이터 페칭에서 매 요청마다 항상 신선한 데이터를 가져오는 대신 적극적인 캐싱(`force-cache`)을 기본값으로 하는가? 이 기본값은 어떤 성능/비용 문제를 해결하며, 개발자가 이를 모를 경우 어떤 정합성(correctness) 문제를 조용히 유발할 수 있는가?
3. **When to Use?** `no-store`가 올바른 선택인 구체적인 실무 시나리오(데이터가 항상 최신이어야 함)와, `revalidate` 간격(예: 60초마다)이 올바른 선택인 별도의 구체적인 시나리오(성능을 위해 데이터가 약간 오래되어도 괜찮음)를 설명하라. 각 선택을 데이터의 성격으로 정당화하라.
4. **What if Absent?** 개발자가 사용자별 또는 빠르게 변하는 데이터(예: 계좌 잔액, 실시간 재고 수량)를 이를 인지하지 못한 채 기본 캐싱 동작으로 가져온다면 어떤 재앙이 발생하며, 사용자는 결과적으로 무엇을 보게 되는지 구체적으로 설명하라.

## Part 2 — Coding Task
`/products`와 같은 라우트에 페이지를 만들고, Server Component의 `searchParams` prop을 통해 쿼리 문자열에서 `category` 필터를 읽어라(예: `/products?category=shoes`) — `next/navigation`의 `useSearchParams`(이것은 Client Component 훅이며 여기서는 범위 밖이다)를 사용하지 말고, 클라이언트에서 `window.location`을 파싱하지도 마라.

mock 제품 데이터셋을 가져오거나 정의하라(하드코딩된 배열도 괜찮고, 공개 API에서 가져와도 된다) 그리고 `searchParams`의 `category` 값에 따라 서버 사이드에서 필터링하라. 필터링된 목록을 렌더링하라. `category`가 없는 경우(모든 제품 표시)와 아무것도 일치하지 않는 경우(빈 페이지가 아니라 명시적인 빈 상태 표시)를 처리하라.

그다음 통제된 캐싱 실험을 수행하라.

- `{ cache: 'force-cache' }`(또는 이와 동일한 기본값)를 사용하는 fetch 호출을 이 페이지(또는 관련 페이지)에서 하나 만들고, fetch가 실제로 실행될 때마다 타임스탬프를 로그로 남겨라(예: `console.log(Date.now(), 'force-cache fetch ran')`).
- `{ cache: 'no-store' }`를 사용하는 두 번째 fetch 호출을 만들고 마찬가지로 타임스탬프를 로그로 남겨라.
- 페이지를 여러 번 리로드하고(강제 새로고침과 일반 내비게이션 모두) 서버 사이드에서 실행되는 터미널 로그를 통해 각 fetch의 타임스탬프가 리로드마다 바뀌는지 그대로인지 관찰하라.
- 추가로 짧은 N(예: 10초)으로 `{ next: { revalidate: <N> } }`을 테스트하라. 즉시 한 번 리로드하고(동일한 캐시된 타임스탬프를 기대), N초가 지난 후 다시 한 번 리로드하라(새로운 타임스탬프를 기대).

세 가지 모드 모두에 대해 예측이 아니라 실제로 관찰한 타임스탬프를 `NOTES.md`에 기록하라.

## Part 3 — Requirements & Edge Cases
- [ ] `searchParams`가 Server Component prop으로 직접 읽힌다 — 클라이언트 사이드 쿼리 문자열 파싱이 없다.
- [ ] `category` 값에 따라 서버 사이드에서 필터링이 이루어진다.
- [ ] `category`가 없으면 모든 아이템을 표시하고, `category`가 아무것도 일치시키지 않으면 명시적인 빈 상태를 표시한다.
- [ ] `force-cache`(또는 기본값), `no-store`, `revalidate` 중 최소 두 가지가 설명만이 아니라 실제로 테스트되었다.
- [ ] `NOTES.md`에 각 fetch가 리로드 시 재실행되었는지 여부를 보여주는 실제 로그된 타임스탬프와 그 이유에 대한 해석이 담겨 있다.
- [ ] 실제 프로덕션 제품 카탈로그에서 이 특정 제품 목록 데이터에 어떤 캐싱 모드를 사용할 것인지와 그 이유가 `NOTES.md`에 설명되어 있다.

## Submission
`workspace/track-9-nextjs-fundamentals/`에 스캐폴딩한 기존 Next.js 앱 안에 이 예제를 위한 라우트/페이지를 추가하라. `NOTES.md`에 어디에 배치했는지와 그 이유를 설명하라.
