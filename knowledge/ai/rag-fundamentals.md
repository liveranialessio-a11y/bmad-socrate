---
topic: rag-fundamentals
domain: ai
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L2-L3
sources: []
connects_to: [embeddings-basics, vector-db-comparison, chunking-strategies, retrieval-quality]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L0 — autodichiarato L1 ('AI che usa file indicizzati') ma sotto domanda L1 secca non regge"}
---

## Concetto
*(Non studiato — calibrato L0.)*
Retrieval-Augmented Generation. Quando serve (KB grande, contenuti che cambiano) vs NON serve (contesto piccolo entra nel prompt). Pipeline: embed query → retrieve top-k chunks → inject in prompt → LLM risponde con context.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0
