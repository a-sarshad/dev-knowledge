# dev-knowledge

دانش مشترک بین همه پروژه‌ها — bugs، patterns، workflow ها

---

## ساختار

```
dev-knowledge/
├── skills/                               ← فایل‌های automation (نصب در Cowork)
│   ├── code-review-tokenization.skill    ← بررسی کد از نظر استفاده صحیح از design tokens
│   ├── commit-dev-knowledge.skill        ← commit message آماده بعد از هر تغییر DN
│   ├── project-init-wizard.skill         ← ساخت پروژه جدید با scaffold کامل
│   ├── session-start.skill               ← briefing وضعیت پروژه در شروع session
│   └── session-update.skill              ← ذخیره وضعیت و آپدیت HANDOFF.md
│
├── universal/                            ← مفاهیم مستقل از stack
│   ├── language.md                       ← RTL/LTR، locale، font، logical CSS
│   ├── git-troubleshoot.md               ← خطاهای رایج git + راه‌حل
│   ├── figma-to-code.md                  ← پیاده‌سازی دیزاین از Figma
│   ├── project-init-wizard.md            ← wizard تعاملی ساخت پروژه
│   ├── readme-template.md                ← template برای README پروژه‌های جدید
│   └── session-management.md             ← مدیریت context و HANDOFF.md
│
├── design-systems/
│   ├── chakra-ui-v3/
│   │   ├── chakra-ui-v3.md               ← مرجع اصلی: RTL، tokens، multilang
│   │   ├── tokens.md                     ← جداول کامل همه token‌ها
│   │   ├── known-bugs.md                 ← باگ‌های تأییدشده + راه‌حل
│   │   ├── setup-checklist.md            ← نصب و setup قدم‌به‌قدم
│   │   └── CLAUDE-template.md            ← template برای CLAUDE.md پروژه‌های Chakra
│   └── bootstrap5/
│       └── rtl.md                        ← RTL setup، logical classes، known fixes
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

**پروژه جدید:**
1. skill `project-init-wizard` رو در Cowork اجرا کن
2. wizard سوال‌به‌سوال پیش می‌ره و پروژه رو scaffold می‌کنه

**شروع session:**
1. skill `session-start` رو اجرا کن → briefing وضعیت فعلی
2. کار کن
3. هر موقع خواستی وضعیت ذخیره کنه: skill `session-update`

**حین کار:**
- bug جدید → `known-bugs.md` آپدیت می‌شه
- خطای git → `universal/git-troubleshoot.md` رو چک کن
- تغییر در DN → skill `commit-dev-knowledge` commit message آماده می‌کنه

---

## پروژه‌ها

| پروژه | Design System | زبان |
|-------|--------------|------|
| Vitrina | Chakra UI v3 | فارسی (RTL) |
| Airport | Bootstrap 5 | فارسی + انگلیسی (RTL/LTR) |
