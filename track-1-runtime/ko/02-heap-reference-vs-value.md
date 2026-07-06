# [Track 1] Exercise 2: Memory Heap과 참조(Reference) vs 값(Value)

## Difficulty
Easy

## Learning Goal
원시 타입(primitive, 값으로 저장/복사됨)과 객체/배열(Heap에 저장되고 Stack에서 포인터로 참조됨)이 함수에 전달되거나 새 변수에 할당될 때 어떻게 다르게 동작하는지 이해하고, 이것이 함수 밖에서 변경(mutation)이 보이는지 여부에 어떤 영향을 미치는지 설명할 수 있어야 한다.

## Prerequisite Reading
- https://developer.mozilla.org/en-US/docs/Glossary/Primitive
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_management

## Part 1 — Understand Before You Code

아래 질문에 코드를 작성하기 전에 `NOTES.md`에 본인의 언어로 답하라. "비동기 처리를 위해서"와 같은 피상적인 답변은 거부된다 — 실제 런타임 동작 원리를 근거로 설명해야 한다.

1. **Why Necessary?** — Call Stack과 Heap을 가진 단일 스레드(Single-threaded) 런타임에서, 모든 변수를 동일하게 취급하지 않고 왜 두 가지 다른 저장/복사 전략(값 vs 참조)이 필요한가?
2. **Why Origin?** — 언어 설계자들이 객체를 Heap에 저장하고 Stack에는 참조만 남기도록 만든 실질적/성능적 한계는 무엇이었는가(예: 매 할당마다 큰 구조체를 깊은 복사하는 비용, 스택 프레임이 작고 고정된 크기를 유지해야 하는 필요성)?
3. **When to Use?** — 참조 시맨틱을 이해하지 못하면 버그가 발생하는 구체적인 시나리오(예: 함수에 설정 객체를 전달하는 경우, UI의 두 부분 간 상태를 공유하는 경우)를 설명하라.
4. **What if Absent?** — 만약 JavaScript가 공유 참조를 전혀 만들 수 없고 모든 할당/함수 호출마다 깊은 복사가 일어난다면, 어떤 구체적인 문제(성능 비용, 가변 상태 공유 불가능, 메모리 사용량)가 발생하겠는가?

## Part 2 — Prediction Quiz or Coding Task

다음 코드를 보라:

```js
function updateUser(user) {
  user.age = 99;
  user = { name: "Replaced", age: 1 };
}

const person = { name: "Ari", age: 30 };
updateUser(person);
console.log(person);
```

`ANSWER.md`에 다음을 예측하고 설명하라:
- `console.log(person)`은 정확히 무엇을 출력하는가?
- 왜 `user.age = 99`는 `person`에 영향을 주지만, `user = { name: "Replaced", age: 1 }` 재할당은 영향을 주지 않는가?

그런 다음 아래 조건을 만족하는 프로그램(`solution.js`)을 직접 작성하라:
1. 최소 3개의 숫자를 가진 배열을 만든다.
2. 이 배열을 받아 제자리에서(in place) 변경하는 함수(예: `.push()`나 인덱스 할당)에 전달한다.
3. 이 배열을 받아 매개변수를 완전히 새로운 배열로 재할당하는 두 번째 함수에 전달한다.
4. 두 호출이 끝난 후 원본 배열을 로그로 남기고, 각 줄이 Heap 객체를 실제로 변경한 것인지 아니면 로컬 Stack 참조만 바뀐 것인지 주석으로 설명한다.

## Part 3 — Requirements & Edge Cases

- [ ] 최소 하나의 제자리 변경(함수 밖에서 보이는 것)과 하나의 재할당(함수 밖에서 보이지 않는 것)을 시연하라.
- [ ] 주석에서 Stack에 있는 것(참조/포인터)과 Heap에 있는 것(실제 객체/배열 데이터)을 명시적으로 구분해 설명하라.
- [ ] 도달 가능성(reachability)에 대한 설명을 추가하라(`NOTES.md` 또는 주석에): 더 이상 어떤 변수/참조도 Heap 객체를 가리키지 않게 되면, JS 엔진의 Garbage Collector가 왜 그 객체를 수거 대상으로 간주하는가?
- [ ] `{ ...obj }`(spread)로 객체를 복사하는 엣지 케이스를 다뤄라 — 이것은 깊은 복사인가, 얕은 복사인가? 중첩된 객체를 사용해 spread 이후에도 내부 객체가 여전히 공유되는지 시연하라.

## Submission
답과 코드를 `workspace/track-1-runtime/02-heap-reference-vs-value/`에 작성하라. `NOTES.md`(Part 1 답변), `ANSWER.md`(퀴즈 스니펫에 대한 예측과 근거), `solution.js`(코딩 과제)를 포함해야 한다.
