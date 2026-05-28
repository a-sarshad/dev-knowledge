# Chakra UI v3 — Skill Reference
> جایگزین: rtl.md + multilang.md + unified-skill-content.md
> updated: 2026-05-18

این فایل محتوای کامل skill Chakra UI است — برای همه پروژه‌ها (RTL، LTR، دوزبانه).
رنگ brand هر پروژه از `projects/[name]/project-context.md` می‌آید — اینجا hardcode نیست.

---

## ۱. قوانین پایه — همیشه

1. **Token فقط** — هیچ hex یا مقدار hardcode — همیشه از token استفاده کن
2. **Logical Props** — `ms/me/ps/pe` نه `ml/mr/pl/pr`
3. **Direction** — هیچ‌وقت hardcode نکن مگر پروژه single-language باشد
4. **قبل از props ناشناخته** → `web_fetch https://chakra-ui.com/docs/components/[name]`
5. **هیچ‌وقت props را حدس نزن** — اشتباه در props = کد غیرقابل اجرا

---

## ۲. Brand Token Pattern

هر پروژه brand color خودش را دارد. در `src/theme/tokens.ts`:

```ts
// BRAND را با رنگ پروژه جایگزین کن (از project-context): teal, blue, violet, orange, ...
export const projectTokens = defineConfig({
  theme: {
    tokens: {
      fonts: {
        body:    { value: 'Vazirmatn, sans-serif' },  // یا Inter برای LTR
        heading: { value: 'Vazirmatn, sans-serif' },
      },
    },
    semanticTokens: {
      colors: {
        brand: {
          solid:      { value: { base: '{colors.BRAND.600}', _dark: '{colors.BRAND.600}' } },
          contrast:   { value: { base: 'white',              _dark: 'white'              } },
          fg:         { value: { base: '{colors.BRAND.700}', _dark: '{colors.BRAND.300}' } },
          subtle:     { value: { base: '{colors.BRAND.100}', _dark: '{colors.BRAND.900}' } },
          muted:      { value: { base: '{colors.BRAND.200}', _dark: '{colors.BRAND.800}' } },
          emphasized: { value: { base: '{colors.BRAND.300}', _dark: '{colors.BRAND.700}' } },
          focusRing:  { value: { base: '{colors.BRAND.500}', _dark: '{colors.BRAND.500}' } },
          border:     { value: { base: '{colors.BRAND.500}', _dark: '{colors.BRAND.400}' } },
          bg:         { value: { base: '{colors.BRAND.50}',  _dark: '{colors.BRAND.950}' } },
        },
      },
    },
  },
})
```

---

## ۳. Semantic Tokens — کلیدی‌ترین‌ها

| کاربرد | Token |
|--------|-------|
| Background | `bg` / `bg.subtle` / `bg.muted` |
| متن | `fg` / `fg.muted` / `fg.subtle` |
| Border | `border` / `border.muted` / `border.emphasized` |
| خطا | `bg.error` / `fg.error` / `border.error` |
| موفقیت | `bg.success` / `fg.success` |
| هشدار | `bg.warning` / `fg.warning` |

> جدول کامل همه توکن‌ها → `tokens.md`

---

## ۴. Typography

| موقعیت | Size | Weight |
|--------|------|--------|
| body / فرم | `sm` (14px) | `normal` (400) |
| label / caption | `xs` (12px) | `medium` (500) |
| عنوان بخش | `md` (16px) | `semibold` (600) |
| عنوان صفحه | `xl` (20px) | `semibold` (600) |
| عنوان بزرگ | `2xl` (24px) | `bold` (700) |

**⚠️ lineHeight Bug:**
```tsx
lineHeight="8"       // ❌ BROKEN — unitless = 192px
lineHeight="1.333"   // ✅ ratio string
lineHeight="normal"  // ✅ = 1.5
lineHeight="tight"   // ✅ = 1.25
```

---

## ۵. Spacing، Radius، Shadow، Component Size

```
Spacing:  2=8px | 3=12px | 4=16px | 6=24px | 10=40px
Radius:   badge/btn/input=md(4px) | card=lg(6px) | modal=xl(8px) | avatar=full
Shadow:   card=sm | modal=lg | dropdown=md | tooltip=xs
Size:     همیشه md (40px) مگر مشخص شود
```

---

## ۶. Direction Setup — سه حالت

### RTL-only (فارسی/عربی)

```html
<!-- index.html -->
<html dir="rtl" lang="fa">
```

```ts
// src/theme/index.ts
const layoutConfig = defineConfig({
  globalCss: { 'html, body': { direction: 'rtl', fontFamily: 'body' } },
})
export const system = createSystem(defaultConfig, projectTokens, layoutConfig)
```

