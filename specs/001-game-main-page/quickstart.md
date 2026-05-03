# Quickstart: Yahtzee Game Main Page

**Feature**: `001-game-main-page`
**Date**: 2026-05-03

## Prerequisites

- Node.js 20+ and npm 10+
- Git (already initialised — branch `001-game-main-page`)

## 1. Initialise the Vite Project

From the repository root (if `package.json` does not yet exist):

```bash
npm create vite@latest . -- --template react-ts
```

Accept any overwrite prompts. This scaffolds `src/`, `index.html`, `vite.config.ts`,
`tsconfig.json`, and `package.json`.

## 2. Install Dependencies

```bash
# MUI (UI component library + icon set)
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material

# Client-side routing
npm install react-router-dom

# Apollo Client + GraphQL (installed now, activated in a future feature)
npm install @apollo/client graphql

# Test dependencies
npm install --save-dev vitest @testing-library/react @testing-library/user-event \
  @testing-library/jest-dom jsdom
```

## 3. Configure TypeScript (strict mode)

Ensure `tsconfig.json` contains:

```json
{
  "compilerOptions": {
    "strict": true,
    "jsx": "react-jsx"
  }
}
```

## 4. Configure Vitest

Update `vite.config.ts`:

```typescript
/// <reference types="vitest" />
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './src/test-setup.ts',
  },
})
```

Create `src/test-setup.ts`:

```typescript
import '@testing-library/jest-dom'
```

Add test script to `package.json`:

```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run"
  }
}
```

## 5. Create Source Structure

```bash
mkdir -p src/components src/mocks src/types
```

Implement files in this order (see `data-model.md` for interfaces):

1. `src/types/game.ts` — TypeScript interfaces
2. `src/mocks/gameState.ts` — mock initial state
3. `src/components/NavBar.tsx`
4. `src/components/Die.tsx`
5. `src/components/DiceArea.tsx`
6. `src/components/ScoreTable.tsx`
7. `src/components/GamePage.tsx`
8. `src/components/RulesPage.tsx`
9. `src/components/StatusPage.tsx`
10. `src/App.tsx` — context + router + routes
11. `src/main.tsx` — entry point

## 6. Start the Dev Server

```bash
npm run dev
```

Open `http://localhost:5173`.

## 7. Validate the Feature

Work through each acceptance scenario in `spec.md`:

- [ ] Five dice are visible in the left section with face values from the mock state.
- [ ] Pressing Roll animates all five dice and shows new values (2 rolls remaining after first click).
- [ ] Clicking a die shows it as locked with a clear visual distinction.
- [ ] Pressing Roll with some locked dice only changes unlocked ones.
- [ ] Roll button disables after 3 rolls.
- [ ] Roll button disables when all dice are locked.
- [ ] Score table on the right shows all 13 categories; Ones=1, Twos=0, rest showing `—`.
- [ ] All 13 categories visible without vertical scrolling on a 1080p screen.
- [ ] Navigation bar shows Game, Rules, Status links; Game link is active on load.
- [ ] Clicking Rules navigates to `/rules`; Rules link becomes active.
- [ ] Clicking Status navigates to `/status`; Status link becomes active.

## 8. Run Tests

```bash
npm run test:run
```

All component tests MUST pass before the feature is considered done (Principle IV).
