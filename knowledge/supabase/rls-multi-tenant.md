---
concept: rls-multi-tenant
domain: supabase
level: L2
last_reviewed: 2026-05-19
status: applied
confidence_layer: L1
sources:
  - type: official_docs
    url: https://github.com/supabase/supabase
    verified: 2026-05-19
connects_to: [jwt-structure, app-metadata, supabase-client-typescript]
pending_review: null
review_failures: 0
applications:
  - project: menu-qr-saas
    date: 2026-05-18
    note: "analisi migration reale: policy anon/authenticated/super_admin su restaurants, catena PostgREST→auth.jwt()→USING/WITH CHECK"
  - project: menu-qr-saas
    date: 2026-05-19
    note: "ha scritto autonomamente policy UPDATE (USING+WITH CHECK) e DELETE (solo USING) su menu_items, ha motivato correttamente cross-tenant attack e ruolo service_role"
---

## Concetto

In un SaaS multi-tenant su un singolo DB Postgres, ogni query deve filtrare automaticamente per ristorante — senza fidarsi del codice applicativo. La policy RLS è l'unica garanzia che non dipende dal codice.

## La catena

1. Client manda request HTTP con JWT nell'**header `Authorization: Bearer <token>`** (non nel body)
2. **PostgREST** valida la firma JWT
3. PostgREST inietta il payload JWT in Postgres come variabile di sessione
4. Postgres espone questa variabile tramite `auth.jwt()`
5. La policy RLS legge `auth.jwt() -> 'app_metadata' ->> 'restaurant_id'`
6. Ogni riga viene filtrata: passa solo se `restaurant_id` colonna = `restaurant_id` JWT

## USING vs WITH CHECK per comando

| Comando | USING | WITH CHECK |
|---------|-------|------------|
| SELECT | ✅ filtra righe lette | ❌ |
| INSERT | ❌ | ✅ valida riga che entra |
| UPDATE | ✅ filtra righe che puoi toccare | ✅ valida riga dopo l'update |
| DELETE | ✅ filtra righe che puoi cancellare | ❌ |

**Perché UPDATE ha entrambi:** senza `WITH CHECK` esplicito, uno staff del ristorante A può fare `SET restaurant_id = 'B'` su un proprio record. USING controlla la riga vecchia (appartiene ad A → ok), poi senza WITH CHECK la riga nuova non viene verificata → il record finisce fisicamente nel ristorante B. Data leak cross-tenant.

## Policy complete su menu_items

```sql
-- SELECT per staff autenticato
CREATE POLICY "tenant select isolation"
ON menu_items FOR SELECT TO authenticated
USING (
  restaurant_id = (auth.jwt() -> 'app_metadata' ->> 'restaurant_id')::uuid
);

-- UPDATE: entrambe le clausole, stessa espressione
CREATE POLICY "tenant update isolation"
ON menu_items FOR UPDATE TO authenticated
USING (
  restaurant_id = (auth.jwt() -> 'app_metadata' ->> 'restaurant_id')::uuid
)
WITH CHECK (
  restaurant_id = (auth.jwt() -> 'app_metadata' ->> 'restaurant_id')::uuid
);

-- DELETE: solo USING (non si scrive nessuna riga nuova)
CREATE POLICY "tenant delete isolation"
ON menu_items FOR DELETE TO authenticated
USING (
  restaurant_id = (auth.jwt() -> 'app_metadata' ->> 'restaurant_id')::uuid
);
```

## Caso sito pubblico (anon)

Il sito single-tenant pubblico (es. sito_cantinadeiconti) non usa JWT autenticato — i visitatori sono `anon`. Il JWT anonimo non ha `app_metadata.restaurant_id`. La soluzione corretta: il server Next.js usa **service_role key lato server** (bypassa RLS) + `WHERE restaurant_id = process.env.RESTAURANT_ID`. Sicuro perché la key non è mai esposta al browser (`NEXT_PUBLIC_`).

## Sintassi operatori JSON — errore ricorrente

`auth.jwt() -> 'app_metadata' ->> 'restaurant_id'`
- `->` naviga JSON → restituisce `jsonb`
- `->>` estrae valore finale → restituisce `text`
- `::uuid` converte `text` in `uuid` (necessario: Postgres rifiuta `uuid = text`)
- Le chiavi JSON vogliono **apici singoli**: `'app_metadata'`, non `app_metadata`

## Regola progetto

`restaurant_id` va estratto **sempre dal JWT**, mai dal body della request. Il body può essere manipolato — il JWT è firmato da Supabase.

## Domanda di review

Uno staff del ristorante A esegue `UPDATE menu_items SET restaurant_id = 'B' WHERE id = '123'`. Hai solo `USING (restaurant_id = jwt.restaurant_id)` senza `WITH CHECK`. Cosa succede e perché?
