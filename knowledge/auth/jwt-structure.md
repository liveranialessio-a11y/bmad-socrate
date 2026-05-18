---
concept: jwt-structure
domain: auth
level: pending
last_reviewed: 2026-05-18
status: studied_not_applied
---

# JWT — Struttura e Firma

## Core

Un JWT è composto da tre parti separate da un punto: `header.payload.signature`.

- **Header**: algoritmo usato per firmare
- **Payload**: dati (claims) in chiaro, codificati Base64 — leggibile da chiunque
- **Signature**: hash di header+payload firmato con la chiave privata del server

**Non è criptato — è firmato.** Differenza critica:
- Criptato = nessuno può leggere il contenuto
- Firmato = tutti possono leggere, nessuno può modificare senza invalidare la firma

## Implicazione per la sicurezza

Per modificare il payload (es. cambiare `restaurant_id`) serve rifirmare con la chiave privata di Supabase — impossibile senza averla.

## Payload tipico Supabase

```json
{
  "sub": "uuid-utente",
  "email": "user@example.com",
  "role": "authenticated",
  "app_metadata": { "restaurant_id": "abc-123" },
  "user_metadata": { "nome": "Mario Rossi" }
}
```

## Applications

_nessuna ancora — da applicare entro 48h_
