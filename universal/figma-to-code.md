# Figma → Code Workflow
> universal — مستقل از design system
> updated: 2026-05-17

---

## فازهای کار

**فاز ۱ — Context**
قبل از شروع، اینا رو داشته باش:
- CLAUDE.md پروژه (stack، RTL rules، known bugs)
- لیست کامپوننت‌های local در Figma
- ساختار layout templates پروژه

**فاز ۲ — Figma بخون**
از Figma MCP یا screenshot برای فهمیدن layout و tokens استفاده کن:
- کدوم layout template؟ (1-col، 2-col، sidebar+main، ...)
- spacing‌ها (padding، gap) چند px؟
- رنگ‌ها از کدوم token یا variable؟
- typography از کدوم text style؟

**فاز ۳ — کد بنویس**
- Figma local components → components پروژه با DS انتخابی
- Figma tokens → DS tokens پروژه (مقادیر hardcode نکن)
- Figma auto layout → Flex/Grid

برای mapping token های Figma به DS خاص:
→ `design-systems/<ds-name>/figma-tokens.md`

---

## نگاشت Figma Auto Layout به CSS

| Figma | CSS |
|-------|-----|
| Horizontal auto layout | `display: flex; flex-direction: row` |
| Vertical auto layout | `display: flex; flex-direction: column` |
| Fill container | `flex: 1` یا `width: 100%` |
| Hug contents | بدون width ثابت |
| Fixed width | `width: 256px` |
| gap | `gap: Npx` |
| padding | `padding: Npx` |

> هر DS این‌ها رو به syntax خودش ترجمه می‌کنه.
> مثال Chakra UI v3: → `design-systems/chakra-ui-v3/rtl.md`

---

## RTL DOM Order

در RTL، اولین child = rightmost. این یه قانون CSS/HTML خالصه، مستقل از DS:

```html
<!-- Figma: [Start(راست)] [Middle] [End(چپ)] -->
<div style="display:flex">
  <div>Start — rightmost</div>
  <div style="flex:1">Middle</div>
  <div>End — leftmost</div>
</div>
```

→ مفاهیم کامل‌تر: `universal/rtl-concepts.md`

---

## چک‌لیست قبل از تحویل

- [ ] هیچ مقدار رنگ hardcode نشده (نه hex، نه rgb)
- [ ] spacing از token/variable استفاده شده
- [ ] RTL DOM order رعایت شده (اولین child = rightmost)
- [ ] logical CSS props استفاده شده (`margin-inline-start` نه `margin-left`)
- [ ] Portal/overlay components `dir="rtl"` دارن
- [ ] روی موبایل بررسی شده
