# Multilingual — مفاهیم پایه
> universal — concepts یکسانه، implementation در هر DS فرق داره
> updated: 2026-05-16

---

## LanguageConfig Type

```ts
type LanguageConfig = {
  locale: string        // 'fa' | 'ar' | 'en' | 'de' | ...
  dir: 'rtl' | 'ltr'
  font: string          // 'Vazirmatn' | 'Inter' | ...
}
```

---

## Locale Presets

| locale | dir | font |
|--------|-----|------|
| `fa` | `rtl` | `Vazirmatn` |
| `ar` | `rtl` | `Vazirmatn` یا `Cairo` |
| `en` | `ltr` | `Inter` |
| `de` | `ltr` | `Inter` |

fallback برای locale ناشناخته: `ltr` + `Inter`

---

## Font Loading

```css
/* فارسی / عربی */
@import url('https://fonts.googleapis.com/css2?family=Vazirmatn:wght@400;500;600;700&display=swap');

/* انگلیسی */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
```

---

## Dynamic Direction Switch

```ts
// وقتی locale در runtime عوض می‌شه
document.documentElement.setAttribute('dir', config.dir)
document.documentElement.setAttribute('lang', config.locale)
```

---

## Arrow/Chevron در Multilang

```tsx
// بر اساس dir، آیکون رو flip کن
const NextIcon = ({ dir }) =>
  dir === 'rtl' ? <ChevronLeftIcon /> : <ChevronRightIcon />
```

---

## Mixed Content

```tsx
// اعداد و کدها همیشه LTR هستن
<Text dir="rtl">
  کد پیگیری:
  <span dir="ltr" style={{ unicodeBidi: 'embed' }}>
    TRK-2024-001
  </span>
</Text>
```

---

## پیاده‌سازی در هر DS

- Chakra UI v3 → `dev-knowledge/design-systems/chakra-ui-v3/chakra-ui-v3.md` (بخش Direction Setup)
- Bootstrap 5 → `dev-knowledge/projects/airport/project-context.md` (بخش Direction Setup)
