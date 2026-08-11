# Feature Specification: Support for Overdue Todo Items

**Feature Branch**: `001-overdue-todo-items`

**Created**: 2026-08-11

**Status**: Draft

**Input**: User description: "# Support for Overdue Todo Items

## User Story

**As a** todo application user
**I want to** easily identify and distinguish overdue tasks in my todo list
**So that** I can prioritize my work and quickly see which tasks are past their due date

## Description

Users need a clear, visual way to identify which todos have not been completed by their due date. This helps users quickly spot overdue items without having to manually check dates against today's date."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Spot overdue todos at a glance (Priority: P1)

As a user viewing my todo list, I want incomplete todos whose due date has passed to be visually distinguished from other todos, so I can immediately tell which tasks need my attention without comparing each due date to today's date myself.

**Why this priority**: This is the core value of the feature - without visual distinction, the feature delivers no benefit. This alone is a complete, shippable improvement.

**Independent Test**: Can be fully tested by creating a todo with a due date in the past and confirming it is visually marked as overdue in the todo list, while a todo with a future or no due date is not marked.

**Acceptance Scenarios**:

1. **Given** an incomplete todo with a due date earlier than today, **When** the todo list is displayed, **Then** that todo is visually marked as overdue (e.g., distinct styling/label).
2. **Given** an incomplete todo with a due date of today, **When** the todo list is displayed, **Then** that todo is not marked as overdue.
3. **Given** an incomplete todo with a due date in the future, **When** the todo list is displayed, **Then** that todo is not marked as overdue.
4. **Given** a todo with no due date set, **When** the todo list is displayed, **Then** that todo is not marked as overdue.

---

### User Story 2 - Completed todos are never marked overdue (Priority: P2)

As a user, I want todos I've already completed to never be shown as overdue, even if their due date has passed, so the overdue indicator only reflects work I still need to do.

**Why this priority**: Prevents a confusing/misleading experience where finished work still appears urgent. Builds directly on User Story 1's visual indicator.

**Independent Test**: Can be fully tested by marking an overdue todo as complete and confirming the overdue indicator disappears immediately.

**Acceptance Scenarios**:

1. **Given** a todo with a past due date, **When** the user marks it complete, **Then** the overdue indicator is removed from that todo.
2. **Given** a completed todo with a past due date, **When** the todo list is displayed, **Then** the todo shows its normal completed styling and no overdue indicator.
3. **Given** a completed, overdue-looking todo, **When** the user marks it incomplete again, **Then** the overdue indicator reappears (since its due date is still in the past).

---

### User Story 3 - Overdue status updates automatically over time (Priority: P3)

As a user, I want a todo's overdue status to reflect the current date automatically, so a todo due "today" that I don't complete becomes marked overdue the next time I view the list without me having to edit it.

**Why this priority**: Ensures the feature remains accurate over time; lower priority since it's a natural consequence of correctly implementing User Story 1 but is called out to make the expected behavior explicit and testable.

**Independent Test**: Can be fully tested by viewing a todo due "today," then viewing the list again after that date has passed (or with the system date advanced) and confirming it is now marked overdue.

**Acceptance Scenarios**:

1. **Given** an incomplete todo due today, **When** the current date advances past the due date without the todo being completed or edited, **Then** the todo becomes marked as overdue the next time the list is viewed.

### Edge Cases

- A todo due exactly "today" is treated as not yet overdue (grace period through end of due day).
- Editing a todo's due date to a past date immediately marks it overdue (once it appears as incomplete); editing it to a future date immediately removes the overdue mark.
- Todos with no due date are never marked overdue, regardless of age.
- Overdue determination relies on the date only (not time of day), consistent with due dates being stored as dates without time components.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST determine a todo to be "overdue" when it is incomplete AND its due date is earlier than the current date.
- **FR-002**: System MUST NOT mark a todo as overdue if it has no due date set.
- **FR-003**: System MUST NOT mark a todo as overdue if it is marked complete, regardless of its due date.
- **FR-004**: System MUST visually distinguish overdue todos from non-overdue todos in the todo list view.
- **FR-005**: System MUST re-evaluate a todo's overdue status whenever the todo list is displayed, based on the current date.
- **FR-006**: System MUST update a todo's overdue indicator immediately when its completion status changes (completing removes the indicator; reopening restores it if the due date has passed).
- **FR-007**: System MUST update a todo's overdue indicator immediately when its due date is edited.
- **FR-008**: A todo due on the current date MUST NOT be considered overdue.

### Key Entities

- **Todo**: Existing entity representing a task; relevant attributes for this feature are its due date (optional) and completion status. This feature adds a derived, non-persisted "overdue" state computed from those attributes and the current date.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Users can identify all overdue tasks in their list within 3 seconds of viewing it, without checking any date manually.
- **SC-002**: 100% of incomplete todos with a due date in the past are visually marked as overdue every time the list is displayed.
- **SC-003**: 0% of completed todos or todos without a due date are ever marked as overdue.
- **SC-004**: A todo's overdue indicator reflects its current due date and completion status with no perceptible delay after the user edits or completes it.

## Assumptions

- Due dates are stored and compared as calendar dates (no time-of-day component), so "overdue" is determined by comparing the due date to today's date.
- "Current date" is the date on the device/browser displaying the todo list (single-user, no server-side timezone reconciliation required).
- The overdue indicator is a visual-only change (e.g., styling, label, or icon) and does not introduce new user actions, notifications, or sorting/filtering behavior.
- This feature builds on the existing todo management functionality described in the functional requirements (create, view, update, delete, complete/incomplete) and does not alter persistence behavior.
