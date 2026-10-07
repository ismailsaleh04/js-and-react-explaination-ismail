# Re-rendering

## Definition

Docs definition:

**Rendering** means React **calls your component function** to get the JSX describing the UI. A **re-render** is calling it again to see if the UI should change.


After a render, React compares the new output with the previous one ***reconciliation***.

too many unnecessary re-renders can still slow down the app.

### What triggers a re-render

1. **State changes**
2. **Parent re-renders**
3. **Context changes**: every component that uses that context re-renders.
4. **Custom hook state changes** 

## Checklist for unnecessary re-renders

- Is state placed higher than it needs to be?
- Are you passing new objects/functions inline to memoized children?
- Is a big Context value changing often? Split it or memoize the value.
- Are list items memoized with stable `keyExtractor` keys?
