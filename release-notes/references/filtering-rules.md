# Filtering & consolidation rules

Three passes: fetch every PR's content, a cheap tag pass, then a semantic pass. The fetch and semantic passes are where the value is — do not skip them.

## Pass 0 — Fetch every PR (source of truth)

Titles are unreliable in both directions — they hide reverts and under-describe what shipped. So before filtering, retrieve each PR's content through the **GitHub MCP tools** or the **`gh` CLI** (never a browser — see SKILL.md → "Source access").

For each PR number in the changelog, capture:

- **title, body, state** (merged vs closed-unmerged — a closed-unmerged PR did not ship)
- **labels** (corroborate/override the changelog tag)
- **changed files** — the strongest signal for the internal-scaffolding test (all under `db/migrate`, `app/models`, `app/policies`, specs → plumbing; templates/views/components/forms → user-facing surface)
- **revert signals** in the body: "Reverts #NNNN", "This reverts commit …", "revert of #NNNN", or a linked reverting/reverted PR
- **backreferences** — a clean-looking feature PR may be reverted by a *later* PR that it never mentions. For each feature PR `#N`, also run `gh search prs "Reverts #N"` (see the revert cascade below); reading `#N` alone will not reveal it

`gh pr view <N> --json title,body,state,labels,files` (add `gh pr diff <N>` when the file list is ambiguous), or the MCP equivalent (`pull_request_read` / `get_pull_request` + files). Fetch each PR once and cache; batch for large releases. This pass is what makes the revert cascade and the detail level reliable — it is the reason the process takes longer, and it is not optional.

## Pass 1 — Tag filter (coarse)

| Tag / signal | Default action |
| --- | --- |
| `[feature]` | Candidate — send to Pass 2 |
| `[bug]` | Candidate, but usually summarized (see Bugs below) |
| `[chore]` | Drop, **except** reverts (see cascade) and chores that are secretly user-facing |
| Version bump, dependency bump, CI/build, test-only, i18n fixes, docs, schema sync | Drop |

Tags lie in both directions. A `[chore]` can ship a real UX win (`Use customer combobox on new point transaction form`), and a `[feature]` is often pure plumbing. Never decide on the tag alone.

## Pass 2 — Semantic filter (the real work)

### Internal-scaffolding test — DROP if the title is only about:

- a **model / migration / table / column** (`Add X model`, `Add currency to tiers`, `Add event_metric_aggregations cache table`)
- a **state machine / concern / STI hierarchy** (`Add Publishable state machine`, `Add EarningRule STI model mirroring PromotionRule`)
- a **policy / authorization** (`Add QuestionPolicy`, `Apply Pundit to …`)
- a **controller / service / API endpoint** with no described UI (`Add Api::V1::EventMetricsController`, `EventMetric::EvaluateFiltersService`)
- **scaffolding language**: "Scaffold", "foundation", "mirroring", "STI", "concern", "Part 1" of something with no later parts present

These are ingredients, not dishes. They may still *belong* to a shipped capability — fold them into that capability's story; don't list them.

### Keep as user-facing when the title implies someone can see or do something:

- new admin/user action, screen, form field, filter, report, column *in the UI*
- a new selectable type the user picks (`Add Channel promotion rule`, `Add DateList promotion rule UI`, `ExternalGateway payment method type`)
- inline editing / UX affordances (`Make customer tags editable`)
- visible values/costs (`see value of Stock transfer Items`, `Add cost related columns to Manufacturing orders`)

### Revert cascade

Use the Pass 0 fetch, not just the changelog text. Reverts hide in four places:

1. **Titled reverts** in Chores/Others — `Revert #2605: calculator-driven price override…`.
2. **Body reverts** — a PR whose fetched body says "Reverts #NNNN" / "This reverts commit …" even though its title doesn't say so.
3. **Closed-unmerged PRs** — a PR listed in the changelog whose fetched state is closed-but-not-merged never shipped.
4. **Backreference from a later PR** — the feature PR itself looks perfectly shipped; only a *different, later* PR undoes it. Reading the feature PR alone can never catch this — you must look for who points back at it.

