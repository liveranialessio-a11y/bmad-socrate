---
topic: nodejs-runtime
domain: infra
studied_first: 2026-05-22
last_reviewed: 2026-05-22
level: 0
confidence_layer: L1
sources: []
connects_to: [env-variables, server-actions, edge-vs-server-runtime]
pending_review: null
review_failures: 0
applications: []
---

## Concetto

Da studiare in sessione dedicata.

Impalcatura minima nota: JavaScript nasce nel browser (motore V8 di Chrome). Node.js prende quel motore e lo fa girare standalone su server Linux — stessa sintassi, fuori dal browser. Next.js usa Node.js come runtime per Server Actions e Route Handlers.

## Da approfondire

- Event loop e non-blocking I/O
- V8 engine vs browser JS
- Come Node.js gestisce richieste HTTP simultanee
- Worker threads e cluster mode
- Differenza Node.js vs Edge runtime in Next.js

## Domanda di review

Perché una Server Action Next.js può usare `process.env.SERVICE_ROLE_KEY` in sicurezza, mentre lo stesso codice in un Client Component sarebbe pericoloso?
