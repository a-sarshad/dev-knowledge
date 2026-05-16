# RTL — مفاهیم پایه
> universal — برای همه design system ها یکسانه
> updated: 2026-05-16

---

## چرا RTL؟

زبان‌های راست‌به‌چپ: فارسی، عربی، عبری، اردو
در RTL:
- متن از راست شروع می‌شه
- layout از راست به چپ جاریه
- اولین DOM child = rightmost visually

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
| `border-bottom-right-radius` | `border-end-start-radius` |
| `border-bottom-left-radius` | `border-end-end-radius` |
| `text-align: left` | `text-align: start` |
| `text-align: right` | `text-align: end` |

---

## HTML Setup

```html
<!-- index.html -->
<html dir="rtl" lang="fa">
```

این کافیه برای cascade — همه child elements ارث می‌برن.

---

## DOM Order = Visual Order

در RTL، flex container عناصر رو از راست می‌چینه:
```
DOM: [A] [B] [C]
RTL visual: C | B | A
```

**قانون:** همیشه DOM order رو طوری بنویس که اولین element باید rightmost باشه.

---

## Unicode Bidi

اعداد و کدهای انگلیسی داخل متن فارسی:
```html
<!-- اعداد LTR هستن حتی در RTL context -->
<p dir="rtl">
  کد پیگیری:
  <span dir="ltr" style="unicode-bidi: embed">TRK-2024-001</span>
</p>
```

---

## Chevron / Arrow Icons

در RTL، جهت "جلو" برعکسه:
```
LTR: → (ChevronRight = next)
RTL: ← (ChevronLeft = next)
```

---

## Portal Components

کامپوننت‌هایی که outside React tree رندر می‌شن (Menu, Modal, Drawer, Tooltip):
- از `<html dir="rtl">` ارث می‌برن از طریق CSS cascade ✅
- ولی DOM order داخلشون رو باید خودت کنترل کنی
- همیشه `dir="rtl"` رو explicit روی Positioner/Portal container اضافه کن
