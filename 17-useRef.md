# useRef

## Definition

`useRef` returns a mutable object `{ current: value }` that **persists across renders**.

difference between useRef and useState:
1. useStates updates cause a re-render. useRef doesn't.
2. return value of useState is an array []. useRef's is an object {}.
3. to reassign useState we call the setter function, useRef's reassigning happens by `.current` method.
4. useState is primarily used with data that affects the UI. useRef with timers ande internal bookkeeping.
5.  BOTH PRESIST ACROSS RENDERS.    