For each, remove **both** the revert **and** the original PR it targets (and drop closed-unmerged PRs outright). That feature did not ship. When a revert points at a PR that isn't in this release's changelog, note it in the drop list so the user can confirm — the target may have been announced in an earlier release and now needs a correction.

**Detect method 4 explicitly.** Do not trust the feature PR to advertise its own reversal. For every surviving feature PR `#N`, search for PRs that revert it:

```
gh search prs --repo <owner>/<name> "Reverts #N" --json number,title,state
```

(or scan every fetched PR body in the release for a `#N` backreference). If a merged PR reverts `#N`, drop `#N`.

#### Worked example — the "clean feature" trap (okya-web #2605)

`#2605 [feature] Allow calculator-driven price override on CreateOrderLineItem promotion` is the exact case where per-PR reading fails:

- Merged, detailed body, integration specs, **and a real UI change** (`backend/promotion_actions/_form.html.erb`) — every signal says "announce this: buy X, get Y at fixed price Z".
- Three weeks later, `#2818 [chore] Revert #2605: …` (merged) undoes all 5 files (**+36 / −256**, the mirror of #2605's +256 / −36).
- Both shipped in the **same** release (`#2820 Release - 1.41.0`) → net-zero. Announce nothing.

Here #2818's title named the target, so title-scanning happened to work. Had it been titled `[chore] Restore CreateOrderLineItem to pre-calculator behavior`, only the **body** (`Reverts #2605`) or the **`gh search prs "Reverts #2605"`** backreference would have caught it. Always run the backreference search; never rely on the revert being clearly titled.

### Part-N / incompleteness

- Group `Part 1` / `Part 2` of the same title into one capability.
- If only early parts are present ("Part 1" with no "Part 2"), or every PR for a capability is backend/model/policy with no UI PR, treat the capability as **not shipped for users** → hold it, or note "groundwork for X, coming soon" in the internal drop list. Do not announce it.

### Bugs

Don't list bugs one by one. Summarize into a short "Fixes & polish" line unless a fix removes real user pain worth calling out (e.g. correct timezone on business date, exact-match search). Drop internal-only bug fixes (`Fix worktree:setup on Rails 7.2`, `Wrap data migration in safety_assured`).

## Consolidation — cluster survivors into capabilities

Group by **Jira epic prefix** (`COR-`, `WEB-`) and **shared domain nouns**. Each cluster becomes one section with one benefit headline.

### Worked example — `Release - 1.41.0` (~130 raw items → ~4 client sections)

| Client capability (headline theme) | Absorbs (examples) | Drop the rest as plumbing |
| --- | --- | --- |
| **Promotions: more ways to target & price discounts** | DateList, Channel, Store Category, Customer, MinimumQuantity rules; FixedPrice calculator; unify include/exclude | rule STI models, concerns, backfills |
| **Loyalty: publish workflow + richer earning rules & segments** | Publishable submit/publish for earning rules/rewards/promotions; birthday & birth-date & metadata segments; expires_at on point transactions | Earning STI/model renames, CustomerBalance model, currency plumbing |
| **Inventory & manufacturing cost visibility** | Stock transfer item value; cost columns on manufacturing orders; ingredient cost on produce; complete/cancel manufacturing order | average_cost/balance columns, stock-movement workflow internals |
| **Small improvements** (bulleted) | Inline customer tag editing; customer combobox on point-transaction form; exact-match user search | — |

Hold / omit with reason:

- **Checklists & SOPs (Questions/Answers)** — all models/policies/STI, no UI → groundwork, coming soon.
- **Event Metrics** — controllers/services/aggregation only, one form → mostly plumbing, hold.
- **Payments (PaymentSession/SetupSession, ExternalGateway)** — foundation + authz, no user flow → hold except if a user-facing payment method actually shipped.
- **Reverted:** price override on CreateOrderLineItem (#2605, reverted by #2818) → excluded.
