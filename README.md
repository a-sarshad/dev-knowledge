# dev-knowledge

دانش مشترک بین همه پروژه‌ها — bugs، patterns، workflow ها

---

## ساختار

```
dev-knowledge/
├── universal/                        ← مفاهیم مستقل از stack
│   ├── rtl-concepts.md               ← RTL، logical CSS، Bootstrap RTL fixes
│   ├── multilang-concepts.md         ← locale، direction، font — concepts
│   ├── git-troubleshoot.md           ← خطاهای رایج git + راه‌حل
│   ├── figma-to-code.md              ← workflow پیاده‌سازی از فیگما
│   ├── new-project-checklist.md      ← شروع پروژه جدید (generic)
│   ├── project-init-wizard.md        ← wizard تعاملی برای scaffold کردن پروژه
│   └── session-management.md         ← مدیریت context و HANDOFF.md
│
├── design-systems/
│   └── chakra-ui-v3/
│       ├── chakra-ui-v3.md           ← مرجع اصلی: RTL، tokens، multilang، patterns
│       ├── tokens.md                 ← جداول کامل همه token‌ها (lookup)
│       ├── known-bugs.md             ← باگ‌های تأییدشده + راه‌حل با مثال کد
│       ├── setup-checklist.md        ← نصب و setup قدم‌به‌قدم
│       └── CLAUDE-template.md        ← template برای CLAUDE.md پروژه‌های Chakra
│
└── projects/
    ├── airport/
    │   └── project-context.md        ← brand tokens، layout، RTL setup
    └── vitrina/
        └── project-context.md        ← brand tokens، breakpoints، feature flags
```

---

## نحوه استفاده

**پروژه جدید:**
1. `universal/new-project-checklist.md` رو دنبال کن
2. اگه Chakra UI → `design-systems/chakra-ui-v3/CLAUDE-template.md` رو کپی کن
3. فقط بخش‌های `[CUSTOMIZE]` رو عوض کن

**حین کار:**
- bug جدید → `known-bugs.md` آپدیت می‌شه
- خطای git → `universal/git-troubleshoot.md` رو چک کن
- pattern جدید → فایل مربوطه آپدیت می‌شه

---

## پروژه‌ها

| پروژه | Design System | زبان |
|-------|--------------|------|
| Vitrina | Chakra UI v3 | فارسی (RTL) |
| Airport | Bootstrap 5 | فارسی + انگلیسی (RTL/LTR) |
