# Responsive Design (React Native)

## Definition

**Responsive design** means the UI adapts to different **screen sizes, orientations, and pixel densities** (small phones, large phones, tablets, foldables, landscape) so it looks and works well everywhere.

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

> React Native units are **density-independent pixels (dp)**, so `width: 100` looks similar in physical size on different devices.

## Examples

### Flexbox layout

```tsx
<View style={{ flex: 1 }}>
  <View style={{ height: 60 }} />              {/* header: fixed */}
  <View style={{ flex: 1 }} />                 {/* content: takes remaining space */}
  <View style={{ flexDirection: 'row', gap: 8 }}>
    <View style={{ flex: 1 }} />               {/* two equal columns */}
    <View style={{ flex: 1 }} />
  </View>
</View>
```

### Breakpoints with useWindowDimensions

```tsx
const ProductsGrid = ({ products }: { products: Product[] }) => {
  const { width } = useWindowDimensions();

  const numColumns = width >= 900 ? 4 : width >= 600 ? 3 : 2;
  const gap = 12;
  const itemWidth = (width - gap * (numColumns + 1)) / numColumns;

  return (
    <FlatList
      key={numColumns}                 // FlatList requires a new key when numColumns changes
      data={products}
      numColumns={numColumns}
      columnWrapperStyle={{ gap, paddingHorizontal: gap }}
      renderItem={({ item }) => <ProductCard product={item} style={{ width: itemWidth }} />}
    />
  );
};
```

### Orientation

```tsx
const { width, height } = useWindowDimensions();
const isLandscape = width > height;

<View style={{ flexDirection: isLandscape ? 'row' : 'column' }}>
  <Image style={{ flex: 1, aspectRatio: 1 }} source={img} />
  <Details style={{ flex: 1 }} />
</View>
```

### Safe areas

```tsx
import { SafeAreaView } from 'react-native-safe-area-context';

<SafeAreaView style={{ flex: 1 }} edges={['top', 'bottom']}>
  <Screen />
</SafeAreaView>
```

### Scaling helper (optional)

```tsx
const BASE_WIDTH = 375; // design width (e.g. iPhone in Figma)

export const useScale = () => {
  const { width } = useWindowDimensions();
  return (size: number) => (width / BASE_WIDTH) * size;
};

const scale = useScale();
<Text style={{ fontSize: scale(16) }}>Title</Text>
```

### Platform-specific styles

```tsx
const styles = StyleSheet.create({
  card: {
    ...Platform.select({
      ios: { shadowColor: '#000', shadowOpacity: 0.1, shadowRadius: 6 },
      android: { elevation: 3 },
    }),
  },
});
```

## Use cases

- Grids that show more columns on tablets.
- Layouts that switch from column to row in landscape.
- Images that keep their proportions with `aspectRatio`.
- Avoiding notches and the home indicator.
- Text that respects the user's accessibility font size.

## Tips

- Prefer **flex** and **percentages** over hard-coded widths.
- Use `useWindowDimensions` instead of `Dimensions.get` so the UI updates on rotation.
- Test on a small phone, a large phone, a tablet, and in landscape.
