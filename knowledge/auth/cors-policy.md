---
topic: cors-policy
domain: auth
studied_first: null
last_reviewed: 2026-05-18
level: L1
confidence_layer: L1-L2
sources: []
connects_to: [csrf-protection, env-variables, middleware-pattern]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L1 marginale — sa 'lista domini consentiti via env'. Non sa default behavior, non sa da cosa CORS NON protegge (è browser-side only)."}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L1.)*

CORS = meccanismo del browser per limitare cross-origin requests. Header `Access-Control-Allow-Origin` lato server. Preflight OPTIONS per richieste "complesse". Punto critico: CORS protegge il browser dell'utente, NON il server — un attaccante che fa richiesta server-to-server ignora CORS.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L1 marginale

## Domanda di review
*(Sarà generata durante studio formale.)*
