# README Template — برای پروژه‌های جدید
> universal
> updated: 2026-05-19

هنگام scaffold، این فایل رو با مقادیر واقعی از جواب‌های wizard پر کن.
هر فیلد به سوال مشخصی map شده — جایگزینی مستقیم، بدون حدس.

---

## نقشه wizard → README

| فیلد README | منبع |
|-------------|------|
| نام پروژه | Q1 |
| توضیح | Q3 |
| Framework | Q4 |
| Language | Q5 |
| Design System | Q7 |
| Package Manager | Q6 |
| State Management | Q21 |
| Data Fetching | Q22 |
| API Base URL | VITE_API_BASE_URL در .env.example |
| زبان / جهت | Q17 |
| ساختار src/i18n | فقط اگه Q17 = دوزبانه |
| ساختار src/services | فقط اگه Q19 = بله |
| ساختار src/types | فقط اگه Q20 = بله |
| دستور نصب | Q6 (pnpm/npm/yarn install) |

---

## خروجی نهایی — README.md پروژه

```markdown
# {Q1: نام پروژه}

{Q3: توضیح یک‌جمله‌ای پروژه}

## Stack

| بخش | تکنولوژی |
|-----|----------|
| Framework | {Q4} |
| Language | {Q5} |
| Design System | {Q7} |
| State | {Q21} |
| Data Fetching | {Q22} |
| Package Manager | {Q6} |
| زبان | {Q17: فارسی RTL / انگلیسی LTR / فارسی + انگلیسی} |

## شروع سریع

\`\`\`bash
{Q6: pnpm / npm / yarn} install
{Q6: pnpm / npm / yarn} dev
\`\`\`

## ساختار پروژه

\`\`\`
src/
├── theme/              ← design tokens و system config
│   ├── tokens.ts       ← single source of truth (رنگ، فونت، spacing)
│   └── index.ts        ← {Q7} system setup
├── providers/          ← app-level providers
├── components/         ← reusable UI components
├── pages/              ← صفحات / routes
[فقط اگه Q19 = بله:]
├── services/           ← API calls و data layer
[فقط اگه Q20 = بله:]
├── types/              ← TypeScript interfaces (api.ts, ...)
[فقط اگه Q21 = Zustand:]
├── stores/             ← Zustand stores
[فقط اگه Q17 = دوزبانه:]
└── i18n/               ← multilang setup (fa / en)
\`\`\`

[فقط اگه Q19 = بله — بخش Backend:]
## برای توسعه‌دهنده Backend

### محیط

\`\`\`bash
cp .env.example .env.local
# مقادیر واقعی رو پر کن
\`\`\`

متغیرهای محیطی مورد نیاز (`.env.example`):

\`\`\`env
VITE_API_BASE_URL=http://localhost:8000   # آدرس backend
VITE_API_TIMEOUT=10000                   # timeout به ms
\`\`\`

---

### افزودن endpoint جدید

**۱. تایپ رو تعریف کن** — `src/types/api.ts`:

\`\`\`ts
// Request payload
export interface CreateOrderPayload {
  productId: string
  quantity: number
}

// Response shape
export interface CreateOrderResponse {
  id: string
  status: 'pending' | 'confirmed'
  createdAt: string
}
\`\`\`

**۲. تابع سرویس بنویس** — `src/services/orderService.ts`:

\`\`\`ts
import { api } from './api'  // axios instance مرکزی
import type { CreateOrderPayload, CreateOrderResponse } from '@/types/api'

export const createOrder = (payload: CreateOrderPayload) =>
  api.post<CreateOrderResponse>('/orders', payload)

export const getOrders = () =>
  api.get<CreateOrderResponse[]>('/orders')
\`\`\`

**۳. در کامپوننت استفاده کن:**

\`\`\`ts
// با {Q22: React Query}
const { mutate } = useMutation({ mutationFn: createOrder })

// با {Q22: SWR}
const { data } = useSWR('/orders', getOrders)
\`\`\`

---

### ساختار services/

\`\`\`
src/services/
├── api.ts              ← axios instance (baseURL، interceptors، auth header)
├── authService.ts      ← login، logout، refresh token
└── {domain}Service.ts  ← هر domain جدید = یه فایل جدید
\`\`\`

### ساختار types/

\`\`\`
src/types/
├── api.ts              ← همه request/response interface‌ها
└── index.ts            ← re-export
\`\`\`

**قرارداد نامگذاری تایپ‌ها:**
- Request body: `Create{Entity}Payload` / `Update{Entity}Payload`
- Response: `{Entity}Response` / `{Entity}ListResponse`
- Shared model: `{Entity}` (بدون پسوند)

---

### Error handling

`src/services/api.ts` interceptor خطاها رو هندل می‌کنه:
- `401` → redirect به login
- `422` → validation errors رو برمی‌گردونه
- بقیه → toast عمومی نمایش می‌ده

## مستندات بیشتر

- `CLAUDE.md` — راهنمای کار با Claude در این پروژه
- `HANDOFF.md` — وضعیت فعلی و قدم بعدی
- `dev-knowledge/projects/{Q1}/project-context.md` — tokens و معماری
```
