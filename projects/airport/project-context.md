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

> منبع: `export.json` از Figma Variables — کالکشن‌های `Brands`, `Grid`, `Theme`
> آخرین آپدیت: 2026-05-18

### Color Palette — `Brands/Colors`

| Step | Hex | کاربرد |
|------|-----|--------|
| 0 | `#f0f6f8` | lightest bg, body background |
| 50 | `#dee9ed` | secondary bg, navbar/row text |
| 100 | `#ccdce1` | secondary bg alt |
| 150 | `#b9cfd6` | hover bg (light mode) |
| 200 | `#a7c2ca` | — |
| 300 | `#83a8b4` | borders, row border |
| 400 | `#5e8e9d` | hover bg (dark mode) |
| **500** | **`#3a7486`** | **Primary — main brand color** |
| 600 | `#2e5d6b` | primary dark, borders |
| 700 | `#234650` | sidebar bg, footer bg, header bg |
| 800 | `#172e36` | navbar bg, sidebar dark bg |
| 850 | `#112328` | — |
| 900 | `#0c171b` | body text |
| 950 | `#060c0d` | — |
| 1000 | `#000000` | black |

### SCSS Variables

```scss
// src/styles/_tokens.scss

// --- Brand Palette ---
$brand-0:   #f0f6f8;
$brand-50:  #dee9ed;
$brand-100: #ccdce1;
$brand-150: #b9cfd6;
$brand-200: #a7c2ca;
$brand-300: #83a8b4;
$brand-400: #5e8e9d;
$brand-500: #3a7486;   // PRIMARY
$brand-600: #2e5d6b;
$brand-700: #234650;
$brand-800: #172e36;
$brand-850: #112328;
$brand-900: #0c171b;
$brand-950: #060c0d;

// --- Semantic Tokens ---
$brand-primary:        $brand-500;   // #3a7486
$brand-primary-hover:  $brand-400;   // #5e8e9d
$brand-primary-dark:   $brand-600;   // #2e5d6b
$brand-primary-darker: $brand-700;   // #234650

// --- Layout ---
$body-bg:         $brand-0;    // #f0f6f8
$body-text:       $brand-900;  // #0c171b
$navbar-bg:       $brand-800;  // #172e36
$navbar-color:    $brand-50;   // #dee9ed
$navbar-border:   $brand-600;  // #2e5d6b
$sidebar-bg:      $brand-700;  // #234650
$sidebar-bg-dark: $brand-800;  // #172e36
$sidebar-border:  $brand-600;  // #2e5d6b
$footer-bg:       $brand-700;  // #234650
$footer-border:   $brand-0;    // #f0f6f8

// --- Status ---
$error:   #DC2626;
$success: #16A34A;
$warning: #EA580C;
```

### Bootstrap 5 Overrides

```scss
// src/styles/_bootstrap-override.scss

// Override Bootstrap primary with brand color
$primary: #3a7486;

$theme-colors: (
  "primary":   #3a7486,
  "secondary": #234650,
  "dark":      #172e36,
  "light":     #f0f6f8,
);

// Body
$body-bg:    #f0f6f8;
$body-color: #0c171b;
```

### Grid Tokens (Dark / Light)

| Token | Dark Mode | Light Mode |
|-------|-----------|------------|
| Header.text | Colors.50 `#dee9ed` | Colors.50 `#dee9ed` |
| Header.background | Colors.700 `#234650` | Colors.700 `#234650` |
| Header.border | Colors.0 `#f0f6f8` | Colors.0 `#f0f6f8` |
| Row.text | Colors.0 `#f0f6f8` | Colors.700 `#234650` |
| Row.background | Colors.500 `#3a7486` | Colors.50 `#dee9ed` |
| Row.background-Alt | Colors.600 `#2e5d6b` | Colors.100 `#ccdce1` |
| Row.background-hover | Colors.400 `#5e8e9d` | Colors.150 `#b9cfd6` |
| Row.background-Dark | Colors.700 `#234650` | Colors.700 `#234650` |
| Row.border | Colors.300 `#83a8b4` | Colors.300 `#83a8b4` |

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

| فایل | مسیر | کاربرد |
|------|------|--------|
| `DashboardLayout` | `src/layouts/DashboardLayout.tsx` | layout اصلی — navbar + sidebar |

- **۹۰٪+ صفحات** از این layout استفاده می‌کنن
- صفحات **بدون layout**: login، signup
- sidebar items بر اساس **permission/role** فیلتر می‌شن
- در موبایل: sidebar مخفی می‌شه، با drawer/hamburger جایگزین می‌شه
- قبل از ساخت هر صفحه جدید این فایل رو بخون

---

## نکات مهم

- Bootstrap 5 RTL: کافیه `dir="rtl"` روی `<html>` باشه — Bootstrap logical classes داره
- `ms-*` / `me-*` / `ps-*` / `pe-*` در Bootstrap 5 direction-aware هستن (مثل Chakra)
- برای فونت: `font-family` باید بر اساس lang تغییر کنه (Vazirmatn ↔ Inter)
- react-bootstrap components استاندارد Bootstrap هستن، فقط به صورت React component
