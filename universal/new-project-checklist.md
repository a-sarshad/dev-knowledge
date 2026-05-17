# New Project Checklist
> updated: 2026-05-17

چک‌لیست پروژه‌های React + Vite + TypeScript با RTL/فارسی.
برای setup کتابخانه UI، فایل مربوطه در `design-systems/` رو چک کن.

---

## ۱. Setup اولیه

```bash
pnpm create vite@latest project-name -- --template react-ts
cd project-name
pnpm install
```

---

## ۲. Packages (generic)

```bash
# Router
pnpm add react-router-dom

# Data fetching
pnpm add @tanstack/react-query axios

# Icons
pnpm add lucide-react
```

برای UI library جداگانه نصب کن:
- Chakra UI v3 → `design-systems/chakra-ui-v3/setup-checklist.md`

---

## ۳. Font — Vazirmatn (RTL/Persian)

در `index.html`:
```html
<link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@100;300;400;500;600;700;800;900&display=swap" rel="stylesheet">
```

---

## ۴. RTL Setup

در `index.html`:
```html
<html dir="rtl" lang="fa">
```

---

## ۵. File Structure (generic)

```
src/
  components/layout/
    Layout.tsx
    Navbar.tsx
    Sidebar.tsx
    Header.tsx
  pages/
  services/
  types/
```

---

## ۶. Git

```bash
git init
git add .
git commit -m "chore: initial setup"
gh repo create project-name --private
git push -u origin main
```
