# Research: Yahtzee Game Main Page

**Feature**: `001-game-main-page`
**Phase**: 0 — Research
**Date**: 2026-05-03

## Dice Roll Animation

**Decision**: Implement the rolling animation using MUI's `keyframes` helper from `@mui/system`
applied via the `styled()` utility on each `Die` component's root element. The keyframe applies
a rapid spin/shake for ~700 ms when the die enters a "rolling" state (a boolean prop), then
settles to display the new face value.

**Rationale**: Constitution Principle III prohibits raw CSS files, CSS modules, and inline `style`
objects. MUI's `keyframes` and `styled()` are part of MUI's approved styling system and produce
CSS-in-JS animations without any `.css` files. The animation runs client-side, well within the
≤1 second performance target.

**Alternatives Considered**:
- `@keyframes` in `.css` files: Prohibited by Principle III.
- `framer-motion` / `react-spring`: Not in the constitution's technology stack; would require a
  constitutional amendment. Excessive complexity for a single rolling animation.
- CSS Modules: Prohibited by Principle III.

## Client-Side Routing

**Decision**: Use `react-router-dom` v6 with `<BrowserRouter>`, `<Routes>`, and `<Route>`.
Three routes: `/` → `<GamePage>`, `/rules` → `<RulesPage>`, `/status` → `<StatusPage>`.
`<NavBar>` uses `<NavLink>` to automatically apply active styling to the current route.

**Rationale**: `react-router-dom` is the de facto standard for React SPA routing. The constitution
restricts UI component libraries to MUI and testing utilities to RTL + Vitest, but does not
prohibit routing infrastructure. The navigation requirement (FR-009, FR-010) cannot be satisfied
without a routing solution. `<NavLink>` makes active-link detection automatic.

**Alternatives Considered**:
- `@tanstack/router`: More powerful but significantly more complex; overkill for 3 routes.
- Hash-based manual routing with `window.location`: Poor UX; non-standard for modern SPAs.
- Next.js / Remix file-based routing: Changes the build infrastructure; not in the constitution.

## State Management

**Decision**: Use React `useContext` + `useReducer` to hold `GameState` in `App.tsx` and expose
it via a `GameStateContext`. `DiceArea` dispatches actions to roll dice and toggle die locks.
`ScoreTable` reads score categories. `DiceArea` also reads the roll count for the disabled-state
logic. The mock initial state is the starting value for the context.

**Rationale**: `DiceArea` and `ScoreTable` are siblings inside `GamePage`, both needing access
to the shared game state. A context is the correct, minimal solution for sharing state between
siblings without prop-drilling through `GamePage`. No external state library is needed for a
single screen of mocked data. Dispatching via a reducer makes state transitions explicit and
testable.

**Alternatives Considered**:
- Redux Toolkit: Overkill for a mocked, single-screen UI.
- Zustand / Jotai: Not in the constitution stack; require a constitutional amendment.
- Prop drilling `GameState` through `GamePage` to children: Functional but creates tight coupling
  and becomes unwieldy when `App.tsx` evolves to support Apollo Client queries.

## Standard Yahtzee Scoring Categories

**Decision**: Implement all 13 standard categories across two conventional sections.

**Upper Section** (6 categories):
| ID | Name | Score Rule |
|----|------|------------|
| ones | Ones | Sum of all 1s |
| twos | Twos | Sum of all 2s |
| threes | Threes | Sum of all 3s |
| fours | Fours | Sum of all 4s |
| fives | Fives | Sum of all 5s |
| sixes | Sixes | Sum of all 6s |

**Lower Section** (7 categories):
| ID | Name | Score Rule |
|----|------|------------|
| three-of-a-kind | Three of a Kind | Sum of all dice (if ≥3 match) |
| four-of-a-kind | Four of a Kind | Sum of all dice (if ≥4 match) |
| full-house | Full House | 25 points (3+2 match) |
| small-straight | Small Straight | 30 points (4 sequential) |
| large-straight | Large Straight | 40 points (5 sequential) |
| yahtzee | Yahtzee | 50 points (all 5 match) |
| chance | Chance | Sum of all dice (no conditions) |

**Note**: Upper Section bonus (+35 if total ≥ 63) and Yahtzee bonus rules are out of scope for
this feature — the score table displays static mocked values only.

## Mocked Game State Strategy

**Decision**: Define a single static typed object in `src/mocks/gameState.ts` as the initial
value for `GameStateContext`. It represents a mid-game snapshot: the player has taken one roll,
two Upper Section categories are already filled.

**Initial values**:
- Dice: `[3, 5, 3, 2, 3]` — three 3s (interesting starting hand)
- Turn: `rollsUsed: 1, maxRolls: 3` (2 rolls remaining)
- Score categories: Ones=1 (filled), Twos=0 (filled as 0, demonstrating a zero score), rest null

**Rationale**: A static predictable mock makes component tests deterministic. The three-3s hand
visually demonstrates the lock-and-re-roll strategy. Two filled categories demonstrate both
filled and unfilled states in the score table.
