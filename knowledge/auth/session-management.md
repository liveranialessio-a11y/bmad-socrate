---
topic: session-management
domain: auth
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [jwt-structure, csrf-protection, supabase-client-typescript]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L0 — crede che JWT in localStorage sia normale (antipattern). Non sa cosa è httpOnly."}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L0.)*

Cookie httpOnly + Secure + SameSite vs localStorage per JWT. Refresh token rotation. Access token short-lived vs refresh token long-lived. XSS può rubare localStorage; httpOnly cookie no.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0

## Domanda di review
*(Sarà generata durante studio formale.)*
