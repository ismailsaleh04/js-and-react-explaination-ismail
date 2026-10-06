# Props vs State

## Definition

Both props and state are plain data that affect what a component renders, and changes to either cause a re-render. The difference is **who owns it**.

- **Props** – data **passed into** a component from its parent. **Read-only** inside the child (like function arguments).
- **State** – data **owned and managed by the component itself**. It can change over time via its setter (like a component's private memory).

| | Props | State |
| --- | --- | --- |
| Owned by | Parent | The component itself |
| Can the component change it? | ❌ No (read-only) | ✅ Yes, with `setState` |
| How it's set | Attributes in JSX: `<Card title="Hi" />` | `useState(initial)` |
| Purpose | Configure / customize a component | Track values that change (input, toggles, data) |
| Flow | Parent ➜ Child (one-way, top-down) | Local, can be passed down as props |

## Examples

### Props

```tsx
interface GreetingProps {
  name: string;
}

const Greeting = ({ name }: GreetingProps) => {
  // name = 'other'; ❌ never modify props
  return <Text>Hello {name}</Text>;
};

<Greeting name="Ismail" />
```

### State

```tsx
const Toggle = () => {
  const [isOn, setIsOn] = useState(false);
  return (
    <Switch value={isOn} onValueChange={setIsOn} />
  );
};
```

### Both together: lifting state up

The parent owns the state and passes it down as props. The child tells the parent about changes through a **callback prop**.

```tsx
// Parent — owns the state
const CartScreen = () => {
  const [quantity, setQuantity] = useState(1);

  return (
    <View>
      <QuantityPicker
        value={quantity}                       // state passed down as a prop
        onChange={setQuantity}                 // callback prop
      />
      <Text>Total: ${quantity * 20}</Text>
    </View>
  );
};

// Child — only uses props, has no state of its own
interface QuantityPickerProps {
  value: number;
  onChange: (n: number) => void;
}

const QuantityPicker = ({ value, onChange }: QuantityPickerProps) => (
  <View style={{ flexDirection: 'row' }}>
    <Button title="-" onPress={() => onChange(Math.max(1, value - 1))} />
    <Text>{value}</Text>
    <Button title="+" onPress={() => onChange(value + 1)} />
  </View>
);
```

## Use cases

**Use props for:**
- Display data (title, image URL, price).
- Configuration (variant, size, disabled).
- Callbacks (`onPress`, `onChange`).

**Use state for:**
- Form input values.
- Toggles, open/closed modals, selected tab.
- Data fetched from an API, loading and error flags.

## Tips

- If a value can be **calculated** from props or other state, don't store it in state — compute it during render.
- If two siblings need the same data, **lift the state up** to their common parent.
- If many distant components need it, consider [Context API](24-context-api.md).
