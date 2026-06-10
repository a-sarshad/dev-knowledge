# Responsive Map — نگاشت Figma به Breakpoint

> universal — مستقل از design system
> مرجع یکجا برای همه پروژه‌ها: Figma frame width → breakpoint key → DS syntax

---

## جدول نگاشت — هر پروژه

### Vitrina (Chakra UI v3)

| Figma frame width | نام breakpoint | Chakra key | استفاده |
|-------------------|---------------|------------|---------|
| ≤ 480px | mobile | `base` | پیش‌فرض (موبایل اول) |
| 481px – 1439px | tablet | `md` | تبلت (اگه طرح جداگانه دارد) |
| 1440px | desktop | `xl` | دسکتاپ استاندارد |
| 1920px | wide | `2xl` | مانیتور بزرگ |

```tsx
// مثال responsive prop در Vitrina:
<Box
  display={{ base: 'none', xl: 'flex' }}
  fontSize={{ base: 'sm', xl: 'md' }}
  px={{ base: 4, xl: 8 }}
/>
```

```tsx
// مثال responsive array (کوتاه‌تر):
// ترتیب: [base, md, xl, 2xl]
<Text fontSize={['sm', null, 'md', 'lg']} />
```

---

### Airport (Bootstrap 5)

| Figma frame width | Bootstrap breakpoint | class prefix | استفاده |
|-------------------|---------------------|--------------|---------|
| < 576px | xs (default) | بدون prefix | موبایل |
| ≥ 576px | sm | `sm:` / `col-sm-` | موبایل بزرگ |
| ≥ 768px | md | `md:` / `col-md-` | تبلت |
| ≥ 992px | lg | `lg:` / `col-lg-` | لپ‌تاپ |
| ≥ 1200px | xl | `xl:` / `col-xl-` | دسکتاپ |
| ≥ 1400px | xxl | `xxl:` / `col-xxl-` | مانیتور بزرگ |

```html
<!-- مثال responsive در Airport: -->
<div class="col-12 col-md-6 col-xl-4">
<div class="d-none d-md-flex">
<div class="fs-6 fs-md-5">
```

---

## چطور از Figma breakpoint بخوانیم

### روش ۱ — Figma frames رو در عرض‌های مختلف ببین

در Figma MCP، همان node رو برای هر frame size بخوان:
```
get_design_context(node_id)  → ببین کدوم frame size طرح داره
```

اگه فقط یه frame هست → responsive نیست، فقط desktop. موبایل رو باید pattern خودت implement کنی.

### روش ۲ — Auto Layout hints

| Figma Auto Layout | رفتار responsive |
|-------------------|-----------------|
| Fill container | `flex: 1` یا `width: 100%` → کامل responsive |
| Fixed width (px) | احتمالاً باید روی موبایل تغییر کنه |
| Hug contents | ممکنه overflow کنه — wrap بررسی کن |
| Min width / Max width | → `min-w` و `max-w` token استفاده کن |

### روش ۳ — Figma Variants اگه responsive داره

اگه component variants مثل `size=mobile` و `size=desktop` داشت:
- هر دو رو بخون
- مقایسه کن چی تغییر کرده (font size؟ padding؟ نمایش/پنهان؟)
- به responsive props ترجمه کن

---

## قوانین responsive کدنویسی

### Chakra UI v3

```tsx
// ✅ درست — responsive object
<Box display={{ base: 'none', xl: 'block' }} />

// ✅ درست — responsive array [base, sm, md, lg, xl, 2xl]
<Box px={[4, null, null, null, 8]} />

// ❌ اشتباه — media query inline
<Box style={{ '@media (min-width: 1440px)': { display: 'block' } }} />

// ❌ اشتباه — hardcode breakpoint
<Box display={window.innerWidth > 1440 ? 'block' : 'none'} />
```

### Bootstrap 5

```html
<!-- ✅ درست — responsive utility classes -->
<div class="d-none d-xl-block">
<div class="col-12 col-xl-8">

<!-- ❌ اشتباه — inline media query -->
<div style="@media (min-width: 1200px) { display: block }">

<!-- ❌ اشتباه — JS-based responsive -->
<div class={window.innerWidth > 1200 ? 'd-block' : 'd-none'}>
```

---

## الگوهای رایج responsive

### نمایش / پنهان کردن

| رفتار | Chakra v3 | Bootstrap 5 |
|-------|-----------|-------------|
| فقط موبایل | `display={{ base: 'block', xl: 'none' }}` | `d-xl-none` |
| فقط دسکتاپ | `display={{ base: 'none', xl: 'block' }}` | `d-none d-xl-block` |
| فقط تبلت | `display={{ base: 'none', md: 'block', xl: 'none' }}` | `d-none d-md-block d-xl-none` |

### Layout تغییر می‌کنه

```tsx
// Chakra — stack عمودی روی موبایل، افقی روی دسکتاپ:
<Stack direction={{ base: 'column', xl: 'row' }} gap={4} />

// Bootstrap — column روی موبایل، row روی دسکتاپ:
<div class="row">
  <div class="col-12 col-xl-8">main</div>
  <div class="col-12 col-xl-4">sidebar</div>
</div>
```

### Typography responsive

```tsx
// Chakra
<Text textStyle={{ base: 'sm', xl: 'md' }} />
<Heading size={{ base: 'lg', xl: '2xl' }} />

// Bootstrap
<p class="fs-6 fs-xl-5">
<h1 class="display-6 display-xl-4">
```

### Spacing responsive

```tsx
// Chakra
<Box p={{ base: 4, xl: 8 }} gap={{ base: 3, xl: 6 }} />

// Bootstrap
<div class="p-3 p-xl-5 gap-3 gap-xl-5">
```

---

## نکات RTL + Responsive

- در RTL، از `ms-*/me-*` Bootstrap بجای `ms-*/me-*` استفاده کن — اینا logical هستن
- در Chakra، همیشه `marginInlineStart` / `paddingInlineStart` — نه `marginLeft`
- وقتی layout تغییر می‌کنه (column ↔ row)، DOM order رو هم چک کن (اولین child = rightmost در RTL)

---

## پروژه جدید — breakpoints رو کجا ثبت کن

در `CLAUDE.md` پروژه، بخش Layout/Responsive، این جدول رو پر کن:

```markdown
| Figma frame | breakpoint | key |
|-------------|-----------|-----|
| 390px       | mobile    | base |
| 1440px      | desktop   | xl  |
```

→ projfix ماژول `responsive-props` از این mapping استفاده می‌کنه
