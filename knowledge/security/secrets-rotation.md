---
topic: secrets-rotation
domain: security
studied_first: null
last_reviewed: 2026-05-18
level: L1
confidence_layer: L1-L2
sources: []
connects_to: [env-variables, jwt-structure]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L1 — sa che si ruota la key + redeploy. Manca audit (è stata usata?), revoca esplicita, prevenzione re-commit, comunicazione team."}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L1.)*

Sequenza operativa di rotazione secret: 1) genera nuova key, 2) revoca esplicitamente la vecchia (Supabase/provider), 3) aggiorna env in tutti gli ambienti, 4) redeploy, 5) audit log per capire se la vecchia è stata usata nei minuti di esposizione, 6) git rewrite/segnalazione GitHub Secret Scanning per evitare archive cache, 7) postmortem.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L1

## Domanda di review
*(Sarà generata durante studio formale.)*
