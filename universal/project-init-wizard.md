# Project Init Wizard
> universal — مستقل از design system
> updated: 2026-05-19

این فایل checklist تعاملی برای ایجاد پروژه جدید است.
Claude این سوال‌ها را مرحله‌به‌مرحله می‌پرسد و بر اساس جواب‌ها پروژه را **واقعاً** می‌سازد —
یعنی shell commands اجرا می‌کند، dependencies نصب می‌کند، و git init می‌زند.

---

## قوانین اجرا

- حداکثر ۳ سوال در هر مرحله — نه همه یکجا
- بعد از هر مرحله، جواب‌ها را به صورت جدول confirm کن
- سوال‌های فنی همیشه گزینه **«پیشنهاد Claude»** دارند
- اگه کاربر گفت «بعداً» یا «نمیدونم» → مقدار را `TBD` بذار و ادامه بده

---

## فاز ۰ — مسیر پروژه

```
Q0. پروژه کجا ساخته بشه؟
    مثال: ~/Documents/GitHub/Projects/
    (پیشنهاد Claude: ~/Documents/GitHub/Projects/)
```

این مسیر رو به عنوان `PARENT_PATH` نگه دار. پروژه در `PARENT_PATH/<project-name>/` ساخته می‌شه.

---

## فاز ۱ — هویت پروژه

```
Q1. نام پروژه چیه؟ (برای folder name و package.json)
Q2. نوع پروژه؟
    - Admin Panel / Dashboard
    - Landing Page / Marketing
    - E-commerce
    - Internal Tool
    - Other: ___
Q3. یه جمله کوتاه — این پروژه برای چیه؟
```

---

## فاز ۲ — Stack فنی

```
Q4. Framework؟
    - Next.js 14+ App Router  ← پیشنهاد Claude برای پروژه‌های بزرگ
    - React + Vite            ← پیشنهاد Claude برای پروژه‌های سبک
    - Vue 3 / Nuxt
    - Other: ___

Q5. زبان برنامه‌نویسی؟
    - TypeScript  ← پیشنهاد Claude
    - JavaScript

Q6. Package Manager؟
    - pnpm  ← پیشنهاد Claude
    - npm
    - yarn
```

---

## فاز ۳ — Design System

```
Q7. Design System؟
    - Chakra UI v3   ← پیشنهاد Claude اگه RTL/فارسی لازمه
    - Tailwind CSS   ← پیشنهاد Claude اگه LTR/انگلیسی
    - MUI / shadcn
    - Custom (بدون DS)

Q8. Figma file داری؟
    - بله → لینک: ___
    - خیر
    - بعداً می‌فرستم
```

---

## فاز ۴ — Brand Tokens ⭐

```
Q9.  رنگ Primary برند؟ (hex)
     یا بگو «از Figma می‌گیرم» یا «نمیدونم → پیشنهاد Claude»

Q10. رنگ Secondary / Accent؟
     (اگه نداری بگو «ندارم»)

Q11. رنگ‌های Semantic خاصی داری؟
     Error / Success / Warning
     (اگه نه → Chakra defaults استفاده می‌شه)

Q12. Dark Mode لازمه؟
     - بله
     - خیر  ← پیشنهاد Claude برای شروع

Q13. فونت اصلی؟
     - Vazirmatn  ← پیشنهاد برای RTL/فارسی
     - Inter      ← پیشنهاد برای LTR/انگلیسی
     - هر دو (دوزبانه)
     - Custom: ___
```

---

## فاز ۵ — Layout و Responsive

```
Q14. Target Platform؟
     - Desktop only
     - Mobile only
     - هر دو (Responsive)  ← پیشنهاد Claude

Q15. Breakpoints؟
     - Standard Chakra: sm/md/lg/xl/2xl
     - Vitrina-style: 480/1440/1920
     - Custom: ___

Q16. Layout اصلی؟
     - Sidebar ثابت + Main
     - Full Width
     - Centered (max-width)
     - Custom per page
```

---

## فاز ۶ — زبان و جهت

```
Q17. زبان‌های پروژه؟
     - فقط فارسی (RTL)
     - فقط انگلیسی (LTR)
     - دوزبانه فارسی + انگلیسی

Q18. Language Switcher در UI لازمه؟
     (فقط اگه دوزبانه انتخاب شد)
     - بله — runtime switching
     - خیر
```

---

## فاز ۷ — معماری کد [سوال‌های فنی]

> همه گزینه‌های این فاز «پیشنهاد Claude» دارند

```
Q19. API-ready باشه؟ (services/ جدا از UI)
     - بله — services/api/ جداگانه بساز
     - خیر — inline کافیه
     ← پیشنهاد Claude: بله (برای هر پروژه قابل رشد)

Q20. TypeScript types برای backend لازمه؟
     - بله — types/api.ts با interface ها
     - خیر
     ← پیشنهاد Claude: بله (اگه TypeScript انتخاب شده)

Q21. State Management؟
     - Zustand       ← پیشنهاد Claude
     - TanStack Query کافیه
     - Context API
     - Redux Toolkit

Q22. Data Fetching؟
     - TanStack Query (React Query)  ← پیشنهاد Claude
     - SWR
     - Axios + Fetch
```

