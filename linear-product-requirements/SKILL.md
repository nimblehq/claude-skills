---
name: linear-product-requirements
description: Create product requirements using Linear's native structure. Use when creating or editing Initiatives, Projects, Milestones, Project Documents (specs), or Issues (user stories) in Linear. Triggers include requests to write PRDs, create user stories, set up projects, write specs, or add features to Linear.
---

# Linear Product Requirements

Create product requirements using Linear's native structure.

## Workflow

1. **Detect entry point** — Determine which level user is working at
2. **Explore codebase** — Find relevant code before writing requirements
3. **Apply templates** — Use correct template for the level
4. **Create in Linear** — Use MCP tools where available

## Entry Points

| Level | Trigger Examples | Linear MCP |
|-------|------------------|------------|
| Initiative | "create an initiative for..." | ❌ Manual |
| Project | "create a project for...", "set up [feature] in Linear" | ✅ `create_project` |
| Milestone | "add a milestone for..." | ❌ Manual |
| Document | "write a spec for...", "create requirements for..." | ✅ `create_document` |
| Issue | "write a user story for...", "create stories from spec" | ✅ `create_issue` |

When Linear MCP is unavailable, provide the content for PM to copy.

## Before Writing Requirements

**Always explore the codebase first.** See `references/codebase.md` for patterns.

Find relevant:
- Models/schemas for data requirements
- Existing APIs for integration points
- Similar features for patterns to follow
- Validation logic for business rules

## Naming Conventions

| Element | Definition | Pattern |
|---------|------------|---------|
| Initiative | Groups projects with same business reason | Strategic theme (noun phrase) |
| Project | The feature being built | Capability (2-3 words) |
| Milestone | How project is split for delivery | Focus area or phase |
| Document | Detailed requirements for a milestone | `Spec: [Milestone Name]` |
| Issue | A task an engineer picks up | Full user story |

**Project naming:**
- ❌ `Customer Data Protection` (problem-focused)
- ✅ `Consent Management` (capability-focused)

## User Story Rules

**Title is the full user story:**
```
As a [role], I can [action] so that [benefit]
```

**Critical rules:**
- User must always be human (customer, admin, staff), never a system
- Include "so that" clause with the benefit
- No short summaries as titles

**Anti-pattern:**
- ❌ `As a client app, I can check pending consents...`
- ✅ `As a customer, when I have pending consents, I am prompted to review them so that...`

## Technical Ownership

**PM owns (WHAT):**
- User goals and outcomes
- Acceptance criteria
- Business rules
- Data requirements (what data, not schema)
- Integration needs (not specific APIs)

**Engineering owns (HOW):**
- Database schema design
- API specifications
- Architecture decisions
- Technology choices

**Framing:**
- ❌ "Create table `consents` with columns..."
- ✅ "System must store consent records"
- ❌ "POST /api/v1/consents endpoint"
- ✅ "Customer can grant consent"

## Templates

See `references/templates.md` for complete templates:
- Initiative (Short Summary + Description)
- Project Overview (Short Summary + TL;DR + Scope + Decisions + Technical Considerations)
- Milestone (name only)
- Project Document (Overview + User Stories + Business Rules + Data Requirements + Edge Cases + Open Questions)
- Issue (Acceptance Criteria + Technical Considerations)

## Example

See `references/examples.md` for complete Consent Management example showing all levels.
