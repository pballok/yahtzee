<!--
SYNC IMPACT REPORT
==================
Version change: (template) → 1.0.0 (initial ratification)

Added sections:
  - Core Principles (5 principles)
  - Technology Stack
  - Development Standards
  - Governance

Modified principles: N/A (new constitution)
Removed sections: N/A (new constitution)

Templates reviewed:
  - .specify/templates/plan-template.md   ✅ No changes required — Constitution Check gate is a per-feature placeholder
  - .specify/templates/spec-template.md   ✅ No changes required — generic structure compatible
  - .specify/templates/tasks-template.md  ⚠️ Minor conflict: comment states tests are OPTIONAL; Principle IV mandates
                                            component testing for all React components. Updated comment to reflect
                                            that tests are mandatory per constitution.

Deferred TODOs: None
-->

# Yahtzee Constitution

## Core Principles

### I. Frontend-Only Scope

This project MUST implement only the frontend of the Yahtzee game as a single-page application.
The backend is out of scope for this repository. The frontend MUST NOT implement any game state
persistence logic — state management and persistence are the exclusive responsibility of the
backend service. All game state MUST be fetched from or committed to the backend API.

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
- **Test Runner**: Vitest
- **Testing Utilities**: React Testing Library (RTL)

Deviations from this stack require a constitutional amendment before any implementation begins.

## Development Standards

- React components MUST be placed in `src/components/`, one component per file.
- The frontend communicates with a backend API for all game state; local persistence mechanisms
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

**Version**: 1.0.0 | **Ratified**: 2026-04-25 | **Last Amended**: 2026-04-25
