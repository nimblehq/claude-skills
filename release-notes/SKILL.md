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

## Source access (required before Fetch)

PR content is read through the **GitHub MCP tools** or the **`gh` CLI** — whichever is available on the host. Never scrape or read PRs through a browser / web UI.

Pick the source once, at the start, and use it for the whole run:

1. If GitHub MCP tools are present (e.g. `pull_request_read`, `get_pull_request`), use them.
2. Else if `gh` is installed and authenticated (`gh auth status`), use `gh pr view <N> --json title,body,state,labels,files` and `gh pr diff <N>`.
3. If neither is available, stop and tell the user — do not fall back to a browser and do not guess PR contents from titles.

Resolve the repo `owner/name` from the release PR or the local git remote (`gh repo view --json nameWithOwner`).

## Pipeline

Run these in order. Details for each step live in the reference files.

1. **Parse** — extract every item: title, tag, PR number, any Jira key (e.g. `COR-207`, `WEB-145`), and Part-N marker.
2. **Fetch** — for **every** PR in the changelog, pull its content via the chosen source: title, body, state (merged/closed), labels, and changed files (`gh pr view --json` / `pull_request_read`). This is not optional and not title-only — it's how we catch reverts that PR titles hide and how we add real detail. See [references/filtering-rules.md](references/filtering-rules.md) → "Fetch pass". For large releases, batch the calls and cache results so each PR is fetched once.
3. **Filter** — drop noise, keep client value. See [references/filtering-rules.md](references/filtering-rules.md). Apply the revert cascade and the internal-scaffolding test here, now informed by PR bodies and diffs, not just titles.
4. **Consolidate** — cluster surviving items into user-facing capabilities (group by Jira epic prefix + shared nouns). One capability = one story, not one-PR-per-line. Flag capabilities that look incomplete (only Part 1 present, or all backend/no UI) — hold or mark "coming soon", never announce a half-shipped feature.
5. **Rewrite** — turn each capability into benefit-led copy. Lead with the user outcome, not the mechanism. See [references/voice.md](references/voice.md).
6. **Assemble** — lay out the note using the house structure ([references/structure.md](references/structure.md)) and fill [assets/note-template.md](assets/note-template.md).
7. **Illustration briefs** — for each major section, emit an `[ILLUSTRATION: …]` placeholder with a caption, suggested visual, and alt text. The image is produced manually; the skill only writes the brief.

## Output

- **Markdown** by default (changelog / blog / in-app "what's new").
- Pick one **hero** capability (highest user impact) for the lead.
- Draft a subject/title line and a 2-3 sentence intro that teases the top highlights.
- End the process by listing, separately, what you **dropped or held** and why (so the user can sanity-check nothing important was cut). Never silently omit a shipped user-facing capability.

## Hard rules

- Never announce a feature that was reverted in the same release. Detect reverts from **PR content**, not just titles: a revert may be a chore titled `Revert #NNNN`, a PR whose body says "Reverts #NNNN" / "This reverts commit …", or a later PR that quietly undoes an earlier one. Remove both the revert and its target.
- Always fetch every PR's content via GitHub MCP or `gh` before writing. Do not describe a PR from its title alone, and never read PRs through a browser.
- Never announce a capability whose PRs are all internal (model / migration / policy / controller / service / concern / STI / state-machine) with no user-facing surface — that's groundwork, not a feature.
- No tier/plan tagging (single plan).
- Don't invent capabilities or benefits not supported by the PR titles. If the "why" is unclear, say what it does plainly rather than embellish.
