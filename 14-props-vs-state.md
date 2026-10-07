# Props vs State

## Definition

Both props and state are plain data that affect what a component renders, and changes to either cause a re-render. The difference is **who owns it**.

- **Props** – data **passed into** a component from its parent. **Read-only** inside the child (like function arguments).
- **State** – data **owned and managed by the component itself**. It can change over time via its setter (like a component's private memory).

---
> *A table I found useful from ChatGPT:*

| | Props | State |
| --- | --- | --- |
| Owned by | Parent | The component itself |
| Can the component change it? | ❌ No (read-only) | ✅ Yes, with `setState` |
| How it's set | Attributes in JSX: `<Card title="Hi" />` | `useState(initial)` |
| Purpose | Configure / customize a component | Track values that change (input, toggles, data) |
| Flow | Parent ➜ Child (one-way, top-down) | Local, can be passed down as props |
