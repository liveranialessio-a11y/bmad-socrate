# Last Session — 2026-05-18 (sessione 3 — creazione plan)

## Cosa è successo

Sessione lunga di creazione completa del learning plan `solutions-architect-ai-reviewer`.

Phase 1 (draft template ricco): 8 nodi, 99 concept con descrizioni, mini-project ancorati a menu-qr-saas, goal raffinato in "Senior reviewer di codice AI-generated" anziché "Solutions Architect generico".

Phase 2 (gap review): 10 gap identificati, 7 accettati (GDPR, pagination, file upload, forms RHF+Zod, llm-pricing split, audit-log, adr-format-basics introdotto al Nodo 2), 1 nota overlap caching, 2 deferiti (WCAG, Postgres triggers).

Phase 3 (calibration): tutti i 109 concept calibrati in sessione unica (~3h equivalenti). Risultato: 10 L1 + 99 L0, zero L2+. Tutti i concept a path "study".

Phase 4 (persist + activate): aggiornati 4 concept file esistenti, creati 105 nuovi, riscritto knowledge-index.md, aggiunta baseline table al plan, status: active, plan spostato in plans/active/.

## Plan archiviato

`ai-solutions-builder` archiviato come `test-plan-discarded` (plan precedente di prova).

## Plan attivo

`solutions-architect-ai-reviewer` — Nodo 1 pending. Prossimo step: iniziare studio formale concept dal Nodo 1 (Security & Authentication).

## Pattern emersi (importanti per studio)

- Lacuna comune sui L1: sa il "cosa" ma non l'"operativo" (RLS sa cos'è, non scrive; JWT rubato sa che dura 1h, non sa cosa fare).
- Punti ciechi pericolosi: JWT in localStorage considerato normale (antipattern), CORS percepito come protezione completa (non lo è), migration considerate modificabili (antipattern grave).
- Prima cosa da correggere nel Nodo 2: regola d'oro migration immutabili.
- Nodo 8: flag `pending_verification`. Possibile L1 da esperienza pratica su `estimation-honest`, `communication-non-tech`, `saying-no-tech-debt`. Verificare con "raccontami l'ultimo preventivo".

## Prossima sessione

1. Iniziare studio formale Nodo 1 partendo da `jwt-structure` (già L1, target L2 con applicazione operativa)
2. Oppure aprire dal concept più "applicato" che ha più impatto sicurezza: `rls-multi-tenant` (scrivere policy SQL completa)
3. Oppure dal concept con antipattern interiorizzato `migrations-versioning` (Nodo 2 first thing)

Decidere in apertura sessione 4.
