# Localization (i18n)

## Definition

**Localization (l10n)** is adapting an app to different **languages and regions**: translated text, date/number/currency formats, and layout direction (LTR / RTL).

**Internationalization (i18n)** is preparing the code so it *can* be localized — e.g. never hard-coding user-facing strings.

In React Native, a common setup is:

- **`i18next` + `react-i18next`** – translation library and React hooks (`useTranslation`).
- **`expo-localization`** or **`react-native-localize`** – detect the device language/region.
- **`I18nManager`** – enable right-to-left (RTL) layout for languages like Arabic.
- **`Intl`** – built-in JS API for formatting dates, numbers, currencies.

## Examples

### 1. Translation files

```json
// locales/en.json
{
  "welcome": "Welcome, {{name}}!",
  "cart": {
    "title": "Your cart",
    "items_one": "{{count}} item",
    "items_other": "{{count}} items"
  }
}
```

```json
// locales/ar.json
{
  "welcome": "أهلاً، {{name}}!",
  "cart": {
    "title": "سلة التسوق",
    "items_one": "عنصر واحد",
    "items_other": "{{count}} عناصر"
  }
}
```

### 2. Setup

```ts
// i18n.ts
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import { getLocales } from 'expo-localization';
import en from './locales/en.json';
import ar from './locales/ar.json';

i18n.use(initReactI18next).init({
  resources: { en: { translation: en }, ar: { translation: ar } },
  lng: getLocales()[0]?.languageCode ?? 'en', // device language
  fallbackLng: 'en',
  interpolation: { escapeValue: false },     // React already escapes
});

export default i18n;
```

```tsx
// App.tsx
import './i18n';
```

### 3. Using translations

```tsx
import { useTranslation } from 'react-i18next';

const CartHeader = ({ name, count }: { name: string; count: number }) => {
  const { t } = useTranslation();
  return (
    <View>
      <Text>{t('welcome', { name })}</Text>
      <Text>{t('cart.title')}</Text>
      <Text>{t('cart.items', { count })}</Text> {/* picks singular/plural automatically */}
    </View>
  );
};
```

### 4. Switching language + RTL

```tsx
import { I18nManager } from 'react-native';
import * as Updates from 'expo-updates';

const changeLanguage = async (lang: 'en' | 'ar') => {
  await i18n.changeLanguage(lang);
  await AsyncStorage.setItem('lang', lang); // remember the choice

  const isRTL = lang === 'ar';
  if (I18nManager.isRTL !== isRTL) {
    I18nManager.allowRTL(isRTL);
    I18nManager.forceRTL(isRTL);
    await Updates.reloadAsync(); // RTL change needs an app reload
  }
};
```

Use direction-aware styles so the layout flips correctly:

```tsx
// ✅ flips automatically in RTL
{ marginStart: 16, paddingEnd: 8, textAlign: 'left' /* 'left' maps to start in RTL */ }

// ❌ stays on the same side
{ marginLeft: 16 }
```

### 5. Formatting numbers, currency, dates

```ts
const { i18n } = useTranslation();
const locale = i18n.language;

new Intl.NumberFormat(locale, { style: 'currency', currency: 'USD' }).format(1234.5);
// en → "$1,234.50"   ar → Arabic digits and separators, e.g. "‏١٬٢٣٤٫٥٠ US$"

new Intl.DateTimeFormat(locale, { dateStyle: 'long' }).format(new Date());
// en → "October 6, 2026"   ar → "٦ أكتوبر ٢٠٢٦"
```

## Use cases

- Apps for multilingual markets (e.g. English + Arabic).
- Following the device language automatically, with an in-app language switcher.
- Supporting RTL layouts.
- Showing prices, dates, and numbers in the user's local format.
- Correct plural forms (`1 item` / `3 items`).

## Tips

- Never hard-code user-facing strings; always use `t('key')`.
- Use nested keys by screen/feature (`cart.title`, `login.button`).
- Use `start`/`end` instead of `left`/`right` in styles.
- Test with long translations and RTL to catch layout issues early.
