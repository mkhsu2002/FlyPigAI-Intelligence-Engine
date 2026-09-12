# FlyPig AI Intelligence Engine

A reusable Topic Intelligence framework for building AI-native research systems that continuously collect signals, promote high-value evidence, test predictions, update theses, and publish only when new information materially changes what should be believed.

This repository generalizes the operating model proven in FlyPig AI's Physical AI workflow and turns it into a reusable template for other domains.

中文摘要：這是一套可重複使用的主題情報框架，用來將高頻訊號蒐集、證據篩選、預測驗證、論點更新與內容發布整合成一個可治理、可追溯、可複製的 AI 研究流程。

## Why this exists

Most automated research systems stop at one of two levels:

1. news collection
2. article generation

The FlyPig AI Intelligence Engine is designed to go further. It separates fast-moving signals from slower-moving evidence and even slower-moving theses.

```text
Topic Definition
    ↓
Source Collectors
    ↓
Raw Signals
    ↓
Signal Gate
    ↓
Evidence Tracker
    ↓
Prediction Registry
    ↓
Thesis Engine
    ↓
Publication Gate
    ↓
Insights / Reports / Alerts
```

The goal is not to publish more. The goal is to learn continuously and publish only when the evidence justifies a stronger conclusion.

中文摘要：核心不是「抓新聞後自動寫文章」，而是把新聞、證據、預測、論點與出版分成不同速度的層級。只有當新證據真的改變判斷時，才進入深度內容發布。

## Core design principles

- Primary sources first
- Deduplicate before creating evidence
- Separate confirmed facts, interpretation, and prediction
- Preserve original prediction text and first-published date
- Every prediction must include falsifiers
- Signals do not automatically become evidence
- Evidence does not automatically become articles
- Publication requires a thesis-level reason
- Risk audit precedes publication
- Git is the source of truth for state and policy
- Schedulers are execution layers, not sources of truth
- Execution must be idempotent and safe to retry

## Recommended architecture

The recommended default is hybrid rather than scheduler-only.

```text
ChatGPT Scheduler / Agent Runtime
        ↓
Research, source verification, semantic deduplication,
evidence scoring, thesis reasoning, drafting
        ↓
Git repository
Config + evidence + predictions + policy + content state
        ↓
GitHub Actions
Schema validation + tests + build + link checks + deploy
```

### Why this split

Use ChatGPT Scheduler or another agent runtime for tasks that require judgment, web research, source comparison, ambiguity handling, semantic deduplication, and editorial reasoning.

Use GitHub Actions for deterministic operations such as schema validation, linting, tests, static builds, metadata checks, link checks, and deployment.

中文摘要：建議採混合架構。AI Scheduler 負責「研究與判斷」，GitHub Actions 負責「可重現的驗證、建置與部署」。不要把所有事情都塞進單一排程工具。

## Repository structure

```text
templates/
  topic-intelligence/
    README.md
    topic-config.template.json
    source-registry.template.json
    evidence-tracker.template.json
    prediction-registry.template.json
    publication-policy.template.json
    scheduler-manifest.template.json
    weekly-research-runbook.md
    publication-gate.md
    risk-audit-checklist.md

docs/
  IMPLEMENTATION_GUIDE.md
  SCHEDULER_ARCHITECTURE.md

examples/
  physical-ai/
    README.md
    topic-config.json
    scheduler-manifest.json
    implementation-checklist.md
```

## Quick start

1. Copy `templates/topic-intelligence/` into a new topic workspace.
2. Define scope in `topic-config.json`.
3. Register source classes in `source-registry.json`.
4. Create initial falsifiable predictions in `prediction-registry.json`.
5. Define publication and risk rules.
6. Define scheduler jobs in `scheduler-manifest.json`.
7. Run one manual dry run before scheduling anything.
8. Verify deduplication, evidence promotion, prediction updates, publication gating, and safe no-op behavior.
9. Only after the dry run passes, activate scheduled execution.
10. Keep deterministic build and validation steps in GitHub Actions or another CI system.

For the full implementation sequence, see `docs/IMPLEMENTATION_GUIDE.md`.

中文摘要：先複製模板，再設定主題、來源、預測與發布規則。一定先做一次人工 dry run，確認去重、證據升級、預測更新、發布門檻與 no-op 都正常，再開啟正式排程。

## What changes between topics

Usually only these parts should change:

1. Topic scope
2. Source registry
3. Taxonomy
4. Initial predictions and falsifiers
5. Publication target
6. Scheduler cadence
7. Domain-specific risk rules

The evidence model, prediction integrity rules, publication gate, audit pattern, and scheduler separation should remain stable.

## Reference implementation

`examples/physical-ai/` maps the current FlyPig Physical AI workflow into this generic framework. It is intended as a working reference for building a new topic intelligence system.

## Status

Current maturity: `v0.2 framework draft`

The next engineering milestone is to add machine-readable schemas, validation scripts, and a bootstrapping utility that can generate a new topic project from this template.