```tsx
// main.tsx
<ChakraProvider value={system}>
  <LocaleProvider locale="fa-IR">
    <ColorModeProvider><App /></ColorModeProvider>
  </LocaleProvider>
</ChakraProvider>
```

### LTR-only (انگلیسی)

```ts
const layoutConfig = defineConfig({
  globalCss: { 'html, body': { direction: 'ltr', fontFamily: 'body' } },
})
// main.tsx → LocaleProvider locale="en-US"
```

### دوزبانه (runtime switching)

```tsx
// src/i18n/LocaleContext.tsx
const LANG_CONFIG = {
  fa: { locale: 'fa-IR', dir: 'rtl', font: 'Vazirmatn' },
  ar: { locale: 'ar',    dir: 'rtl', font: 'Vazirmatn' },
  en: { locale: 'en-US', dir: 'ltr', font: 'Inter'     },
}

export function MultiLangProvider({ defaultLang = 'fa', children }) {
  const [lang, setLang] = useState(defaultLang)
  const config = LANG_CONFIG[lang] ?? LANG_CONFIG['en']

  useEffect(() => {
    document.documentElement.setAttribute('dir',  config.dir)
    document.documentElement.setAttribute('lang', config.locale)
  }, [config])

  return (
    <LocaleCtx.Provider value={{ lang, config, setLang }}>
      <LocaleProvider locale={config.locale}>
        <DirectionProvider dir={config.dir}>
          <Box fontFamily={config.font}>{children}</Box>
        </DirectionProvider>
      </LocaleProvider>
    </LocaleCtx.Provider>
  )
}
```

```tsx
// main.tsx (دوزبانه)
<ChakraProvider value={system}>
  <MultiLangProvider defaultLang="fa">
    <ColorModeProvider><App /></ColorModeProvider>
  </MultiLangProvider>
</ChakraProvider>
```

---

## ۷. RTL — DOM Order و Layout

**قانون اصلی RTL:** اولین child در DOM = راست‌ترین المان بصری

```tsx
// Navbar RTL: [Logo/Hamburger] [Spacer] [Actions] [Bell] [Avatar]
<Flex>
  <HamburgerOrLogo />   {/* راست‌ترین → FIRST در DOM */}
  <Spacer />
  <MinMax />
  <Bell />
  <Avatar />            {/* چپ‌ترین → LAST در DOM */}
</Flex>

// Layout Body RTL: Sidebar سمت راست → FIRST در DOM
<Flex maxW="1920px" mx="auto" alignItems="flex-start">
  <Sidebar w="256px" flexShrink={0} />  {/* FIRST → rightmost در RTL */}
  <Box flex={1} minW={0}>              {/* SECOND → left در RTL */}
    <PageHeader />
    <Content />
  </Box>
</Flex>
```

⚠️ در RTL: `align="flex-start"` = سمت راست، `align="flex-end"` = سمت چپ

---

## ۷-الف. RTL DOM Order — الگوهای component-specific

> **چرا این مهمه؟** Chakra v3 + @ark-ui RTL را از طریق `LocaleProvider locale="fa-IR"` → `dir:"rtl"` به همه کامپوننت‌ها می‌رسونه. CSS `flex-direction:row` در RTL context به طور خودکار main axis رو برعکس می‌کنه. **اما DOM ORDER به دست ما نوشته می‌شه** — اشتباه در DOM order = اشتباه بصری.

### Button + icon

```tsx
// ✅ Start icon (leading) — FIRST در DOM = راست‌ترین در RTL
<Button>
  <Plus size={16} />   {/* راست (start) ✓ */}
  افزودن
</Button>

// ✅ End icon (trailing) — LAST در DOM = چپ‌ترین در RTL
<Button>
  مشاهده بیشتر
  <ChevronLeft size={16} />   {/* چپ (end) ✓ — در RTL forward = چپ */}
</Button>

// ❌ اشتباه: start icon بعد از text
<Button>
  افزودن
  <Plus size={16} />   {/* چپ = end position ✗ */}
</Button>
```

### Switch — standalone toggle

```tsx
// ✅ Switch FIRST = راست‌ترین (position: start)
<Flex align="center" gap="2.5">
  <Switch.Root colorPalette="brand" ...>
    <Switch.HiddenInput />
    <Switch.Control><Switch.Thumb /></Switch.Control>
  </Switch.Root>
  <Text fontSize="sm">ارسال رایگان</Text>   {/* LAST → چپ ✓ */}
</Flex>

// ❌ اشتباه: text قبل از switch
<Flex align="center" gap="2.5">
  <Text>ارسال رایگان</Text>   {/* FIRST → راست ✗ */}
  <Switch.Root ...>...</Switch.Root>
</Flex>
```

### Switch.Label داخل Switch.Root

