# Track 4: Promise Concurrency Methods

This track covers the four static `Promise` concurrency combinators — `Promise.all`, `Promise.race`, `Promise.allSettled`, and `Promise.any` — each of which runs multiple Promises concurrently but resolves/rejects under different rules. Getting these rules precisely right (not "roughly right") matters in production: picking the wrong combinator silently produces the wrong failure behavior under load. This track assumes Track 3 (Promises Deep Dive) is complete.

## Exercises

1. [Promise.all](./01-promise-all.md) — fail-fast concurrency; one rejection loses all other results.
2. [Promise.race](./02-promise-race.md) — settles on whichever Promise finishes first, success or failure.
3. [Promise.allSettled](./03-promise-allsettled.md) — waits for every Promise, returns a full array of outcomes.
4. [Promise.any](./04-promise-any.md) — first success wins; only fails if everything fails, via `AggregateError`.

## Stretch Goal

A senior engineer should be able to explain the difference between `all`, `race`, `allSettled`, and `any` in one sentence each, without hesitation. Once you've finished all four exercises, try writing those four sentences from memory and check them against what you actually observed — no answer is provided here on purpose.

## Reference

- [MDN: Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
- [MDN: Promise.race](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)
- [MDN: Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
- [MDN: Promise.any](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)
- [MDN: AggregateError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)
