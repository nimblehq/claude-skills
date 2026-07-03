---
name: release-notes
description: >
  Turn raw CI-generated release notes into client-friendly, marketing-style release notes for a SaaS product. Use when asked to write release notes, changelog announcements, "what's new" posts, product update notes, or to clean up / rewrite a release PR body or auto-generated changelog for a non-technical audience. Triggers include "release notes", "changelog", "what's new", "announce the release", "product update", "clean up the release notes", and pasting a grouped Features/Bugs/Chores changelog.
---

# Release Notes Skill

## Purpose

Transform an engineer-facing, CI-generated changelog (grouped Features / Bugs / Chores with PR numbers) into a concise, benefit-led release note a non-technical client will actually read and value.

The hard part is **not** filtering by tag — it's semantic judgment: most items tagged `[feature]` are internal building blocks (models, state machines, policies, scaffolding). The skill decides what shipped as a *user-visible capability*, consolidates the scattered PRs behind it into one story, and rewrites it in plain language.

## When to use

The user has a release changelog (typically a release PR body like `Release - 1.41.0`) and wants a client-facing version. Input is usually GitHub release-drafter format:

```
## 🚀 Features
- [feature] [COR-207] As an Admin, I want to associate recipes with store products - Part 1
   - PR: #2486
## 🐛 Bugs
...
## 🧹 Chores
...
```

## Pipeline

Run these in order. Details for each step live in the reference files.

1. **Parse** — extract every item: title, tag, PR number, any Jira key (e.g. `COR-207`, `WEB-145`), and Part-N marker.
2. **Filter** — drop noise, keep client value. See [references/filtering-rules.md](references/filtering-rules.md). Apply the revert cascade and the internal-scaffolding test here.
3. **Consolidate** — cluster surviving items into user-facing capabilities (group by Jira epic prefix + shared nouns). One capability = one story, not one-PR-per-line. Flag capabilities that look incomplete (only Part 1 present, or all backend/no UI) — hold or mark "coming soon", never announce a half-shipped feature.
4. **Rewrite** — turn each capability into benefit-led copy. Lead with the user outcome, not the mechanism. See [references/voice.md](references/voice.md).
5. **Assemble** — lay out the note using the house structure ([references/structure.md](references/structure.md)) and fill [assets/note-template.md](assets/note-template.md).
6. **Illustration briefs** — for each major section, emit an `[ILLUSTRATION: …]` placeholder with a caption, suggested visual, and alt text. The image is produced manually; the skill only writes the brief.

## Output

- **Markdown** by default (changelog / blog / in-app "what's new").
- Pick one **hero** capability (highest user impact) for the lead.
- Draft a subject/title line and a 2-3 sentence intro that teases the top highlights.
- End the process by listing, separately, what you **dropped or held** and why (so the user can sanity-check nothing important was cut). Never silently omit a shipped user-facing capability.

## Hard rules

- Never announce a feature that was reverted in the same release (check Chores for `Revert #NNNN` and remove the target).
- Never announce a capability whose PRs are all internal (model / migration / policy / controller / service / concern / STI / state-machine) with no user-facing surface — that's groundwork, not a feature.
- No tier/plan tagging (single plan).
- Don't invent capabilities or benefits not supported by the PR titles. If the "why" is unclear, say what it does plainly rather than embellish.
