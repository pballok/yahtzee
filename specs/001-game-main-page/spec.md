# Feature Specification: Yahtzee Game Main Page

**Feature Branch**: `001-game-main-page`
**Created**: 2026-05-03
**Status**: Draft
**Input**: User description: "I am building the frontend part of the dice game called Yahtzee. The main part of the page is divided into two sections appearing side-by-side. The left section will be showing five animated dice, that the player can interact with by rolling all or only some of them to get as high score as possible. On the right side, there should be a table showing the current scores of the player. The part with the dice allows the player to lock dice individually as they wish, and then re-roll the unlocked dice with the press of a button. Above the main part there should be a horizontal navigation bar that allows to navigate to the page with the game rules, the page with game status. The game state is mocked for now, no need to make actual calls to the backend yet."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Rolling and Locking Dice (Priority: P1)

A player opens the game page and sees five dice displayed in the left section. They press the
Roll button to roll all dice for the first time. After seeing the result, they click on individual
dice to lock the ones they want to keep. They press Roll again to re-roll only the unlocked dice.
They repeat this process up to 3 rolls total per turn, locking dice strategically to maximize
their score.

**Why this priority**: This is the core gameplay interaction. Without this mechanic, the game
cannot be played. Everything else supports this action.

**Independent Test**: A tester can open the page, roll dice, lock/unlock individual dice, and
observe that re-rolling only changes the unlocked dice — all without any other feature being
complete. The mocked game state ensures dice values and roll counts are available.

**Acceptance Scenarios**:

1. **Given** the page has loaded with mocked game state, **When** the player presses the Roll
   button, **Then** all five unlocked dice animate and display new random face values (1–6).
2. **Given** the player has rolled at least once, **When** they click a die, **Then** the die
   toggles between locked and unlocked states with a clear visual indicator of the current state.
3. **Given** the player has locked some dice and clicks Roll, **When** the roll animation
   completes, **Then** only the unlocked dice show new values; locked dice retain their previous
   values.
4. **Given** the player has used all 3 rolls in a turn, **When** they view the Roll button,
   **Then** the Roll button is disabled and no further rolls are possible until the next turn
   begins.
5. **Given** all five dice are locked, **When** the player views the Roll button, **Then** the
   Roll button is disabled.

---

### User Story 2 - Viewing the Score Table (Priority: P2)

A player can see the full Yahtzee score table on the right side of the page. The table shows all
standard scoring categories (both the Upper Section and Lower Section), along with their current
values or an indicator that they have not yet been filled. The player can use this table to plan
their strategy across turns.

**Why this priority**: The score table is essential context for decision-making during dice
rolling. Without it, the player has no basis for choosing which dice to lock. It is, however,
readable without the dice interaction being functional, making it independently deliverable.

**Independent Test**: A tester can load the page with mocked score data and verify that all
standard Yahtzee categories appear in the table with their current values or a placeholder for
unfilled categories — without requiring any dice interaction.

**Acceptance Scenarios**:

1. **Given** the page has loaded with mocked game state, **When** the player views the right
   section, **Then** a score table displays all standard Yahtzee scoring categories in their
   conventional groupings (Upper Section and Lower Section).
2. **Given** the mocked game state includes some filled and some unfilled categories, **When**
   the player views the score table, **Then** filled categories show their score values and
   unfilled categories show a dash or equivalent placeholder indicating availability.
3. **Given** the player is on a standard desktop screen, **When** the score table is displayed,
   **Then** all scoring categories are visible without requiring vertical scrolling.

---

### User Story 3 - Navigating the Application (Priority: P3)

A player can use a horizontal navigation bar at the top of the page to move between the main
game page, the game rules page, and the game status page. The navigation bar is always visible
and makes it clear which page the player is currently on.

**Why this priority**: Navigation enables access to supporting information (rules, status) but
is not required to play the game. It delivers standalone value as a routing skeleton that can be
completed independently of game logic.

**Independent Test**: A tester can click each navigation link and confirm that routing changes
to the corresponding page, which may contain placeholder content. The active page link is visually
highlighted.

**Acceptance Scenarios**:

1. **Given** the player is on any page, **When** they view the top of the page, **Then** a
   horizontal navigation bar is visible containing links to the Game page, the Rules page, and
   the Status page.
