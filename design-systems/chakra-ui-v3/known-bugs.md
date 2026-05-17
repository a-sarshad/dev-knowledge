# Chakra UI v3 — Known Bugs & Gotchas
> updated: 2026-05-16
> این فایل در حین کار با پروژه‌های واقعی update می‌شه

---

## 🔴 Bugs تأییدشده

### lineHeight numeric tokens — BROKEN
```tsx
// ❌ اشتباه — unitless CSS تولید می‌کنه (8 × font-size = 192px!)
<Text lineHeight="8">

// ✅ درست — از ratio string استفاده کن
<Text lineHeight="1.333">  {/* 32px at 2xl */}
<Text lineHeight="1.14">   {/* 32px at 3xl */}
```

### textAlign="end" در RTL — چپ‌چین میشه
```tsx
// ❌ در RTL، end = inline-end = LEFT
<Text textAlign="end">متن فارسی</Text>  // چپ‌چین!

// ✅ برای راست‌چین در RTL:
<Text textAlign="right">متن فارسی</Text>
// یا
<Text textAlign="start">متن فارسی</Text>  // start = راست در RTL
```
> **چرا؟** در RTL، inline-start = راست، inline-end = چپ.
> پس `end` عکس چیزیه که انتظار داری.

### bg="bg.default" — BROKEN
```tsx
// ❌ CSS var به transparent resolve می‌شه
<Box bg="bg.default">

// ✅
<Box bg="white">   // light mode
<Box bg="bg">      // semantic (safe)
```

### useColorMode — DOES NOT EXIST
```tsx
// ❌ از Chakra import نکن
import { useColorMode } from '@chakra-ui/react'

// ✅ از custom context استفاده کن
import { useColorMode } from '@/contexts/ColorModeContext'
```

### Avatar.Root / Complex Components — asChild ref issue
```tsx
// ❌ ref forward نمی‌کنه برای asChild
<Avatar.Root asChild>
  <button>

// ✅ با Box wrap کن
<Box as="button" type="button">
  <Avatar.Root>
```

---

## 🟡 Gotchas (اشتباه نیستن، ولی غیر‌منتظره‌ان)

### bg="bg.subtle" — کار می‌کنه
```tsx
// ✅ این token درسته
<Box bg="bg.subtle">  // #fafafa در light mode
```

### Tooltip namespace
```tsx
// ✅ روش درست Chakra v3
<Tooltip.Root>
  <Tooltip.Trigger asChild>
    <Button>hover me</Button>
  </Tooltip.Trigger>
  <Tooltip.Content>متن tooltip</Tooltip.Content>
</Tooltip.Root>
```

### Text و Flex — href قبول نمی‌کنن
```tsx
// ❌
<Text href="/path">

// ✅ با <a> wrap کن
<a href="/path"><Text>لینک</Text></a>
```

### Dark Mode — class روی html نه wrapper div
```tsx
// ✅ Portal content باید .dark class روی <html> ببینه
document.documentElement.classList.toggle('dark')

// ❌ روی wrapper div
<div className="dark"><App /></div>
```

---

## 🟢 Patterns کار‌کرده

### Portal + RTL
```tsx
// همیشه dir="rtl" به Positioner اضافه کن
<Menu.Positioner dir="rtl">
<Drawer.Positioner dir="rtl">
<Tooltip.Positioner dir="rtl">
```

### bg="white" در Chakra
```tsx
// ✅ این palette token هست، hardcode نیست
<Box bg="white">
// ❌ hardcode
<Box bg="#ffffff">
```

### Color Mode Storage
```tsx
// localStorage key پروژه Vitrina
localStorage.key = 'vitrina-color-mode'
// برای پروژه‌های دیگه، نام پروژه رو عوض کن
```
