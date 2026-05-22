# dev-knowledge (DN) — راهنمای کار با Claude

این repo دانش مشترک پروژه‌هاست. Claude این فایل رو در هر session می‌خونه
تا بدونه ساختار DN چیه و چطور باهاش کار کنه.

---

## ساختار پوشه‌ها

```
dev-knowledge/
├── skills/             ← فایل‌های .skill (automation)
│   ├── README.md                ← راهنمای کامل همه skill‌ها (trigger/کاربرد/خروجی)
│   ├── dev-delivery-check.skill
│   ├── dev-init-wizard.skill
│   ├── dev-token-review.skill
│   ├── figma-page-implement.skill
│   ├── wf-commit-project.skill
│   ├── wf-session-start.skill
│   └── wf-session-update.skill
├── universal/          ← دانش cross-project (همه جا صدق می‌کنه)
├── projects/           ← context خاص هر پروژه
│   ├── airport/
│   └── vitrina/
└── design-systems/     ← دانش خاص هر design system
    ├── bootstrap5/
    └── chakra-ui-v3/
```

---

## قوانین scope

### universal/
- فقط چیزهایی که بدون تغییر در **همه پروژه‌ها** صدق می‌کنن
- اگه یه نکته project-specific یا DS-specific داره، اینجا نذار

### projects/<name>/
- context، تصمیمات، و نکات خاص اون پروژه
- برای load کردن: skill مربوط به پروژه رو invoke کن

### design-systems/<name>/
- راهنما، token، و نکات خاص اون DS
- قابل استفاده در چند پروژه‌ی مختلف

---

## Workflow استاندارد

1. **خوندن context** — قبل از هر کار مرتبط با پروژه، skill آن پروژه رو load کن
2. **ویرایش فایل‌ها** — با ابزارهای Read/Write/Edit
3. **Commit خودکار** — بعد از هر تغییر، skill `wf-commit-dn` اجرا می‌شه

### مثال:
```
User: روی Vitrina کار می‌کنم
Claude: [loads vitrina-project-context skill] → [reads projects/vitrina/project-context.md]
```

---

## Skills مرتبط

### wf — Workflow (مدیریت session و repo)
| Skill | کاربرد |
|-------|---------|
| `wf-commit-project` | آماده‌سازی commit message برای هر git repo (جنرال) |
| `wf-session-start` | briefing وضعیت پروژه در شروع session |
| `wf-session-update` | ذخیره وضعیت و آپدیت HANDOFF.md در هر مرحله (جنرال) |

### dev — Development (کدنویسی و پروژه)
| Skill | کاربرد |
|-------|---------|
| `dev-init-wizard` | ساخت پروژه جدید با scaffold کامل |
| `dev-token-review` | بررسی کد از نظر استفاده صحیح از design tokens |
| `dev-delivery-check` | بررسی خودکار checklist قبل از تحویل/merge/deploy |
| `figma-page-implement` | pipeline کامل Figma → React با چک‌های token/build/visual/RTL |

### project context
| Skill | کاربرد |
|-------|---------|
| `vitrina-project-context` | Load context پروژه Vitrina |
| `airport-project-context` | Load context پروژه Airport |
| `ds-chakra-ui` | Load دانش Chakra UI v3 |

فایل‌های skill در `skills/` هستن — برای نصب/آپدیت از همانجا استفاده کن.

---

## نکات مهم برای Claude

- **هیچ‌وقت session ID رو hardcode نکن** — از dynamic path detection استفاده کن:
  ```bash
  DN_PATH=$(ls -d /sessions/*/mnt/dev-knowledge 2>/dev/null | head -1)
  ```
- بعد از هر تغییر فایل در این repo، بدون اینکه کاربر بخواد، `wf-commit-dn` اجرا کن
- برای خطاهای git: `universal/git-troubleshoot.md` رو ببین
