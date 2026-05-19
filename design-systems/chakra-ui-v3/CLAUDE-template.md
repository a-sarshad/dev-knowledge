# [PROJECT NAME] — Claude Reference
# کپی این فایل برای پروژه جدید، موارد [CUSTOMIZE] رو عوض کن
# updated: 2026-05-18

---

## Stack
<!-- [CUSTOMIZE] -->
- React 19 + Vite + TypeScript
- Chakra UI v3
- RTL / Persian (Vazirmatn font)
- pnpm

---

## Critical Rules

### RTL
- `dir="rtl"` on `<html>` in `index.html`
- `LocaleProvider locale="fa-IR"` wraps app in `main.tsx`
- RTL flex: **first DOM child = rightmost visually**
- Use logical CSS props: `ms/me/ps/pe` not `ml/mr/pl/pr`
- Portal components: add `dir="rtl"` to Positioner explicitly

### Chakra v3 Known Issues
→ see `dev-knowledge/design-systems/chakra-ui-v3/known-bugs.md`

Key issues:
- `lineHeight="8"` → BROKEN. Use ratio strings: `"1.333"` or `"1.14"`
- `bg="bg.default"` → BROKEN. Use `bg="white"` or `bg="bg"`
- `useColorMode` → DOES NOT EXIST in v3. Use custom ColorModeContext
- Dark mode: toggle `.dark` class on `document.documentElement`
- Avatar.Root + asChild → wrap in `<Box as="button">` first

### Project-Specific Bugs
<!-- باگ‌هایی که در این پروژه کشف شدن — نه DS-level -->
<!-- فرمت: symptom → fix (یه خط) — اگه باگی نیست این section رو خالی بذار -->
<!-- session-update این بخش رو بعد از کشف باگ جدید آپدیت می‌کنه -->

### Layout
<!-- [CUSTOMIZE] مشخصات layout این پروژه -->
- Navbar: full-width sticky header
- Body: `maxW="1920px" mx="auto"`
- Sidebar: `w="256px"` expanded

---

## Brand Tokens
<!-- [CUSTOMIZE] رنگ اصلی این پروژه — از project-context.md بیار -->
| Token | Light | Dark |
|-------|-------|------|
| `brand.solid` | teal.600 | teal.400 |
| `brand.fg` | teal.700 | teal.300 |
| `brand.muted` | teal.100 | teal.900 |

→ کامل: `src/theme/tokens.ts`
→ جدول همه variant‌ها: `dev-knowledge/design-systems/chakra-ui-v3/tokens.md`

---

## File Structure
<!-- [CUSTOMIZE] ساختار این پروژه رو اینجا بنویس -->
```
src/
  components/layout/
  contexts/
    ColorModeContext.tsx
  pages/
  theme/
    index.ts
    tokens.ts
  types/
  services/
```

---

## Knowledge References
- Chakra full reference: `dev-knowledge/design-systems/chakra-ui-v3/chakra-ui-v3.md`
- All tokens: `dev-knowledge/design-systems/chakra-ui-v3/tokens.md`
- Known bugs: `dev-knowledge/design-systems/chakra-ui-v3/known-bugs.md`
- RTL concepts: `dev-knowledge/universal/rtl-concepts.md`
- Figma→Code: `dev-knowledge/universal/figma-to-code.md`
