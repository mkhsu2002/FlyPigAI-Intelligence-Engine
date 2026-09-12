# Implementation Guide

This guide describes how to implement a new FlyPig AI Topic Intelligence system from scratch.

中文摘要：本文件提供從零建立一個 Topic Intelligence 專案的標準步驟，包含設定、dry run、排程、驗證、發布與失敗處理。

## 1. Define the topic boundary

Create `topic-config.json` from the template.

Minimum decisions:

- topic name
- research question
- included domains
- excluded domains
- geography
- taxonomy
- target audience
- publication destination

A good topic is narrow enough to reject irrelevant news but broad enough to accumulate evidence over time.

Validation question: can two independent operators read the config and agree on whether a new event is in scope?

## 2. Build the source registry

Create `source-registry.json`.

Classify sources by role instead of mixing everything into one list:

- primary official sources
- regulators / governments
- company product and investor sources
- research institutions
- trusted secondary discovery sources
- community / weak-signal sources

For every source record, define:

- source owner
- source type
- expected update cadence
- collection method
- trust level
- whether it can support a factual claim directly

Discovery sources may help find an event, but high-impact evidence should be confirmed with primary sources whenever possible.

## 3. Define signal admission rules

A raw event becomes a signal only when it is new, in scope, and materially different from what is already known.

Required checks:

1. Is the event genuinely new?
2. Is it already represented in the tracker?
3. Is it only marketing language without a concrete change?
4. Does it have an authoritative source?
5. Does it alter product, deployment, policy, commercial status, supply chain, platform architecture, or another defined topic dimension?

If the answer is no, no-op.

## 4. Create the initial prediction registry

Do not start with dozens of predictions. Start with three to seven.

Every prediction must include:

- stable ID
- original prediction text
- firstPublishedAt
- confidence
- current status
- supporting evidence IDs
- weakening evidence IDs
- falsifiers

Never rewrite the original prediction after new evidence arrives.

A prediction without a falsifier is commentary, not a tracked prediction.

## 5. Define evidence promotion rules

Signals and evidence are different objects.

Promote a signal into the evidence tracker only when it has material implications for a registered prediction or thesis.

Each evidence item should contain:

- evidence ID
- date
- geography
- category
- subject
- primary source
- factual summary
- linked predictions
- effect: supports / weakens / neutral / invalidates
- strength
- publication status

This prevents the evidence tracker from becoming another news database.

## 6. Define publication gates

A new event is not sufficient reason to publish an article.

Publish or materially update a long-form insight only when at least one condition is met:

- a thesis materially changes
- an important uncertainty is resolved
- several independent signals form a structural pattern
- a new falsifiable prediction becomes defensible
- an existing prediction is materially weakened or invalidated

Before publication, run the risk audit and verify source-to-claim fit.

## 7. Define scheduler jobs

Use `scheduler-manifest.json` to describe jobs independently of the runtime.

Recommended job classes:

1. Collector
2. Signal Publisher
3. Risk Auditor
4. Thesis / Prediction Engine
5. Deterministic Validator / Builder

Recommended default ownership:

- agent scheduler: collectors, research, reasoning, drafting
- CI / GitHub Actions: validation, tests, build, deployment

## 8. Perform a manual dry run

Before activating schedules, manually run the complete flow once.

Required dry-run test cases:

### Case A: duplicate event
Expected result: no new signal, no new evidence, no publication.

### Case B: valid low-impact event
Expected result: signal recorded; no evidence promotion if no prediction impact; no long-form article.

### Case C: high-impact event
Expected result: source verified, evidence created, linked prediction updated, publication gate evaluated.

### Case D: counter-evidence
Expected result: prediction confidence may decline; weakening evidence must be retained rather than ignored.

### Case E: high-risk article
Expected result: publication blocked or held.

### Case F: rerun same job
Expected result: idempotent no-op; no duplicate files or duplicated evidence.

## 9. Activate schedules gradually

Do not enable every job at once.

Suggested sequence:

1. collector only
2. verify one week of signals
3. enable publisher
4. verify publication quality
5. enable risk audit
6. enable thesis engine
7. enable automated long-form publication only after confidence is established

This staged rollout makes failures easier to isolate.

## 10. Validation checklist

A topic implementation is ready only when all items pass:

- scope is machine-readable
- source registry exists
- deduplication rule exists
- signal admission rule exists
- evidence promotion rule exists
- predictions have falsifiers
- prediction history is immutable
- publication gate exists
- risk audit exists
- scheduler jobs are documented
- reruns are idempotent
- failure path is defined
- public output clearly distinguishes fact, interpretation, and prediction
- repository remains the source of truth

## 11. Failure handling

Default behavior should be fail-safe, not publish-safe.

Examples:

- official source unavailable: hold
- source conflict: preserve conflict and lower confidence
- unclear product availability: do not infer shipment status
- media rights unknown: do not use asset
- schema validation fails: do not deploy
- scheduler fails halfway: rerun must not duplicate previous writes

## 12. Definition of done

A new topic is considered implemented when:

1. a manual dry run has passed all six test cases
2. at least one collector job has completed successfully
3. deduplication has been verified
4. one evidence promotion has been verified
5. one prediction update has been verified
6. one publication-gate no-op has been verified
7. CI validation has passed
8. state can be reconstructed from repository files without relying on scheduler memory

中文摘要：完成標準不是「排程有跑」，而是能證明去重、證據升級、反方證據、預測更新、發布門檻、風險阻擋、重跑安全與 repo state reconstruction 都正常。