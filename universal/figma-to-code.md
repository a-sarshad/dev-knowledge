# Figma → Code Workflow
> updated: 2026-05-16

---

## فازهای کار

**فاز ۱ — Context**
قبل از شروع، اینا رو داشته باش:
- CLAUDE.md پروژه (stack، RTL rules، known bugs)
- component-inventory: لیست کامپوننت‌های local فیگما
- page-templates: ساختار layout‌ها

**فاز ۲ — Figma بخون**
از Figma MCP یا screenshot برای فهمیدن layout و tokens استفاده کن:
- کدوم template (1-col، 2-col، 3-col)؟
- spacing‌ها (padding، gap) چند px؟
- colors از کدوم token؟
- typography از کدوم text style؟

**فاز ۳ — کد بنویس**
- Figma local components → React components با Chakra UI
- Figma tokens → DS tokens (نه مقادیر hardcode)
- Figma auto layout → Flex/Grid

---

## نگاشت Figma Auto Layout به CSS

| Figma | CSS/Chakra |
|-------|------------|
| Horizontal auto layout | `Flex` (direction row) |
| Vertical auto layout | `Flex` direction="column" یا `VStack` |
| Fill container | `flex={1}` یا `w="100%"` |
| Hug contents | بدون width ثابت |
| Fixed width | `w="256px"` |
| gap | `gap={n}` |
| padding | `p={n}` یا `px/py` |

---

## نگاشت Figma Layers به React

```
Page
├── Navbar          → <Navbar /> (sticky, full-width)
└── Body (h-flex)
    ├── Sidebar     → <Sidebar /> (fixed width)
    └── Main (v-flex)
        ├── Page-Header → <PageHeader />
        └── Content (h-flex)
            ├── Start   → <Box> (راست در RTL)
            ├── Middle  → <Box flex={1}>
            └── End     → <Box> (چپ در RTL)
```

---

## RTL DOM Order

در RTL، اولین child = rightmost:
```tsx
// Figma: [Start(راست)] [Middle] [End(چپ)]
// DOM:
<Flex>
  <Box>Start</Box>    {/* راست */}
  <Box flex={1}>Middle</Box>
  <Box>End</Box>      {/* چپ */}
</Flex>
```

---

## Tokens — Figma به Chakra

| Figma | Chakra prop |
|-------|-------------|
| spacing/4 (16px) | `p={4}` / `gap={4}` |
| spacing/6 (24px) | `p={6}` |
| spacing/10 (40px) | `gap={10}` |
| color/fg | `color="fg"` |
| color/fg.muted | `color="fg.muted"` |
| color/border | `borderColor="border"` |
| shadow/sm | `shadow="sm"` |
| radius/md (6px) | `borderRadius="md"` |

---

## چک‌لیست قبل از تحویل

- [ ] هیچ مقدار رنگ hardcode نشده
- [ ] spacing از token استفاده شده
- [ ] RTL DOM order رعایت شده
- [ ] logical CSS props استفاده شده (`ms/me/ps/pe`)
- [ ] Portal components `dir="rtl"` دارن
- [ ] responsive mobile بررسی شده
