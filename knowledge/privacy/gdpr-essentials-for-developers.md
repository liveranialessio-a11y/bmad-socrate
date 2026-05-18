---
topic: gdpr-essentials-for-developers
domain: privacy
studied_first: null
last_reviewed: 2026-05-18
level: L1
confidence_layer: L1-L2
sources: []
connects_to: [soft-delete-pattern, audit-log-patterns, structured-logging]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L1 — coglie principio anonimizzazione (ordini stats). Manca distinzione cliente finale vs ristoratore, obblighi fiscali 10 anni, anonymization vs pseudonymization, gestione log/backup."}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L1.)*

GDPR operativo per developer: data minimization, right to erasure (anonimizzazione vs cancellazione, conflitto con obblighi fiscali italiani di conservazione fatture 10 anni), Data Processing Agreement con fornitori cloud, cosa è PII (no in log, no al LLM senza minimizzazione), consenso vs interesse legittimo. Pseudonymization (reversibile, ID interno → mapping) vs anonymization (irreversibile, dati aggregati o scrubbed).

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L1

## Domanda di review
*(Sarà generata durante studio formale.)*
