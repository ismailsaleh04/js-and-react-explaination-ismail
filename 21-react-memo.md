# React.memo

## Definition

`React.memo` is a **higher-order component** that wraps a component and **skips re-rendering it when its props haven't changed**.

```tsx
const MyComponent = React.memo(function MyComponent(props) {
  /* ... */
});
```

By default it does a **shallow comparison** of each prop with `Object.is`:

- Primitives (`string`, `number`, `boolean`) compare by value ✅
- Objects, arrays, and functions compare by **reference** — a new one every render counts as "changed" ❌

That's why `React.memo` usually goes together with [useMemo](19-useMemo.md) and [useCallback](20-useCallback.md).

A memoized component **still re-renders** when its own state changes or a context it uses changes.

| Tool          | Memoizes            |
| ------------- | ------------------- |
| `React.memo`  | A **component**     |
| `useMemo`     | A **value**         |
| `useCallback` | A **function**      |
