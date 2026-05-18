---
topic: oauth-flow-basics
domain: auth
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [jwt-structure, session-management, supabase-client-typescript]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L0 — risposta vaga 'richiesta https + scambio dati', nessun riferimento a redirect/code/callback/state"}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L0.)*

OAuth 2.0 Authorization Code flow + PKCE: redirect a provider (Google/GitHub), utente autorizza, callback con `code` temporaneo, server scambia code per access+refresh token, sessione creata. State parameter previene CSRF sul redirect.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0

## Domanda di review
*(Sarà generata durante studio formale.)*
