# README Template — برای پروژه‌های جدید
> universal
> updated: 2026-05-19

این فایل template ای است که wizard هنگام scaffold از آن استفاده می‌کند.
متغیرهای `[...]` را با اطلاعات واقعی پروژه جایگزین کن.

---

## خروجی نهایی — README.md پروژه

```markdown
# [نام پروژه]

[توضیح یک‌جمله‌ای پروژه — از Q3 wizard]

## Stack

| بخش | تکنولوژی |
|-----|----------|
| Framework | [React + Vite / Next.js / ...] |
| Language | [TypeScript / JavaScript] |
| Design System | [Chakra UI v3 / Tailwind / ...] |
| State | [Zustand / TanStack Query / ...] |
| Package Manager | [pnpm / npm] |
| زبان | [فارسی RTL / انگلیسی LTR / دوزبانه] |

## شروع سریع

\`\`\`bash
pnpm install
pnpm dev
\`\`\`

## ساختار پروژه

\`\`\`
src/
├── theme/          ← brand tokens و design system config
│   ├── tokens.ts   ← single source of truth برای رنگ‌ها، فونت، spacing
│   └── index.ts    ← Chakra UI system
├── providers/      ← app-level providers
├── components/     ← reusable components
├── pages/          ← صفحات اصلی
├── services/       ← API calls (اگه API-ready)
├── types/          ← TypeScript interfaces
└── i18n/           ← multilang setup (اگه دوزبانه)
\`\`\`

## برای توسعه‌دهنده Backend

- API base URL: `TBD`
- Auth: `TBD`
- Types: `src/types/api.ts`
- محیط‌های موجود: `.env.example` را ببینید

## مستندات بیشتر

- `CLAUDE.md` — راهنمای کار با Claude در این پروژه
- `HANDOFF.md` — وضعیت فعلی و قدم بعدی
- `dev-knowledge/projects/[name]/project-context.md` — tokens و معماری
```
