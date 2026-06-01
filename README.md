# dev-knowledge

دانش مشترک بین همه پروژه‌ها — bugs، patterns، workflow ها

---

## ساختار

```
dev-knowledge/
├── skills/                               ← فایل‌های automation (نصب در Cowork)
│
├── universal/                            ← مفاهیم مستقل از stack
│   ├── language.md                       ← RTL/LTR، locale، font، logical CSS
│   ├── git-troubleshoot.md               ← خطاهای رایج git + راه‌حل
│   ├── figma-to-code.md                  ← پیاده‌سازی دیزاین از Figma
│   ├── project-init-wizard.md            ← wizard تعاملی ساخت پروژه (شامل README template)
│   ├── projfix.md                        ← راهنمای CLI projfix، aliases، modules، ignore
│   └── session-management.md             ← مدیریت context و HANDOFF.md
│
├── design-systems/
│   ├── chakra-ui-v3/
│   │   ├── chakra-ui-v3.md               ← مرجع اصلی: RTL، tokens، multilang
│   │   ├── tokens.md                     ← جداول کامل همه token‌ها
│   │   ├── known-bugs.md                 ← باگ‌های تأییدشده + راه‌حل
│   │   ├── components.md                 ← لیست کامل همه component‌ها (بدون tool call)
│   │   └── CLAUDE-template.md            ← template برای CLAUDE.md پروژه‌های Chakra
│   └── bootstrap5/
│       ├── rtl.md                        ← RTL setup، logical classes، known fixes
│       ├── scaffold.md                   ← راهنمای setup پروژه جدید + هشدارهای رایج
│       ├── _tokens.scss                  ← brand vars (per-project — فقط این عوض می‌شه)
│       ├── _overrides.scss               ← mapping tokens → Bootstrap SCSS vars
│       └── bootstrap.scss               ← entry point (copy به src/styles/ پروژه جدید)
│
└── projects/
    ├── airport/
    │   ├── project-context.md            ← brand tokens، layout، RTL setup
    │   └── known-bugs.md                 ← باگ‌های project-specific
    └── vitrina/
        ├── project-context.md            ← brand tokens، breakpoints، feature flags
        └── known-bugs.md                 ← باگ‌های project-specific
```

---

## نحوه استفاده

```
پروژه جدید  → skill dev-init-wizard
شروع session → skill wf-session-start
ذخیره وضعیت → skill wf-session-update
commit       → skill wf-commit-project
بررسی کد    → pf / pfc / pff از terminal (راهنما: universal/projfix.md)
Figma→کد    → skill figma-implement-design + gate در CLAUDE.md پروژه
```

> قوانین اجباری، skill table، و workflow کامل: **`CLAUDE.md`** همین پوشه.

---

## پروژه‌ها

| پروژه | Design System | زبان |
|-------|--------------|------|
| Vitrina | Chakra UI v3 | فارسی (RTL) |
| Airport | Bootstrap 5 | فارسی + انگلیسی (RTL/LTR) |
