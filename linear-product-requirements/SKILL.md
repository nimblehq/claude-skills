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

**Acceptance criteria are expected outcomes only.** Each AC states what the user or business can observe, never how it is built, and never limits how the developer solves it. Keep implementation words out of AC wording (common tells: endpoint, API, token, scope, schema, job, service, query, column). The wordlist is only a hint; the litmus test below is the real rule. The benefit and reasons live in `## Why`.

**Anti-patterns (AC), outcome not mechanism:**
- ❌ `The rewards index returns only rewards in the customer token's scope`
- ✅ `A customer sees only the rewards they are eligible for`
- ❌ `Add a maximum-discount column and clamp it in the calculator`
- ✅ `The discount never exceeds the configured maximum`

**State the use case, not the architecture.** An AC says what the user needs to do or see; it never assumes how data is fetched, joined, or split across services. The engineer owns that and may satisfy one use case with several APIs.
- Use case: while browsing rewards, a customer wants to know which ones they can afford.
- ❌ Bakes in architecture: `Each reward returns how many more points the customer needs` (forces the reward feed to also carry the customer's balance: one combined endpoint)
- ✅ States the use case: `A customer can tell which rewards they can afford` (the engineer may serve rewards and the balance as two separate APIs and let the client compare them)

**Litmus test:** if an AC names which response must carry which data, it has already chosen the architecture; rewrite it. If it names only what the user can do or see, the architecture is still open.

Data the system must capture (timestamp, IP, and similar) is a PM-owned data requirement: state it in the spec's Data Requirements or in `## Why`, not as an AC describing how a response carries it.

**Visuals: add one only where it makes the issue easier to understand, never just to fill a slot.** Less is more: if the words already make it clear, add nothing. When a visual helps, put it where it does the most good, and you may use more than one across the description:
- in `## Why`: a real-world scenario in ASCII (a before/after of values, or a worked example), to ground the reader in the actual situation
- in `## Acceptance Criteria`: a Mermaid diagram (a user flow or a user-side sequence), when the flow or ordering is the hard part
- in `## Resources`: the Figma design, linked to the actual frame

Use none, one, or several. Each must earn its place by helping the developer understand the issue; drop anything that does not.

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
