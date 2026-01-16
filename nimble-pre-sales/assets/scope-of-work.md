# Scope of Work Template

Standard structure for Nimble pre-sales proposals. Adapt sections based on project scope.

## Document Structure

```
1. Header (Client name, Nimble, date, version)
2. Description & Scope
   - User Stories (link to detailed spreadsheet)
3. Solution
   - Technical Approach
     - Methodology
     - Development
     - Communication
   - Design (if applicable)
4. Team Structure
5. Deliverables
   - Source Files
   - Working Application
6. Tools Required
7. Signatures
```

## Section Guidance

### Description & Scope

High-level summary of what will be built. Keep it outcome-focused.

Link to user stories spreadsheet for detailed task breakdown. Note that the linked list represents original scope for historical reference.

### Technical Approach

#### Methodology

Standard text (adapt as needed):

> We follow an Agile methodology with 2-week sprints. This allows you to track progress easily and re-align scope if required.
>
> Work and progress are tracked in a project management tool. All communications happen on Slack and within the project management tool—you'll have full visibility into our internal discussions.
>
> Nimble's Product Manager is the point of contact with external parties. Escalation involving you may be needed to remove blockers.

#### Development

Standard text:

> Our quality-oriented development process follows Test Driven Development (TDD). Each feature goes through:
>
> 1. Feature code with corresponding tests
> 2. Automated test run on push to version control
> 3. Peer review (requires two approvals)
> 4. Deployment to staging for manual QA
> 5. Production deployment after QA approval
>
> Rejections at any step restart from step 2.

#### Communication

Standard text:

> All communication (written and verbal) is in English—our official business language.
>
> Some team members may speak your native language, but all documents and multi-person meetings remain in English to ensure the entire team understands the project.

### Design

Include when project has UX/UI work. Standard process:

1. **Research** — Goals, market research, usability research
2. **User Journey / UX Flow**
3. **Wireframing**
4. **Final UI**

Reviews conducted at each stage.

### Team Structure

Use [team-roles.md](../references/team-roles.md) to select composition. Format:

> As a product-oriented company, we structure the team as follows:
>
> - A Product Manager, your single point of contact, with weekly meetings
> - [X] Web Engineers
> - [X] iOS Engineers
> - [X] Android Engineers
> - 1 UX/UI Designer
> - [If applicable] A Solution Architect for technical direction

### Deliverables

#### Source Files

> We use version control (GitHub or Bitbucket). You receive not just source files but the entire repository with full project history.

#### Working Application

> You receive the working application on [hosting provider], plus a staging environment for testing.

### Tools Required

Client is responsible for billing. Common tools:

- Amazon Web Services / Google Cloud — hosting
- GitHub — source control
- GitHub Actions — CI/CD
- Terraform Cloud — infrastructure automation
- Mailgun / SendGrid — email
- Phraseapp / Lokalise — localization
- Firebase — mobile distribution and monitoring

Add project-specific tools as needed. Note: "This list is not exhaustive."

### Signatures

Standard signature block for both parties.

## Action Plan Section

When including timeline/milestones, structure as:

| Phase | Duration | Key Deliverables |
|-------|----------|------------------|
| Discovery | X weeks | Requirements, user stories, designs |
| Phase 1 | X weeks | [Core functionality] |
| Phase 2 | X weeks | [Extended features] |
| Launch | X weeks | Production deployment, handover |

Include assumptions that affect timeline (e.g., "assumes client feedback within 48 hours").
