# Last Session — 2026-05-18 (sessione 2)

## Cosa è successo

Applicazione guidata su `.env.example` del progetto menu-qr-saas.

Alessio ha analizzato tutte le variabili d'ambiente e ragionato sul prefisso NEXT_PUBLIC_.

## Concetti toccati

- **jwt-structure** → portato a L1. Gap risolti: JWT_SECRET come chiave di firma (ingrediente del frullatore), distinzione firmato vs criptato (payload leggibile da chiunque, non modificabile senza il secret), token rubato usabile senza JWT_SECRET fino a scadenza, vettori di furto (HTTP, XSS/localStorage).
- **env-variables** → applicazione in corso (analisi .env.example). Ragionamento corretto su NEXT_PUBLIC_ e ALLOWED_ORIGIN. Da chiudere con domanda finale.
- **rls-multi-tenant**, **app-metadata** → ancora pending, non toccati.

## Livello Alessio

Principiante backend. Capisce bene se guidato passo-passo. Tende a confondere "usare un token" con "creare un token" — monitorare. Non mostrare codice prima del concetto a parole.

## In sospeso

- `env-variables`: quasi chiuso, manca domanda finale
- `rls-multi-tenant`: priorità alta — da applicare nella prossima fase
- `app-metadata`: collegato a jwt-structure, da completare

## Prossima sessione

Chiudere env-variables → poi rls-multi-tenant con applicazione su codice reale del progetto.
