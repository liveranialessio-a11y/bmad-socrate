# Last Session — 2026-05-18 (sessione 2 — completa)

## Cosa è successo

Sessione di applicazione + creazione learning plan.

Applicazione guidata su .env.example → portati a L1: jwt-structure, env-variables, rls-multi-tenant.
Brainstorming goal professionale → definito goal "AI Solutions Builder".
Creato plan attivo: `plans/active/ai-solutions-builder.md` (5 nodi, ~60 settimane).

## Concetti portati a L1

- **jwt-structure** → L1. Header/payload/signature, JWT_SECRET come chiave firma, payload leggibile da chiunque (firmato non criptato), token rubato usabile fino a scadenza.
- **env-variables** → L1. NEXT_PUBLIC_ solo per il browser, .env.local mai in git, ALLOWED_ORIGIN server-side, verifica .gitignore su file reale.
- **rls-multi-tenant** → L1. Catena PostgREST→session variable→auth.jwt()→policy, USING vs WITH CHECK, operatori -> e ->>, multi-policy OR, sito pubblico usa WHERE esplicito non RLS.

## Ancora pending

- **app-metadata** → non toccato, da aprire nella prossima sessione.

## Gap segnalati

- Supabase client TypeScript (createServerClient, SSR, cookies) — non capito, concetto da aggiungere al Nodo 1.
- RLS filtra silenziosamente (0 righe, non 401) — spiegato ma da consolidare.

## Piano attivo

`ai-solutions-builder` — Nodo 1 in_progress. Prossimo concetto: app-metadata.

## Prossima sessione

1. Chiudere app-metadata (ultimo concetto pending del Nodo 1)
2. Studiare supabase-client-typescript (gap identificato oggi)
3. Valutare se il Nodo 1 è pronto per esame nodo
