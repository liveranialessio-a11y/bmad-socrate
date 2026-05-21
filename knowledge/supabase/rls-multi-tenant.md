---
concept: rls-multi-tenant
domain: supabase
level: L2
last_reviewed: 2026-05-22
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

## Multi-policy: OR implicito — la trappola

Quando esistono più policy per lo stesso comando, Postgres le combina con **OR** (comportamento permissivo default). Una riga è visibile se almeno una policy dice sì.

```sql
-- BUG: queste due policy insieme permettono cross-tenant leak
CREATE POLICY "staff legge ordini" ON orders FOR SELECT TO authenticated
  USING (restaurant_id = jwt_restaurant_id());

CREATE POLICY "tutti leggono ordini aperti" ON orders FOR SELECT TO authenticated
  USING (status = 'open');  -- nessun filtro tenant!
```

Lo staff del ristorante A vede gli ordini aperti del ristorante B perché la seconda policy non filtra per tenant.

**Fix 1 — policy unica con AND:**
```sql
USING (restaurant_id = jwt_restaurant_id() AND status = 'open')
```

**Fix 2 — AS RESTRICTIVE** (quando le regole hanno origini diverse: una di sicurezza, una di business):
```sql
CREATE POLICY "tutti leggono ordini aperti" ON orders FOR SELECT TO authenticated
  AS RESTRICTIVE
  USING (status = 'open');
```
`AS RESTRICTIVE` fa AND con tutte le altre policy invece di OR.

## jwt_restaurant_id() — helper obbligatorio nel progetto

Il cast diretto `::uuid` esplode con HTTP 500 PostgREST se il valore nel JWT non è un UUID valido.

```sql
-- VIETATO nel progetto
USING (restaurant_id = (auth.jwt() -> 'app_metadata' ->> 'restaurant_id')::uuid)

-- CORRETTO
USING (restaurant_id = jwt_restaurant_id())
```

`jwt_restaurant_id()` usa regex + SELECT CASE: se il valore matcha il formato UUID → casta; altrimenti → NULL. Policy con NULL = 0 righe, nessun 500. Regola del progetto: mai cast diretto, sempre l'helper.

## ENABLE ROW LEVEL SECURITY — obbligatorio

Senza questa riga le policy non hanno effetto:
```sql
ALTER TABLE menu_items ENABLE ROW LEVEL SECURITY;
```

## RLS senza policy = deny by default

Se RLS è abilitato ma non esiste nessuna policy per un'operazione (es. INSERT), quell'operazione è negata implicitamente per tutti i ruoli tranne service_role. Non serve una policy di deny esplicita.

## Domanda di review

Hai due policy SELECT authenticated sulla stessa tabella: una filtra per `restaurant_id = jwt_restaurant_id()`, l'altra filtra per `status = 'active'`. Un utente del ristorante A può vedere i record attivi del ristorante B? Come risolvi?
