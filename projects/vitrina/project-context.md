# Vitrina — Project Context
> updated: 2026-05-18

---

## معرفی پروژه

| فیلد | مقدار |
|------|-------|
| نام | Vitrina |
| نوع | Dashboard / E-commerce admin |
| Framework | React + Vite + TypeScript |
| Design System | Chakra UI v3 |
| زبان | فارسی — RTL only |
| فونت | Vazirmatn |
| Package Manager | pnpm |

---

## Brand Tokens

| کاربرد | Chakra Color | Hex (500) |
|--------|-------------|-----------|
| Primary | `teal` | `#0D9488` |
| Primary Hover | `teal.600` | `#0F766E` |
| Error | `red` | `#DC2626` |
| Success | `green` | `#16A34A` |
| Warning | `orange` | `#EA580C` |

```ts
// src/theme/tokens.ts
brand: {
  solid:     { value: { base: '{colors.teal.600}', _dark: '{colors.teal.400}' } },
  fg:        { value: { base: '{colors.teal.700}', _dark: '{colors.teal.300}' } },
  muted:     { value: { base: '{colors.teal.100}', _dark: '{colors.teal.900}' } },
  subtle:    { value: { base: '{colors.teal.50}',  _dark: '{colors.teal.950}' } },
  emphasized:{ value: { base: '{colors.teal.200}', _dark: '{colors.teal.800}' } },
  contrast:  { value: { base: 'white',             _dark: '{colors.teal.950}' } },
  focusRing: { value: { base: '{colors.teal.600}', _dark: '{colors.teal.400}' } },
  border:    { value: { base: '{colors.teal.300}', _dark: '{colors.teal.700}' } },
}
```

---

## Grid System

| Grid | ستون | Gutter | Margin |
|------|------|--------|--------|
| Main | 12 | 16px | 16px |
| Card Stretch | 10 | 16px | 24px |

---

## Breakpoints

| نام | عرض |
|-----|-----|
| Mobile | 480px |
| Desktop | 1440px |
| Wide | 1920px |

### Layout در هر breakpoint

| متغیر | 480px | 1440px | 1920px |
|--------|-------|--------|--------|
| Navbar width | 512px | 1440px | 1920px |
| Main width | 512px | 1184px | 1664px |
| Sidebar width | 0 | 256px | 256px |
| Content start | 0 | 256px | 256px |

---

## Feature Flags (Boolean Variables)

| Variable | Default | کاربرد |
|----------|---------|--------|
| Theme/Light | true | حالت روشن |
| Theme/Dark | false | حالت تاریک |
| Products/Show Discount | false | نمایش تخفیف |
| Badge | true | نمایش badge |

---

## نکات مهم پروژه

1. سه breakpoint: 480 (mobile)، 1440 (desktop)، 1920 (wide)
2. Navbar عرض کامل — Content از سمت راست 256px offset داره
3. سیستم discount و currency جداگانه
4. Dark/Light mode از طریق boolean variable
