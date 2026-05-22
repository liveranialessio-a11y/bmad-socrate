# Last Session — 2026-05-22 (sessione 5 — RLS avanzato + app_metadata flusso completo)

## Cosa è successo

Sessione lunga, due macro-blocchi.

### Blocco 1 — RLS avanzato (continuazione rls-multi-tenant)

Analisi file reale `2a-1a-schema-menu-foundation-con-rls.md`:
- **Multi-policy OR implicito**: due policy permissive → bug cross-tenant (ha sbagliato la risposta iniziale, poi corretto)
- **AS RESTRICTIVE**: fa AND invece di OR, usare quando le regole hanno origini diverse
- **ENABLE ROW LEVEL SECURITY**: obbligatorio, senza questa riga le policy non hanno effetto
- **RLS senza policy = deny by default**: capito autonomamente ✅
- **jwt_restaurant_id()**: helper try-cast safe — regex + SELECT CASE → NULL invece di 500 su JWT malformato. Cast diretto `::uuid` vietato nel progetto
- **SELECT CASE e regex**: vocabolario sconosciuto, spiegato minimale
- **Policy anon senza restaurant_id**: accettabile per dati pubblici per design

### Blocco 2 — app_metadata flusso completo

Concetti acquisiti:
- `app_metadata` è un campo JSON in `auth.users` scritto solo con service_role (Admin API)
- Trigger `trg_validate_user_app_metadata`: BEFORE INSERT OR UPDATE su auth.users — valida coerenza (non scrive)
- Hash bcrypt: password non in chiaro, confronto hash al login
- Cookie HTTP: browser li salva e rimanda automaticamente ad ogni richiesta
- **JWT falsificazione impossibile**: HMAC-SHA256, chiave segreta solo su server Supabase → capito dalla domanda spontanea "se intercetto la chiamata posso modificare il payload?" ✅
- **HTTP stateless**: PostgREST inietta JWT ad ogni richiesta, non una volta sola al login ← errore iniziale corretto
- Node.js basics: framework full-stack (server + browser), Server Actions solo su server Node.js

## Feedback metodo (applicato in sessione)

Check vocabolario prima di interrogare — applicato per: SELECT CASE, regex ~*, DECLARE/BEGIN, cookie HTTP, trigger, hash.

## Concetti promossi

- `rls-multi-tenant` → aggiornato con multi-policy OR, AS RESTRICTIVE, jwt_restaurant_id, deny by default (rimane L2)
- `app-metadata` → L1 → L2 (flusso completo acquisito)
- `jwt-structure` → aggiornato con sezione falsificazione HMAC-SHA256

## Concept aggiunti

- `nodejs-runtime` → L0 placeholder (da studiare in sessione dedicata: event loop, V8, non-blocking I/O)

## Prossima sessione — Nodo 2: Database Design & SQL Mastery

Alessio ha chiesto di capire la sintassi SQL/Postgres completa per leggere qualsiasi riga del progetto. Obiettivo già nel plan (Nodo 2). 

Iniziare da: **DDL e migration** — `CREATE TABLE`, `ALTER TABLE`, tipi Postgres, constraints. È il foundation che ha incontrato più volte senza mai studiarlo formalmente. Usare le migration reali del progetto come materiale.

Metodo: codice reale del progetto → vocabolario sconosciuto spiegato al momento → interrogazione → applicazione. NON lezione frontale di sintassi.

## Pattern da tenere a mente

- Errore ricorrente: HTTP stateless (tende a pensare che il server "ricordi" lo stato tra richieste)
- Apici JSON nelle policy ancora instabili (verificare sempre in apertura sessione)
- Curiosità genuina sugli internals — quando emerge, dare impalcatura minima e salvare come concept da approfondire
