---
slug: ai-solutions-builder
goal: "Diventare un Technical Solutions Architect con specializzazione AI — sa revisionare codice (sa cosa controllare per dominio), sa prendere e difendere decisioni tecniche davanti a un team di developer, sa identificare e costruire soluzioni AI-powered. Non scrive codice: lo governa."
created: 2026-05-18
status: active
estimated_weeks: 70
max_active_check: respected
nodes:
  - id: 1
    name: "Fondamenti dei sistemi"
    concepts: [jwt-structure, rls-multi-tenant, app-metadata, env-variables, supabase-client-typescript, api-rest-basics, database-schema-basics]
    layer: L1
    mini_project: "Spiegare a voce l'architettura di sicurezza completa del menu-qr-saas — JWT, RLS, variabili d'ambiente, flusso autenticazione — e identificare almeno 3 pattern di errore AI in una migration reale"
    status: in_progress
    completion_criteria: "Alessio spiega senza aiuto il flusso completo dalla request HTTP alla riga filtrata da RLS; identifica almeno 3 errori critici in codice AI-generato su auth e database"
    started: 2026-05-18
    completed: null
    examination_level: null

  - id: 2
    name: "Come funziona un LLM davvero"
    concepts: [llm-context-window, tokenization, hallucination-patterns, tool-use-function-calling, cost-latency-tradeoffs, prompt-engineering-applied]
    layer: L1-L2
    mini_project: "Costruire un agente con tool use via API Claude: dato un ordine in linguaggio naturale, l'agente estrae i dati strutturati e li salva su Supabase"
    status: pending
    completion_criteria: "Alessio spiega perché un LLM alucina su un caso specifico; sa stimare il costo approssimativo di una chiamata API; ha costruito un agente funzionante con almeno 2 tool"
    started: null
    completed: null
    examination_level: null

  - id: 3
    name: "Pattern AI: RAG e Agenti"
    concepts: [embeddings-basics, vector-search, rag-architecture, agent-react-pattern, agent-memory-patterns, multi-agent-basics]
    layer: L2
    mini_project: "Costruire un sistema RAG su documenti reali (menu e policy del ristorante) con retrieval semantico e risposta con citazione della fonte"
    status: pending
    completion_criteria: "Alessio progetta su carta l'architettura RAG per un caso cliente reale; sa spiegare quando RAG non serve; ha un sistema RAG funzionante"
    started: null
    completed: null
    examination_level: null

  - id: 4
    name: "Integrazione sistemi"
    concepts: [webhooks, rest-api-consumption, n8n-vs-custom-code, supabase-as-ai-backend, file-storage-for-ai, rate-limiting-basics]
    layer: L1-L2
    mini_project: "Costruire un workflow end-to-end: messaggio WhatsApp → agente AI → risposta automatica + salvataggio dati su Supabase"
    status: pending
    completion_criteria: "Alessio ha consegnato un'integrazione funzionante con almeno 3 sistemi; sa spiegare quando n8n batte codice custom e viceversa"
    started: null
    completed: null
    examination_level: null

  - id: 5
    name: "Valutare e proporre"
    concepts: [ai-fit-analysis, scoping-ai-project, roi-estimation-ai, risk-assessment-ai, proposta-tecnica-ai]
    layer: L1
    mini_project: "Produrre una proposta completa per un cliente reale o simulato: analisi problema, soluzione proposta, architettura, stima costi e tempi, rischi"
    status: pending
    completion_criteria: "Alessio presenta la proposta — Socrate fa da cliente scettico. Deve rispondere a obiezioni tecniche e di budget senza prepararsi in anticipo"
    started: null
    completed: null
    examination_level: null

  - id: 6
    name: "Code Review & Decisioni Tecniche"
    concepts: [code-review-security-checklist, code-review-database-checklist, code-review-api-checklist, code-review-ai-checklist, architectural-decision-reasoning, technical-tradeoffs]
    layer: L2
    mini_project: "Ricevere un PR reale o simulato AI-generato su ogni dominio (security, database, API, AI) e produrre una review scritta con problemi trovati, decisioni proposte e motivazioni"
    status: pending
    completion_criteria: "Alessio consegna 4 review scritte (una per dominio) — Socrate valuta se i problemi critici sono stati trovati e se le motivazioni delle decisioni reggono all'interrogatorio"
    started: null
    completed: null
    examination_level: null

