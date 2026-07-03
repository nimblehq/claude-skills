# Voice & rewriting

Derived from the Metabase and Notion examples. The goal: a non-technical client reads a headline and immediately knows what they can now do and why they'd care.

## Principles

- **Lead with the outcome, not the mechanism.** The user cares what they can do, not that a model or state machine exists.
- **Plain language.** No `STI`, `polymorphic`, `concern`, `enum`, `Pundit`, `scope`, `migration`, model/class names, or Jira keys in the output.
- **Second person, active voice.** "You can now…", "Your team can…".
- **Concrete benefit.** Say what it unlocks: faster, clearer, fewer steps, new thing possible.
- **Warm but tight.** Metabase/Notion are friendly and confident, never hypey. No "revolutionary", "game-changing", exclamation spam.
- **Honest.** Don't overstate scope or announce partial work as done.

## Do / don't

| Don't | Do |
| --- | --- |
| "Add EarningRule STI model mirroring PromotionRule" | *(drop — internal)* |
| "Add DateList promotion rule UI" | "Target promotions to specific dates" |
| "Make customer tags editable in backend (Tom Select tag input)" | "Edit customer tags inline — no more digging through menus" |
| "Add expires_at field to point transaction form" | "Set an expiry date when you award loyalty points" |
| "Add cost related columns to Manufacturing orders" | "See the cost of every manufacturing order at a glance" |
| "[COR-196] see value of Stock transfer Items" | "Know the value of stock as it moves between locations" |

## Rewrite recipe

For each consolidated capability:

1. State the **user action or outcome** in the headline (imperative or "You can now…").
2. One sentence: **what it does** in the product's own everyday terms.
3. One sentence: **why it matters** — the pain it removes or the new thing it enables.
4. Optional: **where to find it** in the UI.

## Intro pattern

> Hey there — <one line naming the release theme>. This release <verb-phrase summarizing the biggest win>, plus <highlight 2> and <highlight 3>.

Example (1.41.0):

> This release gives you far more control over promotions and loyalty — new ways to target discounts, a publish workflow for your rules and rewards, and richer customer segments. Plus clearer cost visibility across inventory and manufacturing.

## Tone guardrails

- Cut filler: "we're excited to", "we're thrilled", "it's worth noting".
- One idea per sentence.
- Prefer verbs over nouns ("filter by channel" > "channel-based filtering").
- If a benefit is speculative, describe the capability plainly instead of inventing an outcome.
