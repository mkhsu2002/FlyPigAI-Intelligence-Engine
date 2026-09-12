# Weekly Thesis Engine Runbook

## Goal

Turn the newest qualified evidence into an auditable update of the topic model, without turning the system into a news-summary engine.

## Step 1. Read the source of truth

Read:

- `topic-config.json`
- `source-registry.json`
- `evidence-tracker.json`
- `prediction-registry.json`
- `publication-policy.json`
- the most recent research log

Do not rely on conversation memory as the authoritative state.

## Step 2. Review existing collectors first

Before searching broadly, inspect the topic's existing collectors and event stores.

Goal:

- avoid rediscovering events already captured
- reuse existing verified evidence
- avoid duplicate publication

## Step 3. Add only strategic external signals

Search beyond existing collectors only for events that could materially affect:

- the topic stack
- a platform control point
- commercialization structure
- supply-chain structure
- standards or interfaces
- a registered prediction
- a falsifier

## Step 4. Verify sources

Prefer official, primary, technical, regulatory, academic, or standards sources.

For each candidate:

- confirm event date
- confirm subject identity
- confirm product / policy / deployment status
- distinguish announced, previewed, sampled, piloted, deployed, and commercially available states
- preserve unresolved source conflicts

## Step 5. Deduplicate

Check:

- source URL
- normalized subject
- event date
- existing collector IDs
- existing Evidence Tracker entries

If already represented, link rather than duplicate.

## Step 6. Score evidence

For each accepted item:

- assign category
- write a factual summary
- state uncertainties
- link affected predictions
- mark effect: supports / weakens / neutral / invalidates
- mark strength: weak / medium / strong
- explain rationale

## Step 7. Review counter-evidence

Do not search only for confirming signals.

For each prediction ask:

- what evidence this week works against it?
- has any falsifier become more plausible?
- is an apparently supportive signal actually vendor positioning only?

## Step 8. Update prediction confidence

Change confidence only when new evidence justifies it.

Do not modify:

- original prediction wording
- first-published date

If the idea changes materially, create a new prediction ID.

## Step 9. Decide publication

Apply `publication-policy.json`.

Possible outcomes:

```text
TRACKER ONLY
Evidence retained; no article.

MATERIAL UPDATE
Existing article materially updated.

NEW INSIGHT
New article or report justified.

HOLD
Evidence is important but publication risk is unresolved.
```

## Step 10. Run editorial risk audit

Before any public publication, verify:

- factual accuracy
- source-to-claim fit
- uncertainty language
- counter-evidence
- rights and attribution
- non-infringing original wording
- no invented commercial facts
- substantive depth

## Step 11. Write the research log

Record:

- run date
- candidate signals reviewed
- accepted evidence IDs
- rejected signals and reasons
- confidence changes
- status changes
- publication decision
- published or updated content
- watchlist

## Step 12. Validation and deployment

After agent research and content mutation, let deterministic CI handle validation, build, and deployment where available.
