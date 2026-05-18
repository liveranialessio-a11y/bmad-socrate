---
topic: sql-injection-prevention
domain: security
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [owasp-top10-web, request-validation]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L0"}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L0.)*

SQL injection: input utente concatenato in query SQL grezza permette di eseguire SQL arbitrario. Prepared statements (parametrized queries) lo prevengono. Supabase client `.from().eq()` è safe by default. Caso edge: RPC con stringhe dinamiche, raw SQL via `pg` library senza prepared, ORM con `raw()` non sanitizzato.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0

## Domanda di review
*(Sarà generata durante studio formale.)*
