---
slug: ai-solutions-builder
goal: "Diventare un AI Solutions Builder — identificare opportunità AI nelle aziende, progettare e costruire soluzioni AI-powered (agenti, RAG, automazioni integrative), supervisionare l'implementazione con autorevolezza tecnica"
created: 2026-05-18
status: active
estimated_weeks: 60
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

esame_finale:
  fase_corrente: null
  domande_risposte: []
  last_attempt: null
  outcome: null
---

## Contesto e motivazione

Alessio vuole entrare nelle aziende come figura esperta di AI: identificare dove l'AI crea valore reale, progettare la soluzione, costruirla o supervisarne l'implementazione. Non developer tradizionale — AI Solutions Builder. Il mercato italiano delle PMI è quasi vuoto su questa figura: chi arriva preparato adesso ha un vantaggio strutturale.

Ha già iniziato a costruire SaaS con AI (menu-qr-saas). Il gap è la comprensione tecnica che permette di supervisionare l'AI invece di fidarsi ciecamente dell'output.

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

## Note

- Nodo 1 già in_progress dalla sessione 2026-05-18. Concetti L1 acquisiti: jwt-structure, rls-multi-tenant, env-variables.
- Il Nodo 1 è anche fondamenta per gli altri nodi — non si può saltare.
- Nessun assessment prerequisiti formale fatto sul Nodo 2 — da fare all'inizio della prima sessione del Nodo 2.
- Stima conservative: 60 settimane (15 mesi) con ritmo 2-3 ore/settimana. Ritmo sostenuto può ridurre a 10-12 mesi.
