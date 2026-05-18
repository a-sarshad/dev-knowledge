# Language & Direction — مفاهیم پایه
> universal — مستقل از design system
> ادغام rtl-concepts.md + multilang-concepts.md
> updated: 2026-05-18

---

## زبان‌های RTL

فارسی، عربی، عبری، اردو
در RTL:
- متن از راست شروع می‌شه
- layout از راست به چپ جاریه
- اولین DOM child = rightmost visually

---

## HTML Setup

```html
<!-- RTL-only -->
<html dir="rtl" lang="fa">

<!-- LTR-only -->
<html dir="ltr" lang="en">
```

این کافیه برای cascade — همه child elements ارث می‌برن.

---

## LanguageConfig Type

```ts
type LanguageConfig = {
  locale: string        // 'fa' | 'ar' | 'en' | 'de' | ...
  dir: 'rtl' | 'ltr'
  font: string          // 'Vazirmatn' | 'Inter' | ...
}
```

### Locale Presets

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

## Dynamic Direction Switch (runtime)

```ts
// وقتی locale در runtime عوض می‌شه
document.documentElement.setAttribute('dir', config.dir)
document.documentElement.setAttribute('lang', config.locale)
document.documentElement.style.fontFamily = config.font
```

---

## Logical CSS Properties

به جای physical (left/right) از logical استفاده کن — خودشون با direction adapt می‌شن:

| Physical ❌ | Logical ✅ |
|------------|-----------|
| `margin-left` | `margin-inline-start` |
| `margin-right` | `margin-inline-end` |
| `padding-left` | `padding-inline-start` |
| `padding-right` | `padding-inline-end` |
| `border-left` | `border-inline-start` |
| `border-right` | `border-inline-end` |
| `left: 0` | `inset-inline-start: 0` |
| `right: 0` | `inset-inline-end: 0` |
| `text-align: left` | `text-align: start` |
| `text-align: right` | `text-align: end` |

---

## DOM Order = Visual Order

در RTL، flex container عناصر رو از راست می‌چینه:
```
DOM: [A] [B] [C]
RTL visual: C | B | A
```

**قانون:** همیشه DOM order رو طوری بنویس که اولین element باید rightmost باشه.

---

## Chevron / Arrow Icons

در RTL، جهت "جلو" برعکسه:
```
LTR: → (ChevronRight = next)
RTL: ← (ChevronLeft = next)
```

```tsx
const NextIcon = ({ dir }) =>
  dir === 'rtl' ? <ChevronLeftIcon /> : <ChevronRightIcon />
```

---

## Mixed Content — اعداد و کدهای انگلیسی

اعداد و کدها همیشه LTR هستن حتی در RTL context:

```html
<p dir="rtl">
  کد پیگیری:
  <span dir="ltr" style="unicode-bidi: embed">TRK-2024-001</span>
</p>
```

---

## Portal Components

کامپوننت‌هایی که outside React tree رندر می‌شن (Menu, Modal, Drawer, Tooltip):
- از `<html dir="rtl">` ارث می‌برن از طریق CSS cascade ✅
- ولی DOM order داخلشون رو باید خودت کنترل کنی
- همیشه `dir="rtl"` رو explicit روی Positioner/Portal container اضافه کن

---

## ⚠️ دام رایج — text-align در RTL

```css
/* در RTL: */
text-align: start  →  راست‌چین  ✅ (start = inline start = راست در RTL)
text-align: end    →  چپ‌چین   ⚠️ (end = inline end = چپ در RTL)
```

**قانون عملی:** برای راست‌چین از `text-align: right` یا `text-align: start` استفاده کن — از `text-align: end` در RTL-only پرهیز کن.

---

## پیاده‌سازی در هر DS

- Chakra UI v3 → `design-systems/chakra-ui-v3/chakra-ui-v3.md` (بخش Direction Setup)
- Bootstrap 5 → `design-systems/bootstrap5/rtl.md`
