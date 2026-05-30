---
name: nimble-user-stories
description: Comprehensive guidance for writing user stories following Nimble's product development standards. Use when creating product requirements, organizing backlogs, writing user stories, documenting bugs, or creating chores for digital product development at Nimble. Covers backlog organization (modules, features, labels), user story types (features, bugs, chores), story templates, naming conventions, and estimation guidelines.
---

# Nimble User Stories

## Process Workflow

Follow this sequence when writing user stories:

1. **Fetch latest documentation** - Always start by fetching the relevant documentation URL(s) to get current standards
2. **Read the PRD** - Read the feature's `prd.md` for requirements, business rules (BR-NNN), and edge cases
3. **Identify story type** - Determine if this is a feature, bug, or chore
4. **Apply appropriate template** - Use the correct format for the story type
5. **Enforce vertical slices** - Each feature story must deliver end-to-end user value
6. **Generate truths** - Derive verification contracts from AC + business rules + edge cases
7. **Right-size check** - Validate against the right-sizing checklist
8. **Check naming conventions** - Verify labels, titles, and formatting match standards

## Documentation Sources

Before writing any user story, fetch the relevant live documentation:

- **Backlog Organization**: https://nimblehq.co/compass/product/backlog-management/organization/
- **Writing Stories**: https://nimblehq.co/compass/product/backlog-management/user-stories/
- **Feature Stories**: https://nimblehq.co/compass/product/backlog-management/user-stories/features/
- **Bug Stories**: https://nimblehq.co/compass/product/backlog-management/user-stories/bugs/
- **Chore Stories**: https://nimblehq.co/compass/product/backlog-management/user-stories/chores/

**Important**: Always fetch the documentation before writing stories. The live docs contain the most current standards, templates, examples, and best practices.

## Writing Feature Stories

**Always fetch latest documentation first:**
https://nimblehq.co/compass/product/backlog-management/user-stories/features/

### Feature Story Template

```
As a [Who], [When], I [can/must] [action verb] [What] so that [Why]

## Why
What user problem this solves and what business value it creates. (1-2 sentences)

## Acceptance Criteria
- Simple fact or constraint (bullet point)
- Given [precondition], when [action], then [expected result] (for state transitions)
- Given [condition A], when [action], then [different result] (for conditional behavior)

## Truths
- [User-observable verification contract derived from AC + business rules]
- [Each truth can pass or fail definitively]
- [Written from end-user perspective: "Customer can X", "Admin sees Y"]

## Business Rules
- BR-001: [Rule from PRD]
- BR-002: [Rule from PRD]

## Complexity
[S / M / L]

## Technical Considerations
(Optional) Implementation guidance or constraints

## Design
(Optional) Figma links or design screenshots

## Resources
(Optional) API docs, references, implementation examples
```

### Title Format

The title IS the full user story — not a short summary.

```
As a [Who], [When], I [can/must] [action verb] [What] so that [Why]
```

**Rules:**
- User must always be human (customer, admin, staff), never a system or API
- Include "so that" clause with the benefit
- No short summaries as titles

**Anti-patterns:**
- `As a client app, I can check pending consents...` — system, not human
- `Implement GCash wallet linking` — task, not story
- `Wallet linking` — summary, not story

### Acceptance Criteria: Mixed Format

Choose the format based on the type of behavior:

| AC Type | Format | Example |
|---------|--------|---------|
| Simple fact | Bullet point | `- Returns 404 if not found` |
| State transition | Given/When/Then | `- Given a user with linked wallet, when they unlink, then wallet is removed from payment options` |
| Conditional behavior | Given/When/Then | `- Given a guest user, when they select GCash, then only one-time payment is available` |
| Constraint | Bullet point | `- Only one GCash wallet can be linked per user` |

