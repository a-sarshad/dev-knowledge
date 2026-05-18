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

---

## ⚠️ دام رایج — Bootstrap modal close button در RTL

Bootstrap 5 روی `.modal-header .btn-close` این رو hardcode می‌کنه:
```css
margin-left: auto;   /* دکمه × رو همیشه راست نگه می‌داره */
margin-right: calc(-.5 * var(--bs-modal-header-padding-x));
```

در RTL flex، `margin-left: auto` باز هم دکمه رو سمت **راست** نگه می‌داره — چون فضای خالی سمت چپه.
نتیجه: دکمه × کنار title می‌مونه، نه گوشه چپ modal.

**فیکس:**
```css
:root[dir="rtl"] .modal-header .btn-close {
  margin-left: calc(-.5 * var(--bs-modal-header-padding-x));
  margin-right: auto;
}
```

این رو در `BOOTSTRAP OVERRIDES` یا بخش RTL overrides پروژه اضافه کن.

> **ریشه مشکل:** Bootstrap LTR-only CSS — نسخه RTL رسمی Bootstrap (`bootstrap.rtl.min.css`) این رو درست می‌کنه ولی اگه از نسخه معمولی استفاده می‌کنی باید override دستی بزنی.

---

## ⚠️ دام رایج — text-align در RTL

جدول logical props بالا نشون میده `text-align: right` → `text-align: end`.
این از نظر تئوری درسته، ولی در پروژه‌های RTL-only می‌تونه گیج‌کننده باشه:

```css
/* در RTL: */
text-align: start  →  راست‌چین  ✅ (start = inline start = راست در RTL)
text-align: end    →  چپ‌چین   ⚠️ (end = inline end = چپ در RTL)
```

**قانون عملی برای پروژه فارسی/عربی:**
- برای راست‌چین: از `text-align: right` یا `text-align: start` استفاده کن
- از `text-align: end` در RTL-only projects پرهیز کن — چپ‌چین می‌شه
