---
topic: constraints-checks
domain: db
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [foreign-keys-constraints, data-types-postgres, transactions-acid]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L0"}
---

## Concetto
*(Non studiato — calibrato L0.)*
NOT NULL, UNIQUE (anche partial), CHECK constraints, deferred constraints. Validation DB-level vince su validation solo applicativa: regge anche se altro client (psql, SQL editor) inserisce.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0
