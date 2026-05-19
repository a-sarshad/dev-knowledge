# README Template — برای پروژه‌های جدید
> universal
> updated: 2026-05-19

هنگام scaffold، این فایل رو با مقادیر واقعی از جواب‌های wizard پر کن.
هر فیلد به سوال مشخصی map شده — جایگزینی مستقیم، بدون حدس.

---

## نقشه wizard → README

| فیلد README | منبع |
|-------------|------|
| نام پروژه | Q1 |
| توضیح | Q3 |
| Framework | Q4 |
| Language | Q5 |
| Design System | Q7 |
| Package Manager | Q6 |
| State Management | Q21 |
| Data Fetching | Q22 |
| زبان / جهت | Q17 |
| ساختار src/i18n | فقط اگه Q17 = دوزبانه |
| ساختار src/services | فقط اگه Q19 = بله |
| ساختار src/types | فقط اگه Q20 = بله |
| دستور نصب | Q6 (pnpm/npm/yarn install) |

---

## خروجی نهایی — README.md پروژه

```markdown
# {Q1: نام پروژه}

{Q3: توضیح یک‌جمله‌ای پروژه}

## Stack

| بخش | تکنولوژی |
|-----|----------|
| Framework | {Q4} |
| Language | {Q5} |
| Design System | {Q7} |
| State | {Q21} |
| Data Fetching | {Q22} |
| Package Manager | {Q6} |
| زبان | {Q17: فارسی RTL / انگلیسی LTR / فارسی + انگلیسی} |

## شروع سریع

\`\`\`bash
{Q6: pnpm / npm / yarn} install
{Q6: pnpm / npm / yarn} dev
\`\`\`

## ساختار پروژه

\`\`\`
src/
├── theme/              ← design tokens و system config
│   ├── tokens.ts       ← single source of truth (رنگ، فونت، spacing)
│   └── index.ts        ← {Q7} system setup
├── providers/          ← app-level providers
├── components/         ← reusable UI components
├── pages/              ← صفحات / routes
[فقط اگه Q19 = بله:]
├── services/           ← API calls و data layer
[فقط اگه Q20 = بله:]
├── types/              ← TypeScript interfaces (api.ts, ...)
[فقط اگه Q21 = Zustand:]
├── stores/             ← Zustand stores
[فقط اگه Q17 = دوزبانه:]
└── i18n/               ← multilang setup (fa / en)
\`\`\`

[فقط اگه Q19 = بله — بخش Backend:]
## برای توسعه‌دهنده Backend

- API base URL: `TBD`
- Auth: `TBD`
- Types: `src/types/api.ts`
- Environment: `.env.example` را ببینید

## مستندات بیشتر

- `CLAUDE.md` — راهنمای کار با Claude در این پروژه
- `HANDOFF.md` — وضعیت فعلی و قدم بعدی
- `dev-knowledge/projects/{Q1}/project-context.md` — tokens و معماری
```
