# Projects

هر پوشه یک پروژه است. هر پروژه شامل:
- `project-context.md` — brand tokens، layout، تنظیمات خاص
- `known-bugs.md` — باگ‌های project-specific (نه DS-level)

Claude این فایل‌ها را از طریق skill پروژه load می‌کند.

---

## پروژه‌های موجود

| پروژه | پوشه | Design System | وضعیت |
|-------|------|---------------|--------|
| Vitrina | `vitrina/` | Chakra UI v3 / RTL | فعال |
| Kish Airport | `airport/` | Chakra UI v3 / RTL+LTR | فعال |

→ [vitrina/project-context.md](vitrina/project-context.md)
→ [airport/project-context.md](airport/project-context.md)

---

## افزودن پروژه جدید

```
projects/
  <name>/
    project-context.md   ← از CLAUDE-template.md در chakra-ui-v3/ کپی کن
    known-bugs.md        ← فقط وقتی باگ project-specific پیدا شد بساز
```
