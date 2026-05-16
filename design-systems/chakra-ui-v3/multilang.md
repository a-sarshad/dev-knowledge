# Chakra UI v3 — Multilingual Implementation
> updated: 2026-05-16

---

## Provider Pattern

```tsx
// src/i18n/LocaleContext.tsx
import { createContext, useContext, useState } from 'react'
import { DirectionProvider, LocaleProvider } from '@chakra-ui/react'

type LanguageConfig = {
  locale: string
  dir: 'rtl' | 'ltr'
  font: string
}

const LANG_CONFIG: Record<string, LanguageConfig> = {
  fa: { locale: 'fa-IR', dir: 'rtl', font: 'Vazirmatn' },
  ar: { locale: 'ar',    dir: 'rtl', font: 'Vazirmatn' },
  en: { locale: 'en-US', dir: 'ltr', font: 'Inter' },
}

const LocaleCtx = createContext<{
  lang: string
  config: LanguageConfig
  setLang: (l: string) => void
}>({ lang: 'fa', config: LANG_CONFIG.fa, setLang: () => {} })

export function MultiLangProvider({ defaultLang = 'fa', children }) {
  const [lang, setLang] = useState(defaultLang)
  const config = LANG_CONFIG[lang] ?? LANG_CONFIG['en']

  // sync با HTML element
  useEffect(() => {
    document.documentElement.setAttribute('dir', config.dir)
    document.documentElement.setAttribute('lang', config.locale)
  }, [config])

  return (
    <LocaleCtx.Provider value={{ lang, config, setLang }}>
      <LocaleProvider locale={config.locale}>
        <DirectionProvider dir={config.dir}>
          <Box fontFamily={config.font}>
            {children}
          </Box>
        </DirectionProvider>
      </LocaleProvider>
    </LocaleCtx.Provider>
  )
}

export const useLocale = () => useContext(LocaleCtx)
```

---

## Direction-Aware Spacing

```tsx
// ✅ همیشه از logical props استفاده کن
<Box ms={4}>   // margin-inline-start
<Box me={4}>   // margin-inline-end
<Box ps={4}>   // padding-inline-start

// ❌ هیچ‌وقت از physical props استفاده نکن
<Box ml={4}>
<Box mr={4}>
```

---

## Locale Switcher Component

```tsx
export function LocaleSwitcher() {
  const { lang, setLang } = useLocale()

  return (
    <Flex gap={2}>
      {[
        { code: 'fa', label: 'فارسی' },
        { code: 'en', label: 'English' },
      ].map(opt => (
        <Button
          key={opt.code}
          size="sm"
          variant={lang === opt.code ? 'solid' : 'outline'}
          onClick={() => setLang(opt.code)}
          fontFamily={opt.code === 'fa' ? 'Vazirmatn' : 'Inter'}
        >
          {opt.label}
        </Button>
      ))}
    </Flex>
  )
}
```

---

## Integration در main.tsx

```tsx
// جایگزین LocaleProvider ثابت
<ChakraProvider value={system}>
  <MultiLangProvider defaultLang="fa">
    <ColorModeProvider>
      <App />
    </ColorModeProvider>
  </MultiLangProvider>
</ChakraProvider>
```

---

## نکات

- اگه پروژه فقط یک زبان داره، به این پیچیدگی نیاز نیست — همان `LocaleProvider locale="fa-IR"` ثابت کافیه
- `DirectionProvider` از Chakra v3، Flex و logical props رو auto-handle می‌کنه
- برای persist کردن locale انتخابی → `localStorage`