**Rules:**
- Use Given/When/Then only when a precondition changes the expected behavior
- Simple behaviors get bullet points — do not over-formalize
- Each AC must be independently verifiable
- Target 3-8 AC per story
- Start bullet-point AC with action verbs (Accept, Validate, Display, Send, etc.)

### Truths

Truths are verification contracts embedded in the story. They are:

- **User-observable** — Written from the end-user's perspective ("Customer can X", "Admin sees Y")
- **Testable** — Each truth can pass or fail definitively
- **Derived** — Generated from the story's AC, business rules, and edge cases

**How to generate truths:**

1. Read all AC for the story
2. Read all referenced business rules from the PRD
3. Read applicable edge cases from the PRD
4. For each AC, write one truth that captures the observable outcome
5. For each business rule, write one truth that captures the constraint from the user's perspective
6. Merge overlapping truths — aim for 3-5 truths per story

**Example:**

AC: "Call GT API with `?actionType=saveAndPay`"
Business Rule: "One wallet per user"
Edge Case: "User closes GCash tab without completing"

Truths:
- Logged-in user can link GCash wallet directly from Checkout
- Only one GCash wallet can be linked per account
- Incomplete linking attempt leaves Checkout in its original state

### AC vs Truths

| | Acceptance Criteria | Truths |
|---|---|---|
| **Granularity** | Detailed, implementation-specific | High-level, outcome-focused |
| **Audience** | Engineers (how to build it) | PM + QA (did we build it right?) |
| **Example** | "Call GT API `me/saved-payments` with `?actionType=saveAndPay`" | "User can link GCash wallet from Checkout without navigating away" |
| **Count** | 3-8 per story | 3-5 per story |

### Business Rule Mapping

Each feature story must reference applicable business rules from the PRD using their BR-NNN IDs:

```
## Business Rules
- BR-003: One GCash wallet per user account
- BR-005: Linked wallet payment requires no redirect
```

This creates traceability: PRD defines rules → stories reference them → implementation satisfies them.

### Complexity Sizing

Every feature story must have a complexity size:

| Size | Meaning | Heuristics |
|------|---------|------------|
| **S** | Clear AC, one domain area, minimal judgment needed | 3-4 AC, straightforward |
| **M** | Some ambiguity, may touch multiple areas, moderate judgment | 5-6 AC, some edge cases |
| **L** | Significant complexity, architectural decisions, heavy human guidance | 7-8 AC, many edge cases |

### Vertical Slice Enforcement

Every feature story must deliver end-to-end user value. **Reject** stories that:

- Only create a model/schema without user-facing behavior
- Only build an API endpoint without a consumer
- Only add a UI component without backend integration
- Are purely technical tasks with no observable user outcome

When you encounter a horizontal slice, merge it into the story that completes the feature end-to-end. If infrastructure is truly prerequisite, document it as a Technical Consideration within the first story that needs it.

### Right-Sizing Checklist

A story is the right size when:

- [ ] **One verifiable outcome** — Can be tested against its AC in isolation
- [ ] **One PR** — If it needs multiple PRs, split into separate stories
- [ ] **3-8 acceptance criteria** — Fewer than 3 suggests it's a sub-task; more than 8 suggests it should be split
- [ ] **One domain area** — Touches one bounded context
- [ ] **3-5 truths** — If you can't write 3 truths, the story may be too small; more than 5, it may be too big

### Story Frontmatter

Each story file must include frontmatter:

```yaml
---
jira_project: JB
jira_epic_key: JB-4432
jira_issue_type: Story
prd_requirement: GCASH-WALLET-FR-007
complexity: M
---
```

## Writing Bug Reports

**Always fetch latest documentation first:**
https://nimblehq.co/compass/product/backlog-management/user-stories/bugs/

### Quick Reference Template (verify with live docs)

