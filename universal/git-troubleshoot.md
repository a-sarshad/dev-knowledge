# Git — خطاهای رایج و راه حل
> universal — مستقل از پروژه
> updated: 2026-05-18

---

## 🔒 Lock file errors

### `index.lock` یا `HEAD.lock` موجوده

**پیام خطا:**
```
fatal: Unable to create '.git/index.lock': File exists.
fatal: cannot lock ref 'HEAD': Unable to create '.git/HEAD.lock': File exists.
```

**علت:** یه فرآیند git قبلی کرش کرده و lock file رو پاک نکرده.

**راه حل:**
```bash
# پیدا کردن همه lock fileها
find /path/to/repo/.git -name "*.lock"

# پاک کردن (هر کدوم که وجود داشت)
rm "/path/to/repo/.git/index.lock"
rm "/path/to/repo/.git/HEAD.lock"

# بعد دوباره commit کن
git commit -m "..."
```

> **نکته:** اگه GitHub Desktop همین خطا داد، از Terminal پاک کن — Desktop نمی‌تونه خودش lock رو حذف کنه.

**اگه `rm` هم کار نکرد (permission denied):** از temp index workaround استفاده کن:
```bash
cd "/path/to/repo"
export GIT_INDEX_FILE=/tmp/git-temp-index
cp .git/index /tmp/git-temp-index
git add -A
git commit -m "..."
unset GIT_INDEX_FILE
```

---

## 🔀 Commit از GitHub Desktop کار نمی‌کنه

اگه Desktop خطای عجیب داد، مستقیم از Terminal:
```bash
cd "/path/to/repo"
git add -A
git commit -m "type: message"
git push
```

---

## 📦 Push رد شد — branch diverged

**پیام خطا:**
```
! [rejected] master -> master (fetch first)
```

**راه حل:**
```bash
git pull --rebase origin master
git push
```

---

## 🔑 Authentication failed هنگام push

**راه حل:** توی macOS Keychain، credential قدیمی GitHub رو پاک کن:
- Keychain Access → جستجوی `github.com` → حذف → دوباره push کن (پسورد/token می‌خواد)

---

## 📝 Commit زده شد ولی push نشد

```bash
git log --oneline -5   # ببین commit هست؟
git push               # push کن
```

---

## ↩️ Undo — برگشت از آخرین commit (هنوز push نشده)

```bash
git reset --soft HEAD~1   # commit رو برمی‌گردونه ولی تغییرات می‌مونن
```

---

## 🗑️ Discard همه تغییرات unstaged

```bash
git checkout -- .
# یا در git جدید:
git restore .
```
