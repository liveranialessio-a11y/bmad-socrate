---
slug: ai-solutions-builder
goal: "Diventare un Technical Solutions Architect con specializzazione AI — sa revisionare codice (sa cosa controllare per dominio), sa prendere e difendere decisioni tecniche davanti a un team di developer, sa identificare e costruire soluzioni AI-powered. Non scrive codice: lo governa."
created: 2026-05-18
status: active
estimated_weeks: 75
max_active_check: respected
nodes:
  - id: 1
    name: "Security & Authentication"
    concepts:
      - jwt-structure
      - rls-multi-tenant
      - env-variables
      - app-metadata
      - supabase-client-typescript
      - oauth-flow-basics
      - owasp-top10-web
      - session-management
    layer: L1-L2
    mini_project: "Security review del menu-qr-saas: analizzare 3 file reali (migration, API route, componente), trovare tutti i problemi di sicurezza presenti, produrre un report scritto con problema + impatto + fix proposto"
    status: in_progress
    completion_criteria: "Identifica almeno 5 problemi critici di sicurezza in codice AI-generato; spiega il flusso completo dalla request HTTP alla riga filtrata da RLS senza aiuto; sa distinguere cosa è responsabilità del frontend vs backend vs database"
    started: 2026-05-18
    completed: null
    examination_level: null

  - id: 2
    name: "Database Design"
    concepts:
      - schema-normalization
      - foreign-keys-constraints
      - migrations-versioning
      - indexing-strategies
      - soft-delete-pattern
      - transactions-basics
      - sql-joins
      - data-types-postgres
    layer: L1-L2
    mini_project: "Analizzare il data model del menu-qr-saas: trovare almeno 3 scelte discutibili, proporre miglioramenti motivati per ciascuna. Poi progettare da zero uno schema per un caso cliente nuovo (es. sistema prenotazioni palestra)"
    status: pending
    completion_criteria: "Progetta uno schema completo con FK cascade, soft delete dove serve, indexing motivato, RLS pensata in anticipo; sa spiegare perché ogni scelta è stata fatta e quali alternative ha escluso"
    started: null
    completed: null
    examination_level: null

  - id: 3
    name: "API & Backend Architecture"
    concepts:
      - rest-principles
      - http-methods-status-codes
      - server-vs-client-logic
      - middleware-pattern
      - error-handling-api
      - rate-limiting-basics
      - edge-functions-vs-server-actions
      - api-versioning
    layer: L1-L2
    mini_project: "Analizzare le API routes del menu-qr-saas: produrre un report con errori di design trovati (logica sbagliata lato client, status code errati, mancanza di validazione, etc.) e proporre la versione corretta motivata"
    status: pending
    completion_criteria: "Riceve un'API mal-progettata AI-generata e produce review con almeno 5 problemi specifici e motivazione tecnica; sa scegliere tra Edge Function e Server Action con reasoning solido"
    started: null
    completed: null
    examination_level: null

  - id: 4
    name: "Frontend Architecture"
    concepts:
      - ssr-vs-csr-vs-ssg
      - nextjs-rendering-models
      - server-components-vs-client-components
      - component-architecture-basics
      - state-management-concepts
      - hydration-basics
      - web-vitals-performance-basics
    layer: L1
    mini_project: "Analizzare 5 pagine del sito_cantinadeiconti: per ognuna spiegare quale rendering model viene usato, se la scelta è corretta per quel caso, e cosa cambierebbe con una scelta diversa"
    status: pending
    completion_criteria: "Dato un requisito di pagina (frequenza aggiornamento dati, SEO, interattività), sa scegliere il rendering model corretto con motivazione; sa spiegare cosa succede durante l'hydration e perché importa"
    started: null
    completed: null
    examination_level: null

  - id: 5
    name: "AI Systems Architecture"
    concepts:
      - llm-context-window
      - tokenization
      - hallucination-patterns
      - tool-use-function-calling
      - rag-architecture
      - embeddings-basics
      - agent-patterns
      - ai-cost-latency-tradeoffs
      - prompt-injection-security
      - when-to-use-ai
    layer: L1-L2
    mini_project: "Costruire un agente funzionante con tool use via Claude API su un caso concreto (es. dato un ordine in linguaggio naturale, l'agente lo classifica e lo salva su Supabase); poi costruire un mini-RAG su documenti reali del progetto"
    status: pending
    completion_criteria: "Dato un problema aziendale, sa scegliere tra RAG, agente, semplice LLM call, o nessuna AI — con motivazione tecnica e stima di costo; sa identificare i rischi di sicurezza in un sistema AI-powered"
    started: null
    completed: null
    examination_level: null

  - id: 6
    name: "System Design & Decisioni Architetturali"
    concepts:
      - monolith-vs-microservices
      - scalability-concepts
      - caching-strategies
      - event-driven-architecture-basics
      - architectural-patterns
      - adr-architecture-decision-records
      - cost-modeling-basics
      - observability-basics
    layer: L2
    mini_project: "Scrivere 3 ADR reali per il menu-qr-saas — decisioni già prese nel progetto (es. perché Supabase vs Firebase, perché soft delete, perché multi-tenant su DB singolo). Formato: problema → opzioni considerate → decisione → conseguenze"
    status: pending
    completion_criteria: "Riceve un requisito nuovo non banale; produce un ADR con almeno 3 opzioni architetturali, pro/contro di ciascuna, decisione motivata; difende la scelta davanti a Socrate che propone l'alternativa peggiore come se fosse migliore"
    started: null
    completed: null
    examination_level: null

  - id: 7
    name: "Code Review & Technical Leadership"
    concepts:
      - code-review-security-checklist
      - code-review-database-checklist
      - code-review-api-checklist
      - code-review-frontend-checklist
      - code-review-ai-checklist
      - technical-feedback-communication
      - architectural-decision-reasoning
      - working-with-dev-teams
    layer: L2-L3
    mini_project: "4 review complete su codice AI-generato — una per dominio (security, database, API, AI). Ogni review: lista problemi trovati ordinati per severità, motivazione tecnica, fix proposto. Poi presentazione orale delle 4 review in sequenza"
    status: pending
    completion_criteria: "Socrate fa da developer + cliente scettico: contesta ogni problema trovato e propone soluzioni alternative sbagliate. Alessio difende le review con reasoning solido senza prepararsi in anticipo. Non bastano le risposte giuste — devono reggere all'interrogatorio"
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

