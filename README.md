# Promise Practice — JS Runtime / Async / React / Next.js Training

[한국어는 아래를 참고하세요 ↓](#한국어)

## Rules (read before you start)

1. **No AI.** You may read MDN, the React docs, and the Next.js docs. You may not
   paste any exercise into ChatGPT/Claude/Copilot and ask for the answer. This
   is a training program for your own mental model — if AI writes it, you
   learn nothing and it will show immediately when your mentor questions you.
2. **One exercise at a time, in order.** Tracks 1 → 9, exercises inside a
   track in order. Do not skip ahead even if a later exercise looks more fun.
3. **Write everything yourself**, in your own editor. Every exercise has a
   "Part 1 — Understand Before You Code" section: answer those questions in
   your own words in a `NOTES.md` file next to your solution, **before**
   writing code. Shallow one-line answers ("for async processing") will get
   rejected in review.
4. **No solution files are provided anywhere in this repo.** That's
   intentional. If you're stuck, re-read the MDN link, open your browser
   console, use `debugger`, and reason from first principles.
5. When you finish a track, commit your work, then push the whole repo to
   your own GitHub and tell your reviewer. Don't wait until everything is
   done — push per track so your progress is visible.

## Structure

```
track-1-runtime/            JS Runtime: Call Stack, Memory Heap, Web APIs, single-threaded
track-2-event-loop/         Event Loop: MacroTask Queue vs MicroTask Queue
track-3-promises/           Promise states, .then/.catch/.finally, chaining
track-4-promise-concurrency/ Promise.all / race / allSettled / any
track-5-async-await/        async/await under the hood, try/catch, sequential vs parallel
track-6-fetch-devtools/     Fetch API, AbortController, Network tab, status codes
track-7-url-and-web/        URL anatomy, URLSearchParams, origin/domain, CORS
track-8-react-fundamentals/ React state/render model, useEffect, data fetching
track-9-nextjs-fundamentals/ App Router, Server vs Client Components, data fetching
workspace/                  Put ALL your code here (see below)
```

Each track has `en/` and `ko/` folders with the **same exercises**, just in
two languages — pick whichever you're more comfortable reading. There's no
difference in difficulty or content between them.

## Where to write your code

Create your own subfolder per exercise inside `workspace/`, e.g.:

```
workspace/track-1-runtime/01-call-stack/solution.js
workspace/track-1-runtime/01-call-stack/NOTES.md
```

For track 8 and 9 (React / Next.js) you will scaffold your own project
(`npx create-next-app@latest` etc.) inside `workspace/track-8-react-fundamentals/`
or `workspace/track-9-nextjs-fundamentals/` — the exercise text tells you
when and how.

## Submission

Push to your own GitHub repo (fork or new repo, your choice) and share the
link with your reviewer once a track is complete. Commit messages should
reference the track/exercise, e.g. `track-3 ex-2: promise chaining`.

---

## 한국어

## 규칙 (시작 전 필독)

1. **AI 사용 금지.** MDN, React 공식 문서, Next.js 공식 문서는 읽어도 됩니다.
   하지만 문제를 ChatGPT/Claude/Copilot에 붙여넣고 답을 받는 행위는 금지합니다.
   이 트레이닝은 여러분 스스로의 사고 구조를 만드는 과정입니다. AI가 대신
   작성하면 아무것도 남지 않고, 리뷰 때 바로 드러납니다.
2. **한 번에 한 문제씩, 순서대로.** Track 1부터 9까지, 각 트랙 내부의 문제도
   순서대로 진행하십시오. 뒤에 있는 문제가 더 재미있어 보여도 건너뛰지
   마십시오.
3. **모든 코드는 본인이 직접 작성**하십시오. 모든 문제에는 "Part 1 — 코드
   작성 전에 이해하기" 섹션이 있습니다. 코드를 작성하기 **전에** 해당
   질문들에 대한 답을 본인 언어로 `NOTES.md` 파일에 작성하십시오. "비동기
   처리를 위해서" 같은 얕은 한 줄짜리 답변은 리뷰에서 반려됩니다.
4. **이 저장소 어디에도 정답 파일은 없습니다.** 의도된 설계입니다. 막히면
   해당 MDN 링크를 다시 읽고, 브라우저 콘솔을 열고, `debugger`를 사용해서
   원리부터 다시 추론하십시오.
5. 한 트랙을 끝낼 때마다 커밋하고, 저장소 전체를 본인 GitHub에 push한 뒤
   리뷰어에게 알리십시오. 전부 끝날 때까지 기다리지 말고 트랙 단위로
   push해서 진행 상황이 보이게 하십시오.

## 구조

```
track-1-runtime/            JS 런타임: Call Stack, Memory Heap, Web APIs, Single-threaded
track-2-event-loop/         Event Loop: MacroTask Queue vs MicroTask Queue
track-3-promises/           Promise 상태, .then/.catch/.finally, 체이닝
track-4-promise-concurrency/ Promise.all / race / allSettled / any
track-5-async-await/        async/await 내부 동작, try/catch, 순차 vs 병렬 실행
track-6-fetch-devtools/     Fetch API, AbortController, Network 탭, status code
track-7-url-and-web/        URL 구조, URLSearchParams, origin/domain, CORS
track-8-react-fundamentals/ React state/렌더링 모델, useEffect, 데이터 페칭
track-9-nextjs-fundamentals/ App Router, Server vs Client Component, 데이터 페칭
workspace/                  본인 코드는 전부 여기에 작성 (아래 참고)
```

각 트랙에는 `en/`과 `ko/` 폴더가 있으며 **문제 내용은 동일**하고 언어만
다릅니다. 편한 언어로 읽으면 됩니다. 난이도나 내용 차이는 없습니다.

## 코드 작성 위치

`workspace/` 아래에 문제별로 본인 폴더를 만드십시오. 예:

```
workspace/track-1-runtime/01-call-stack/solution.js
workspace/track-1-runtime/01-call-stack/NOTES.md
```

Track 8, 9 (React / Next.js)는 `npx create-next-app@latest` 등으로 본인이
직접 프로젝트를 생성해서 `workspace/track-8-react-fundamentals/` 또는
`workspace/track-9-nextjs-fundamentals/` 안에 넣으십시오. 언제 어떻게
생성할지는 문제 본문에 안내되어 있습니다.

## 제출

트랙이 끝나면 본인 GitHub 저장소(fork 또는 새 repo)에 push하고 리뷰어에게
링크를 공유하십시오. 커밋 메시지에는 트랙/문제 번호를 명시하십시오. 예:
`track-3 ex-2: promise chaining`.
