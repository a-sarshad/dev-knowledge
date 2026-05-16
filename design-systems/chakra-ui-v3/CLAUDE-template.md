# [PROJECT NAME] — Claude Reference
# کپی این فایل برای پروژه جدید، موارد مشخص‌شده رو customize کن
# updated: 2026-05-16

---

## Stack
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

### Layout
<!-- [CUSTOMIZE] مشخصات layout پروژه -->
- Navbar: full-width sticky header
- Body: `maxW="1920px" mx="auto"`
- Sidebar: `w="256px"` expanded

---

## Brand Tokens
<!-- [CUSTOMIZE] رنگ اصلی پروژه -->
| Token | Light | Dark |
|-------|-------|------|
| `brand.solid` | teal.600 | teal.400 |
| `brand.fg` | teal.700 | teal.300 |
| `brand.muted` | teal.100 | teal.900 |

→ برای کامل: `src/theme/tokens.ts`

---

## Spacing Scale
```
0.5→2px | 1→4px | 2→8px | 3→12px | 4→16px | 5→20px
6→24px  | 8→32px | 10→40px | 12→48px | 16→64px
```

## Border Radius
```
sm(2px) | md(4px) | lg(6px) | xl(8px) | 2xl(12px) | full(9999px)
```

## Typography
```
xs(12) | sm(14) | md(16) | lg(18) | xl(20) | 2xl(24) | 3xl(30)
normal(400) | medium(500) | semibold(600) | bold(700)
```

---

## File Structure
```
src/
  components/layout/
    Layout.tsx
    Navbar.tsx
    Sidebar.tsx
    SidebarItem.tsx
    Header.tsx
    UserMenu.tsx
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
- RTL concepts: `dev-knowledge/universal/rtl-concepts.md`
- Chakra bugs: `dev-knowledge/design-systems/chakra-ui-v3/known-bugs.md`
- Tokens: `dev-knowledge/design-systems/chakra-ui-v3/tokens.md`
- Figma→Code: `dev-knowledge/universal/figma-to-code.md`
