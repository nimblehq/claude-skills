# Filtering & consolidation rules

Two passes: a cheap tag pass, then a semantic pass. The semantic pass is where the value is — do not skip it.

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

Scan Chores/Others for `Revert #NNNN` (e.g. `Revert #2605: calculator-driven price override…`). Remove **both** the revert chore **and** the original PR it targets. That feature did not ship.

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
