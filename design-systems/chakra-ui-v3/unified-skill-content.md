# Chakra UI v3 — Unified Skill Reference
> این فایل محتوای کامل skill واحد Chakra UI است (جایگزین ds-chakra-ui-fa + ds-chakra-ui-multilang)
> updated: 2026-05-18

---

## چرا این فایل وجود دارد

قبلاً دو skill جداگانه بود:
- `ds-chakra-ui-fa` ← برای RTL/فارسی
- `ds-chakra-ui-multilang` ← برای دوزبانه

این دو ادغام شدن به یک skill واحد `ds-chakra-ui` که هر سه حالت RTL، LTR و دوزبانه رو پوشش میده.

---

## نکته مهم: Brand Colors

**هیچ رنگ brand در skill مشترک وجود ندارد.**
هر پروژه رنگ brand خودش را در `projects/[name]/project-context.md` تعریف می‌کند.

```ts
// الگو در src/theme/tokens.ts هر پروژه
// BRAND را با رنگ پروژه جایگزین کن: teal, blue, violet, orange, ...
brand: {
  solid: { value: { base: '{colors.BRAND.600}', _dark: '{colors.BRAND.400}' } },
  // ...
}
```

---

## قوانین پایه — همیشه

1. Token فقط — هیچ hex مستقیم
2. Logical props — `ms/me/ps/pe` نه `ml/mr/pl/pr`
3. Direction هیچ‌وقت hardcode نشه مگر single-language
4. قبل از props ناشناخته → web_fetch از chakra-ui.com/docs

---

## سه حالت Direction

### RTL-only

```ts
// src/theme/index.ts
const layoutConfig = defineConfig({
  globalCss: { 'html, body': { direction: 'rtl', fontFamily: 'body' } },
})
export const system = createSystem(defaultConfig, projectTokens, layoutConfig)
```

```tsx
// main.tsx
<ChakraProvider value={system}>
  <LocaleProvider locale="fa-IR">
    <ColorModeProvider><App /></ColorModeProvider>
  </LocaleProvider>
</ChakraProvider>
```

### LTR-only

```ts
const layoutConfig = defineConfig({
  globalCss: { 'html, body': { direction: 'ltr', fontFamily: 'body' } },
})
// main.tsx → LocaleProvider locale="en-US"
```

### دوزبانه (runtime switching)

```tsx
// src/i18n/LocaleContext.tsx
const LANG_CONFIG = {
  fa: { locale: 'fa-IR', dir: 'rtl', font: 'Vazirmatn' },
  ar: { locale: 'ar',    dir: 'rtl', font: 'Vazirmatn' },
  en: { locale: 'en-US', dir: 'ltr', font: 'Inter'     },
}

export function MultiLangProvider({ defaultLang = 'fa', children }) {
  const [lang, setLang] = useState(defaultLang)
  const config = LANG_CONFIG[lang] ?? LANG_CONFIG['en']

  useEffect(() => {
    document.documentElement.setAttribute('dir',  config.dir)
    document.documentElement.setAttribute('lang', config.locale)
  }, [config])

  return (
    <LocaleCtx.Provider value={{ lang, config, setLang }}>
      <LocaleProvider locale={config.locale}>
        <DirectionProvider dir={config.dir}>
          <Box fontFamily={config.font}>{children}</Box>
        </DirectionProvider>
      </LocaleProvider>
    </LocaleCtx.Provider>
  )
}
```

```tsx
// main.tsx (دوزبانه)
<ChakraProvider value={system}>
  <MultiLangProvider defaultLang="fa">
    <ColorModeProvider><App /></ColorModeProvider>
  </MultiLangProvider>
</ChakraProvider>
```

---

## Known Bugs

```tsx
bg="bg.default"  // ❌ → ✅ bg="bg"
lineHeight="8"   // ❌ → ✅ lineHeight="1.333" یا "normal"
useColorMode()   // ❌ وجود ندارد → ✅ custom ColorModeContext
// dark: document.documentElement.classList.toggle('dark')
// Avatar+asChild: <Box as="button"><Avatar.Root>
```

---

## مراجع

- توکن‌های کامل: `tokens.md`
- باگ‌های کامل: `known-bugs.md`
- Docs: `https://chakra-ui.com/docs/components/[name]`
