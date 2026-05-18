---
concept: jwt-structure
domain: auth
level: L1
last_reviewed: 2026-05-18
status: applied
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

## SUPABASE_JWT_SECRET

`SUPABASE_JWT_SECRET` è la stringa segreta usata come terzo ingrediente nella firma:
`signature = HMAC-SHA256(base64(header) + "." + base64(payload), JWT_SECRET)`

Senza di essa non si può né creare né verificare un JWT valido. Per questo non ha `NEXT_PUBLIC_` — chiunque la conosca può firmare token arbitrari.

## Applications

- 2026-05-18 — analisi `.env.example` del progetto menu-qr-saas: identificate tutte le variabili, ragionato sul prefisso NEXT_PUBLIC_ e sul ruolo di JWT_SECRET
- 2026-05-18 — calibrazione plan `solutions-architect-ai-reviewer`: confermato L1 (sa scadenza 1h Supabase). Gap: non sa cosa fare operativamente per fermare un JWT rubato (revoke non possibile a runtime).
