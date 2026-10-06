# useRef

## Definition

`useRef` returns a mutable object `{ current: value }` that **persists across renders**.

```tsx
const ref = useRef(initialValue);
ref.current; // read / write
```

It has two main jobs:

1. **Referencing a native element / component** (focus an input, scroll a list).
2. **Storing a mutable value that should NOT trigger a re-render** (timer ids, previous values, flags).

