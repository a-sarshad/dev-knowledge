# Skills — راهنمای کامل

این پوشه شامل تمام skill‌های automation پروژه هست.
هر skill یه فایل `.skill` هست که در Cowork نصب می‌شه.

> **نصب:** روی فایل `.skill` کلیک کن → Save skill

---

## فهرست سریع

| Skill | دسته | کاربرد خلاصه |
|-------|------|--------------|
| [wf-session-start](#wf-session-start) | workflow | شروع session — briefing وضعیت پروژه |
| [wf-session-update](#wf-session-update) | workflow | ذخیره وضعیت + آپدیت HANDOFF/CLAUDE/README — هر پروژه |
| [wf-commit-project](#wf-commit-project) | workflow | commit message آماده — هر git repo |
| [dev-init-wizard](#dev-init-wizard) | dev | scaffold پروژه جدید قدم‌به‌قدم |
| [dev-token-review](#dev-token-review) | dev | بررسی hardcode vs design token |
| [dev-delivery-check](#dev-delivery-check) | dev | checklist قبل از merge/deploy |
| [dev-projfix](#dev-projfix) | dev | اجرای projfix — بررسی و auto-fix کد |

> **Figma → code:** skill جدا توی این repo نیست. از skill رسمی `figma-implement-design` استفاده کن. قانون اجباری (Component Resolution / DS MCP / DoD) در **CLAUDE.md هر پروژه** هست (always-on). جزئیات: بخش [Figma → Code](#figma--code) پایین.

---

## Workflow Skills — مدیریت session و repo

### wf-session-start

**فایل:** `wf-session-start.skill`

**کاربرد:** در شروع هر session کاری، یه briefing سریع از وضعیت پروژه می‌ده — کجا بودیم، آخرین تغییرات چی بود، قدم بعدی چیه.

**چه موقع فعال می‌شه:**
- «بریم سراغ [پروژه]»
- «شروع کنیم» / «ادامه بدیم»
- «کجا بودیم؟» / «وضعیت پروژه چیه؟»
- «start session» / «let's continue» / «where were we»

**خروجی:** briefing ۳-۴ خطی از git log + HANDOFF.md

**وابستگی:** HANDOFF.md پروژه

---

### wf-session-update

**فایل:** `wf-session-update.skill`

**کاربرد:** روی **هر پروژه‌ای** کار می‌کنه. وضعیت فعلی رو snapshot می‌کنه — HANDOFF.md رو آپدیت می‌کنه، و اگه تغییر معماری/باگ/ساختار داشتیم CLAUDE.md و README.md پروژه رو هم آپدیت می‌کنه. اگه این فایل‌ها وجود نداشتن کار می‌کنه، اگه وجود داشتن آپدیت می‌کنه.

**چه موقع فعال می‌شه:**
- «آپدیت کن» / «ذخیره کن» / «save کن»
- «وضعیت رو ثبت کن» / «HANDOFF رو آپدیت کن»
- «session-update بزن» / «update کن»

**خروجی:** آپدیت HANDOFF.md + بررسی و آپدیت CLAUDE.md / README.md در صورت نیاز

**تشخیص پروژه:** از workspace folder مانت‌شده، CLAUDE.md در context، یا package.json

---

### wf-commit-project

**فایل:** `wf-commit-project.skill`

**کاربرد:** برای **هر git repo** — Vitrina، Airport، dev-knowledge، یا هر پروژه جدیدی — یه commit message آماده می‌کنه. مسیر رو خودش از context/CLAUDE.md/package.json تشخیص می‌ده. هیچ‌وقت git commit نمی‌زنه — فقط دستور آماده برای copy-paste در Terminal می‌ده.

**چه موقع فعال می‌شه:**
- **خودکار** — بعد از هر عملیات روی فایل‌های هر پروژه
- «commit کن» / «push کن» / «commit message بساز»
- «save کن» / «sync کن» / «بزن رو git»

**خروجی:** دستور `git add -A && git commit -m "..." && git push` آماده برای Terminal

**نکته مهم:** هیچ‌وقت از sandbox git write نمی‌زنه — فقط `git status` می‌خونه

**جایگزین:** این skill جایگزین `wf-commit-dn` شده — برای DN هم همین skill رو استفاده کن

---

## Dev Skills — کدنویسی و پروژه

### dev-init-wizard

**فایل:** `dev-init-wizard.skill`

**کاربرد:** یه wizard تعاملی برای scaffold کردن پروژه React جدید از صفر. سوال‌به‌سوال پیش می‌ره (نام، DS، RTL، feature flags) و بعد تمام فایل‌های پروژه رو با tokenization کامل می‌سازه.

**چه موقع فعال می‌شه:**
- «پروژه جدید بساز» / «پروژه جدید می‌خوام»
- «scaffold project» / «create new project» / «init project»
- «شروع پروژه» / «پروژه رو راه بنداز» / «ساخت پروژه»

**خروجی:** ساختار کامل پروژه با CLAUDE.md، HANDOFF.md، config files، token setup

**وابستگی:** مشخص کردن DS (Chakra UI / Bootstrap 5) و RTL preference

---

### dev-token-review

**فایل:** `dev-token-review.skill`

**کاربرد:** کد JSX/TSX/CSS/SCSS رو بررسی می‌کنه و هر مقدار hardcode (رنگ، spacing، font size) رو پیدا می‌کنه و token معادلش رو پیشنهاد می‌ده.

**چه موقع فعال می‌شه:**
- «review کن» / «توکن بررسی کن» / «hardcode داره؟»
- «token درست استفاده شده؟» / «کد رو از نظر DS بررسی کن»
- «این رنگ‌ها درسته؟» / «spacing درسته؟»
- «token review» / «design system review»
- وقتی کد paste می‌شه و پروژه‌ای active هست

**خروجی:** جدول مشکلات + token پیشنهادی + خلاصه کیفیت (A/B/C/D)

**وابستگی:** شناخت DS پروژه (Chakra UI v3 یا Bootstrap 5)

---

### dev-projfix

**فایل:** `dev-projfix.skill`

**کاربرد:** projfix رو روی پروژه اجرا می‌کنه — violations پیدا می‌کنه، auto-fix می‌زنه، و موارد manual رو راهنمایی می‌کنه. برای هر پروژه‌ای با `.projfix.json` کار می‌کنه — هر DS، هر direction.

**چه موقع فعال می‌شه:**
- «review-fix» / «projfix بزن» / «run projfix»
- «fix issues» / «بررسی کد» / «check and fix» / «اصلاح کن»
- بعد از پیاده‌سازی Figma یا تغییرات چند-فایلی

**خروجی:** گزارش violations (error/warning) + auto-fix + راهنمای manual fixes

**وابستگی:** وجود `.projfix.json` در پروژه + نصب `projfix` CLI

---

### dev-delivery-check

**فایل:** `dev-delivery-check.skill`

**کاربرد:** قبل از هر merge، PR، یا deploy — یه checklist کامل اجرا می‌کنه: git status، build، tests، token review، RTL، responsive. بدون سوال همه چک‌ها رو می‌زنه.

**چه موقع فعال می‌شه:**
- «تحویل بده» / «آماده‌ست؟» / «merge کن» / «PR بزن»
- «deploy کن» / «ready for review» / «review آماده‌ست؟»
- «بررسی کن قبل از تحویل» / «checklist بزن»
- «می‌تونیم merge کنیم؟» / «بریم production» / «push کنیم؟»

**خروجی:** گزارش ✅/⚠️/❌ برای هر چک + نتیجه کلی (آماده‌ست یا نه)

**وابستگی:** وجود `package.json` و git repo در پروژه

---

## Figma → Code

> skill اختصاصی `figma-page-implement` **بازنشسته شد**. دلیل: نصب نبود و قانون اجباریش هیچ‌وقت لود نمی‌شد. محتوای اجباریش به دو جای always-on منتقل شد.

**حالا چطور Figma رو کد می‌کنیم:**

| لایه | کجا | نقش |
|------|-----|-----|
| **قانون اجباری (gate)** | `## Figma → Code Protocol` در CLAUDE.md هر پروژه | همیشه‌فعال — Component Resolution + DoD. هیچ‌وقت فراموش نمیشه |
| **مرجع عمیق** | `universal/figma-to-code.md` | فازها، MCP tool names، pitfalls، verification |
| **pipeline قدم‌به‌قدم** | skill رسمی `figma-implement-design` | شتاب‌دهنده — atoms→page |

**کدوم Figma skill کِی:**

| می‌خوای... | skill |
|-----------|-------|
| Figma → کد (پیاده‌سازی) | `figma-implement-design` + gate پروژه |
| کد/ایده → Figma (طراحی) | `figma-generate-design` (+ `vitrina-figma-rules` برای Vitrina) |
| اجرای JS در Figma | `figma-use` (prerequisite) |
| library در Figma | `figma-generate-library` |
| اتصال کد ↔ Figma | `figma-code-connect` |

**اصل:** قانونِ «نباید فراموش شه» → CLAUDE.md پروژه (always-on). skill = شتاب، نه منبع قانون.

---

## نحوه استفاده از یه skill جدید

۱. فایل `.skill` رو نصب کن (Save skill در Cowork)
۲. در session بعدی، skill به طور خودکار در لیست available skills هست
۳. Claude بر اساس trigger phrases خودش load می‌کنه

## ساختن skill جدید

از skill `skill-creator` در Cowork استفاده کن.
