# BLUEPRINT — قانون اساسی سیستم

> **این فایل چیه:** نقطه‌ی مرجع واحد برای کل سیستم. هر وقت گیج شدی
> «این فایل کجا بره؟»، «این قانون کجا زندگی کنه؟»، «فلوی Figma→code دقیقاً چیه؟»
> — جواب اینجاست. این سند مقصد (target) رو تعریف می‌کنه، نه لزوماً وضع فعلی رو.
> updated: 2026-06-04

---

## ۰. مشکلی که این سند حل می‌کنه

دانش پخش شده روی ۳ repo و چند لایه. مراحل Figma→code زیاد شده.
ریسک: چیزی وسط کار فراموش بشه، یا فایل اشتباه جای اشتباه ساخته بشه.

**اصل بنیادی:** چیزی که نباید فراموش شه، نباید فقط «نوشته» بشه —
باید **اجراشونده** بشه. نوشته فراموش می‌شه؛ CLI که step به step اجرا می‌شه
و وسط fail می‌کنه، نمی‌تونه فراموش کنه.

→ راه‌حل sprawl: **کمتر کردن + یک نقطه‌ی ورود واحد**، نه اضافه کردن فایل.

---

## ۱. سه repo — سه مسئولیت (هیچ overlap)

| repo | مسئولیت واحد | چی توش هست | چی **نباید** توش باشه |
|------|--------------|-----------|----------------------|
| `Tools/dev-agents` | **ENGINE** — همه‌ی check دترمینیستیک (صفر توکن) | dev-engine CLI + subcommandها | دانش، context، قانون نرم |
| `Tools/dev-knowledge` | **دانش cross-project** + source اسکیل‌ها | universal/، design-systems/، skills/ (source) | context یک پروژه‌ی خاص |
| `Projects/<X>` | **قانون + context خودِ پروژه** | CLAUDE.md (always-on)، .claude/context/، .dev-engine.json | دانشی که روی همه پروژه‌ها صدق می‌کنه |

**قانون طلایی جای فایل:**
```
روی همه پروژه‌ها صدق می‌کنه؟        → dev-knowledge/universal یا dev-agents
فقط یک پروژه؟                       → repo همون پروژه
باید هرگز فراموش نشه؟               → CLAUDE.md پروژه (always-on)
check دترمینیستیک (قابل اجرا)؟      → dev-agents (CLI)
شتاب‌دهنده‌ی فلو (نه منبع قانون)؟   → skills/
```

---

## ۲. چهار لایه‌ی قابلیت اطمینان (کِی چی لود می‌شه)

| لایه | محل | کِی لود می‌شه | برای چی | ریسک فراموشی |
|------|-----|---------------|---------|--------------|
| **RULE** | `{project}/CLAUDE.md` | هر پیام، خودکار | gate اجباری + DoD | صفر |
| **ENGINE** | `dev-agents` (dev-engine) | وقتی CLI صدا زده شه | check + auto-fix | صفر (fail-hard) |
| **REFERENCE** | `dev-knowledge/*.md` | وقتی صریح read شه | دانش عمیق | بالا — فقط on-demand |
| **ACCELERATOR** | `skills/*.skill` | فقط trigger match | orchestration فلو | متوسط — اگه trigger نخوره |

> **duplication بین لایه‌ها = منبع گیجی.** هر فکت فقط **یک خونه** داره.
> مثال: قانون «token نه hardcode» → خونه‌اش `token-replacer` در dev-engine (ENGINE) +
> یک خط در gate (RULE). توضیح عمیقش در figma-to-code.md (REFERENCE). تکرار کامل ممنوع.

---

## ۳. Pipeline واحد Figma → Code

**یک نقطه‌ی ورود:** skill `dev-impl` (جایگزین figma-impl-checklist + impl-session پیشنهادی).
این skill ترتیب رو enforce می‌کنه — step‌ها رو نمی‌شه پرید.

