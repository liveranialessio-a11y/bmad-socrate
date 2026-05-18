# Last Session — 2026-05-18

## Cosa è successo

Prima sessione Socrate — cold start, registrazione progetto `menu-qr-saas` come applicazione di riferimento.

Argomento: meccanismo di sicurezza multi-tenant del SaaS (JWT + RLS + app_metadata).

## Concetti toccati

- **jwt-structure**: struttura header.payload.signature, differenza firma vs cifratura, payload leggibile ma non modificabile
- **rls-multi-tenant**: catena PostgREST → Postgres → auth.jwt() → policy RLS, regola restaurant_id mai dal body
- **app-metadata**: differenza app_metadata (solo service_role) vs user_metadata (utente), dove vive (auth.users), flusso onboarding
- **env-variables**: .env.local vs .env.example, prefisso NEXT_PUBLIC_, Secrets Supabase per Edge Functions

## Livello Alessio

Principiante su concetti backend/API. Capisce bene i ragionamenti se si parte da analogie concrete. NON mostrare codice prima di aver spiegato il concetto a parole. Salire un gradino alla volta.

## In sospeso

- Nessun concetto applicato ancora — tutti in stato `pending` (regola 48h)
- Flusso completo onboarding ristorante non ancora implementato nel codice
- Edge Functions: concetto capito, codice non ancora scritto

## Prossima sessione

Verificare se Alessio ha applicato qualcuno dei concetti (regola 48h). Se sì, portare a level L3. Altrimenti proporre applicazione pratica guidata su uno dei concetti pending.