```
[Clear title stating the issue - NOT user story format]

## Environment
- Platform: Web/Android/iOS
- Device: e.g., iPhone 10
- OS: e.g., iOS 13
- Version: e.g., 0.12.0 (519)
- Environment: staging/production

## Prerequisites
Specific conditions needed to recreate the issue.

## Steps to Reproduce
1. Step 1
2. Step 2
3. Step 3

## Expected Behavior
What should happen (with screenshots if possible)

## Actual Behavior
What actually happens (with screenshots/videos)
```

Example title: "On the Homepage, the heading is the wrong font"

Fetch the documentation for complete guidance on environment details, prerequisites, reproduction steps, and attachment requirements.

## Writing Chores

**Always fetch latest documentation first:**
https://nimblehq.co/compass/product/backlog-management/user-stories/chores/

### Quick Reference Template (verify with live docs)

```
[Clear title stating the task]

## Why
Purpose and benefit of this chore

## Acceptance Criteria
- Task 1
- Task 2
- Behaviors to verify in QA

## Resources
Links, documentation, or materials needed
```

### Key Guidelines (verify with live docs)

- Product Manager adds chores to sprint
- Should be completable within one day
- Do not require estimation
- Common types: refactoring, POC, research, dependencies, test coverage, setup tasks

Fetch the documentation for complete details on chore types, handling, and acceptance criteria.

## Backlog Organization

**Always fetch latest documentation first:**
https://nimblehq.co/compass/product/backlog-management/organization/

### Standard Labels (verify with live docs)

- **Module**: `#<module-name>` (e.g., `#authentication`)
- **Feature**: `$<feature-name>` (e.g., `$user-list-tickets`)
- **Chore**: `!<chore-name>` (e.g., `!setup-ci-cd`)
- **Version**: `@<semver>` (e.g., `@1.2.2`)
- **Initial scope**: `initial-scope` (for all initial project stories)

### Priority Levels (verify with live docs)

- **High**: Time-sensitive, blocks work (e.g., users can't login, API down)
- **Medium**: Default priority
- **Low**: No urgency (e.g., minor UI changes, documentation)

### Workflow States (verify with live docs)

Backlog → Ready for Development → In Development → In Code Review → Ready for QA → Completed

### Versioning (verify with live docs)

Semantic Versioning: `MAJOR.MINOR.PATCH`
- MAJOR: Breaking changes
- MINOR: New features (backward compatible)
- PATCH: Bug fixes (backward compatible)

Fetch the documentation for complete details on modules, features, workflow states, and label usage.

## Best Practices

**Fetch documentation for complete best practices guidance.**

### Core Principles

**DO:**
- Use specific user roles (not "system" or "developer")
- Validate meaningful user value
- Keep stories small and focused (right-sizing checklist)
- Write clear, testable acceptance criteria (mixed format)
- Generate truths for every feature story
- Map business rules from PRD (BR-NNN)
- Assign complexity sizing (S/M/L) to every feature story
- Enforce vertical slices — every story delivers end-to-end value
- Use standardized labels consistently
- Write "Why" at feature level

**DON'T:**
- Split stories by technical layers (horizontal slices)
- Use generic/vague feature names
- Let technical complexity dictate boundaries
- Forget version labels
- Write truths that duplicate AC verbatim — truths are higher-level
- Skip the "so that" clause in story titles

## Example Workflow

**User request**: "Create user stories for the GCash Checkout wallet linking feature"

**Response process**:
1. Fetch https://nimblehq.co/compass/product/backlog-management/user-stories/features/
2. Fetch https://nimblehq.co/compass/product/backlog-management/organization/
3. Read the PRD (`features/GCASH-WALLET/prd.md`) for requirements, business rules, and edge cases
4. Review the fetched documentation for current templates and examples
5. Identify applicable FRs and BRs from the PRD
6. Break down into vertical-slice user stories following the template
7. For each story: write mixed-format AC → derive truths → map BRs → assign complexity
8. Validate each story against right-sizing checklist
9. Apply appropriate labels (`$feature-name`, `@version`, etc.)
