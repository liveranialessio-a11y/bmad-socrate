---
topic: csrf-protection
domain: security
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [session-management, cors-policy, owasp-top10-web]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L0"}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L0.)*

CSRF (Cross-Site Request Forgery): un sito malevolo induce il browser dell'utente loggato a fare richieste autenticate verso la tua app (sfruttando il fatto che il browser allega cookie automaticamente). Difese: SameSite cookie (default Lax oggi neutralizza la maggior parte dei CSRF classici), anti-CSRF token espliciti per casi residui.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0

## Domanda di review
*(Sarà generata durante studio formale.)*
