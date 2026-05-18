# Last Session — 2026-05-18 (sessione 2 — completa)

## Cosa è successo

Sessione di applicazione guidata. Tutti e 4 i concetti pending toccati.

## Concetti portati a L1

- **jwt-structure** → L1. Capisce: header/payload/signature, JWT_SECRET come chiave di firma, payload leggibile da chiunque (firmato non criptato), token rubato usabile fino a scadenza, vettori furto (HTTP/XSS).
- **env-variables** → L1. Capisce: NEXT_PUBLIC_ solo per il browser, .env.local mai in git, ALLOWED_ORIGIN è server-side, verifica .gitignore sul file reale.
- **rls-multi-tenant** → L1. Capisce: catena PostgREST→session variable→auth.jwt()→policy, USING vs WITH CHECK, operatori -> e ->>, multi-policy OR, sito pubblico usa WHERE esplicito non RLS.

## Ancora pending

- **app-metadata** → non toccato questa sessione.

## Gap segnalati

- Supabase client TypeScript (createServerClient, SSR cookies) — non capito, da studiare come concetto separato.
- Distinzione 401 vs 0 righe (RLS filtra silenziosamente) — spiegato ma da consolidare.

## Livello Alessio

Cresce bene sulla parte SQL/meccanismo. Il codice TypeScript resta poco chiaro — non mostrare codice TypeScript senza prima spiegare il layer a parole. Procede bene con analogie concrete.

## Prossima sessione

- Chiudere **app-metadata** (ultimo concetto pending)
- Aprire nuovo concetto: **Supabase client TypeScript** (createServerClient, SSR, cookies)
- Poi: learning plan per definire obiettivi strutturati
