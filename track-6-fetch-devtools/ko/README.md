# Track 6 — Fetch API & DevTools 디버깅

이 트랙은 Fetch API의 실제 실패 동작 원리(HTTP 에러 status에서 reject되지
않는다는 점), `AbortController`를 통한 요청 취소/타임아웃, 그리고 Chrome
DevTools에서 실제 네트워크 동작을 읽는 방법을 다룹니다 — 코드가
`localhost`를 벗어나 실제 지연 시간과 실제 에러 응답을 만나는 순간부터
중요해지는 능력들입니다.

먼저 MDN 트랙 랜딩 페이지를 읽으십시오:
https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API

## Exercises

1. [01 — 기본 Fetch와 수동 Status Code 처리](./01-fetch-status-codes.md)
   실제 공개 API에서 fetch하고 404/500 응답을 수동으로 처리합니다.
   `fetch()`는 네트워크 실패에서만 reject되고 HTTP 에러 status에서는 절대
   reject되지 않기 때문입니다.
2. [02 — `AbortController`와 수동 Fetch 타임아웃](./02-abortcontroller-timeout.md)
   `AbortController`를 사용해 fetch 타임아웃을 구현하고, 결과로 발생하는
   `AbortError`를 다른 실패 유형과 구분합니다.
3. [03 — Network 탭과 Throttling 조사](./03-network-tab-throttling.md)
   DevTools 조사 exercise: Slow 3G로 throttle하고, 타이밍 워터폴을 읽고,
   캐시 동작을 문서화합니다 — solution code가 필요 없습니다.

제출 규칙(AI 사용 금지, 코드 작성 전 `NOTES.md`, 트랙 단위 push)은 저장소
루트의 `README.md`를 참고하십시오.
