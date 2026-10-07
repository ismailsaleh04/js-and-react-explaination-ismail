# Responsive Design (React Native)

## Definition

**Responsive design** means the UI adapts to different **screen sizes, orientations, and pixel densities** (small phones, large phones, tablets, foldables, landscape) so it looks and works well everywhere.

---

> *ANALOGY I FOUND USEFUL BY CHATGPT:*

In React Native, you achieve it with:

| Tool                              | What it does                                                        |
| --------------------------------- | ------------------------------------------------------------------- |
| **Flexbox**                       | Default layout system — `flex`, `flexDirection`, `justifyContent`, `alignItems`, `gap`, `flexWrap` |
| **Percentages**                   | `width: '50%'`                                                       |
| **`useWindowDimensions()`**       | Current window width/height, **updates on rotation/resize**          |
| `Dimensions.get('window')`        | Same values, but **doesn't update** automatically                     |
| **`aspectRatio`**                 | Keeps proportions (`aspectRatio: 16 / 9`)                            |
| **Safe area**                     | `react-native-safe-area-context` to avoid notches / home indicator   |
| `PixelRatio` / `fontScale`        | Handle screen density and user font size settings                    |
| `Platform`                        | Platform-specific styles (`Platform.OS`, `Platform.select`)          |

