# Physical AI Implementation Checklist

Use this checklist when reproducing the reference implementation in another topic.

## Configuration

- [ ] `topic-config.json` exists and defines a narrow primary question
- [ ] included and excluded domains are explicit
- [ ] taxonomy is defined
- [ ] publication targets are known

## Sources

- [ ] source registry separates primary sources from discovery sources
- [ ] every production source has a trust level
- [ ] official-source verification is required before high-impact evidence is accepted

## Signals

- [ ] signal admission rules reject duplicates and pure marketing noise
- [ ] event identity is stable enough for idempotent reruns
- [ ] low-impact signals may remain signals without evidence promotion

## Evidence

- [ ] every evidence item links to at least one prediction or thesis
- [ ] effect is one of supports / weakens / neutral / invalidates
- [ ] strength is scored independently from direction
- [ ] factual summary is separated from interpretation

## Predictions

- [ ] each prediction has a stable ID
- [ ] original text and first date are immutable
- [ ] every prediction has at least one falsifier
- [ ] confidence changes are append-only or historically recoverable
- [ ] counter-evidence is retained

## Publishing

- [ ] signal publication and insight publication use different gates
- [ ] risk audit runs before publication
- [ ] high-risk content is held
- [ ] source disclosure is visible
- [ ] fact, interpretation and prediction are distinguishable

## Scheduling

- [ ] scheduler manifest exists
- [ ] every write-producing job has an idempotency key
- [ ] failure behavior is explicit
- [ ] no two active schedulers write the same state without a shared lock
- [ ] deterministic CI remains separate from research reasoning

## Dry-run verification

- [ ] duplicate-event test passes
- [ ] low-impact-event no-op test passes
- [ ] high-impact evidence-promotion test passes
- [ ] counter-evidence test passes
- [ ] high-risk publication hold test passes
- [ ] rerun/idempotency test passes

## Production readiness

- [ ] repository alone is sufficient to reconstruct current state
- [ ] collector has completed successfully at least once
- [ ] evidence promotion has been observed at least once
- [ ] prediction confidence update has been observed at least once
- [ ] publication gate has produced at least one intentional no-op
- [ ] CI validation passes on current main

中文摘要：只有當「重複事件不重複寫入、低影響事件不亂升級、反方證據不被忽略、高風險內容能被阻擋、重跑不產生副作用」都實際驗證後，才算完成 implementation。