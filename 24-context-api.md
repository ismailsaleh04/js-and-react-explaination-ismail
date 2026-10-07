# Context API

## Definition

The **Context API** lets you share data with **any component in a tree** without passing it through every level as props (**prop drilling**).

Three pieces:

1. **`createContext`** – creates the context object (with a default value).
2. **`<Context.Provider value={...}>`** – wraps part of the tree and supplies the value.
3. **`useContext(Context)`** – reads the value in any component inside the provider.

When the provider's `value` changes, **every component using that context re-renders**.

