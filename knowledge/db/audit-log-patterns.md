---
topic: audit-log-patterns
domain: db
studied_first: null
last_reviewed: 2026-05-18
level: L1
confidence_layer: L1-L2
sources: []
connects_to: [gdpr-essentials-for-developers, incident-response-basics, foreign-keys-constraints]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "L1 marginale — ha idea base 'trigger reagisce a azione'. Manca: tabella audit dedicata, struttura who/when/old/new, come si propaga auth.uid() dentro trigger Supabase."}
---

## Concetto
*(Non studiato formalmente — calibrato L1.)*
Tabella audit dedicata con (user_id, table, row_id, action, old_value jsonb, new_value jsonb, at timestamptz). Trigger Postgres AFTER INSERT/UPDATE/DELETE che inserisce automaticamente. In Supabase: `auth.uid()` accessibile dentro trigger.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L1
