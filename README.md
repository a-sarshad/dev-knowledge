# dev-knowledge

دانش مشترک بین همه پروژه‌ها — bugs، patterns، workflow ها

---

## ساختار

```
dev-knowledge/
├── universal/                        ← مفاهیم مستقل از stack
│   ├── language.md                   ← RTL/LTR، locale، font، logical CSS، DOM order
│   ├── git-troubleshoot.md           ← خطاهای رایج git + راه‌حل
│   ├── figma-to-code.md              ← پیاده‌سازی دیزاین از Figma (نه project init)
│   ├── project-init-wizard.md        ← شروع پروژه جدید — wizard تعاملی (skill)
│   └── session-management.md         ← مدیریت context و HANDOFF.md
│
├── design-systems/
│   ├── chakra-ui-v3/
│   │   ├── chakra-ui-v3.md           ← مرجع اصلی: RTL، tokens، multilang، patterns
│   │   ├── tokens.md                 ← جداول کامل همه token‌ها (lookup)
│   │   ├── known-bugs.md             ← باگ‌های تأییدشده + راه‌حل با مثال کد
│   │   ├── setup-checklist.md        ← نصب و setup قدم‌به‌قدم
│   │   └── CLAUDE-template.md        ← template برای CLAUDE.md پروژه‌های Chakra
│   └── bootstrap5/
│       └── rtl.md                    ← RTL setup، logical classes، known fixes
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
1. `universal/project-init-wizard.md` رو دنبال کن (wizard تعاملیه — سوال‌به‌سوال)
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
