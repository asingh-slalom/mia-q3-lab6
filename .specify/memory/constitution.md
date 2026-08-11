<!--
Sync Impact Report
Version change: TEMPLATE (unratified) → 1.0.0
Modified principles: N/A (initial ratification, all placeholders replaced)
Added sections:
  - Core Principles I-V (Code Quality & Consistency, Test-First & Comprehensive Coverage,
    User Experience Consistency, Simplicity & Scope Discipline, Reliability Through Error Handling)
  - Technology Stack & Architecture Constraints
  - Development Workflow & Quality Gates
  - Governance
Removed sections: None
Templates requiring updates:
  - .specify/templates/plan-template.md ⚠ pending manual review (verify Constitution Check gates
    reference these principles)
  - .specify/templates/spec-template.md ✅ no principle-specific references to update
  - .specify/templates/tasks-template.md ✅ no principle-specific references to update
Follow-up TODOs: None
-->

# Todo App Constitution

## Core Principles

### I. Code Quality & Consistency
All code MUST follow the naming, formatting, and organization rules in
[coding-guidelines.md](../../docs/coding-guidelines.md): `camelCase` for variables/functions,
`PascalCase` for components and classes, `UPPER_SNAKE_CASE` for constants, 2-space indentation,
and the mandated import order (external → internal → styles). Code MUST adhere to DRY, KISS, and
SOLID principles — duplicated logic is extracted into shared utilities, functions do one thing,
and components depend on minimal, focused interfaces. Comments explain *why*, not *what*; obvious
comments are prohibited. No linting errors or `console.log` statements may reach a pull request.
Rationale: shared conventions keep a multi-package (frontend/backend) monorepo readable and let
any contributor navigate unfamiliar code quickly.

### II. Test-First & Comprehensive Coverage (NON-NEGOTIABLE)
Every new feature or bug fix MUST be accompanied by tests colocated in `__tests__/` directories
per [testing-guidelines.md](../../docs/testing-guidelines.md). Tests verify behavior, not
implementation, follow the Arrange-Act-Assert pattern, and remain independent (no shared state,
all external dependencies mocked). The project targets 80%+ coverage across packages, with
critical user workflows (create, view, complete, edit, delete todo) held to 100% coverage. Bug
fixes MUST include a regression test written before the fix. Tests MUST pass locally before a pull
request is opened. Rationale: the app's value is its reliability for core task-tracking workflows;
untested changes risk silent regressions in a small, actively evolving codebase.

### III. User Experience Consistency
All UI work MUST conform to [ui-guidelines.md](../../docs/ui-guidelines.md): the defined color
palette and typography scale for both light and dark modes, the 8px spacing grid, and existing
component patterns (todo cards, confirmation dialogs, inline forms). Destructive actions (e.g.,
delete) MUST prompt a confirmation dialog before executing. Interactive elements MUST be keyboard
accessible, meet WCAG AA contrast, and expose descriptive labels/aria-attributes. New UI MUST NOT
introduce animation/motion beyond what guidelines allow, and MUST preserve the single-column,
desktop-focused layout. Rationale: a consistent, accessible design system prevents visual and
interaction drift as multiple contributors add features.

### IV. Simplicity & Scope Discipline
The application scope is bounded by [functional-requirements.md](../../docs/functional-requirements.md):
a single-user todo list with create, view, complete/incomplete, edit, and delete operations,
backed by the existing Express API. Features explicitly out of scope (auth, multi-user support,
priorities/categories, recurring todos, reminders, undo/redo, bulk operations, search/filtering,
mobile-specific optimization) MUST NOT be implemented without a corresponding update to the
functional requirements and this constitution. When a design choice has multiple viable
approaches, the simplest one that satisfies the requirement MUST be chosen (KISS/YAGNI).
Rationale: an unbounded scope undermines the app's purpose as a focused, learnable bootcamp
reference implementation.

### V. Reliability Through Error Handling
Operations that can fail (API calls, persistence, data parsing) MUST be wrapped in explicit error
handling that surfaces a meaningful, user-facing message and logs the underlying error for
debugging. Silent failures are prohibited. User-initiated actions (create, update, delete) MUST
give the user feedback on success or failure. Rationale: as a single-user app with no
authentication safety net, clear error feedback is the primary way users trust the app persisted
their data correctly.

## Technology Stack & Architecture Constraints

The project is an npm-workspaces monorepo with two packages, per
[project-overview.md](../../docs/project-overview.md): `packages/frontend` (React + Jest) and
`packages/backend` (Node.js/Express + Jest). Node.js v16+ and npm v7+ are the minimum supported
versions. New functionality MUST fit within this frontend/backend split — the frontend calls the
backend's REST API for all persistence; it MUST NOT embed its own persistence mechanism. Cross-package
code sharing MUST go through each package's existing `services/` layer rather than ad hoc imports
across package boundaries.

## Development Workflow & Quality Gates

Contributors work on feature branches (e.g., `feature/<name>`) and submit pull requests for review;
direct commits to the default branch are avoided. Commits MUST be atomic and use descriptive
messages explaining the "why". Before opening a pull request, contributors MUST: run the full test
suite (`npm test`) and confirm all tests pass, run the linter and resolve all errors/warnings, and
verify their change against the Code Review Checklist in
[coding-guidelines.md](../../docs/coding-guidelines.md). Pull requests MUST NOT be merged with
failing tests or unresolved linter errors.

## Governance

This constitution supersedes ad hoc conventions and prior undocumented practices. All pull
requests and code reviews MUST verify compliance with the principles above; any deviation MUST be
explicitly justified in the pull request description and, if it represents a recurring need, MUST
trigger a constitution amendment rather than a silent exception.

Amendments are made by editing this file and following semantic versioning: MAJOR for backward
incompatible governance or principle removals/redefinitions, MINOR for new principles or materially
expanded guidance, PATCH for clarifications and wording fixes. Each amendment updates the Sync
Impact Report at the top of this file and the `Last Amended` date below. Complexity or scope
expansion beyond `Simplicity & Scope Discipline` MUST be justified in the relevant spec/plan before
implementation begins.

**Version**: 1.0.0 | **Ratified**: 2026-08-11 | **Last Amended**: 2026-08-11
