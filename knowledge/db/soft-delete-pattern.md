---
topic: soft-delete-pattern
domain: db
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [foreign-keys-constraints, constraints-checks, gdpr-essentials-for-developers]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L0"}
---

## Concetto
*(Non studiato — calibrato L0.)*
deleted_at vs hard delete. Gotcha UNIQUE constraint con righe soft-deleted (serve UNIQUE partial con WHERE deleted_at IS NULL). Conflict con GDPR right-to-erasure.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0