2. **Given** the player is currently on the main game page, **When** they view the navigation
   bar, **Then** the Game link is visually distinguished as active.
3. **Given** the player clicks the Rules link, **When** navigation completes, **Then** the Rules
   page is displayed and the Rules link is shown as active in the navigation bar.
4. **Given** the player clicks the Status link, **When** navigation completes, **Then** the Status
   page is displayed and the Status link is shown as active in the navigation bar.

---

### Edge Cases

- What happens when all five dice are locked and the player attempts to roll?
  The Roll button MUST be disabled; no roll action is triggered.
- What happens when the player has exhausted all 3 rolls for the turn?
  The Roll button MUST be disabled for the remainder of the turn.
- What happens if the player navigates away from the game page mid-turn?
  The mocked game state resets to its initial values on return (no persistence required).
- What happens on the first roll of a turn?
  All five dice start unlocked and can be rolled freely.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The main game page MUST display five dice in the left section, each showing a face
  value from 1 to 6.
- **FR-002**: Each die MUST animate when it is rolled, providing clear visual feedback that a roll
  occurred.
- **FR-003**: Players MUST be able to click any individual die to toggle its locked state; a locked
  die is visually distinguished from an unlocked die.
- **FR-004**: Players MUST be able to press a Roll button to roll all currently unlocked dice; locked
  dice MUST retain their current face values.
- **FR-005**: The Roll button MUST be disabled when all dice are locked or the player has used all
  3 allowed rolls in the current turn.
- **FR-006**: The page MUST display a counter or indicator showing the number of rolls remaining
  in the current turn.
- **FR-007**: The right section MUST display a score table containing all standard Yahtzee scoring
  categories: Ones, Twos, Threes, Fours, Fives, Sixes (Upper Section) and Three of a Kind, Four
  of a Kind, Full House, Small Straight, Large Straight, Yahtzee, and Chance (Lower Section).
- **FR-008**: The score table MUST show the current score value for filled categories and a clear
  placeholder (e.g., a dash) for unfilled categories.
- **FR-009**: A horizontal navigation bar MUST appear above the main content area and contain
  links to the Game page, the Rules page, and the Status page.
- **FR-010**: The navigation bar MUST visually highlight the currently active page link.
- **FR-011**: All game state displayed on the page MUST be sourced from mocked data; no real
  backend API calls are made.
- **FR-012**: The left (dice) and right (score table) sections MUST be displayed side by side on
  the main game page.

### Key Entities

- **Die**: Represents one of the five game dice. Has a current face value (1–6) and a locked
  boolean state. Locked dice are excluded from the next roll.
- **Turn**: Tracks the current turn's roll count (maximum 3 rolls). Resets at the start of each
  new turn.
- **ScoreCategory**: A named scoring slot in the Yahtzee score table. Has a name, a grouping
  (Upper or Lower Section), an optional filled score value, and a boolean indicating whether it
  has been claimed.
- **GameState**: The top-level mocked state containing the current five dice, the active turn, and
  the list of score categories with their current values.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A player can complete a full 3-roll turn — rolling, locking dice, and re-rolling —
  in under 30 seconds on a standard desktop browser.
- **SC-002**: Dice roll animations complete within 1 second, ensuring the page feels immediately
  responsive to player interaction.
- **SC-003**: All 13 Yahtzee scoring categories are visible simultaneously in the score table
  without requiring the player to scroll vertically on a 1080p display.
- **SC-004**: Navigation between the Game, Rules, and Status pages completes without a full page
  reload, and the active link updates immediately.
- **SC-005**: The locked/unlocked state of a die is unambiguous — 95% of first-time users can
  correctly identify which dice are locked by visual inspection alone.

## Assumptions

- Standard Yahtzee rules apply: players have 3 rolls per turn and 13 scoring categories to fill.
- The game is single-player; multiplayer support is out of scope.
- The Rules page and Status page display placeholder content only; their full content is out of
  scope for this feature.
- Desktop browser is the primary target platform; mobile responsiveness is out of scope for this
  version.
- Mocked game state provides a fixed initial set of dice values, roll count, and partial score
  table entries sufficient to visually demonstrate the full UI.
- Scoring logic (calculating which categories are eligible and their point values) is out of
  scope; the score table displays static mocked values only.
- The player begins each session at the start of a fresh turn with all dice unlocked and 3 rolls
  available.