```
USER: "این frame رو implement کن: [Figma URL]"
   │
   ▼
┌─ LAYER 0 — AUTO (بدون trigger) ───────────────────────────┐
│ {project}/CLAUDE.md gate همیشه فعال:                       │
│  ✓ Figma tool fail → STOP، حدس نزن                         │
│  ✓ Component Resolution order: Local → DS MCP → Build last │
└────────────┬───────────────────────────────────────────────┘
             ▼
┌─ STEP 0 — PRE-FLIGHT (CLI) ──────────────────────────────┐
│ dev-engine preflight                                          │
│  ✓ DS installed?  ✓ figma-tokens.json fresh?              │
│  ✓ component-map موجود؟  → fail-hard اگه چیزی کمه        │
└────────────┬───────────────────────────────────────────────┘
             ▼
┌─ STEP 1 — CONTEXT LOAD (Claude reads LOCAL، نه MCP) ──────┐
│ reads: {project}/.claude/context/figma-tokens.json        │
│ reads: {project}/.claude/context/component-map.json       │
│ reads: {project}/.claude/context/project-context.md       │
│  → tokens و component map از local؛ MCP call صرفه‌جویی    │
└────────────┬───────────────────────────────────────────────┘
             ▼
┌─ STEP 2 — FIGMA FETCH (targeted، نه scatter) ────────────┐
│ MCP: فقط frame مورد نظر + componentهای داخلش              │
│  → checklist مخصوص این frame ساخته و pin می‌شه           │
└────────────┬───────────────────────────────────────────────┘
             ▼
┌─ STEP 3 — IMPLEMENT (Claude) ────────────────────────────┐
│  Resolution: grep src/ → ds-registry → Build last         │
│  Token: از figma-tokens.json local، صفر hardcode          │
│  Responsive: breakpoints از snapshot/map                   │
│  RTL: logical props، DOM order (ref: universal/language.md)│
└────────────┬───────────────────────────────────────────────┘
             ▼
┌─ STEP 4 — VERIFY (CLI-first) ────────────────────────────┐
│ dev-engine ./src --changed --fix   ← code checks + auto-fix  │
│ dev-engine dod / build-git         ← build + DoD + git state │
│ dev-engine visual-diff (اختیاری)   ← screenshot vs Figma     │
└────────────┬───────────────────────────────────────────────┘
             ▼
┌─ STEP 5 — COMMIT ────────────────────────────────────────┐
│ skill wf-commit-project → دستور آماده (هرگز از sandbox)  │
└───────────────────────────────────────────────────────────┘
```

**DS-agnostic چطوریه:** هر چیز DS-specific از `.dev-engine.json` (`ds: chakra-v3`)
و MCP خونده می‌شه — نه hardcode در flow. همین pipeline برای Bootstrap/Chakra/هر DS کار می‌کنه.

**تقسیم کار CLI vs Claude:**
- **CLI (dev-agents):** preflight، token-sync، dod، build-git، visual-diff — هر چیز دترمینیستیک
- **Claude:** فقط STEP 1-3 (read context، Figma fetch، implement)
- **skill `dev-impl`:** orchestrator — step‌ها رو به ترتیب صدا می‌زنه، نمی‌پره

---

## ۴. درخت تصمیم — «این چیز کجا بره؟»

```
یه فکت/قانون/فایل جدید داری؟
│
├─ قابل اجرا با grep/script هست؟ (check دترمینیستیک)
│   └─ بله → dev-agents: dev-engine module یا subcommand
│
├─ روی همه پروژه‌ها صدق می‌کنه؟
│   ├─ بله، DS-agnostic → dev-knowledge/universal/
│   └─ بله، DS-specific → dev-knowledge/design-systems/<ds>/
│
├─ فقط یک پروژه؟
│   ├─ قانون اجباری (نباید فراموش شه) → {project}/CLAUDE.md
│   └─ context/reference          → {project}/.claude/context/
│
└─ شتاب‌دهنده‌ی فلو؟
    └─ skill (source در dev-knowledge/skills، نصب global در Cowork)
```

---

## ۵. جای skillها و contextها (تصمیم Cowork)

**واقعیت Cowork:** skill با کلیک روی `.skill` به‌صورت **global** نصب می‌شه
(نه per-project). پروژه‌ها `.claude/skills` ندارن.

