---
topic: schema-normalization
domain: db
studied_first: null
last_reviewed: 2026-05-18
level: L1
confidence_layer: L1-L2
sources: []
connects_to: [foreign-keys-constraints, data-types-postgres]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L1+ — coglie problema items_json (no JOIN, no aggregazioni). Manca framework 1NF/2NF/3NF esplicito e tradeoff snapshot vs normalizzato per order_items."}
---

## Concetto
*(Non studiato formalmente — calibrato L1.)*
1NF/2NF/3NF, denormalizzazione consapevole (read-heavy, snapshot storico tipo `order_items.price`).

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L1+
