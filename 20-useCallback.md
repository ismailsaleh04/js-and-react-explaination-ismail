# useCallback

## Definition

`useCallback` **caches a function definition** between renders, returning the **same function reference** until one of its dependencies changes.

```tsx
const handlePress = useCallback(() => {
  doSomething(id);
}, [id]);
```

Why it matters: in JavaScript, functions are objects, and every render creates a **new** function:

```js
(() => {}) === (() => {}); // false
```

So passing an inline function to a child wrapped in `React.memo` makes it re-render every time, because the prop "changed".

`useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`.

