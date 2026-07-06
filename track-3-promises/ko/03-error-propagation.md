# [Track 3] Exercise 3: 체인 전반에 걸친 에러 전파

## Difficulty
Medium

## Learning Goal
`.then()` 체인 안에서 던져진 에러(또는 reject된 Promise)가 중간의 `.then()` 핸들러들을 건너뛰고 가장 가까운 `.catch()`로 곧바로 전달된다는 사실, 그리고 `.catch()` 자체도 (기본적으로는 fulfilled 상태의) 새로운 Promise를 반환하므로 — `.catch()` 핸들러가 명시적으로 다시 throw하지 않는 한 — 체인이 이후에도 "복구"되어 계속될 수 있다는 사실을 이해한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

## Part 1 — 코드를 작성하기 전에 이해하기
아래 질문에 대해 코드를 작성하기 전에 자신의 언어로 `NOTES.md`에 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 인정되지 않는다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. Why Necessary? — 체인의 각 단계마다 개별적으로 `try/catch`에 준하는 처리를 요구하는 대신, 왜 체인 끝의 단 하나의 `.catch()`가 체인 어느 지점에서 발생한 에러든 가로챌 수 있어야 하는가?
2. Why Origin? — 중첩된 콜백 피라미드에서는 모든 개별 콜백이 자신의 `err` 인자를 스스로 체크(하거나 깜빡 잊어버리고 체크하지 않아야) 했던 Callback Hell의 구체적인 고통을, "가장 가까운 핸들러로 건너뛰는" 이 동작이 어떻게 해결하는지 설명하라.
3. When to Use? — 여러 단계로 이루어진 체인(예: 입력 검증 → API 호출 → 데이터베이스 저장)이 있는 구체적인 프로덕션 시나리오를 설명하고, 매 단계마다 에러를 처리하는 대신 마지막의 `.catch()` 하나에 에러 처리를 집중시키는 것이 왜 더 나은지 서술하라.
4. What if Absent? — 체인에 `.catch()`가 전혀 없고 어느 `.then()` 단계에서 에러가 던져졌다면, 실제로 런타임에서(Node.js와 브라우저 각각에서) 무슨 일이 일어나며, 프로덕션에서 이것이 어떤 재앙(예: 조용한 실패, 프로세스 크래시)을 초래할 수 있는가?

## Part 2 — 코딩 과제
다음 정확한 구조와 동작을 갖는 `.then()` 체인을 만들어라.

1. resolve되는 초기 Promise.
2. 값을 정상적으로 변형하는 `.then()` 호출 최소 2개.
3. 체인 중간에서 의도적으로 `Error`를 `throw`하는 세 번째 `.then()`.
4. throw 이후, `.catch()` 이전에 위치한 하나 이상의 추가 `.then()` 호출 — 이들은 절대 실행되어서는 안 되며, 실행되지 않음을 직접 증명/관찰해야 한다.
5. 에러를 잡는 단일 마지막 `.catch()`.
6. 그 `.catch()` **이후에** 위치하여, 전달받은 값을 로그로 남기는 `.then()`.

이 변형이 동작하고 결과를 기록했다면, 같은 파일에 **두 번째 변형**을 작성하라: `.catch()` 핸들러가 에러를 삼키는 대신 **다시 throw**(또는 새 에러를 throw)하도록 바꾸고, 그 뒤에 재throw된 에러를 잡는 두 번째, 마지막 `.catch()`를 추가하라. 이 변형에서 첫 번째 `.catch()` 이후(두 번째 `.catch()` 이전)에 위치한 `.then()`이 실행되는지 여부를 예측한 뒤 검증하라.

`NOTES.md`에 다음을 명확히 답하라: `.catch()`가 에러를 처리한 뒤(다시 throw하지 않는다고 가정할 때) 실행이 정말로 "복구"되어 정상적으로 계속되는가? 만약 catch 핸들러가 명시적으로 아무것도 `return`하지 않는다면, 다시 throw하지 않는 `.catch()`가 반환하는 Promise의 resolve 값은 무엇인가?

## Part 3 — 요구사항 및 엣지 케이스
- [ ] throw와 `.catch()` 사이에 위치한 `.then()` 호출들이 실행되지 않음이 증명되었다 (예: 출력에 결코 나타나지 않는 로그문을 통해).
- [ ] (재throw하지 않는) `.catch()` 이후의 마지막 `.then()`이 실행되어 값을 로그로 남기며, 복구를 시연한다.
- [ ] `.catch()`가 재throw하는 두 번째 변형이 존재하고, 두 catch 사이에 (있다면) `.then()`이 실행되지 않음이 증명되며, 마지막 `.catch()`는 재throw된 에러를 실제로 잡는다.
- [ ] `NOTES.md`에 재throw하지 않는 `.catch()` 이후의 `.then()`으로 정확히 어떤 값이 흘러 들어가는지, catch 핸들러에 명시적 `return`이 없는 경우를 기준으로 서술되어 있다.
- [ ] 에러는 문자열 등 다른 원시값이 아니라 실제 `Error` 객체(예: `new Error('message')`)로 생성되었다 — `Error` 객체를 사용하는 것이 왜 특별히 중요한지 한 문장으로 설명하라 (힌트: 스택 트레이스).

## Submission
`workspace/track-3-promises/03-error-propagation/`에 답안/코드를 작성하라. `NOTES.md`(Part 1 답변 및 복구/재throw 분석 포함)와 `solution.js`를 포함해야 한다.
