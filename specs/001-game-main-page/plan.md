# Implementation Plan: Yahtzee Game Main Page

**Branch**: `001-game-main-page` | **Date**: 2026-05-03 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `specs/001-game-main-page/spec.md`

## Summary

Build the main Yahtzee game page as a React SPA: five interactive, animated dice on the left
(per-die lock toggling and a Roll button), a full 13-category score table on the right, and a
top navigation bar routing between Game, Rules, and Status pages. All game state is mocked
locally via React context; no backend calls are made in this feature. Future backend integration
will use Apollo Client over GraphQL as mandated by the project constitution.

## Technical Context

**Language/Version**: TypeScript 5.x (strict mode)
**Primary Dependencies**: React 18, Material UI (MUI) v6, Vite 6,
  Apollo Client 3 (installed but inactive — mocked state only for this feature),
  react-router-dom v6
**Storage**: N/A — game state held in React context; no local or remote persistence
**Testing**: Vitest + React Testing Library (RTL)
**Target Platform**: Desktop web browser (Chrome, Firefox, Edge — latest stable)
**Project Type**: Single-page web application (SPA)
**Performance Goals**: Dice roll animation ≤ 1 second; client-side navigation ≤ 100 ms
**Constraints**: Desktop-only; mocked state only (no backend calls); scoring logic out of scope
**Scale/Scope**: 1 interactive page + 2 placeholder pages; 7 React components; single player

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| # | Principle | Status | Notes |
|---|-----------|--------|-------|
| I | Frontend-Only Scope | ✅ PASS | All game state is mocked locally in React context. Apollo Client is installed but no queries/mutations are called. No backend calls are made. |
| II | Component Isolation | ✅ PASS | NavBar, DiceArea, Die, ScoreTable, GamePage, RulesPage, and StatusPage each reside in their own file under `src/components/`. No inline components. |
| III | Material UI as Sole UI Framework | ✅ PASS | All UI built with MUI components. Styling via `sx` prop or `styled()` only. No raw CSS files, CSS modules, or inline `style` attribute objects. |
| IV | Mandatory Component Testing | ✅ PASS | All 7 components will have RTL + Vitest tests written post-implementation. Tests placed alongside component files. |
| V | TypeScript Strictness | ✅ PASS | All source files are `.tsx` / `.ts`. TypeScript strict mode enabled. No `any` types. |

Post-Phase-1 re-check: ✅ No violations introduced by design artifacts.

## Project Structure

### Documentation (this feature)

```text
specs/001-game-main-page/
├── plan.md                            # This file
├── research.md                        # Phase 0 output
├── data-model.md                      # Phase 1 output
├── quickstart.md                      # Phase 1 output
├── contracts/
│   └── future-graphql-operations.md  # Phase 1 output — future Apollo Client operations
└── tasks.md                           # Phase 2 output (/speckit-tasks command)
```

### Source Code (repository root)

```text
src/
├── components/
│   ├── NavBar.tsx        # Top navigation bar (Game | Rules | Status links)
│   ├── GamePage.tsx      # Main game layout — left/right two-column split
│   ├── DiceArea.tsx      # Left column: five dice + Roll button + rolls-remaining indicator
│   ├── Die.tsx           # Individual die: face value display, roll animation, lock toggle
│   ├── ScoreTable.tsx    # Right column: 13-category score table (Upper + Lower sections)
│   ├── RulesPage.tsx     # Placeholder Rules page
│   └── StatusPage.tsx    # Placeholder Status page
├── mocks/
│   └── gameState.ts      # Static typed mock: initial dice, turn state, score categories
├── types/
│   └── game.ts           # TypeScript interfaces: DieValue, Die, Turn, ScoreCategory, GameState
├── App.tsx               # Root component: GameStateContext + BrowserRouter + route definitions
└── main.tsx              # Vite entry point — renders <App /> into DOM
```

**Structure Decision**: Single-project SPA. All React components live under `src/components/`
per constitution Principle II. Non-component source (types, mocks) lives in co-located
subdirectories under `src/`. `App.tsx` is the context and router host at the root of `src/`.
`main.tsx` is the Vite entry-point renderer (not itself a React component) at the root of `src/`.
