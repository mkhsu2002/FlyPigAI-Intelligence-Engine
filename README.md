# FlyPig AI Intelligence Engine

A reusable Topic Intelligence Template for building AI-native research, evidence tracking, prediction, and publication pipelines across different domains.

This repository standardizes the operating model proven in FlyPig AI's Physical AI research workflow and turns it into a reusable framework.

## Core idea

A topic intelligence system should not behave like a generic news scraper.

It should move through a governed sequence:

```text
Topic Definition
    ↓
Source Collectors
    ↓
Raw Signals
    ↓
Evidence Gate
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

The framework is designed to separate high-frequency signal collection from low-frequency thesis formation.

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
```

## What changes from topic to topic

Only a small number of components should need to change:

1. Topic scope
2. Source registry
3. Taxonomy
4. Initial predictions / theses
5. Publication target
6. Scheduler cadence

The core operating rules should remain stable.

## Design principles

- Primary sources first
- Deduplicate before creating new evidence
- Separate facts, interpretation, and prediction
- Preserve original prediction text and first-published date
- Every prediction must include falsifiers
- High-frequency signals do not automatically become articles
- Publication requires a thesis-level reason, not merely a new event
- Risk audit precedes publication
- Repository files are the source of truth for state and policy
- Schedulers are execution layers, not sources of truth

## Recommended execution architecture

```text
ChatGPT Scheduler / Codex / IDE Agent
        ↓
Topic config + source registry
        ↓
Research and reasoning
        ↓
Evidence / prediction state in Git
        ↓
GitHub Actions
Validation / build / deploy
```

ChatGPT Scheduler is useful for research, judgment, semantic deduplication and cross-source reasoning. GitHub Actions is better for deterministic validation, builds, schema checks and deployment.

## First template

Start with:

`templates/topic-intelligence/README.md`

That file defines the reusable Topic Intelligence lifecycle and the required artifacts for creating a new intelligence project.
