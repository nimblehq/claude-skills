# House structure

Derived from the Metabase and Notion "what's new" emails. Both follow the same skeleton; reproduce it in Markdown.

## Skeleton

1. **Title** — `Product vX.Y is live` or a hook naming the marquee item.
   - Metabase: "Metabase 62 is now live! What's new?"
   - Notion: "Notion 3.6: Bring agents like Claude & Cursor into Notion"
2. **Intro (2-3 sentences)** — one sentence naming the release theme, then a "there's more" line that teases 2-3 highlights by name. Warm, direct.
3. **Hero section** — the single highest-impact capability. Illustration + benefit headline + what it does / why it matters + a link or next step.
4. **3-6 feature sections**, each:
   - **Benefit-led H2** (an outcome, not a component name).
   - 1-3 short sentences: what it does, then why it matters to the user.
   - A "how to get to it" line where relevant (`Settings → …`).
   - Illustration placeholder.
5. **"And a few more…"** — bulleted minor-but-nice items, one line each.
6. **Fixes & polish** — one short paragraph or a few bullets summarizing bug fixes. No PR numbers, no internal fixes.
7. **Sign-off** — friendly close; optional roadmap/link.

## Section anatomy (the repeating unit)

```
## <Benefit-led headline>

[ILLUSTRATION: <what to show> — caption: "<caption>" — alt: "<alt text>"]

<What it does, in plain language.> <Why it matters / what it unlocks.>

<How to reach it, if applicable: e.g. "Find it under Promotions → Rules.">
```

## Ordering

- Lead with the capability that affects the most users / is most visible.
- Cluster related capabilities; don't alternate topics.
- Keep the note skimmable: bold headlines carry the message on their own.

## Length

- Aim for 4-6 sections for a normal release; more collapses into "And a few more…".
- Every section earns its place by user value. If you can't state the user benefit in one sentence, it probably belongs in the drop list.
