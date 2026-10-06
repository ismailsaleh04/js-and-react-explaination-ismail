# Custom Hooks

## Definition

A **custom hook** is a function whose name starts with **`use`** and that calls other hooks. It lets you **extract and reuse stateful logic** across components.

- Each component that calls a custom hook gets its **own independent state** — hooks share *logic*, not *data*.
- Follows the same rules as built-in hooks: call at the top level, only from components or other hooks.
- Can return anything: a value, an array, an object, functions.

## Examples

### useToggle

```tsx
const useToggle = (initial = false) => {
  const [value, setValue] = useState(initial);
  const toggle = useCallback(() => setValue((v) => !v), []);
  return [value, toggle] as const;
};

// Usage
const [isVisible, toggleVisible] = useToggle();
<Button title="Show" onPress={toggleVisible} />
```

### useFetch (generic)

```tsx
function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const controller = new AbortController();
    setLoading(true);

    fetch(url, { signal: controller.signal })
      .then((res) => {
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        return res.json();
      })
      .then((json: T) => setData(json))
      .catch((e) => {
        if (e.name !== 'AbortError') setError(e.message);
      })
      .finally(() => setLoading(false));

    return () => controller.abort();
  }, [url]);

  return { data, loading, error };
}

// Usage
const { data, loading, error } = useFetch<Product[]>('https://api.example.com/products');
```

### useDebounce

```tsx
function useDebounce<T>(value: T, delay = 500) {
  const [debounced, setDebounced] = useState(value);

  useEffect(() => {
    const t = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(t);
  }, [value, delay]);

  return debounced;
}

// Usage
const [search, setSearch] = useState('');
const debouncedSearch = useDebounce(search);
const { data } = useFetch<Product[]>(`/api/search?q=${debouncedSearch}`);
```

### useForm

```tsx
function useForm<T extends Record<string, string>>(initial: T) {
  const [values, setValues] = useState(initial);

  const setField = (field: keyof T) => (text: string) =>
    setValues((prev) => ({ ...prev, [field]: text }));

  const reset = () => setValues(initial);

  return { values, setField, reset };
}

// Usage
const { values, setField } = useForm({ email: '', password: '' });
<TextInput value={values.email} onChangeText={setField('email')} />
```

### Wrapping a context

```tsx
export const useAuth = () => {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth must be used inside AuthProvider');
  return ctx;
};
```

## Use cases

- Data fetching (`useFetch`, `useProducts`).
- Form handling and validation (`useForm`).
- Debounce / throttle (`useDebounce`).
- Device info: `useKeyboard`, `useAppState`, `useOrientation`.
- Persisting values (`useAsyncStorage`).
- Wrapping context access (`useAuth`, `useTheme`).
- Keeping screen components short and focused on UI.
