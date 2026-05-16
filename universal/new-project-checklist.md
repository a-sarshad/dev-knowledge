# New Project Checklist
> updated: 2026-05-16

---

## ۱. Setup اولیه

```bash
pnpm create vite@latest project-name -- --template react-ts
cd project-name
pnpm install
```

---

## ۲. Packages

```bash
# Chakra UI v3
pnpm add @chakra-ui/react @emotion/react @emotion/styled

# Router + Data fetching
pnpm add react-router-dom @tanstack/react-query axios

# Icons
pnpm add lucide-react
```

---

## ۳. Font — Vazirmatn (RTL)

در `index.html`:
```html
<link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@100;300;400;500;600;700;800;900&display=swap" rel="stylesheet">
```

---

## ۴. RTL Setup

در `index.html`:
```html
<html dir="rtl" lang="fa">
```

---

## ۵. File Structure

```
src/
  components/layout/
    Layout.tsx
    Navbar.tsx
    Sidebar.tsx
    SidebarItem.tsx
    Header.tsx
    UserMenu.tsx
  contexts/
    ColorModeContext.tsx
  pages/
  theme/
    index.ts
    tokens.ts
  types/
  services/
```

---

## ۶. Theme Setup

```ts
// src/theme/index.ts
import { createSystem, defaultConfig, defineConfig } from '@chakra-ui/react'
import { projectTokens } from './tokens'

const layoutConfig = defineConfig({
  globalCss: {
    'html, body': {
      direction: 'rtl',
      fontFamily: 'body',
      bg: 'bg.subtle',
    },
  },
})

export const system = createSystem(defaultConfig, projectTokens, layoutConfig)
```

---

## ۷. main.tsx

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

## ۸. CLAUDE.md

از `dev-knowledge/design-systems/chakra-ui-v3/CLAUDE-template.md` کپی کن و customize کن.

---

## ۹. Git

```bash
git init
git add .
git commit -m "chore: initial setup"
gh repo create project-name --private
git push -u origin main
```
