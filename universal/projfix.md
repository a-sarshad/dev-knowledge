# projfix — راهنمای استفاده

ابزار CLI برای بررسی و auto-fix مشکلات کد. بدون Claude کار می‌کنه — صفر توکن.

---

## نصب

```bash
# از dev-agents build شده (یه بار)
cd ~/Documents/GitHub/Tools/dev-agents/packages/projfix
pnpm build

# یا اگه publish شد
npm i -g projfix
```

---

## راه‌اندازی پروژه

`.projfix.json` را در root پروژه (کنار `package.json`) بساز:

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

> اگه `.projfix.json` نباشه، skill `dev-projfix` آن را auto-detect و می‌سازه.

---

## دستورات Terminal

```bash
# بررسی کامل src/
projfix ./src

# فقط فایل‌های تغییرکرده (git diff HEAD)
projfix ./src --changed

# auto-fix موارد قابل fix
projfix ./src --fix

# فقط changed + fix
projfix ./src --changed --fix

# فقط یه module خاص
projfix ./src --module css-logical-props
projfix ./src --module dom-order,chakra-known-bugs

# فقط گزارش بدون خروجی رنگی (برای CI)
projfix ./src --json

# همه فایل‌ها (حتی clean) نمایش بده
projfix ./src --verbose
```

---

## Aliases — اضافه کن به `~/.zshrc`

```bash
# projfix shortcuts
alias pf='projfix ./src'
alias pfc='projfix ./src --changed'
alias pff='projfix ./src --fix'
alias pfcf='projfix ./src --changed --fix'
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
<NativeSelect.Root size="xs"> // projfix-ignore

// یه block — چند خط
// projfix-disable
<NativeSelect.Root>
  <NativeSelect.Field>...</NativeSelect.Field>
</NativeSelect.Root>
// projfix-enable
```

---

## workflow پیشنهادی

```
کد نوشتی؟
  → pfc (فقط changed — سریع)
  → اگه violation داشت → pff (auto-fix)
  → اگه manual violation موند → Claude skill dev-projfix
قبل از commit؟
  → pf (full scan)
CI؟
  → projfix ./src --json (خروجی machine-readable)
```
