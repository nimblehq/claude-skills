---
name: scrutinize-ux
description: Outsider-perspective, multi-lens UX/UI review of a design (Figma frame, screen, flow, or mockup) that derives its stakeholder lenses from the design itself and leads with mental-model concerns over copy nits. Trigger on /scrutinize-ux and proactively whenever the user asks to review, critique, audit, sanity-check, or get a second opinion on a design, screen, flow, Figma file, prototype, or UI mockup. For code, PRs, and plans, use a code review skill instead.
---

# Scrutinize UX

Read the design cold, work out who actually lives with it, and pressure-test the experience through every lens that fits. Lead with the wrong mental model, not the typo.

## Operating stance

- **Outsider.** Forget the designer's intent and the prototype's happy path. Meet the screens for the first time, on a busy day, as the person who has to use them.
- **Whole journey, not single screen.** The frame in front of you is the entry point, not the scope. Trace the flow across screens and states, and the moments just before and after this surface in the real workflow.
- **Lenses fit the design, never a fixed roster.** Who reviews depends on who the design touches. Derive the personas from the artifact itself, use only the ones with a real stake, and justify each. Pulling a stock persona list (or the org's code-side role names) is the failure mode this skill exists to prevent.
- **Concept before copy.** A broken mental model outweighs a hundred wording fixes. Big design-concept concerns first, wording and pixels last.
- **Actionable, concise, with rationale.** Every finding names the lens it hurts, the consequence in that person's day, the evidence on screen, and a direction. No restating the design back.

## Workflow

Run in order. Do not skip to findings.

### 1. Read & frame: what is this surface, and what job does it do?

- State in one sentence what the design is and the job a user hires it for. If you cannot, it is underspecified: say so and ask.
- Map the flow: list the screens/states and how they connect (entry, branches, exit). Note what happens just before and just after this surface in the real workflow, not just inside the frame.
- Inventory states present vs missing: default, empty, loading, partial, error, max/overflow, multi-instance. Missing states are signal.
- **Walk the unhappy path, not just the happy one.** Trace every exception branch the flow implies and judge it as a first-class journey: variance / rejection / send-back and any approval or recount loop, empty and partial, error and retry, offline or no signal, overflow (long lists, long names), permission or role denied. The happy path rarely breaks; the design's real quality is how it handles the exceptions, and these branches often carry the Blockers.

### 2. Derive the lenses: who actually touches or is affected by this design? (the core step)

Infer the personas from the design itself. Read these signals off the screens:

- **Surface / device:** phone, mounted tablet POS, kitchen display, customer-facing display, desktop admin. Tells you who physically operates it.
- **Data altitude:** aggregate cross-store numbers imply HQ / regional; a single store's operational list implies store manager / staff; a personal task queue implies front-line staff.
- **Actions afforded:** configure / govern → HQ or admin; approve / authorize / override → manager; execute / enter / complete → staff; browse / select / pay → customer.
- **Scope selector:** all stores / region / one store / one station marks the altitude the design serves.
- **Environment & frequency cues:** rush, one-handed, gloves, wall-mounted, on the move, many-times-a-day vs once-a-period.

Then name who is **affected but not on screen**: who consumes this screen's output (HQ trusting the numbers), who inherits its state (the next shift), who waits on it (the customer in line).

**Mind the circularity.** You are inferring who the design is for from the design itself, then judging whether it serves them. That cannot catch the deepest failure: built well for the wrong user. Guard against it two ways. Lean on the affected-but-not-on-screen actors, since they are the ones the artifact tends to forget. And state your assumed primary user as a claim to confirm, not a fact: if the real primary user could be someone the screens do not put first, say so.

**Selection rule.** Include a lens only if they (a) operate this surface, (b) are materially affected by its output, or (c) are accountable for what it governs. Drop the rest. Aim for the 3 to 5 that matter. Write one line per lens you include, and one line for any obvious lens you deliberately exclude.

**Illustrative ladder: a multi-location QSR. Adapt or replace per design.** A single-store shop collapses the top three into "owner". A customer-facing screen swaps most of these for the customer. Use it as a thinking aid, not a checklist.

| Lens | Altitude | What they need from THIS design |
|---|---|---|
| HQ / Management | all stores | standardization, auditability, output that is comparable and trustworthy across every store, safe rollout |
| Area / Regional manager | a cluster of stores | spot the outlier store fast, drill in, intervene |
| Shop / Store manager | one store | get the task done right and on time, supervise, reconcile, see who did what |
| Front-line staff | one task, live | speed, few taps, hard to make a mistake, legible on the real device under pressure |
| Customer (only if customer-facing) | self | clarity, trust, speed, never exposed to internal mess |

**Secondary cross-cutting lenses, add only when the context invites them:** first run / new hire, accessibility (contrast, touch target, one-handed reach), localization (e.g. multiple locales, long or translated strings), field conditions (offline, glare, gloves), peak / rush pressure, error & recovery.

### 3. Review through each lens: does the design serve this person?

For each chosen lens, a short block:

- **Goal & context of use:** what they came to do, on what device, how often, under what pressure.
- **Walk their path:** trace the actual flow from their standpoint, screen by screen. "They land on X to do Y. Path: A → B. At B, [observation]."
- **Where it fails them:** the specific moment the design adds friction, hides what they need, lets them err (especially an irreversible or destructive action with no undo, or silent data loss), or breaks their mental model. Cite the frame.
- **Assumptions it bakes in:** what does this screen assume is true for *this* person that may not be: that they know the prior step or the current system state, that the content fits (long names, translated strings), that the data is there (no nulls, the list stays short), that the network or device cooperates. For each load-bearing one, ask what they see when it is false. The persona walk catches device and environment assumptions; this names the content, data, and knowledge ones it tends to miss.
- **Simpler-alternative breath (mandatory, once per lens):** does a step or screen need to exist for this person? Could it reuse an existing pattern, collapse a tap, or be done by the system instead? Name it if so.

### 4. Synthesize across lenses

- **Conflicts:** where serving one lens hurts another (HQ control vs staff speed). Name the tradeoff and who should win on this surface, and why.
- **Gaps:** any lens with no path, an orphaned state, or a job the design silently assumes someone else does.
- **Dominant concern:** the single thing that most undermines the design across lenses. This is the headline.

### 5. Report

**One screen by default. Findings read first, the rest only if needed.** The reader scans the Finding column and stops there; the Explanation and Suggestion are the fallback for when a finding line is not self-evident. A wall of equal-weight blocks is the failure mode this step exists to prevent.

Open with one line: the **verdict** (ship / fix-then-ship / rework / reject) plus the dominant concern.

Then one severity-sorted table holding every finding, Blockers first, then Majors, then Minors:

| Sev | Finding | Explanation | Suggestion | Where |
|-----|---------|-------------|------------|-------|
| Blocker / Major / Minor | self-contained one-liner, actionable on its own | why it matters and the consequence for the lens, read only if the finding is unclear | the fix as an outcome, kept to a phrase | frame name(s) |

Rules for the table:

- **Finding must stand alone.** Write it so a reader who never opens the other cells can still act. It is the default read, so it carries the weight.
- **Explanation is the fallback**, not a restatement of the finding: why-it-matters and the lens consequence only.
- **Suggestion is a direction, not a design.** A fix outcome ("blank the recount field", "gate submit behind an uncounted-items confirm"), never a grid or pixel spec. This is where the skill's critique-not-design rule lives.
- **Severity drives order:** Blocker (the surface fails its job or loses data), then Major (real friction or a broken mental model), then Minor (nit, copy, polish).
- **Show minors in the same pass**, one row each, cells kept terse. Do not defer them to "reply to expand". Group near-identical nits into one row when they share a fix (e.g. several plural/spelling typos as one row).
- **Merge same-root-cause findings** into one row instead of listing siblings.

Close with two one-liners: **Lenses** (who you ran, who you excluded, the primary-user assumption to confirm) and **The call** (the sharpest cross-lens conflict and who should win). Lens rationale and per-finding evidence stay off unless the reader asks.

Expand any row to full evidence only on request.

## Operating rules

- **No rubber-stamps.** "Looks good" is not an output. If you genuinely find little, say which lenses you ran and what you checked, so the user can judge the coverage.
- **Cite the pixels.** Every claim references a specific frame, screen, or element. No vague "this feels cluttered".
- **Lenses fit the design.** Never run a default persona list. Including "Customer" on a back-office screen, or "HQ" on a one-store shop, means step 2 was skipped.
- **Distinguish observation from inference.** "The design shows X" and "from lens L this implies Y" are different: keep them separate. Absence findings (no offline state, no error state) are valid, but label them as absence-to-confirm, not confirmed defects. Likewise, when a deliberate-looking decision reads as wrong but may have a rationale you cannot see, label it intent-to-confirm: name the concern and ask the designer's reason instead of asserting a defect. Use it sparingly, a genuine blocker is still a blocker.
- **Critique outcomes, suggest direction, do not design.** Flag the problem and where it bites, point at a direction, but do not dictate grid, hierarchy, sizing, or component choice. The designer owns the UX.
- **Concept over copy.** If steps 1 to 4 surface a model-level problem, lead with it. Never open with typos while a structural issue is unaddressed.
- **No flattery, no hedging.** State the finding.

## Reading a Figma design (when given a figma.com URL)

1. Parse the URL: `figma.com/design/:fileKey/:name?node-id=:a-:b` → fileKey and nodeId `a:b`.
2. `get_metadata` on the node for structure. If it is large, parse the saved file (jq/python or a subagent) to reconstruct the frame list, names, positions, and the flow order. Frame names usually reveal the screen sequence and states.
3. `get_screenshot` the overview first, then the distinct screens individually at higher `maxDimension` so the UI is legible. Review one state per concept; treat duplicate-named frames as state variants, not new destinations.
4. Trace the prototype connectors / flow labels to confirm which screen leads to which before judging the flow.
