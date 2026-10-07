# Localization (i18n)

## Definition

**Localization (l10n)** is adapting an app to different **languages and regions**: translated text, date/number/currency formats, and layout direction (LTR / RTL).

**Internationalization (i18n)** is preparing the code so it *can* be localized — e.g. never hard-coding user-facing strings.

In React Native, a common setup is:

- **`i18next` + `react-i18next`** – translation library and React hooks (`useTranslation`).
- **`expo-localization`** or **`react-native-localize`** – detect the device language/region.
- **`I18nManager`** – enable right-to-left (RTL) layout for languages like Arabic.
- **`Intl`** – built-in JS API for formatting dates, numbers, currencies.
