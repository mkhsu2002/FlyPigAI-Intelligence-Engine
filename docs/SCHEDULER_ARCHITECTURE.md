# Scheduler Architecture

The FlyPig AI Intelligence Engine is runtime-agnostic. Scheduling is an execution concern, not the source of truth.

中文摘要：本框架不綁定單一排程工具。排程只是執行層，真正的 state、policy、evidence 與 prediction 必須保存在 repo。

## Recommended default

Use a hybrid architecture:

```text
Agent Scheduler
Research / reasoning / source verification / semantic judgment
        ↓
Git repository
State / policy / evidence / prediction / content
        ↓
GitHub Actions or CI
Validation / build / deploy
```

## What ChatGPT Scheduler is good at

Use ChatGPT Scheduler when a task requires:

- open-ended web research
- source comparison
- semantic deduplication
- ambiguous classification
- evidence scoring
- prediction reasoning
- editorial drafting
- exception handling that benefits from language-model judgment

It is especially effective during early-stage topic incubation because it minimizes custom orchestration code.

## What GitHub Actions is good at

Use GitHub Actions for deterministic tasks:

- JSON schema validation
- linting
- tests
- static build
- link checking
- metadata validation
- sitemap generation
- deployment
- repeatable scripts

## When another scheduler may be needed

Consider moving or duplicating scheduling outside ChatGPT when:

- active task limits become a bottleneck
- you need execution more frequent than the supported cadence
- enterprise audit logs or SLA are required
- jobs must be centrally managed across many topics
- exact cost accounting per job is required
- secrets, queues, retries, concurrency and dependency graphs need engineering-level control
- the system becomes a multi-tenant product

Possible runtimes include GitHub Actions cron, Cloudflare Workers Cron, serverless schedulers, workflow orchestration systems, or an internal agent runtime.

## Runtime abstraction rule

Every scheduled job should be describable independently from the runtime with:

- job ID
- purpose
- cadence
- inputs
- outputs
- source policy
- idempotency key
- failure behavior
- notification behavior
- write permissions

That description belongs in `scheduler-manifest.json`.

The runtime-specific schedule is an implementation detail.

## Migration rule

Never activate two independent schedulers for the same write-producing job unless they share a hard idempotency lock.

Safe migration:

1. implement the new runtime
2. run it in read-only or dry-run mode
3. compare output with the current scheduler
4. enable writes on the new runtime
5. disable the previous writer
6. verify no duplicate state was created

## Current recommendation for FlyPig

For a small number of exploratory intelligence topics, use ChatGPT Scheduler as the primary research scheduler and GitHub Actions as the deterministic CI / publishing layer.

Do not replace the working Physical AI schedules merely for architectural purity. Standardize the manifests first. Revisit runtime migration only when scale, cost, observability, or task limits create a real constraint.

中文結論：目前最適合的是 ChatGPT Scheduler 負責研究型排程，GitHub Actions 負責驗證與部署。等到議題數量、成本控管、SLA 或排程上限真的成為問題，再遷移到更工程化的 scheduler。