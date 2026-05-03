# Future GraphQL Operations: Yahtzee Game Main Page

**Feature**: `001-game-main-page`
**Phase**: 1 — Design
**Date**: 2026-05-03
**Status**: Placeholder — mocked in current feature; will replace mock when backend is ready

The project constitution mandates Apollo Client for all backend communication over GraphQL.
This document defines the GraphQL operations that will replace the mocked `GameStateContext`
initial state in a future feature. No operations are called in this feature.

## Schema Types (anticipated)

```graphql
enum ScoreSection {
  UPPER
  LOWER
}

type Die {
  id: Int!
  value: Int!    # 1–6
  locked: Boolean!
}

type Turn {
  rollsUsed: Int!
  maxRolls: Int!
}

type ScoreCategory {
  id: String!
  name: String!
  section: ScoreSection!
  score: Int          # null = unfilled
}

type GameState {
  dice: [Die!]!
  turn: Turn!
  scoreCategories: [ScoreCategory!]!
}
```

## Queries

### GetGameState

Fetches the current game state for the active session.

```graphql
query GetGameState {
  gameState {
    dice {
      id
      value
      locked
    }
    turn {
      rollsUsed
      maxRolls
    }
    scoreCategories {
      id
      name
      section
      score
    }
  }
}
```

**Current replacement**: `initialGameState` from `src/mocks/gameState.ts`

## Mutations

### RollDice

Re-rolls all currently unlocked dice and returns the updated game state.

```graphql
mutation RollDice {
  rollDice {
    dice {
      id
      value
      locked
    }
    turn {
      rollsUsed
      maxRolls
    }
  }
}
```

**Current replacement**: `ROLL_DICE` action dispatched to `useReducer` in `App.tsx`

### ToggleDieLock

Toggles the lock state of a single die.

```graphql
mutation ToggleDieLock($dieId: Int!) {
  toggleDieLock(dieId: $dieId) {
    id
    locked
  }
}
```

**Current replacement**: `TOGGLE_DIE_LOCK` action dispatched to `useReducer` in `App.tsx`

## Migration Path

When the backend GraphQL API is available:

1. Install and configure Apollo Client in `App.tsx` with the backend URI.
2. Replace `initialGameState` mock with a `useQuery(GET_GAME_STATE)` call.
3. Replace `ROLL_DICE` reducer action with a `useMutation(ROLL_DICE)` call.
4. Replace `TOGGLE_DIE_LOCK` reducer action with a `useMutation(TOGGLE_DIE_LOCK)` call.
5. Remove `src/mocks/gameState.ts` and the `useReducer` in `App.tsx`.
6. The `GameStateContext` shape (`GameState` interface) remains unchanged — components
   are unaffected by the migration.
