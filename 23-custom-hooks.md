# Custom Hooks

## Definition

A **custom hook** is a function whose name starts with **`use`** and that calls other hooks. It lets you **extract and reuse stateful logic** across components.

- Each component that calls a custom hook gets its **own independent state** — hooks share *logic*, not *data*.
- Follows the same rules as built-in hooks: call at the top level, only from components or other hooks.
- Can return anything: a value, an array, an object, functions.
