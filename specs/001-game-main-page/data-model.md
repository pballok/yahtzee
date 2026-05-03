# Data Model: Yahtzee Game Main Page

**Feature**: `001-game-main-page`
**Phase**: 1 — Design
**Date**: 2026-05-03
**Source file**: `src/types/game.ts`

## TypeScript Interfaces

### DieValue

```typescript
type DieValue = 1 | 2 | 3 | 4 | 5 | 6;
```

A union type constraining die face values to valid Yahtzee values.

### Die

```typescript
interface Die {
  id: number;      // 0–4, stable identity for React keys and lock toggling
  value: DieValue; // current face value shown to the player
  locked: boolean; // true = excluded from the next roll
}
```

### Turn

```typescript
interface Turn {
  rollsUsed: number; // increments each time the Roll button is pressed (0–3)
  maxRolls: 3;       // literal type; always 3 in standard Yahtzee
}
```

Derived values (computed by components, not stored):

```typescript
const rollsRemaining = turn.maxRolls - turn.rollsUsed; // 0, 1, 2, or 3
const canRoll = rollsRemaining > 0 && dice.some(d => !d.locked);
```

### ScoreSection

```typescript
type ScoreSection = 'upper' | 'lower';
```

### ScoreCategory

```typescript
interface ScoreCategory {
  id: string;          // stable key, e.g. 'ones', 'full-house', 'yahtzee'
  name: string;        // display label, e.g. 'Ones', 'Full House', 'Yahtzee'
  section: ScoreSection;
  score: number | null; // null = unfilled slot; number = claimed score (including 0)
}
```

### GameState

```typescript
interface GameState {
  dice: Die[];                       // exactly 5 elements, ordered 0–4
  turn: Turn;
  scoreCategories: ScoreCategory[];  // exactly 13 elements, ordered Upper then Lower
}
```

### GameStateAction (reducer actions)

```typescript
type GameStateAction =
  | { type: 'ROLL_DICE' }
  | { type: 'TOGGLE_DIE_LOCK'; dieId: number };
```

`ROLL_DICE`: Replaces the `value` of all unlocked dice with new random values and increments
`turn.rollsUsed`. No-op if `canRoll` is false.

`TOGGLE_DIE_LOCK`: Flips `locked` on the die with the given `dieId`.

## State Transitions

```
Initial state: mock from src/mocks/gameState.ts

TOGGLE_DIE_LOCK (dieId):
  die[dieId].locked = !die[dieId].locked

ROLL_DICE (guard: canRoll):
  for each die where !locked:
    die.value = random(1..6)
  turn.rollsUsed += 1
```

## Score Category Catalogue

All 13 entries in display order (matches ScoreTable rendering order):

| ID | Name | Section | Initial mock score |
|----|------|---------|--------------------|
| ones | Ones | upper | 1 |
| twos | Twos | upper | 0 |
| threes | Threes | upper | null |
| fours | Fours | upper | null |
| fives | Fives | upper | null |
| sixes | Sixes | upper | null |
| three-of-a-kind | Three of a Kind | lower | null |
| four-of-a-kind | Four of a Kind | lower | null |
| full-house | Full House | lower | null |
| small-straight | Small Straight | lower | null |
| large-straight | Large Straight | lower | null |
| yahtzee | Yahtzee | lower | null |
| chance | Chance | lower | null |

## Mock Initial State

Defined in `src/mocks/gameState.ts`:

```typescript
import { GameState } from '../types/game';

export const initialGameState: GameState = {
  dice: [
    { id: 0, value: 3, locked: false },
    { id: 1, value: 5, locked: false },
    { id: 2, value: 3, locked: false },
    { id: 3, value: 2, locked: false },
    { id: 4, value: 3, locked: false },
  ],
  turn: { rollsUsed: 1, maxRolls: 3 },
  scoreCategories: [
    { id: 'ones',            name: 'Ones',            section: 'upper', score: 1    },
    { id: 'twos',            name: 'Twos',            section: 'upper', score: 0    },
    { id: 'threes',          name: 'Threes',          section: 'upper', score: null },
    { id: 'fours',           name: 'Fours',           section: 'upper', score: null },
    { id: 'fives',           name: 'Fives',           section: 'upper', score: null },
    { id: 'sixes',           name: 'Sixes',           section: 'upper', score: null },
    { id: 'three-of-a-kind', name: 'Three of a Kind', section: 'lower', score: null },
    { id: 'four-of-a-kind',  name: 'Four of a Kind',  section: 'lower', score: null },
    { id: 'full-house',      name: 'Full House',      section: 'lower', score: null },
    { id: 'small-straight',  name: 'Small Straight',  section: 'lower', score: null },
    { id: 'large-straight',  name: 'Large Straight',  section: 'lower', score: null },
    { id: 'yahtzee',         name: 'Yahtzee',         section: 'lower', score: null },
    { id: 'chance',          name: 'Chance',          section: 'lower', score: null },
  ],
};
```
