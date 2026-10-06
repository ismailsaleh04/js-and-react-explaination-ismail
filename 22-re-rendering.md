# Re-rendering

## Definition

**Rendering** means React **calls your component function** to get the JSX describing the UI. A **re-render** is calling it again to see if the UI should change.

After a render, React compares the new output with the previous one (**reconciliation**) and only updates the native views that actually changed. So a re-render is not the same as redrawing the screen — but too many unnecessary re-renders can still slow down the app.

### What triggers a re-render

1. **State changes** — calling a `setState` with a different value.
2. **Parent re-renders** — by default, all children re-render too (even if their props didn't change).
3. **Context changes** — every component that uses that context re-renders.
4. **Custom hook state changes** — a hook's state is the component's state.

> ⚠️ Changing **props alone** doesn't trigger anything — props change *because* the parent re-rendered.
> Changing a **ref** or a normal variable never triggers a re-render.

## Examples

### Parent re-render cascades to children

```tsx
const Child = () => {
  console.log('Child render');
  return <Text>I'm a child</Text>;
};

const Parent = () => {
  const [count, setCount] = useState(0);
  console.log('Parent render');
  return (
    <View>
      <Button title={`${count}`} onPress={() => setCount((c) => c + 1)} />
      <Child /> {/* re-renders every time, although it has no props */}
    </View>
  );
};
```

### Fix 1: move state down

Keep state in the smallest component that needs it.

```tsx
const CounterButton = () => {
  const [count, setCount] = useState(0);
  return <Button title={`${count}`} onPress={() => setCount((c) => c + 1)} />;
};

const Parent = () => (
  <View>
    <CounterButton />
    <Child /> {/* no longer re-renders */}
  </View>
);
```

### Fix 2: memoize

```tsx
const Child = React.memo(() => <Text>I'm a child</Text>);
```

### Fix 3: pass components as children

```tsx
const ScrollTracker = ({ children }: { children: React.ReactNode }) => {
  const [y, setY] = useState(0);
  return (
    <ScrollView onScroll={(e) => setY(e.nativeEvent.contentOffset.y)}>
      {children} {/* children were created by the parent, so they don't re-render */}
    </ScrollView>
  );
};
```

### Same value → no re-render

```tsx
setCount(5); // if count is already 5, React bails out

// ❌ Mutation — same reference, React sees no change
items.push(newItem);
setItems(items);

// ✅ New reference
setItems([...items, newItem]);
```

### Debugging

```tsx
console.log('render', Date.now());
```

Or use **React DevTools → Profiler → "Highlight updates when components render"**.

## Use cases / why it matters

- Fixing laggy lists, inputs, or animations in React Native.
- Knowing why a component shows stale or unexpected data.
- Deciding where to put state and when to use `React.memo` / `useMemo` / `useCallback`.

## Checklist for unnecessary re-renders

- Is state placed higher than it needs to be?
- Are you passing new objects/functions inline to memoized children?
- Is a big Context value changing often? Split it or memoize the value.
- Are list items memoized with stable `keyExtractor` keys?
