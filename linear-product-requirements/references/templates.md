# Templates

## Initiative

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

## Milestone

Milestones are just names. Common patterns:

**By actor:**
- Admin Management
- Customer Actions
- Integration

**By release phase:**
- Internal
- Beta
- GA

---

## Project Document (Spec)

**Title:** `Spec: [Milestone Name]`

```markdown
# Spec: [Milestone Name]

## Overview

[2-3 sentences: What this milestone delivers and why it matters]

## User Stories

| ID | Story | Priority |
|----|-------|----------|
| US-001 | [Role] can [action] [what] | Must-have |
| US-002 | [Role] can [action] [what] | Must-have |
| US-003 | [Role] can [action] [what] | Should-have |

## Business Rules

| ID | Rule | Rationale |
|----|------|-----------|
| BR-001 | [Rule description] | [Why this rule exists] |
| BR-002 | [Rule description] | [Why this rule exists] |

## Data Requirements

| Data Element | Required | Format | Notes |
|--------------|----------|--------|-------|
| [Element] | Yes/No | [Type] | [Constraints] |

### Entity Relationships (Conceptual)

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

**Title:** Full user story (this IS the title, not a summary)
```
As a [role], I can [action] so that [benefit]
```

**Description:**
```markdown
## Acceptance Criteria

- [Observable behavior 1]
- [Observable behavior 2]
- [Success condition]

## Technical Considerations

(Optional) [Implementation guidance or constraints from codebase exploration]

## Design

(Optional) [Figma links]

## Resources

(Optional) [API docs, references]
```
