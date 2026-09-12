# TOPIC INTELLIGENCE TEMPLATE

## 1. Purpose

This template turns a broad topic into a governed, continuously updated intelligence system.

It is designed for domains where the goal is not merely to collect news, but to:

- detect important changes
- preserve evidence
- test theses over time
- generate falsifiable predictions
- distinguish noise from structural change
- publish only when new evidence materially improves understanding

Examples:

- Physical AI
- AI Agents
- Semiconductor supply chain
- Climate Tech
- Canadian housing policy
- Outdoor industry
- Licensing / IP business
- Cruise industry

## 2. Operating model

```text
TOPIC DEFINITION
      ↓
SOURCE COLLECTORS
      ↓
RAW SIGNALS
      ↓
SIGNAL GATE
      ↓
EVIDENCE TRACKER
      ↓
PREDICTION REGISTRY
      ↓
THESIS ENGINE
      ↓
PUBLICATION GATE
      ↓
ARTICLE / REPORT / ALERT
```

The key rule is that each layer has a different job.

### Source Collectors

Find new events and documents.

They should be high recall and may run frequently.

### Signal Gate

Decide whether a discovered event is meaningful enough to retain.

Not every discovered item becomes evidence.

### Evidence Tracker

Stores durable evidence that may support, weaken, invalidate, or remain neutral toward a registered thesis or prediction.

### Prediction Registry

Stores predictions in a form that cannot be rewritten after evidence arrives.

### Thesis Engine

Periodically examines accumulated evidence, counter-evidence, confidence changes, structural patterns, and prediction performance.

### Publication Gate

Prevents routine signal accumulation from becoming low-value article spam.

A new article or major article update should occur only when there is a thesis-level reason.

## 3. Required files for every topic

A new topic should contain the following artifacts:

```text
<topic>/
  README.md
  topic-config.json
  source-registry.json
  evidence-tracker.json
  prediction-registry.json
  publication-policy.json
  scheduler-manifest.json
  research-log/
```

## 4. Topic configuration

`topic-config.json` defines what the project is actually studying.

It must include:

- topic ID
- topic name
- central question
- scope
- explicit exclusions
- geography
- taxonomy
- preferred source types
- evidence categories
- intended audience
- publication targets

A topic without explicit exclusions will eventually become a generic news collector.

## 5. Source architecture

Sources should be grouped by role rather than placed into one undifferentiated list.

Recommended source classes:

```text
Primary official sources
Company / regulator / government / standards body

Research sources
Papers / labs / universities / technical repositories

Market implementation sources
Deployment / procurement / production / integration evidence

Discovery sources
Media / newsletters / social / community discussions
```

Discovery sources may identify an event, but material evidence should seek primary confirmation whenever possible.

## 6. Signal qualification

A signal should enter the Evidence Tracker only when it does at least one of the following:

- changes a tracked market or technology layer
- affects a prediction or thesis
- provides real deployment evidence
- changes product availability or commercialization status
- changes platform control points
- introduces a new standard, interface, reference design, runtime, business model, procurement pattern, or supply-chain structure
- materially weakens a current assumption

Routine marketing, opinion, duplicate coverage, and generic thought leadership should not become tracked evidence.

## 7. Evidence schema

Each evidence item should contain:

- immutable ID
- event date
- tracker-added date
- geography
- category
- subject
- source type
- source URL
- concise factual summary
- confidence / source quality
- linked predictions
- effect on each prediction
- evidence strength
- publication status
- uncertainty or caveats

Allowed prediction effects:

```text
supports
weakens
neutral
invalidates
```

Allowed strength should be defined by the topic, but a recommended base vocabulary is:

```text
weak
medium
strong
```

Direction and strength must remain separate.

## 8. Prediction integrity

Predictions are not marketing claims.

Each prediction must contain:

- Prediction ID
- Original prediction text
- First-published date
- Initial confidence
- Current confidence
- Status
- Supporting evidence IDs
- Weakening evidence IDs
- Falsifiers
- Review history

The original prediction text and first-published date must never be rewritten after evidence arrives.

If the thesis evolves enough to require different wording, create a new prediction ID.

## 9. Falsifiers

Every prediction must include at least one plausible condition that would prove it wrong or materially weaken it.

Examples:

- predicted standard fails to gain cross-vendor adoption
- dominant architecture becomes fully vertical rather than modular
- supposed software control point remains economically subordinate to hardware vendors
- portability does not materialize across deployments

A prediction without a falsifier is commentary, not a tracked forecast.

## 10. Thesis review

A scheduled Thesis Engine review should answer:

1. What meaningful evidence was added since the last review?
2. Which predictions gained support?
3. Which predictions weakened?
4. Did any falsifier become more plausible?
5. Did a new structural pattern emerge?
6. Did previously separate signals converge into one thesis?
7. Does any registered prediction need a confidence change?
8. Is a new prediction justified?
9. Is publication warranted?

The review should explicitly separate:

### Confirmed evidence

