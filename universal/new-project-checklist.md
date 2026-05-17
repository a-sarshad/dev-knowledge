# New Project Checklist
> universal — React + Vite + TypeScript + RTL/فارسی
> updated: 2026-05-17

---

## ۱. Setup اولیه

```bash
pnpm create vite@latest project-name -- --template react-ts
cd project-name
pnpm install
```

---

## ۲. Packages — از کاربر بپرس

قبل از نصب، با کاربر confirm کن که پروژه به چه چیزی نیاز داره:

**Routing** (اگه چند صفحه داره)
- `react-router-dom` — رایج‌ترین گزینه

**Data fetching** (اگه API call داره)
- `@tanstack/react-query` + `axios` — برای REST
- `@tanstack/react-query` + `graphql-request` — برای GraphQL
- `fetch` native — اگه ساده‌ست

**Icons** (اگه آیکون لازمه)
- `lucide-react` — سبک و tree-shakeable
- یا هر icon library که پروژه تعریف کرده

**UI Library**
→ فایل مخصوص DS انتخابی در `design-systems/` رو ببین

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

## ۵. File Structure — از کاربر بپرس

قبل از ساختن فایل‌ها، از کاربر بپرس layout کلی سایت چیه:

> «پروژه‌ات چه قسمت‌هایی داره؟
> مثلاً: navbar بالا دارش؟ sidebar کناری داره؟ footer داره؟
> یا یه ساختار متفاوت داره؟»

بر اساس جواب، ساختار رو modular بساز:

```
src/
  components/
    layout/         ← فقط اگه layout پیچیده داره
      Layout.tsx    ← همیشه
      Navbar.tsx    ← اگه navbar داره
      Sidebar.tsx   ← اگه sidebar داره
      Footer.tsx    ← اگه footer داره
      Header.tsx    ← اگه page-level header داره
  pages/
  services/         ← اگه API call داره
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
