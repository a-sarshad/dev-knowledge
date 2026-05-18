# Kish Airport — Project Context
> updated: 2026-05-18

---

## معرفی پروژه

| فیلد | مقدار |
|------|-------|
| نام | Kish Airport Admin Panel |
| نوع | Admin Panel / Dashboard |
| Framework | React + react-bootstrap |
| Design System | Bootstrap 5 (via react-bootstrap) |
| زبان | فارسی + انگلیسی — دوزبانه (RTL/LTR) |
| فونت RTL | Vazirmatn |
| فونت LTR | Inter |
| Package Manager | TBD |

---

## Brand Tokens

> ⚠️ هنوز confirm نشده — قبل از کدنویسی از Ali بپرس:
> «رنگ primary برند فرودگاه کیش چیه؟»

```scss
// src/styles/tokens.scss یا variables.scss
// بعد از confirm با Ali پر کن:
$brand-primary:       ???;
$brand-primary-hover: ???;
$brand-secondary:     ???;
$error:   #DC2626;
$success: #16A34A;
$warning: #EA580C;
```

---

## Direction Setup — دوزبانه با react-bootstrap

```tsx
// src/i18n/LocaleContext.tsx
const LANG_CONFIG = {
  fa: { locale: 'fa-IR', dir: 'rtl', font: 'Vazirmatn' },
  en: { locale: 'en-US', dir: 'ltr', font: 'Inter'     },
}

export function AppLocaleProvider({ defaultLang = 'fa', children }) {
  const [lang, setLang] = useState(defaultLang)
  const config = LANG_CONFIG[lang] ?? LANG_CONFIG['en']

  useEffect(() => {
    document.documentElement.setAttribute('dir',  config.dir)
    document.documentElement.setAttribute('lang', config.locale)
    document.documentElement.style.fontFamily = config.font
  }, [config])

  return (
    <LocaleCtx.Provider value={{ lang, config, setLang }}>
      {children}
    </LocaleCtx.Provider>
  )
}
```

---

## RTL در Bootstrap 5

```html
<!-- اگه RTL-only بود -->
<link rel="stylesheet" href="bootstrap.rtl.min.css">

<!-- اگه دوزبانه — toggle در runtime -->
<!-- نیاز به bootstrap.min.css + bootstrap.rtl.min.css هر دو داری -->
```

```tsx
// toggle در runtime با تغییر dir روی html
// Bootstrap 5 از dir="rtl" روی <html> برای RTL استفاده می‌کنه
useEffect(() => {
  document.documentElement.dir = config.dir
  // اگه دو stylesheet داری، اینجا toggle کن
}, [config.dir])
```

---

## Layout

> TBD — بعد از دریافت Figma یا briefing تکمیل کن

---

## نکات مهم

- Bootstrap 5 RTL: کافیه `dir="rtl"` روی `<html>` باشه — Bootstrap logical classes داره
- `ms-*` / `me-*` / `ps-*` / `pe-*` در Bootstrap 5 direction-aware هستن (مثل Chakra)
- برای فونت: `font-family` باید بر اساس lang تغییر کنه (Vazirmatn ↔ Inter)
- react-bootstrap components استاندارد Bootstrap هستن، فقط به صورت React component
