# Chakra UI v3 — Setup Checklist
> updated: 2026-05-17

پیش‌نیاز: مراحل generic در `universal/new-project-checklist.md` انجام شده باشن.

---

## ۱. Install

```bash
pnpm add @chakra-ui/react @emotion/react @emotion/styled
```

---

## ۲. File Structure (Chakra-specific additions)

```
src/
  contexts/
    ColorModeContext.tsx   ← dark mode toggle
  theme/
    index.ts               ← createSystem entry
    tokens.ts              ← custom tokens
```

---

## ۳. Theme Setup

```ts
// src/theme/index.ts
import { createSystem, defaultConfig, defineConfig } from '@chakra-ui/react'
import { projectTokens } from './tokens'

const layoutConfig = defineConfig({
  globalCss: {
    'html, body': {
      direction: 'rtl',
      fontFamily: 'body',
    },
  },
})

export const system = createSystem(defaultConfig, projectTokens, layoutConfig)
```

---

## ۴. main.tsx

```tsx
import { ChakraProvider, LocaleProvider } from '@chakra-ui/react'
import { system } from './theme'
import { ColorModeProvider } from './contexts/ColorModeContext'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <ChakraProvider value={system}>
      <LocaleProvider locale="fa-IR">
        <ColorModeProvider>
          <QueryClientProvider client={queryClient}>
            <App />
          </QueryClientProvider>
        </ColorModeProvider>
      </LocaleProvider>
    </ChakraProvider>
  </StrictMode>,
)
```

---

## ۵. CLAUDE.md

از `dev-knowledge/design-systems/chakra-ui-v3/CLAUDE-template.md` کپی کن و customize کن.

---

## ۶. Known Issues مهم

قبل از شروع کد نویسی حتماً بخوان:
→ `known-bugs.md` — باگ‌های رایج Chakra v3 (lineHeight، bg.default، useColorMode و...)
