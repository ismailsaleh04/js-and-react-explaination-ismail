# Keys

## Definition

A **key** is a special prop that gives each item in a list a **stable, unique identity** among its siblings.

When a list changes, React uses keys to match old items with new ones so it knows which items were **added, removed, or moved** — instead of re-creating everything. This keeps the right state attached to the right item.

Rules:

- Keys must be **unique among siblings** (not globally).
- Keys must be **stable** — the same item should always get the same key.
- Use an ID from your data. Avoid `Math.random()`; avoid array **index** when the list can be reordered, filtered, or have items inserted.
- `key` is not passed to the component as a prop.

## Examples

### With `map`

```tsx
<View>
  {users.map((user) => (
    <UserRow key={user.id} user={user} />
  ))}
</View>
```

### With `FlatList`

`FlatList` uses `item.key` or `item.id` automatically if present; otherwise use `keyExtractor`.

```tsx
<FlatList
  data={products}
  keyExtractor={(item) => item.id.toString()}
  renderItem={({ item }) => <ProductCard product={item} />}
/>
```

### Why index as key is a problem

```tsx
// Each row has its own TextInput (with internal state)
{todos.map((todo, index) => (
  <TodoRow key={index} todo={todo} />  // ❌
))}
```

If you delete the first todo, every item shifts one index down. React thinks item `0` still exists and just changed its data, so the **internal state (e.g. typed text, checked status, animation)** stays with the wrong row.

```tsx
{todos.map((todo) => (
  <TodoRow key={todo.id} todo={todo} />  // ✅ state follows the right item
))}
```

Index is acceptable only when the list is **static** (never reordered, filtered, or changed).

### Using `key` to reset a component

Changing a component's key makes React **unmount it and mount a fresh one**, resetting all its state.

```tsx
// Form resets automatically when switching to another user
<EditProfileForm key={selectedUser.id} user={selectedUser} />
```

## Use cases

- Rendering any list with `map`, `FlatList`, or `SectionList`.
- Keeping input/selection/animation state attached to the right list item.
- Resetting a component's state by changing its key.

## Warning you'll see without keys

```
Warning: Each child in a list should have a unique "key" prop.
```
