---
topic: migrations-versioning
domain: db
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [schema-normalization, foreign-keys-constraints]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L0 con ANTIPATTERN INTERIORIZZATO: ha detto 'modifico la migration vecchia' che è esattamente la cosa da non fare. Prima cosa da correggere nello studio formale."}
---

## Concetto
*(Non studiato — calibrato L0 con antipattern interiorizzato.)*
Regola d'oro: migration applicata = IMMUTABILE. Sempre nuova migration anche se sono 1000. Drift tra ambienti se si modifica una vecchia. Tool (Supabase/Prisma) calcolano hash e si bloccano.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0 (antipattern da correggere)