What the sources directly establish.

### Interpretation

What the evidence plausibly means.

### Prediction

What may happen next and can later be tested.

## 11. Publication gate

Do not publish just because there is new information.

Publication should require at least one of these conditions:

- thesis changed materially
- major uncertainty was resolved
- counter-evidence changed confidence
- multiple independent signals formed a structural pattern
- a prediction crossed a confidence threshold
- a falsifier was activated
- enough evidence exists for a defensible new forecast
- a development changes the expected value chain or control point

Otherwise update the tracker only.

## 12. Publication quality requirements

Before publication, verify:

- claims match sources
- roadmap, prototype, pilot, sample, production, and general availability are not collapsed into one state
- vendor performance claims remain attributed
- no unsupported pricing, customer, partnership, lead-time, shipment, or benchmark claim is invented
- conflicting sources are surfaced rather than silently reconciled
- original reporting language is used rather than close paraphrase
- third-party media rights are documented
- trademark or endorsement ambiguity is avoided
- counter-evidence is represented fairly
- the article contains substantive new information beyond existing work

## 13. Risk levels

Recommended editorial risk model:

### Low

Evidence is well supported and language is appropriately qualified.

Action: publish.

### Medium

Evidence contains unresolved source conflicts, vendor-only claims, uncertain availability, or interpretive risk.

Action: publish only with explicit attribution and uncertainty.

### High

Material unsupported claim, source fabrication, misleading availability, rights issue, false commercial implication, or close-copy risk.

Action: hold publication.

## 14. Scheduler architecture

The scheduler is an execution layer, not the source of truth.

All durable state and policies should live in the repository.

Recommended jobs:

```text
Collector Job
Frequent
Find and record eligible source events

Publisher Job
Frequent or daily
Turn approved signals into structured reporting

Audit Job
Daily or event-driven
Recheck newly published or materially changed content

Thesis Engine Job
Weekly
Update evidence relationships, confidence, falsifiers, and insights

Deep Review Job
Monthly or quarterly
Evaluate prediction performance and topic architecture
```

## 15. ChatGPT Scheduler vs GitHub Actions

Use an AI scheduler or agent runtime for:

- web research
- semantic deduplication
- source interpretation
- evidence scoring
- cross-source reasoning
- thesis review
- prediction updates
- editorial drafting

Use GitHub Actions for:

- schema validation
- deterministic tests
- build
- lint
- broken-link checks
- sitemap generation
- deployment
- regression validation

Recommended model:

```text
Agent Scheduler
Research + judgment
       ↓
Git repository
State + policies
       ↓
GitHub Actions
Validation + build + deploy
```

## 16. Idempotency

Every automated stage should be safe to rerun.

Minimum protections:

- stable event IDs
- source URL deduplication
- normalized subject names
- canonical slugs
- duplicate prediction-link detection
- publish-only-if-not-existing checks
- explicit publication state

Never depend only on the scheduler remembering what it did last time.

## 17. Research log

Each thesis iteration should create a short machine-readable or Markdown record containing:

- run date
- new evidence IDs
- rejected signals and reason
- confidence changes
- prediction status changes
- new counter-evidence
- publication decision
- publication outputs
- unresolved watchlist

This makes the intelligence process auditable.

## 18. New topic bootstrap

To create a new topic:

1. Copy this template directory.
2. Rename the topic folder.
3. Fill `topic-config.json`.
4. Populate `source-registry.json`.
5. Create 3 to 10 initial falsifiable predictions.
6. Define the publication target.
7. Configure the scheduler manifest.
8. Run one manual research iteration.
9. Review false positives and taxonomy gaps.
10. Enable recurring jobs only after the first manual run is satisfactory.

## 19. Maturity stages

### Stage 0: Manual research

Human or agent performs one-off research.

### Stage 1: Signal collection

Recurring sources and deduplication established.

### Stage 2: Evidence tracking

Signals are connected to a durable tracker.

### Stage 3: Prediction engine

Falsifiable predictions and confidence history exist.

### Stage 4: Publication engine

Evidence-driven writing and publication gates are automated.

### Stage 5: Intelligence product

Multiple topics share common schemas, scheduler tooling, dashboards, and prediction scorecards.

## 20. Definition of done for a topic

A topic is operational when:

- source scope is explicit
- exclusions are explicit
- collector workflow is reproducible
- evidence tracker is populated
- predictions contain falsifiers
- confidence changes are auditable
- publication requires a gate
- risk audit exists
- scheduler jobs are documented
- runs are idempotent
- a new maintainer can understand the system without relying on chat history

## 21. Strategic objective

The end product is not a large news archive.

The end product is a compounding knowledge asset containing:

```text
Evidence history
+
Prediction history
+
Confidence changes
+
Thesis evolution
+
Published analysis
```

Over time this allows the system to answer not only:

"What happened?"

but also:

"What does it mean?"

"What should change in our model?"

and eventually:

"How accurate have our predictions been?"
