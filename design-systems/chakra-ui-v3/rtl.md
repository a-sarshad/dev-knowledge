# Chakra UI v3 — RTL Implementation
> updated: 2026-05-16

---

## Setup

```html
<!-- index.html -->
<html dir="rtl" lang="fa">
```

```ts
// theme/index.ts
const layoutConfig = defineConfig({
  globalCss: {
    'html, body': {
      direction: 'rtl',
      fontFamily: 'body',
    },
  },
})
```

```tsx
// main.tsx
<LocaleProvider locale="fa-IR">
  <App />
</LocaleProvider>
```

---

## Logical Props در Chakra

Chakra v3 از logical props پشتیبانی می‌کنه:

```tsx
// ✅ درست
<Box ms={4}>    // margin-inline-start
<Box me={4}>    // margin-inline-end
<Box ps={4}>    // padding-inline-start
<Box pe={4}>    // padding-inline-end
<Box borderStartWidth="1px">   // border-inline-start
<Box borderEndWidth="1px">     // border-inline-end
<Box insetInlineEnd={0}>       // right در LTR، left در RTL
```

---

## Flex DOM Order

```tsx
// RTL: اولین child = rightmost visual
// Navbar: [Hamburger/Logo] [فضا] [Min/Max] [Bell] [Avatar]
// DOM order ← ترتیب واقعی کد

<Flex>
  {/* راست‌ترین المان اول */}
  <HamburgerOrLogo />
  <Spacer />
  {/* چپ‌ترین المان آخر */}
  <MinMax />
  <Bell />
  <Avatar />
</Flex>
```

---

## Layout Body

```tsx
// RTL: اولین child = rightmost
// Sidebar سمت راست → باید FIRST در DOM باشه
<Flex maxW="1920px" mx="auto" alignItems="flex-start">
  <Sidebar w="256px" flexShrink={0} />  {/* FIRST → rightmost در RTL ✅ */}
  <Box flex={1} minW={0}>              {/* SECOND → left در RTL ✅ */}
    <PageHeader />
    <Content />
  </Box>
</Flex>

// ⚠️ counterintuitive — column flex:
// align="flex-start" = RIGHT side در RTL
// align="flex-end"   = LEFT  side در RTL
```

---

## Portal Components

```tsx
// همیشه dir="rtl" به Positioner اضافه کن
<Menu.Positioner dir="rtl">
<Drawer.Positioner dir="rtl">
<Popover.Positioner dir="rtl">

// Drawer از راست باز بشه
<Drawer.Root placement="start">  // "start" = راست در RTL ✅
```

---

## Sidebar Collapse

```tsx
// Desktop
<Box w="256px">  // expanded
<Box w="16">     // collapsed (64px)

// Mobile — Drawer
<Drawer.Root placement="start">
  <Drawer.Positioner dir="rtl" maxW="256px">
```

---

## Compact/Mobile Mode

```tsx
// اگه compact mode داری
<Box maxW={isCompact ? "512px" : "1920px"} mx="auto">
  {/* sidebar در compact مخفیه */}
  <Box display={isCompact ? "none" : "block"}>
    <Sidebar />
  </Box>
</Box>
```
