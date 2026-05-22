# Figma → Code — پیاده‌سازی دیزاین
> universal — مستقل از design system
> کاربرد: وقتی دیزاین آماده‌ست و می‌خوای کدش کنی — نه برای شروع پروژه جدید
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

**فاز ۳ — Component Resolution (اجباری)**

قبل از نوشتن هر کد، برای هر کامپوننت این ترتیب رو دنبال کن:

```
1. Local first  → src/components/ پروژه رو بگرد
2. DS second    → داکس DS یا MCP server چک کن
3. Build last   → فقط اگه هیچ‌کدام موجود نبود بساز — از primitives، نه custom HTML
```

**Local components:**
```bash
# قبل از ساختن هر چیزی:
find src/components -name "*.tsx" | xargs grep -l "<ComponentName"
# یا:
ls src/components/ui/ src/components/<feature>/
```
اگه موجود بود → import کن و استفاده کن.
اگه variant جدیدی نیاز داری → extend کن، نساز.

**DS components:**
```
# روش ۱ — Figma MCP:
search_design_system("<component-name>")

# روش ۲ — داکس DS (URL از CLAUDE.md پروژه):
web_fetch("<ds-docs-url>/<component-name>")
```
اگه موجود بود → prop‌ها رو بخون، از همون کامپوننت استفاده کن.
**هیچ‌وقت** یه کامپوننت DS رو با HTML خالص rebuild نکن.

**Build new (فقط اگه هیچ‌کدوم موجود نبود):**
- با primitives DS بساز (Box، Flex، Text، ...)
- هیچ رنگ یا spacing hardcode نکن
- در `src/components/ui/` یا محل مناسب پروژه قرار بده

---

**فاز ۴ — کد بنویس**
- Figma local components → پس از Component Resolution بالا
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
