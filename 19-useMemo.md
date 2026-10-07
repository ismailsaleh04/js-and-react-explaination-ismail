# useMemo

## Definition

`useMemo` **CACHES THE RESULTS OF CALUCULATIONS** between renders. It only recalculates when one of its dependencies changes.

```tsx
const value = useMemo(() => expensiveCalculation(a, b), [a, b]);
```

- On the first render it runs the function and stores the result.
- On later renders, if `a` and `b` are the same (`Object.is`), it returns the cached result without running the function again.

It solves two problems:

1. **Performance**: skip expensive recalculations on every render.
2. **Referential equality**: keep the same object/array reference so children wrapped in `React.memo`, or effects that depend on it, don't re-run unnecessarily.
