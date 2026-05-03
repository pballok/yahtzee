<!--
SYNC IMPACT REPORT
==================
Version change: 1.0.0 → 1.1.0 (MINOR — Apollo Client and GraphQL added to Technology Stack
and Development Standards as mandatory, non-negotiable technology choices)

Modified principles: None (Principles I–V unchanged)

Added sections: N/A

Removed sections: N/A

Technology Stack updates:
  - Added: Apollo Client (latest stable) — GraphQL client for all backend communication

Development Standards updates:
  - Added: All backend communication MUST use Apollo Client GraphQL operations; direct
    fetch/REST calls for game state are prohibited.

Templates reviewed:
  - .specify/templates/plan-template.md   ✅ No changes required — Technical Context section
                                           captures dependencies generically; planners will
                                           reference Apollo Client per the updated constitution.
  - .specify/templates/spec-template.md   ✅ No changes required — generic structure compatible.
  - .specify/templates/tasks-template.md  ✅ No changes required — Foundational phase tasks will
                                           include Apollo Client setup per constitution guidance.
  - .specify/templates/commands/          ✅ No command files found; nothing to update.

Deferred TODOs: None
-->

# Yahtzee Constitution

## Core Principles

### I. Frontend-Only Scope

This project MUST implement only the frontend of the Yahtzee game as a single-page application.
The backend is out of scope for this repository. The frontend MUST NOT implement any game state
persistence logic — state management and persistence are the exclusive responsibility of the
backend service. All game state MUST be fetched from or committed to the backend via GraphQL
operations through Apollo Client.

### II. Component Isolation

Every React component MUST reside in its own dedicated file under the `src/components/` directory.
No component may be defined inline within another component's file or in non-component source files.
This rule is absolute; no exceptions for "simple" or "shared" helper components.

### III. Material UI as the Sole UI Framework

All UI elements MUST be built using the latest stable version of Google's Material UI (MUI).
Raw CSS files, CSS modules, and alternative component/styling libraries are prohibited.
Styling MUST follow MUI's in-code best practices as documented in the official MUI documentation —
primarily the `sx` prop or the `styled()` utility. Raw inline `style` attribute objects MUST NOT
be used as a substitute for MUI's styling system.

### IV. Mandatory Component Testing (No TDD)

All React components MUST have corresponding tests written with React Testing Library (RTL) and
Vitest. Test-Driven Development (TDD) is explicitly not followed; tests are written after the
component is implemented, not before. Components without test coverage MUST NOT be merged. Tests
SHOULD be placed alongside the component file they cover.

### V. TypeScript Strictness

All source code MUST be written in TypeScript. JavaScript (`.js`) files are prohibited in `src/`.
Type safety MUST be maintained throughout; `any` types MUST be avoided unless no viable typed
alternative exists, in which case the usage MUST be accompanied by an explanatory comment.

## Technology Stack

The following technology choices are non-negotiable for this project:

- **Bundler / Dev Server**: Vite (latest stable)
- **Language**: TypeScript
- **UI Library**: React
- **Component Library**: Material UI (MUI) — latest stable version
- **API Client**: Apollo Client (latest stable) — all backend communication via GraphQL
- **Test Runner**: Vitest
- **Testing Utilities**: React Testing Library (RTL)

Deviations from this stack require a constitutional amendment before any implementation begins.

## Development Standards

- React components MUST be placed in `src/components/`, one component per file.
- All backend communication MUST use Apollo Client GraphQL operations (queries, mutations,
  subscriptions). Direct `fetch` or REST calls for game state are prohibited.
- The frontend communicates with the backend exclusively via GraphQL; local persistence mechanisms
  (localStorage, IndexedDB, cookies used for state) are prohibited in the frontend.
- Styling MUST use MUI's `sx` prop or `styled()` utility — not raw CSS files or inline `style`
  objects.
- All React components MUST be covered by RTL + Vitest tests before a feature is considered done.
- Implementation plans MUST reference the Technology Stack section and verify compliance with all
  five Core Principles before work begins (Constitution Check gate in plan.md).

## Governance

This constitution supersedes all other development guidelines for this project. Amendments require:

1. Documenting the proposed change and rationale.
2. Verifying that no existing working features are broken by the amendment.
3. Updating `LAST_AMENDED_DATE` and incrementing `CONSTITUTION_VERSION` per semantic versioning:
   - MAJOR: backward-incompatible principle removal or redefinition.
   - MINOR: new principle or section added or materially expanded.
   - PATCH: clarifications, wording fixes, non-semantic refinements.

All implementation plans and feature specs MUST reference and comply with this constitution.

**Version**: 1.1.0 | **Ratified**: 2026-04-25 | **Last Amended**: 2026-05-03
