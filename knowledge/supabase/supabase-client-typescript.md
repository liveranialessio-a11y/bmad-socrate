---
topic: supabase-client-typescript
domain: supabase
studied_first: null
last_reviewed: 2026-05-18
level: L0
confidence_layer: L1-L2
sources: []
connects_to: [server-vs-client-components, session-management, app-router-fundamentals]
pending_review: null
review_failures: 0
applications:
  - {project: calibration-solutions-architect-ai-reviewer, date: 2026-05-18, note: "calibrato L0 — non sa la differenza createServerClient vs createBrowserClient, gap già noto da last-session"}
---

## Concetto
*(Non studiato formalmente — calibrato durante creazione plan solutions-architect-ai-reviewer con level L0.)*

In Next.js App Router il client Supabase ha due factory:
- `createServerClient` (per server components, route handlers, server actions — legge cookie SSR)
- `createBrowserClient` (per client components — legge da localStorage/sessionStorage)

Usare il wrong client nel context wrong = sessione non recuperata, auth rotta.

## Applicazioni
- solutions-architect-ai-reviewer calibration 2026-05-18 — L0

## Domanda di review
*(Sarà generata durante studio formale.)*
