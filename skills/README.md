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
| [figma-page-implement](#figma-page-implement) | dev | Figma → React با تمام چک‌های کیفیت |

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

### figma-page-implement

**فایل:** `figma-page-implement.skill`

**کاربرد:** Pipeline کامل برای تبدیل یه صفحه/کامپوننت Figma به کد React production-ready. از خوندن طرح تا مقایسه visual نهایی، همه چیز رو cover می‌کنه.

**چه موقع فعال می‌شه:**
- «implement کن» / «پیاده‌سازی کن» / «کد بنویس از فیگما»
- «این صفحه رو بزن» / «build this page» / «implement this screen»
- «از فیگما بساز» / «بیا این طرح رو کد کنیم»
- هر Figma URL + درخواست کد

**مراحل pipeline:**
1. **Context Setup** — شناسایی پروژه، load skill مناسب
2. **Figma Reading** — design context، component inventory، token mapping، screenshot
3. **Implementation Plan** — تأیید قبل از کد نوشتن
4. **Implementation** — atoms → molecules → organisms → page، فقط tokens، logical CSS
5. **Verification** — token grep + TypeScript build + visual comparison + RTL + responsive
6. **Handoff** — آپدیت HANDOFF.md + commit message آماده

**خروجی:** کد React کامل + گزارش verification + commit message آماده برای Terminal

**وابستگی:** `figma-use` skill (باید قبلاً load شده باشه) + Figma MCP connection

---

## نحوه استفاده از یه skill جدید

۱. فایل `.skill` رو نصب کن (Save skill در Cowork)
۲. در session بعدی، skill به طور خودکار در لیست available skills هست
۳. Claude بر اساس trigger phrases خودش load می‌کنه

## ساختن skill جدید

از skill `skill-creator` در Cowork استفاده کن.
