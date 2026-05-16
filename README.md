# dev-knowledge

دانش مشترک بین همه پروژه‌ها — bugs، patterns، workflow ها

---

## ساختار

```
dev-knowledge/
├── universal/                      ← مفاهیم برای همه stack ها
│   ├── rtl-concepts.md             ← RTL، logical CSS، DOM order
│   ├── multilang-concepts.md       ← locale، direction، font
│   ├── figma-to-code.md            ← workflow پیاده‌سازی از فیگما
│   └── new-project-checklist.md    ← شروع پروژه جدید
│
└── design-systems/
    └── chakra-ui-v3/
        ├── known-bugs.md           ← باگ‌های تأییدشده + راه‌حل
        ├── rtl.md                  ← پیاده‌سازی RTL در Chakra
        ├── tokens.md               ← همه token ها
        ├── multilang.md            ← پیاده‌سازی دوزبانه
        └── CLAUDE-template.md      ← template برای CLAUDE.md پروژه جدید
```

---

## نحوه استفاده

**پروژه جدید:**
1. `new-project-checklist.md` رو دنبال کن
2. `CLAUDE-template.md` رو کپی کن به پروژه جدید به عنوان `CLAUDE.md`
3. فقط بخش‌های `[CUSTOMIZE]` رو عوض کن

**حین کار:**
- وقتی bug جدید پیدا می‌شه → `known-bugs.md` آپدیت می‌شه
- وقتی pattern جدید کشف می‌شه → فایل مربوطه آپدیت می‌شه

---

## پروژه‌هایی که از این استفاده می‌کنن

| پروژه | DS | مسیر |
|-------|-----|-------|
| Vitrina | Chakra UI v3 | `GitHub/Claude Code/Vitrina` |
