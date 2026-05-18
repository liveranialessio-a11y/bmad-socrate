---
concept: app-metadata
domain: supabase
level: pending
last_reviewed: 2026-05-18
status: studied_not_applied
---

# app_metadata vs user_metadata

## Differenza chiave

| | app_metadata | user_metadata |
|---|---|---|
| Chi può scrivere | Solo backend con `service_role` | L'utente stesso |
| Uso tipico | `restaurant_id`, ruoli, piano SaaS | Nome, preferenze, avatar |
| Sicurezza | Alta — non manipolabile dal client | Bassa — l'utente può cambiarla |

## Dove vive

Nella tabella `auth.users` di Supabase, colonna `raw_app_meta_data` (jsonb).
Non è un file — è una riga nel database.

## Come si scrive (solo server, service_role)

```typescript
await supabaseAdmin.auth.admin.updateUserById(userId, {
  app_metadata: { restaurant_id: "abc-123" }
})
```

## Flusso onboarding ristorante

1. Ristorante completa registrazione
2. Backend crea record in tabella `restaurants` → ottiene UUID
3. Edge Function chiama `updateUserById` con quell'UUID in `app_metadata`
4. Da quel momento ogni JWT dell'utente contiene `restaurant_id`
5. RLS funziona automaticamente

## Applications

_nessuna ancora — da applicare entro 48h_