```tsx
// ✅ Control FIRST = راست‌ترین (order:-1 در theme به عنوان CSS fallback هم داریم)
<Switch.Root>
  <Switch.HiddenInput />
  <Switch.Control><Switch.Thumb /></Switch.Control>   {/* FIRST → راست ✓ */}
  <Switch.Label>فعال / غیرفعال</Switch.Label>         {/* SECOND → چپ ✓ */}
</Switch.Root>

// ❌ اشتباه: label قبل از control
<Switch.Root>
  <Switch.Label>فعال</Switch.Label>   {/* FIRST → راست ✗ */}
  <Switch.Control>...</Switch.Control>
</Switch.Root>
```

> **CSS Defensive Fix** — در `globalCss` پروژه‌های RTL اضافه کن:
> ```ts
> '[data-scope="switch"][data-part="control"]': { order: -1 }
> ```
> این CSS حتی اگه DOM order اشتباه باشه، Control را بصری اول می‌آره (راست در RTL).

### Settings row (full-width)

```tsx
// ✅ Switch راست، Label چپ، فاصله‌دار
<Flex align="center" gap="2.5" w="full">
  <Switch.Root flexShrink={0}>...</Switch.Root>   {/* FIRST → راست ✓ */}
  <Text flex="1" fontSize="sm">عنوان تنظیم</Text> {/* LAST → چپ ✓ */}
</Flex>

// توجه: برعکس LTR settings (که label چپ و toggle راست)
// در RTL: toggle/control همیشه start = راست
```

### Flex row عمومی

```tsx
// ✅ ترتیب صحیح برای هر row component
<Flex align="center" gap="3">
  <Icon />          {/* FIRST = راست‌ترین ✓ */}
  <Text flex="1">متن اصلی</Text>
  <ActionBtn />     {/* LAST = چپ‌ترین ✓ */}
</Flex>
```

---

## ۸. Logical Props — قانون مطلق

```tsx
<Box ms={4} me={2} ps={3} pe={2}>   // ✅ logical — direction-aware
<Box ml={4} mr={2} pl={3} pr={2}>   // ❌ هیچ‌وقت physical

// Arrow/Chevron در RTL
dir === 'rtl' ? <ChevronLeftIcon /> : <ChevronRightIcon />
```

---

## ۹. Portal Components در RTL

```tsx
// همیشه dir="rtl" به Positioner اضافه کن
<Menu.Positioner dir="rtl">
<Drawer.Positioner dir="rtl" maxW="256px">
<Popover.Positioner dir="rtl">
<Tooltip.Positioner dir="rtl">

// Drawer از راست باز بشه
<Drawer.Root placement="start">   // "start" = راست در RTL ✅
```

---

## ۱۰. Sidebar Patterns

```tsx
// Desktop — expanded/collapsed
<Box w="256px">   // expanded
<Box w="16">      // collapsed (64px)

// Mobile — Drawer
<Drawer.Root placement="start">
  <Drawer.Positioner dir="rtl" maxW="256px">

// Compact Mode
<Box maxW={isCompact ? "512px" : "1920px"} mx="auto">
  <Box display={isCompact ? "none" : "block"}><Sidebar /></Box>
</Box>
```

---

## ۱۱. Known Bugs — خلاصه

```tsx
bg="bg.default"  // ❌ → ✅ bg="bg"
lineHeight="8"   // ❌ → ✅ lineHeight="1.333"
useColorMode()   // ❌ وجود ندارد → ✅ custom ColorModeContext
textAlign="end"  // ❌ در RTL = LEFT → ✅ textAlign="start"
// dark mode: document.documentElement.classList.toggle('dark')
// Avatar+asChild: ❌ <Avatar.Root asChild><button> → ✅ <Box as="button"><Avatar.Root>
```

> لیست کامل با مثال‌های کد → `known-bugs.md`

---

## ۱۲. Chakra Docs — web_fetch URLs

```
Button  → https://chakra-ui.com/docs/components/button
Input   → https://chakra-ui.com/docs/components/input
Select  → https://chakra-ui.com/docs/components/select
Table   → https://chakra-ui.com/docs/components/table
Dialog  → https://chakra-ui.com/docs/components/dialog
Drawer  → https://chakra-ui.com/docs/components/drawer
Badge   → https://chakra-ui.com/docs/components/badge
Tabs    → https://chakra-ui.com/docs/components/tabs
```

---

## مراجع این پوشه

| فایل | کاربرد |
|------|--------|
| `tokens.md` | جداول کامل همه توکن‌ها (lookup) |
| `known-bugs.md` | لیست زنده باگ‌ها با مثال کد |
| `setup-checklist.md` | نصب و setup قدم‌به‌قدم |
| `CLAUDE-template.md` | template برای CLAUDE.md پروژه‌های جدید |
