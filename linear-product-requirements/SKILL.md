---
name: linear-product-requirements
description: Create product requirements using Linear's native structure. Use when creating or editing Initiatives, Projects, Milestones, Project Documents (specs), or Issues (user stories) in Linear. Triggers include requests to write PRDs, create user stories, set up projects, write specs, or add features to Linear.
---

# Linear Product Requirements

All product requirements live in Linear. No external docs needed.

## Linear Hierarchy

| Linear Level | Content | Who Writes | Who Reviews |
|--------------|---------|------------|-------------|
| **Initiative** | Strategic context (why, target outcome) | PM | Leadership |
| **Project Overview** | TL;DR, scope, decisions, constraints | PM | EL approves |
| **Project Milestones** | Delivery phases | PM | Team |
| **Project Documents** | Detailed specs per milestone | PM | Engineering |
| **Issues** | User stories | PM | Engineering |

## Visual Structure

```
INITIATIVE: Platform Compliance
│
│   Why: [business driver]
│   Target Outcome: [what we're trying to achieve]
│
└── PROJECT: Consent Management
    │
    ├── Project Overview (EL reviews this)
    │   └── TL;DR, Scope, Decisions, Technical Considerations
    │
    ├── Milestones
    │   ├── Admin Management
    │   ├── Customer Actions
    │   └── Customer Visibility
    │
    ├── Project Documents (Engineering reference)
    │   ├── Spec: Admin Management
    │   ├── Spec: Customer Actions
    │   └── Spec: Customer Visibility
    │
    └── Issues (created from specs)
        ├── As an admin, I can create a new consent so that...
        ├── As an admin, I can publish a consent version so that...
        └── ...
```

## Workflow

```
1. PM creates Initiative (if new strategic area)
       ↓
2. PM creates Project under Initiative
       ↓
3. PM writes Project Overview → EL reviews & approves
       ↓
4. PM creates Milestones in Linear (manual)
       ↓
5. PM writes Project Document per Milestone
       ↓
6. PM creates Issues from specs, assigning to Milestones
       ↓
7. Engineering works Issues, posts Project Updates
```

> **Note:** Milestones must be created manually in Linear before creating Issues.

## Entry Points & Linear MCP

| Level | Where in Linear | MCP Available |
|-------|-----------------|---------------|
| Initiative | Initiatives → New Initiative | ❌ Manual |
| Project | Projects → New Project | ✅ `create_project` |
| Milestone | Project → Milestones | ❌ Manual |
| Document | Project → Documents → New | ✅ `create_document` |
| Issue | Project → Issues | ✅ `create_issue` |

## Before Writing Requirements

**Always explore the codebase first.** See `references/codebase.md` for patterns.

Find relevant:
- Models/schemas for data requirements
- Existing APIs for integration points
- Similar features for patterns to follow
- Validation logic for business rules

## Naming Conventions

| Element | Definition | Pattern | Example |
|---------|------------|---------|---------|
| **Initiative** | Groups projects with same business reason | Strategic theme | `Platform Compliance` |
| **Project** | The feature being built | Capability (2-3 words) | `Consent Management` |
| **Milestone** | How project is split for delivery | Focus area or phase | `Admin Management`, `Beta` |
| **Document** | Detailed requirements for a milestone | `Spec: [Milestone]` | `Spec: Admin Management` |
| **Issue** | A task an engineer completes | Full user story | `As a customer, I can...` |

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
- ✅ `As a customer, when I have pending consents, I am prompted to review them...`

## Technical Ownership

Clear ownership keeps collaboration smooth:

| PM Owns (WHAT) | Engineering Owns (HOW) |
|----------------|------------------------|
| User goals and outcomes | Database schema design |
| Acceptance criteria | API endpoint specifications |
| Business rules and logic | Architecture decisions |
| Data requirements (what data) | Technology choices |
| Integration needs | Implementation approach |

### What PMs Should Include

| Include | Example |
|---------|---------|
| Data requirements | "Must capture: timestamp, user ID, IP address" |
| Business rules | "Points expire 365 days after earning" |
| Integration needs | "Must integrate with existing auth" |
| Open questions | "How should we handle versioning?" |

### What PMs Should Avoid

| ❌ Avoid | ✅ Instead |
|---------|-----------|
| "Create table `consents` with columns..." | "System must store consent records" |
| "POST /api/v1/consents endpoint" | "Customer can grant consent" |
| "Use JSONB for content field" | "Content must support multiple languages" |
| "Use FIFO with FOR UPDATE locks" | "Deduct from oldest batches first" |

### When Technical Context Helps

Frame as considerations, not requirements:

| Instead of... | Try... |
|---------------|--------|
| "Use double-entry accounting" | "Similar systems use double-entry patterns. Engineering to evaluate." |
| "Add idempotency_key column" | "API must prevent duplicate transactions. Reference: Stripe idempotency." |

## Bug Card Rules

Bug-labeled issues follow the [Nimble Bugs standard](https://nimblehq.co/compass/product/backlog-management/user-stories/bugs/): a plain title plus 5 required sections. We add one optional `## Resources` section on top of that standard. The fill-in template lives in `references/templates.md`.

**Title:** Plain statement of what is broken, NOT a user story. Use "On" for a screen and "In" for a feature or area; pick one, do not write the literal "On/In".
```
On/In [screen or context], [what is wrong]
```

Examples:
- ✅ `In POS settings, turning off order manager does not disable kitchen ticket printing`
- ✅ `On the order detail screen, the refund status is not updated after void`
- ❌ `As a store staff, I can see the correct refund status` (user story format, wrong for bugs)

**Description:** the 5 required sections in this exact order, then the optional `## Resources` and nothing else:

1. `## Environment`
2. `## Prerequisites`
3. `## Steps to Reproduce`
4. `## Expected Behavior`
5. `## Actual Behavior`

**Strict rules:**
- The 5 required sections (Environment, Prerequisites, Steps to Reproduce, Expected Behavior, Actual Behavior) appear in this order, even if brief. Use the full heading names, not "Expected" / "Actual".
- **No** Acceptance Criteria, Edge Cases, Out of Scope, Why, or other sections
- `## Resources` is the only allowed optional addition. It is our local extension, not part of the Nimble 5-section standard; omit it when there are no links
- If reproduction steps are unknown, write "Unknown, investigate first" rather than omitting the section
- Tag the issue with the `Bug` label in Linear

## Templates

See `references/templates.md` for complete templates.

## Example

See `references/examples.md` for complete Consent Management example.

## Quick Reference

### EL Review Checklist

EL only reviews **Project Overview**:

- [ ] Problem is clear
- [ ] Solution makes sense
- [ ] Scope is reasonable (in/out)
- [ ] Decisions are answered or flagged
- [ ] No technical over-specification

### PM Checklist Before Creating Issues

- [ ] Project Overview approved by EL
- [ ] Milestones created in Linear (manual)
- [ ] Project Document written for each milestone
- [ ] User stories identified in spec
- [ ] Business rules documented
- [ ] Edge cases covered
- [ ] Open questions flagged for engineering

### Engineering Reference

For any Project, find requirements in:

1. **Project Overview** → Scope, constraints
2. **Project Documents** → Detailed specs per milestone
3. **Issues** → Individual stories with acceptance criteria
