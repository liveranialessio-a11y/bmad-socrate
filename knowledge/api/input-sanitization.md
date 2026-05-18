---
topic: input-sanitization
domain: api
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [request-validation, sql-injection-prevention]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L0 batch"}
---

## Concetto
*(Non studiato — calibrato L0.)*
Sanitize HTML (DOMPurify quando serve), escape per SQL (coperto da prepared statements), shell/eval (mai). Distinguere dove serve davvero vs teatro.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0