---

## فاز ۸ — کیفیت کد [سوال‌های فنی]

```
Q23. ESLint + Prettier لازمه؟
     - بله  ← پیشنهاد Claude
     - خیر

Q24. Git Hooks با Husky لازمه؟
     - بله (lint-staged)  ← پیشنهاد Claude برای تیمی
     - خیر

Q25. Testing Setup لازمه؟
     - بله → Vitest  ← پیشنهاد Claude
     - خیر
```

---

## فاز ۹ — Git

```
Q26. Git Repository URL داری؟ (اختیاری)
Q27. Branch Strategy؟
     - main / develop  ← پیشنهاد Claude
     - main only
```

---

## فاز ۱۰ — Scaffold واقعی 🚀

بعد از جمع‌بندی همه جواب‌ها و تأیید کاربر، این مراحل را **به ترتیب** اجرا کن:

### گام ۱ — ساخت پروژه با CLI

```bash
cd <PARENT_PATH>

# React + Vite (اگه Q4 = React + Vite)
pnpm create vite <project-name> --template react-ts

# Next.js (اگه Q4 = Next.js)
pnpm dlx create-next-app@latest <project-name> --typescript --eslint --app --src-dir --import-alias "@/*" --no-tailwind

# Vue 3 (اگه Q4 = Vue 3)
pnpm create vite <project-name> --template vue-ts

# Nuxt (اگه Q4 = Nuxt)
pnpm dlx nuxi@latest init <project-name>
```

### گام ۲ — نصب dependencies

```bash
cd <PARENT_PATH>/<project-name>
pnpm install

# Chakra UI v3 (اگه Q7 = Chakra UI)
pnpm add @chakra-ui/react

# Tailwind (اگه Q7 = Tailwind)
pnpm add -D tailwindcss postcss autoprefixer && pnpm dlx tailwindcss init -p

# State Management
pnpm add zustand                          # اگه Q21 = Zustand
pnpm add @tanstack/react-query            # اگه Q22 = TanStack Query

# فونت فارسی
pnpm add @fontsource/vazirmatn            # اگه Q13 = Vazirmatn یا هر دو

# i18n (اگه Q17 = دوزبانه)
pnpm add i18next react-i18next
```

### گام ۳ — ساخت فایل‌های پروژه

این فایل‌ها را بساز (با محتوای واقعی بر اساس جواب‌های wizard):

**همیشه اجباری:**
```
src/theme/
  tokens.ts        ← brand colors، fonts، spacing
  index.ts         ← createSystem با projectTokens

src/providers/
  AppProviders.tsx ← ChakraProvider + سایر providers

CLAUDE.md          ← از design-systems/chakra-ui-v3/CLAUDE-template.md کپی و customize
HANDOFF.md         ← از universal/session-management.md الگو بگیر
README.md          ← از universal/readme-template.md با اطلاعات پروژه پر کن
```

**شرطی:**
```
src/i18n/LocaleContext.tsx    ← اگه Q17 = دوزبانه
src/services/api.ts           ← اگه Q19 = بله
src/types/api.ts              ← اگه Q20 = بله
src/contexts/ColorModeContext.tsx ← اگه Q12 = Dark Mode
```

### گام ۴ — dev-knowledge

```
projects/<project-name>/
  project-context.md  ← brand tokens، stack، layout این پروژه
```

### گام ۵ — Git

```bash
cd <PARENT_PATH>/<project-name>
git init
git add -A
git commit -m "feat: scaffold <project-name> project"

# اگه Q26 = URL داره:
git remote add origin <url>
git branch -M main
git push -u origin main
```

---

## چک‌لیست تحویل

- [ ] `pnpm dev` بدون خطا اجرا می‌شه
- [ ] همه رنگ‌ها از `tokens.ts` می‌آن — هیچ hex مستقیم در component نیست
- [ ] `CLAUDE.md` و `HANDOFF.md` ساخته شدن
- [ ] `README.md` با اطلاعات واقعی پر شده
- [ ] `project-context.md` در dev-knowledge ساخته شد
- [ ] git commit انجام شد

---

## Commit Message نهایی

```
feat: scaffold [project-name] project

- pnpm create vite / create-next-app: base project
- src/theme/tokens.ts: brand tokens (colors, fonts, spacing)
- src/theme/index.ts: Chakra UI theme system
- src/providers/AppProviders.tsx: provider setup
- src/services/api.ts: API service layer        (اگه API-ready)
- src/types/api.ts: backend TypeScript types    (اگه types لازمه)
- src/i18n/LocaleContext.tsx: multilang setup   (اگه دوزبانه)
- CLAUDE.md + HANDOFF.md + README.md
- dev-knowledge/projects/[name]/project-context.md
```

---

## مراجع

- Setup Chakra: `design-systems/chakra-ui-v3/setup-checklist.md`
- Tokens کامل: `design-systems/chakra-ui-v3/tokens.md`
- Known bugs: `design-systems/chakra-ui-v3/known-bugs.md`
- Language & RTL: `universal/language.md`
- Multilang + Direction Setup: `design-systems/chakra-ui-v3/chakra-ui-v3.md`
- Figma→Code: `universal/figma-to-code.md`
