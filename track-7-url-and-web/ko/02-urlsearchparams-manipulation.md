# [Track 7] Exercise 2: URLSearchParams 조작하기

## Difficulty
Medium

## Learning Goal
`URLSearchParams`를 사용해 쿼리 문자열 파라미터를 추가, 수정, 삭제, 조회하는 방법을 익히고 — 반복되는 키를 포함하여 — 이 API가 percent-encoding을 어떻게 자동으로 처리하는지 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams
- https://developer.mozilla.org/en-US/docs/Web/API/URL/searchParams
- https://developer.mozilla.org/en-US/docs/Glossary/Percent-encoding

## Part 1 — 코드 작성 전에 이해하기
아래 질문에 코드를 작성하기 전, `NOTES.md`에 자신의 언어로 답하라. 피상적인 답변은 반려된다 — 막연한 용어가 아니라 구체적인 동작 원리를 근거로 답하라.

1. **Why Necessary?** 특수 문자, 여러 파라미터, 반복되는 키를 고려할 때 쿼리 문자열을 단순 문자열 결합(`url + '?foo=' + value`)만으로 안정적으로 만들거나 읽을 수 없는 이유는 무엇인가?
2. **Why Origin?** 애초에 쿼리 문자열 문법이 공백, `&`, `=`, `#` 같은 문자를 percent-encoding 하도록 요구하는 이유는 무엇인가 — 이런 문자들이 키나 값 안에 그대로(raw) 들어간다면 파서에는 어떤 일이 벌어지는가?
3. **When to Use?** `URLSearchParams`로 쿼리 파라미터를 만들거나 변경해야 하는 구체적인 운영 시나리오를 서술하라 (예: 페이지네이션을 위한 `page`, `pageSize` 파라미터 추가, 사용자가 컬럼 헤더를 클릭했을 때 `sort` 파라미터 갱신, 로깅 전 `token` 파라미터 제거 등).
4. **What if Absent?** 수동 쿼리 문자열 구성이 일으키는 구체적인 버그를 서술하라 — 예를 들어 검색어에 `&`가 포함되어 조용히 엉뚱한 추가 파라미터로 쪼개지는 경우, 또는 공백이 `%20`/`+`로 변환되지 않고 쿼리 문자열 자체를 깨뜨리는 경우.

## Part 2 — 코딩 과제
다음 기본 URL에서 시작한다:

```
https://example.com/search?tag=js&tag=react&tag=node&sort=asc
```

다음 기능을 제공하는 모듈을 작성하라. 모두 `URLSearchParams`만 사용하고(쿼리 문자열에 대한 수동 분리/결합 금지):

1. `addTag(url, newTag)` — 기존 `tag` 값들을 제거하지 않고 새 `tag` 값을 추가한다.
2. `replaceSort(url, newSortValue)` — 단일 `sort` 파라미터 값을 새 값으로 갱신한다 (중복 추가가 아님).
3. `removeTag(url, tagToRemove)` — 특정 `tag` 값 하나만 제거하고 나머지 `tag` 값들은 그대로 유지한다.
4. `getAllTags(url)` — 모든 `tag` 값을 배열로 반환한다.
5. `getFirstTag(url)` — 첫 번째 `tag` 값만 반환한다.
6. `buildQuery(paramsObject)` — 일반 객체(값은 문자열이거나 반복 키를 위한 문자열 배열일 수 있음)를 받아 전체 쿼리 문자열을 만들되, 공백, `&`, `=`, 비-ASCII 문자(예: `"café & bar"`)가 포함된 값을 올바르게 percent-encoding 한다.

각 함수에 대해 변경 전/후의 `toString()`(또는 전체 `href`)을 출력하여 변화를 확인할 수 있게 하라.

## Part 3 — Requirements & Edge Cases
- [ ] 쿼리 문자열을 읽거나 쓸 때 `.split('&')`, `.split('=')`, 수동 `encodeURIComponent` 결합, 정규식을 절대 사용하지 말 것 — 모든 파싱/인코딩은 `URLSearchParams`가 담당해야 한다.
- [ ] 위 예시 URL을 사용해 `.get('tag')`(첫 번째 일치 값만 반환)와 `.getAll('tag')`(모든 일치 값 반환)의 차이를 시연하라 — 미숙한 구현이 어느 쪽을 "전체를 반환한다"고 잘못 가정하기 쉬운지 명시하라.
- [ ] `removeTag`는 하나만 제거하려는 상황에서 `.delete('tag')`를 그대로 사용하면 모든 `tag` 값이 삭제되므로, 이를 단순하게 사용해서는 안 된다 — 올바른 접근법을 찾아라 (힌트: 모든 값을 읽고, 필터링한 뒤, 해당 키를 다시 구성한다).
- [ ] `"café & bar"`를 포함한 `URLSearchParams`의 `.toString()`이 올바르게 percent-encoding된 결과를 만든다는 것을 보이고, 인코딩된 원본 문자열을 증거로 출력하라.
- [ ] `URLSearchParams`가 `.toString()` 출력에서 공백을 `+`로 인코딩하는지 `%20`으로 인코딩하는지 확인하고, 실제로 관찰한 결과를 서술하라.
- [ ] `buildQuery`에서 배열 값을 가진 파라미터 객체를 처리하라 (예: `{ tag: ['a', 'b'] }`는 `tag=a&tag=b`를 만들어야 한다).

## Submission
답안/코드는 `workspace/track-7-url-and-web/02-urlsearchparams-manipulation/`에 작성하라. `NOTES.md`와 `solution.js`를 포함할 것.
