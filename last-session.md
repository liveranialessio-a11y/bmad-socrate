# Last Session — 2026-05-19 (sessione 4 — studio rls-multi-tenant L1→L2)

## Cosa è successo

Studio maieutico su `rls-multi-tenant`. Target: portare da L1 a L2 con applicazione concreta.

Errori apertura corretti:
- JWT sta nell'header Authorization (non nel body) ← confusione corretta
- `restaurant_id` nel JWT sta in `app_metadata` (non nel .env) ← errore grave corretto
- service_role bypassa RLS: sicuro solo lato server, key mai in NEXT_PUBLIC_

Concetti acquisiti:
- USING vs WITH CHECK per ogni comando (SELECT/INSERT/UPDATE/DELETE)
- Cross-tenant attack su UPDATE: senza WITH CHECK, staff A può spostare record nel ristorante B
- Perché ::uuid: Postgres rifiuta `uuid = text` senza cast esplicito
- Caso anon su sito pubblico: service_role + WHERE server-side, non policy anon

Policy scritte autonomamente: UPDATE (USING+WITH CHECK) e DELETE (solo USING) — concetto corretto. Sintassi con errori ripetuti: apici JSON mancanti (`-> app_metadata` invece di `-> 'app_metadata'`), parentesi USING mancanti. Non è problema di comprensione, è memoria muscolare.

Promosso a **L2** con caveat: sintassi operatori JSON ancora instabile.

## Cosa è rimasto fuori

- Policy INSERT (non toccata)
- Policy SELECT per anon — risposta: service_role bypassa, non serve policy anon per sito pubblico single-tenant
- `supabase-client-typescript` (createServerClient, SSR cookies) — ancora L0, collegato a rls-multi-tenant

## Prossima sessione

Opzioni per Nodo 1:
1. **`jwt-structure`** L1→L2: operativamente cosa fare quando il JWT scade, refresh token, LocalStorage antipattern dimostrato
2. **`supabase-client-typescript`** L0→L1: createServerClient vs createBrowserClient, SSR cookies, collegato direttamente a rls-multi-tenant
3. **`app-metadata`** L1→L2: come si settano custom claims, differenza user_metadata vs app_metadata, perché solo admin può scrivere app_metadata

Pattern da tenere a mente: gli apici JSON sono errore ricorrente. Nella prossima sessione SQL va chiesto subito la catena completa per verificare se si è fissata.
