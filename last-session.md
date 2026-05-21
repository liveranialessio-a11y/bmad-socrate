# Last Session — 2026-05-22 (sessione 5 — rls avanzato + lettura codice reale)

## Cosa è successo

Continuazione studio RLS. Analisi del file reale `2a-1a-schema-menu-foundation-con-rls.md`.

Concetti acquisiti:
- Multi-policy OR vs AND: due policy permissive fanno OR (bug cross-tenant se non stai attento)
- AS RESTRICTIVE: fa AND con le altre policy — usare quando le regole hanno origini diverse (sicurezza vs business)
- ENABLE ROW LEVEL SECURITY: obbligatorio, senza questa riga le policy non hanno effetto
- RLS senza policy INSERT = deny by default per authenticated ← capito autonomamente ✅
- Policy anon senza restaurant_id: accettabile per dati pubblici (menu), non per dati sensibili
- jwt_restaurant_id(): funzione helper try-cast safe — usa regex + SELECT CASE per restituire NULL invece di 500 su JWT malformato. Cast diretto `::uuid` è **vietato** nel progetto

Gap identificati in sessione:
- SELECT CASE: non conosciuto, spiegato minimal (≈ if/else in SQL)
- Regex `~*`: non conosciuto, spiegato minimal (controllo di forma su stringa)
- Server Actions Next.js: non conosciuto, spiegato minimal (funzione server-only, client non vede codice né credenziali)

## Feedback utente — cambio metodo (IMPORTANTE)

Alessio ha chiesto esplicitamente: **prima di interrogare su un concetto, verificare se conosce il vocabolario necessario per ragionare**. Se manca una parola base (regex, SELECT CASE, AS RESTRICTIVE), dare l'impalcatura minima PRIMA di interrogare. Il metodo "interroga prima di spiegare" vale solo quando il vocabolario esiste già.

Applicare da subito e in ogni sessione futura.

## Concetti da esplorare nel Nodo 1 ancora

- sql-injection-prevention: Alessio non sa come funziona il prepared statement lato Supabase
- SELECT CASE SQL: visto solo in contesto, non studiato come concetto
- Regex basics: visto solo nominalmente

## Prossima sessione

Opzioni per Nodo 1:
1. **`jwt-structure`** L1→L2: operativamente cosa fare quando il JWT scade
2. **`supabase-client-typescript`** L0→L1: createServerClient vs createBrowserClient
3. **`app-metadata`** L1→L2: custom claims, user_metadata vs app_metadata

Gap da chiudere prima di proseguire: apici JSON nelle policy (errore ricorrente) — aprire con test rapido catena completa.
