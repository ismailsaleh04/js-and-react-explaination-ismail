# Context API

## Definition

The **Context API** lets you share data with **any component in a tree** without passing it through every level as props (**prop drilling**).

Three pieces:

1. **`createContext`** – creates the context object (with a default value).
2. **`<Context.Provider value={...}>`** – wraps part of the tree and supplies the value.
3. **`useContext(Context)`** – reads the value in any component inside the provider.

When the provider's `value` changes, **every component using that context re-renders**.

```
Without context (prop drilling):        With context:
App → Layout → Header → Avatar(user)    App(Provider) ··· Avatar(useContext)
```

## Examples

### Theme context

```tsx
// ThemeContext.tsx
import { createContext, useContext, useMemo, useState, ReactNode } from 'react';

type Theme = 'light' | 'dark';

interface ThemeContextValue {
  theme: Theme;
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextValue | undefined>(undefined);

export const ThemeProvider = ({ children }: { children: ReactNode }) => {
  const [theme, setTheme] = useState<Theme>('light');

  const value = useMemo(
    () => ({
      theme,
      toggleTheme: () => setTheme((t) => (t === 'light' ? 'dark' : 'light')),
    }),
    [theme]
  );

  return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
};

export const useTheme = () => {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be used within ThemeProvider');
  return ctx;
};
```

```tsx
// App.tsx
export default function App() {
  return (
    <ThemeProvider>
      <HomeScreen />
    </ThemeProvider>
  );
}

// Any nested component
const ThemeSwitch = () => {
  const { theme, toggleTheme } = useTheme();
  return <Switch value={theme === 'dark'} onValueChange={toggleTheme} />;
};
```

### Auth context

```tsx
interface AuthContextValue {
  user: User | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextValue | undefined>(undefined);

export const AuthProvider = ({ children }: { children: ReactNode }) => {
  const [user, setUser] = useState<User | null>(null);

  const login = useCallback(async (email: string, password: string) => {
    const u = await api.login(email, password);
    setUser(u);
  }, []);

  const logout = useCallback(() => setUser(null), []);

  const value = useMemo(() => ({ user, login, logout }), [user, login, logout]);

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
};

// Switch navigation stacks based on auth
const RootNavigator = () => {
  const { user } = useAuth();
  return user ? <AppStack /> : <AuthStack />;
};
```

## Use cases

- Theme (light/dark).
- Authenticated user and auth actions.
- Language / localization.
- Cart in a shopping app.
- App-wide settings.

## Tips

- **Memoize the `value`** with `useMemo` so consumers don't re-render on every provider render.
- **Split contexts** by concern (e.g. `AuthContext`, `ThemeContext`) so a change in one doesn't re-render consumers of another.
- Wrap `useContext` in a **custom hook** with an error check.
- Context is great for **low-frequency** global data. For large, frequently-updated state, consider Zustand / Redux Toolkit.