esame_finale:
  fase_corrente: null
  domande_risposte: []
  last_attempt: null
  outcome: null
---

## Contesto e motivazione

Alessio vuole diventare un Technical Solutions Architect con specializzazione AI. Il goal NON è scrivere codice — è governarlo: revisionare output AI e developer, sapere cosa controllare per dominio, prendere e difendere decisioni tecniche in una stanza con programmatori esperti.

Vuole entrare nelle aziende, capire i problemi tecnici reali, proporre soluzioni architetturali (AI o tradizionali), e avere abbastanza controllo da non dipendere ciecamente dall'AI o dai developer per le decisioni che contano.

Ha già iniziato a costruire SaaS con AI (menu-qr-saas). Il gap è la comprensione sistematica dei domini tecnici e la capacità di revisione.

## Nodi

**Nodo 1 — Fondamenti dei sistemi** *(in_progress)*
La base senza la quale non si può valutare nessuna soluzione AI. Sicurezza (JWT, RLS), database, variabili d'ambiente, client Supabase TypeScript, API REST. Già iniziato: jwt-structure L1, rls-multi-tenant L1, env-variables L1. Rimane: app-metadata, supabase-client-typescript, api-rest-basics, database-schema-basics.

**Nodo 2 — Come funziona un LLM davvero**
Non uso superficiale. Capire context window, tokenizzazione, allucinazioni, tool use, costi. Senza questo si vendono soluzioni che non si sa perché falliscono. Mini-progetto: agente con tool use reale via Claude API.

**Nodo 3 — Pattern AI: RAG e Agenti**
I due pattern che coprono il 90% dei casi aziendali. RAG per dare memoria e documenti al modello. Agenti per workflow autonomi multi-step. Mini-progetto su documenti reali del progetto esistente.

**Nodo 4 — Integrazione sistemi**
Un agente senza sistemi reali non serve. Connettere API esterne, webhook, database, file storage. Mini-progetto end-to-end su un caso concreto (WhatsApp + Supabase + Claude).

**Nodo 5 — Valutare e proporre**
La parte che trasforma le competenze tecniche in valore professionale vendibile. Analisi fit AI, scoping, ROI, risk assessment. Mini-progetto: proposta completa per un cliente, difesa davanti a Socrate-cliente-scettico.

**Nodo 6 — Code Review & Decisioni Tecniche**
Il nodo che risponde direttamente al goal: sapere cosa controllare per dominio (security, database, API, AI), produrre review scritte con problemi trovati e motivazioni delle decisioni. La checklist mentale automatica che un SA usa nei primi 30 minuti su un codebase nuovo. Mini-progetto: 4 review reali su codice AI-generato, una per dominio.

## Note

- Nodo 1 già in_progress dalla sessione 2026-05-18. Concetti L1 acquisiti: jwt-structure, rls-multi-tenant, env-variables.
- Il Nodo 1 è anche fondamenta per gli altri nodi — non si può saltare.
- Nessun assessment prerequisiti formale fatto sul Nodo 2 — da fare all'inizio della prima sessione del Nodo 2.
- Stima aggiornata: 70 settimane (~17 mesi) con ritmo 2-3 ore/settimana. Ritmo sostenuto può ridurre a 12-14 mesi.
- Goal ridefinito in sessione: non "scrivere codice" ma "revisionare, controllare, decidere". Nodo 6 aggiunto specificamente per questo.
- Goal definitivo confermato da Alessio il 2026-05-18.