Vuole entrare nelle aziende, capire i problemi tecnici reali, proporre soluzioni architetturali (AI o tradizionali), avere abbastanza controllo da non dipendere ciecamente né dall'AI né dai developer per le decisioni che contano.

Ha già iniziato a costruire SaaS con AI (menu-qr-saas). Il gap è la comprensione sistematica dei domini tecnici e la capacità di revisione strutturata.

## Nodi

**Nodo 1 — Security & Authentication** *(in_progress)*
La base non negoziabile. JWT, RLS, OAuth, OWASP, variabili d'ambiente, client Supabase. Senza questo ogni sistema che costruisce o supervisiona è insicuro senza saperlo. Già iniziato: jwt-structure L1, rls-multi-tenant L1, env-variables L1.

**Nodo 2 — Database Design**
Il dominio dove l'AI fa più danni silenziosi. Schema sbagliato, FK mancanti, indexing assente, soft delete ignorato — tutto viene a galla mesi dopo in produzione. Alessio deve saper leggere un data model e trovare i problemi prima che arrivino.

**Nodo 3 — API & Backend Architecture**
Dove vive la logica business e come si espone. REST design, scelte server vs client, middleware, error handling. Un SA che non capisce perché certi endpoint sono sbagliati non può dirigere un backend developer.

**Nodo 4 — Frontend Architecture**
Non implementazione — comprensione delle scelte. SSR vs CSR vs SSG, Server vs Client Components, performance. Un SA deve capire perché il frontend è fatto così e quando la scelta è sbagliata.

**Nodo 5 — AI Systems Architecture**
La specializzazione. Capire cosa fa davvero un LLM, quando RAG serve, come funzionano gli agenti, cosa costa, dove si rompe. Senza questo non si propone AI con credibilità.

**Nodo 6 — System Design & Decisioni Architetturali**
Il cuore del ruolo SA. Pattern architetturali, trade-off, ADR, scalabilità, costi. La differenza tra chi propone soluzioni e chi le subisce.

**Nodo 7 — Code Review & Technical Leadership**
La sintesi operativa di tutto il percorso. Una checklist per dominio, la capacità di leggere un PR in 30 minuti trovando i problemi critici, e la competenza di comunicarli a un team. Il nodo finale è un esame pratico: 4 review reali difese sotto interrogatorio.

## Note

- Nodo 1 in_progress dal 2026-05-18. Concetti L1: jwt-structure, rls-multi-tenant, env-variables. Rimanenti: app-metadata, supabase-client-typescript, oauth-flow-basics, owasp-top10-web, session-management.
- Goal definitivo confermato da Alessio il 2026-05-18 dopo brainstorming lungo.
- Piano precedente era AI-centrico e generico — riscritto completamente su 7 nodi di dominio.
- Stima: 75 settimane (~18 mesi) a 2-3 ore/settimana. Ritmo sostenuto: 12-14 mesi.
