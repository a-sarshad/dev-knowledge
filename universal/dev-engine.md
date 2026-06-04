# dev-engine — راهنمای استفاده

ابزار CLI برای بررسی و auto-fix مشکلات کد. بدون Claude کار می‌کنه — صفر توکن.

---

## نصب

```bash
# از dev-agents build شده (یه بار)
cd ~/Documents/GitHub/Tools/dev-agents/packages/dev-engine
pnpm build

# یا اگه publish شد
npm i -g dev-engine
```

---

## راه‌اندازی پروژه

`.dev-engine.json` را در root پروژه (کنار `package.json`) بساز:

```json
{
  "direction": "rtl",
  "locale": "fa-IR",
  "calendar": "jalali",
  "ds": "chakra-v3",
  "icon_lib": "lucide",
  "ignore": ["node_modules", "dist", "build", "public"]
}
```

| فیلد | مقادیر |
|------|--------|
| `direction` | `rtl` · `ltr` · `both` |
| `ds` | `chakra-v3` · `chakra-v2` · `mui` · `antd` · `mantine` · `generic` |
| `icon_lib` | `lucide` · `heroicons` · `fa` · `generic` |

> **سریع‌تر:** `dev-engine init` رو اجرا کن — سوال‌به‌سوال پیش می‌ره و فایل رو می‌سازه.
> اگه `.dev-engine.json` نباشه، skill `dev-engine` آن را auto-detect و می‌سازه.

---

## دستورات Terminal

```bash
# بررسی کامل src/
dev-engine ./src

# فقط فایل‌های تغییرکرده (git diff HEAD)
dev-engine ./src --changed

# auto-fix موارد قابل fix
dev-engine ./src --fix

# فقط changed + fix
dev-engine ./src --changed --fix

# فقط یه module خاص
dev-engine ./src --module css-logical-props
dev-engine ./src --module dom-order,chakra-known-bugs

# فقط گزارش بدون خروجی رنگی (برای CI)
dev-engine ./src --json

# CI — report بدون fail کردن pipeline
dev-engine ./src --json --exit-zero

# همه فایل‌ها (حتی clean) نمایش بده
dev-engine ./src --verbose

# config در مسیر دیگه (monorepo)
dev-engine ./packages/ui/src --config ./packages/ui/.dev-engine.json

# watch — اجرای خودکار روی تغییر فایل
dev-engine ./src --watch

# ساخت .dev-engine.json به صورت interactive
dev-engine init
```

---

## Aliases — اضافه کن به `~/.zshrc`

```bash
# dev-engine shortcuts
alias den='dev-engine ./src'
alias denc='dev-engine ./src --changed'
alias denf='dev-engine ./src --fix'
alias dencf='dev-engine ./src --changed --fix'
alias denw='dev-engine ./src --watch'
alias den-ci='dev-engine ./src --json --exit-zero'
```

بعد از اضافه کردن: `source ~/.zshrc`

---

## Modules

| Module | کاربرد | DS |
|--------|--------|-----|
| `css-logical-props` | physical props → logical (mr → me، borderRight → borderInlineEnd) | همه |
| `dom-order` | RTL DOM order — icon/switch باید FIRST باشه | RTL |
| `chakra-known-bugs` | lineHeight="8"، bg.default، noOfLines، NativeSelect | chakra-v3 |
| `ds-component-usage` | raw `<select>`/`<table>` → DS components | chakra-v3 |
| `debug-artifacts` | console.log، debugger، TODO/FIXME | همه |
| `token-replacer` | hex color → design token | همه |
| `persian-numerals` | اعداد فارسی در کد | fa-IR |
| `icon-direction` | آیکون‌های جهت‌دار (arrow) در RTL | RTL |
| `build-git` | ۳ چک project-level (نه per-file): **①** `pnpm type-check` / build failure — **②** uncommitted files + unpushed commits — **③** HANDOFF.md staleness (N commits behind) | همه |

---

## Ignore — موارد intentional

```tsx
// یه خط — inline ignore
<NativeSelect.Root size="xs"> // dev-engine-ignore

// یه block — چند خط
// dev-engine-disable
<NativeSelect.Root>
  <NativeSelect.Field>...</NativeSelect.Field>
</NativeSelect.Root>
// dev-engine-enable
```

---

## workflow پیشنهادی

```
پروژه جدید؟
  → dev-engine init (interactive setup .dev-engine.json)
کد نوشتی؟
  → denc (فقط changed — سریع)
  → اگه violation داشت → denf (auto-fix)
  → اگه manual violation موند → Claude skill dev-engine
حین توسعه؟
  → denw (watch mode — auto-rerun روی تغییر)
قبل از commit؟
  → den (full scan)
CI؟
  → den-ci (= dev-engine ./src --json --exit-zero)
```
