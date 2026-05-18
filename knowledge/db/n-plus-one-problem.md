---
topic: n-plus-one-problem
domain: db
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [query-performance, sql-joins-advanced]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L0 — classico antipattern AI-generated (forEach + await su query). Da imparare a riconoscere a colpo d'occhio in review."}
---

## Concetto
*(Non studiato — calibrato L0.)*
1 query iniziale + N query in loop = N+1 round-trip al DB. Fix: batch fetch con WHERE...IN (...), eager loading via JOIN, oppure Promise.all parallelo (mitigante non risolutivo).

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0
