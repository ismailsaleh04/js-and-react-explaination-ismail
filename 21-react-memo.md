# React.memo

## Definition

`React.memo` is a **higher-order component** that wraps a component and **skips re-rendering it when its props haven't changed**.

```tsx
const MyComponent = React.memo(function MyComponent(props) {
  /* ... */
});
```

---
> *Analogy I found useful by chatGPT*

| Tool          | Memoizes            |
| ------------- | ------------------- |
| `React.memo`  | A **component**     |
| `useMemo`     | A **value**         |
| `useCallback` | A **function**      |
