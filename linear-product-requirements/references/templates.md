# Templates

## Initiative

**Where in Linear:** Initiatives → New Initiative → Overview

**Length:** 3-5 lines

**Short Summary** (separate field in Linear):
```
[One line description for list view]
```

**Description:**
```markdown
## Why

[1-2 sentences: What business driver or problem makes this initiative necessary?]

## Target Outcome

[1 sentence: What are we trying to achieve? No metrics.]
```

> Projects are linked via Linear's relation field, not listed in the description.

---

## Project Overview

**Where in Linear:** Project → Overview tab → Description field

**Length:** ~15 lines (EL reviews only this)

**Short Summary** (separate field in Linear):
```
[One line description for list view]
```

**Description:**
```markdown
## TL;DR

- **Problem:** [One sentence]
- **Solution:** [One sentence]
- **Impact:** [One sentence: why this matters]

## Scope

**In:**
- [Core capability 1]
- [Core capability 2]
- [Core capability 3]

**Out:**
- [Explicitly excluded 1]
- [Explicitly excluded 2]

## Decisions Needed

[Questions requiring EL input, or "None — ready for implementation"]

## Technical Considerations

- **Constraint:** [Must work with X]
- **Constraint:** [Must support Y]
- **Question:** [Open question for engineering]
```

---

## Project Milestones

**Where in Linear:** Project → Milestones

**Naming:** Short, clear phase names

Common patterns:
- By capability: `Admin Management`, `Customer Actions`, `Reporting`
- By release: `Internal`, `Beta`, `GA`, `Post-launch`
- By priority: `Core`, `Enhanced`, `Nice-to-have`

---

## Project Document (Spec)

**Where in Linear:** Project → Documents → New Document

**Naming:** `Spec: [Milestone Name]`

**Length:** As detailed as needed (Engineering reference)

```markdown
# Spec: [Milestone Name]

## Overview

[2-3 sentences: What this milestone delivers and why it matters]

## User Stories

| ID | Story | Priority |
|----|-------|----------|
| US-001 | [User] can [action] [what] | Must-have |
| US-002 | [User] can [action] [what] | Must-have |
| US-003 | [User] can [action] [what] | Should-have |

## Business Rules

| ID | Rule | Rationale |
|----|------|-----------|
| BR-001 | [Rule description] | [Why this rule exists] |
| BR-002 | [Rule description] | [Why this rule exists] |

## Data Requirements

> What data must be captured. Schema design is engineering's decision.

| Data Element | Required | Format | Notes |
|--------------|----------|--------|-------|
| [Element] | Yes/No | [Type] | [Constraints] |

### Entity Relationships (Conceptual)

> Concepts only, not database tables.

```
[Entity A] ──has many──▶ [Entity B]
[Entity B] ──belongs to──▶ [Entity C]
```

## Edge Cases

| Scenario | Expected Behavior | Priority |
|----------|-------------------|----------|
| [Edge case] | [How to handle] | Must-have |
| [Edge case] | [How to handle] | Should-have |

## Open Questions for Engineering

1. [Question about implementation]
2. [Question about tradeoffs]
```

---

## Issue (User Story)

**Where in Linear:** Created from Project Document specs

**Format:** Follow Nimble user story standards

**Title:** Full user story (this IS the title, not a summary)
```
As a [Who], [When], I [can/must] [action verb] [What] so that [Why]
```

**Description:**
```markdown
## Acceptance Criteria

- [Observable behavior 1]
- [Observable behavior 2]
- [Success condition]

## Technical Considerations

(Optional) [Implementation guidance or constraints]

## Design

(Optional) [Figma links]

## Resources

(Optional) [API docs, references]
```

> For complete user story guidance, refer to the Nimble user stories skill and documentation at https://nimblehq.co/compass/product/backlog-management/user-stories/
