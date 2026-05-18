---
concept: rls-multi-tenant
domain: supabase
level: L1
last_reviewed: 2026-05-18
status: applied
---

# RLS Multi-Tenant — Meccanismo Completo

## Il problema

In un SaaS multi-tenant su un singolo DB Postgres, ogni query deve filtrare automaticamente per ristorante — senza fidarsi del codice applicativo.

## La catena

1. Client manda request HTTP con JWT nell'header `Authorization: Bearer <token>`
2. **PostgREST** (layer Supabase tra API e DB) valida la firma JWT
3. PostgREST inietta il payload JWT in Postgres come variabile di sessione
4. Postgres espone questa variabile tramite `auth.jwt()`
5. La policy RLS legge `auth.jwt() -> 'app_metadata' ->> 'restaurant_id'`
6. Ogni riga viene filtrata: passa solo se `restaurant_id` colonna = `restaurant_id` JWT

## Policy SQL

```sql
CREATE POLICY "tenant isolation" ON menu_items
FOR SELECT TO authenticated
USING (
  restaurant_id = (auth.jwt() -> 'app_metadata' ->> 'restaurant_id')::uuid
);
```

## Regola progetto

`restaurant_id` va estratto **sempre dal JWT**, mai dal body della request.
Il body può essere manipolato dall'utente — il JWT no (firmato da Supabase).

## Gap da coprire (prossima sessione)

- Supabase client TypeScript (createServerClient, SSR cookies, come il JWT viene allegato automaticamente)
- Differenza 401 vs 0 righe: RLS non lancia errori, filtra silenziosamente

## Applications

- 2026-05-18 — analisi migration reale del progetto: policy anon/authenticated/super_admin su restaurants, catena PostgREST→session variable→auth.jwt(), USING vs WITH CHECK, operatori -> e ->>
