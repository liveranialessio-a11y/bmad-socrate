---
concept: env-variables
domain: infra
level: pending
last_reviewed: 2026-05-18
status: studied_not_applied
---

# Variabili d'Ambiente

## Cos'è

Un file `.env.local` contiene coppie chiave=valore con configurazione e segreti.
Non viene mai committato su Git — è in `.gitignore`.
Il `.env.example` è il template pubblico (valori vuoti) che si può committare.

## Next.js: prefisso NEXT_PUBLIC_

| Variabile | Esposta al browser? |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Sì — incorporata nel JS del browser |
| `SUPABASE_SERVICE_ROLE_KEY` | No — rimane solo sul server |

Next.js decide al momento della build: tutto ciò che ha `NEXT_PUBLIC_` entra nel bundle JavaScript visibile nel browser.

**⚠️ MAI mettere `NEXT_PUBLIC_` su chiavi segrete** — chiunque apra i DevTools le vede.

## Edge Functions (Deno)

Non leggono `.env.local` di Next.js. Leggono:
- **Sviluppo locale**: `supabase/.env`
- **Produzione**: Secrets configurati nel pannello Supabase dashboard

Accesso via: `Deno.env.get("NOME_VARIABILE")`

## Applications

_nessuna ancora — da applicare entro 48h_
