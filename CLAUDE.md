# dev-knowledge (DN) — راهنمای کار با Claude

این repo دانش مشترک پروژه‌هاست. Claude این فایل رو در هر session می‌خونه
تا بدونه ساختار DN چیه و چطور باهاش کار کنه.

---

## ساختار پوشه‌ها

```
dev-knowledge/
├── skills/             ← فایل‌های .skill (automation) — source برای نصب در Cowork
│   ├── README.md                ← راهنمای کامل همه skill‌ها (trigger/کاربرد/خروجی)
│   ├── dev-init-wizard.skill
│   ├── dev-projfix.skill
│   ├── dev-token-review.skill
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
3. **Commit خودکار** — بعد از هر تغییر، skill `wf-commit-project` اجرا می‌شه

### اصل طلایی — قانون اجباری در always-on، نه skill

> **چرا قبلاً مرحله‌ها فراموش می‌شدن:** قوانین حیاتی (مثل «DS رو از MCP بگیر، rebuild نکن») فقط در skillهای on-demand بودن. skill اگه trigger نشه یا نصب نباشه = قانون در context نیست.

سلسله‌مراتب قابلیت اطمینان:

| محل | کِی لود میشه | برای چی |
|-----|-------------|---------|
| **CLAUDE.md پروژه** | هر پیام، خودکار | قانون اجباری + DoD (gate) |
| **dev-knowledge/*.md** | وقتی صریح read شه | مرجع عمیق |
| **Skills** | فقط trigger match | شتاب‌دهنده (نه منبع قانون) |

→ هر چیزی که **نباید فراموش شه** باید در CLAUDE.md پروژه باشه. این قانونه، نه سلیقه.

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
| `dev-init-wizard` | ساخت پروژه جدید با scaffold کامل (gate Figma→Code رو در CLAUDE.md پروژه bake میکنه) |
| `dev-token-review` | بررسی کد از نظر استفاده صحیح از design tokens |
| `dev-delivery-check` | بررسی خودکار checklist قبل از تحویل/merge/deploy |

### Figma (رسمی — figma plugin، نصب‌شده)
| Skill | جهت | کاربرد |
|-------|-----|--------|
| `figma-use` | — | اجرای JS در Figma (prerequisite برای write/read منحصربه‌فرد) |
| `figma-implement-design` | Figma → code | pipeline قدم‌به‌قدم پیاده‌سازی |
| `figma-generate-design` | code → Figma | ساخت صفحه در Figma از کد |
| `figma-generate-library` | — | ساخت library در Figma |
| `figma-code-connect` | — | اتصال کد به کامپوننت Figma |

> **مهم:** قانون اجباری Figma→code (Component Resolution / DS MCP / DoD) در **CLAUDE.md هر پروژه** زندگی میکنه (always-on)، نه در skill. skill فقط شتاب‌دهنده‌ست. مرجع عمیق: `universal/figma-to-code.md`.
> skill قدیمی `figma-page-implement` بازنشسته شد — محتوای اجباریش در `figma-to-code.md` + gate پروژه‌ها ادغام شد.

### project context (نصب‌شده)
| Skill | کاربرد |
|-------|---------|
| `vitrina-project-context` | Load context پروژه Vitrina |
| `airport-project-context` | Load context پروژه Airport |
| `vitrina-figma-rules` | قوانین طراحی Figma برای Vitrina (code→Figma) |
| `ds-chakra-ui` | Load دانش Chakra UI v3 |

فایل‌های skill در `skills/` هستن — برای نصب/آپدیت از همانجا در Cowork استفاده کن.

---

## نکات مهم برای Claude

- **هیچ‌وقت session ID رو hardcode نکن** — از dynamic path detection استفاده کن:
  ```bash
  DN_PATH=$(ls -d /sessions/*/mnt/dev-knowledge 2>/dev/null | head -1)
  ```
- بعد از هر تغییر فایل در این repo، بدون اینکه کاربر بخواد، `wf-commit-project` اجرا کن
- برای خطاهای git: `universal/git-troubleshoot.md` رو ببین
- برای استفاده از projfix CLI: `universal/projfix.md` رو ببین