نتیجه:
- **source اسکیل** → مرکزی در `dev-knowledge/skills/` می‌مونه (یک‌جا مدیریت/نصب)
- **محتوای project-specific** → از dev-knowledge منتقل می‌شه به repo خود پروژه:
  `{project}/.claude/context/`
- **skill project-context** → یه loader نازک می‌شه که محتوا رو از repo پروژه read می‌کنه،
  نه اینکه خودش محتوا نگه داره (پایان duplication)

> **توجه:** CLAUDE.md ویترینا الان ~۲۵KB‌ـه — خودش بزرگه. محتوای بیشتر
> **نباید** بره توش. context تفصیلی → `.claude/context/`، فقط قانون اجباری → CLAUDE.md.

---

## ۶. ساختار مقصد (target tree)

```
Tools/dev-agents/                          ← ENGINE
└── packages/dev-engine/
    ├── modules/ (موجود)                   ← token-replacer، css-logical-props، ...
    └── subcommands/ (جدید)
        ├── preflight                       ← env/token/DS ready check
        ├── figma-sync                      ← Figma Variables → local JSON
        └── visual-diff                     ← screenshot vs Figma

Tools/dev-knowledge/                        ← دانش cross-project + skill source
├── BLUEPRINT.md                            ← همین فایل (constitution)
├── CLAUDE.md                               ← قوانین کار با DN
├── universal/                              ← DS-agnostic
├── design-systems/<ds>/                    ← DS-specific
└── skills/
    ├── dev-impl.skill        (جدید)        ← orchestrator واحد Figma→code
    ├── dev-engine.skill
    ├── wf-*.skill
    └── <project>-context.skill (نازک)      ← فقط loader، محتوا در repo پروژه

Projects/<X>/                               ← قانون + context خودِ پروژه
├── CLAUDE.md                               ← فقط قانون اجباری (gate + DoD)
├── .dev-engine.json
└── .claude/context/
    ├── project-context.md                  ← منتقل‌شده از dev-knowledge/projects/<X>/
    ├── known-bugs.md                       ← منتقل‌شده از dev-knowledge/projects/<X>/
    ├── figma-tokens.json     (generated)   ← خروجی dev-engine figma-sync
    └── component-map.json    (generated)   ← خروجی dev-engine figma-sync
```

---

## ۷. نقشه‌ی مهاجرت (فعلی → مقصد)

| # | کار | از | به | نوع |
|---|-----|----|----|-----|
| 1 | محتوای context پروژه‌ها | `dev-knowledge/projects/<X>/` | `Projects/<X>/.claude/context/` | MOVE |
| 2 | skill project-context | محتوای کامل | loader نازک (read از پروژه) | REFACTOR |
| 3 | `dod-check` پیشنهادی | — | حذف (dev-engine پوشش می‌ده) | DROP |
| 4 | ۶ script پیشنهادی | فایل‌های جدا | subcommand‌های dev-engine | MERGE → dev-agents |
| 5 | figma-impl-checklist + impl-session | دو skill | یک skill `dev-impl` | MERGE |
| 6 | duplication قانون token بین لایه‌ها | چند جا | یک خونه (dev-engine + یک خط gate) | DEDUPE |

**ترتیب اجرا (وقتی شروع کردیم):**
1. اول این BLUEPRINT تثبیت (الان ✓)
2. مهاجرت context پروژه‌ها (#1، #2) — کم‌ریسک، duplication رو می‌بنده
3. ساخت skill `dev-impl` (#5) — نقطه‌ی ورود واحد
4. extend dev-engine با subcommandها (#4) در dev-agents
5. حذف/dedupe باقی‌مونده (#3، #6)

---

## ۸. چک‌لیست «آیا درست دارم کار می‌کنم؟»

هر وقت شک کردی:
- [ ] قانونی که نباید فراموش شه، در CLAUDE.md پروژه‌ست؟ (نه فقط skill)
- [ ] check قابل‌اجرا، CLI شده؟ (نه دستی توسط Claude)
- [ ] فکت فقط یک خونه داره؟ (نه duplicate بین لایه‌ها)
- [ ] context پروژه‌ی خاص، در repo همون پروژه‌ست؟ (نه dev-knowledge)
- [ ] فلوی Figma→code از `dev-impl` می‌گذره؟ (نه ad-hoc)
