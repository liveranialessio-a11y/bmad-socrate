---
concept: app-metadata
domain: supabase
level: L2
last_reviewed: 2026-05-22
confidence_layer: L1
sources:
  - type: official_docs
    url: https://github.com/supabase/supabase
    verified: 2026-05-19
connects_to: [jwt-structure, rls-multi-tenant, nodejs-runtime]
pending_review: null
review_failures: 0
applications:
  - project: menu-qr-saas
    date: 2026-05-18
    note: "calibrazione: distinzione user_metadata vs app_metadata confermata L1"
  - project: menu-qr-saas
    date: 2026-05-22
    note: "analisi codice reale: flusso completo creazione utente con app_metadata, trigger validazione, Admin API, perché non modificabile"
---

## Concetto

`app_metadata` è un oggetto JSON salvato in `auth.users.raw_app_meta_data`. Viene incluso nel payload del JWT ad ogni login. Solo il backend con `service_role` può scriverlo — mai l'utente, mai il browser.

## Differenza app_metadata vs user_metadata

| | app_metadata | user_metadata |
|---|---|---|
| Chi può scrivere | Solo backend con `service_role` | L'utente stesso |
| Uso tipico | `restaurant_id`, ruolo, piano SaaS | Nome, preferenze, avatar |
| Sicurezza | Alta — non manipolabile dal client | Bassa — l'utente può cambiarla |

## Flusso completo — dalla creazione al filtro RLS

```
[1] Super_admin compila form "crea ristorante"

[2] Server Action (Node.js su Vercel, mai nel browser)
    usa adminClient con service_role key
    chiama: adminClient.auth.admin.createUser({
      app_metadata: { role: "gestore", restaurant_id: "UUID" }
    })

[3] Supabase Auth riceve la richiesta
    Trigger BEFORE INSERT si attiva su auth.users
    Valida: role canonico? restaurant_id UUID valido? super_admin senza restaurant_id?
    → ok: RETURN NEW
    → errore: RAISE EXCEPTION (blocca tutto)

[4] Utente creato in auth.users con app_metadata salvato
    (una volta sola — non ad ogni login)

[5] Il gestore fa login (email + password)
    Supabase verifica le credenziali (confronto hash bcrypt)
    Genera JWT con app_metadata incluso nel payload

[6] Il gestore fa una richiesta al DB
    JWT nell'header Authorization: Bearer <token>
    PostgREST inietta il JWT come variabile di sessione Postgres

[7] Policy RLS si attiva
    jwt_restaurant_id() legge app_metadata.restaurant_id dal JWT
    Filtra: solo righe dove restaurant_id = UUID del gestore
```

## Trigger di validazione (codice reale del progetto)

`trg_validate_user_app_metadata` su `auth.users` BEFORE INSERT OR UPDATE:
- role deve essere uno di: `super_admin`, `gestore`, `staff`
- `super_admin` NON deve avere `restaurant_id`
- `gestore`/`staff` DEVONO avere `restaurant_id` UUID valido

Il trigger **valida** — non scrive. La scrittura avviene sempre tramite Admin API con service_role.

## Perché non è modificabile dal gestore

`app_metadata` sta nel payload del JWT. Il payload è leggibile (base64) ma non modificabile: la firma HMAC-SHA256 garantisce l'integrità. Per modificare il payload bisogna rifirmare con `SUPABASE_JWT_SECRET` — che sta solo sul server Supabase, mai esposta.

## Domanda di review

Uno staff prova a fare una richiesta con un JWT in cui ha modificato manualmente `app_metadata.restaurant_id`. Cosa succede e in quale punto esatto del flusso viene bloccato?
