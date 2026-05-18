---
slug: solutions-architect-ai-reviewer
goal: |
  Acquisire la profondità tecnica di un Senior Engineer che fa code review affidabile su codice prodotto da AI,
  in stack web moderno (Next.js + Supabase + AI APIs), per clienti PMI italiane su commessa custom.
  Sa decidere tra alternative architetturali ("monolite vs functions", "SSR vs CSR", "vector DB vs full-text search",
  "pgvector vs Pinecone") motivando in 3 frasi davanti a un cliente o a un developer.
  Sa identificare falle di sicurezza, performance, scalabilità, costi in codice AI-generated.
  Non scrive lui il codice — lo governa.
non_goals:
  - "ML Engineering: no training modelli da zero, no PyTorch/TensorFlow, no fine-tuning custom, no MLOps"
  - "DevOps/SRE profondo: no Kubernetes, no Terraform avanzato, no cluster Postgres in HA, no infra on-prem"
  - "Marketing / pre-sales / consulting puro: il plan è tech-deep, non commerciale"
  - "Diventare developer che fa fare tutto all'AI senza review: il punto è esattamente l'opposto"
  - "Mobile development (iOS/Android nativo, React Native)"
  - "Microservizi / event-driven distribuiti (Kafka, RabbitMQ): over-engineering per PMI"
  - "Design Patterns OOP classici (GoF): stack target è funzionale React"
  - "Project management / Agile / Scrum: non tech-deep"
context: |
  Freelance / consulenza per clienti PMI italiane su commessa custom.
  Stack target: Next.js (App Router) + Supabase + AI APIs (OpenAI/Anthropic/Gemini).
  Progetto di riferimento per applicazione: menu-qr-saas (SaaS per menu QR di ristoranti).
  2 ore al giorno target di studio+applicazione (~14h/settimana).
created: 2026-05-18
status: active
phase: 4
activated: 2026-05-18
estimated_weeks: 60
nodes:
  - id: 1
    name: "Security & Authentication"
    why: |
      Senza fondamenta di sicurezza, ogni review di codice AI è cieca: AI sbaglia routinariamente su auth,
      JWT, multi-tenancy, secrets. È anche il dominio dove un errore in produzione fa danni più gravi
      e meno reversibili (data leak, privilege escalation). Nodo di apertura non opzionale.
    prerequisites: []
    concepts:
      - slug: jwt-structure
        description: "Struttura header.payload.signature, claims, scadenza, perché non va messo in localStorage e perché è firmato non criptato."
      - slug: rls-multi-tenant
        description: "Row-level security come pattern di isolamento multi-tenant in Postgres. Policy per SELECT/INSERT/UPDATE/DELETE, USING vs WITH CHECK, catena PostgREST→auth.jwt()→policy."
      - slug: env-variables
        description: "Gestione secrets, distinzione NEXT_PUBLIC_ (browser) vs server-only, .env.local mai in git, verifica .gitignore."
      - slug: app-metadata
        description: "Campo Supabase auth per claims non modificabili dall'utente (vs user_metadata), come si propaga nel JWT, custom claims pattern."
      - slug: supabase-client-typescript
        description: "createServerClient vs createBrowserClient, gestione cookies SSR, perché lo stesso client non vale ovunque."
      - slug: oauth-flow-basics
        description: "Authorization code flow, PKCE, redirect URI, state parameter, differenza rispetto a magic link e password auth."
      - slug: session-management
        description: "Cookie httpOnly + secure + SameSite, refresh token rotation, scadenza access vs refresh, sliding session."
      - slug: cors-policy
        description: "CORS, ALLOWED_ORIGIN header, preflight OPTIONS, perché si gestisce lato server e cosa significa per API pubbliche."
      - slug: owasp-top10-web
        description: "I 10 punti OWASP 2021, focus su A01 (broken access control) e A07 (auth failures) che sono i più comuni nel codice AI-generated."
      - slug: csrf-protection
        description: "Cosa è CSRF, perché SameSite cookie aiuta, quando servono anti-CSRF token, scenari reali in Next.js."
      - slug: sql-injection-prevention
        description: "Prepared statements, perché Supabase client è safe by default, edge case (RPC con stringhe dinamiche), come riconoscere codice vulnerabile."
      - slug: secrets-rotation
        description: "Cosa fare quando un secret leaka (JWT_SECRET, service_role, OpenAI API key): rotazione senza downtime, blast radius, audit log."
      - slug: gdpr-essentials-for-developers
        description: "GDPR operativo per developer (non legale teorico): data minimization, right to erasure, Data Processing Agreement con fornitori cloud (Vercel/Supabase/OpenAI), cosa è PII, cosa NON va in log e non va al LLM, consenso vs interesse legittimo. Hard rule del progetto per clienti PMI italiane EU."
    learning_outcomes:
      - "Spiega senza aiuto il flusso completo HTTP → Next.js middleware → Supabase client → RLS policy → riga restituita, indicando dove può fallire."
      - "Identifica almeno 5 problemi critici di sicurezza in 100 righe di codice AI-generated (API route + componente con auth)."
      - "Distingue cosa è responsabilità del frontend, del backend (Next.js Route Handler), del database (RLS) per ogni layer di sicurezza."
      - "Sa decidere quando usare anon key vs service_role key e perché service_role nel browser è game over."
      - "Sa progettare schema auth completo per nuovo cliente PMI: cosa va in JWT claims, cosa in app_metadata, quando servono custom claims."
      - "Identifica violazioni GDPR concrete in codice (PII in log, LLM call senza minimizzazione, hard delete dove serve scrubbing) e propone fix operativi."
    layer: L1-L2
    mini_project: |
      Security review del menu-qr-saas su 3 file reali: una migration con RLS policies, una API route critica
      (es. place-order), un componente con auth context. Produrre un report scritto con: problema + impatto
      (data leak / privilege escalation / DoS) + fix proposto + codice del fix. Almeno 5 problemi critici totali.
      Discussione con Socrate su ogni finding per verificare la solidità del ragionamento.
    completion_criteria: |
      Identifica almeno 5 problemi critici in 3 file reali del progetto, spiega il flusso completo dalla request
      HTTP alla riga filtrata da RLS senza aiuto, dimostra di saper scegliere tra anon/service_role/server-side
      context per ogni caso con motivazione.
    resources:
      - "OWASP Top 10 (release 2021) — focus sezioni A01 (Broken Access Control), A02 (Cryptographic Failures), A07 (Identification/Authentication Failures)"
      - "Supabase docs — Row Level Security, Auth, Server-Side Auth helpers per Next.js"
      - "Next.js docs — Authentication patterns, Route Handlers, Middleware"
      - "RFC 7519 — JSON Web Token (sezioni 1-5)"
      - "Garante Privacy — Guida applicazione GDPR (sezioni per sviluppatori) + EDPB guidelines su AI processing"
    estimated_hours: 88
    status: pending
    started: null
    completed: null
    examination_level: null

  - id: 2
    name: "Database Design & SQL Mastery"
    why: |
      Lo schema decide ciò che si può fare 12 mesi dopo. AI propone schemi mediocri (FK mancanti,
      indici sbagliati, soft-delete inconsistente) e tu devi vedere i difetti prima che diventino debito.
      SQL letto e scritto fluentemente è prerequisito per progettare API non-ingenue (nodo 3).
    prerequisites: [rls-multi-tenant, app-metadata]
    concepts:
      - slug: schema-normalization
        description: "1NF/2NF/3NF, quando rispettare le forme normali e quando denormalizzare consapevolmente (read-heavy, JSON blob, cache calcolata)."
      - slug: foreign-keys-constraints
        description: "FK come garanzia di integrità, ON DELETE CASCADE/RESTRICT/SET NULL, quando ognuno serve, anti-pattern del FK opzionale ovunque."
      - slug: migrations-versioning
        description: "Migrations come storia incrementale immutabile, mai modificare migration già applicata, strategie di rollback, branching schema."
      - slug: indexing-strategies
        description: "B-tree vs GIN vs GiST, indici composti (ordine colonne conta), partial indexes, indice su FK frequenti, quando NON serve un indice."
      - slug: soft-delete-pattern
        description: "deleted_at vs hard delete, vantaggi (audit, integrità storica) e svantaggi (query più complesse, UNIQUE constraint gotchas)."
      - slug: transactions-acid
        description: "Proprietà ACID, isolation levels (Read Committed, Repeatable Read, Serializable), lock, deadlock, quando aprire una transaction esplicita."
      - slug: sql-joins-advanced
        description: "INNER/LEFT/RIGHT/FULL, performance differences, quando JOIN batte subquery e viceversa, lateral join."
      - slug: data-types-postgres
        description: "text vs varchar(N) (perché text vince quasi sempre in Postgres), jsonb vs json, numeric precision, uuid, timestamptz (mai timestamp)."
      - slug: query-performance
        description: "EXPLAIN ANALYZE, leggere il piano di esecuzione, identificare seq scan problematici, sort costoso, hash join vs nested loop."
      - slug: postgres-extensions
        description: "pgcrypto, uuid-ossp, pgvector, quando attivarle in Supabase, conseguenze (vendor lock-in light)."
      - slug: constraints-checks
        description: "NOT NULL, UNIQUE (incluso UNIQUE parziale), CHECK constraints, deferred constraints, quando spostare validation dal codice al DB."
      - slug: n-plus-one-problem
        description: "Il problema (1 query + N query in loop), come riconoscerlo in codice AI-generated (forEach con await su query), come risolverlo (batch fetch, eager loading)."
      - slug: audit-log-patterns
        description: "Audit log a livello DB: tabella audit dedicata, trigger Postgres per CDC (who/what/when/old/new), alternativa con temporal extension. Cruciale per right-to-know GDPR e incident response."
      - slug: adr-format-basics
        description: "Architecture Decision Record nel formato minimale: titolo, contesto, decisione, conseguenze. Versione light per documentare scelte mentre le fai (il Nodo 8 approfondisce la pratica avanzata, RFC, role-play cliente)."
    learning_outcomes:
      - "Progetta da zero schema completo per nuovo caso cliente (es. sistema prenotazioni palestra multi-sede) con FK cascade, soft delete dove serve, RLS pensata in anticipo, indici motivati."
      - "Trova almeno 3 difetti motivati nel data model del menu-qr-saas con proposte alternative (migration di fix scritta)."
      - "Spiega senza aiuto un EXPLAIN ANALYZE di una query lenta e identifica il fix (indice mancante / query da riscrivere)."
      - "Sa decidere tra normalizzare e denormalizzare in caso concreto e difende la scelta in 3 frasi."
    layer: L1-L2
    mini_project: |
      Due deliverable.
      (a) Analizzare lo schema del menu-qr-saas: trovare almeno 3 scelte discutibili (es. FK senza cascade dove
          serve, indici mancanti su FK frequentemente joinati, soft delete inconsistente tra tabelle correlate),
          proporre fix con migration files reali.
      (b) Progettare da zero schema per nuovo caso cliente (sistema prenotazioni palestra multi-sede) con
          migration files, RLS policies, indici motivati. Almeno 5 tabelle. README di 1 pagina che motiva ogni
          scelta architetturale.
    completion_criteria: |
      Schema nuovo cliente completo (≥5 tabelle), motivato in 1 pagina di README, con RLS, indici, soft delete
      dove serve. Analisi schema esistente trova ≥3 problemi reali con proposte di fix concrete (migration scritta).
    resources:
      - "Postgres docs — Performance Tips, EXPLAIN, Indexes"
      - "Supabase docs — Database, Indexes, RLS performance tuning"
      - "Use The Index, Luke! (gratis online) — capitoli su B-tree e composite indexes"
      - "Designing Data-Intensive Applications (Martin Kleppmann) — capitoli 2-3 per fondamenti relazionali"
      - "Michael Nygard — Documenting Architecture Decisions (articolo originale)"
    estimated_hours: 90
    status: pending
    started: null
    completed: null
    examination_level: null

  - id: 3
    name: "API Design & Backend Architecture"
    why: |
      AI mette logica nei posti sbagliati (prezzo calcolato lato client, validation assente, status code random)
      e tu in review devi vederlo prima del merge. REST corretto, error handling robusto, validazione al boundary,
      idempotenza dove serve. Senza profondità qui, ogni route AI-generated passa "perché sembra giusta".
    prerequisites: [rls-multi-tenant, jwt-structure, foreign-keys-constraints, transactions-acid]
    concepts:
      - slug: rest-principles
        description: "Risorse vs azioni, stateless, idempotenza, HATEOAS (sapere che esiste anche se quasi nessuno la usa), cosa è REST e cosa è RPC mascherato."
      - slug: http-methods-status-codes
        description: "GET/POST/PUT/PATCH/DELETE semantica, 2xx (200/201/204) 4xx (400/401/403/404/409/422/429) 5xx (500/502/503) semantica corretta e non casuale."
      - slug: request-validation
        description: "Validare al boundary con schema (Zod), mai fidarsi del client, schema = singola fonte di verità per shape e tipo TypeScript."
      - slug: server-vs-client-logic
        description: "Cosa va lato server obbligatoriamente (prezzi, autorizzazione, business rules, calcoli sensibili) vs cosa lato client (UI, pre-submit validation per UX)."
      - slug: error-handling-api
        description: "Error types tipizzati, status code matching, response shape consistente (es. {error: {code, message}}), no leak di stack trace o messaggi DB grezzi."
      - slug: idempotency-keys
        description: "Idempotency keys per POST critici (pagamenti, ordini), pattern Stripe-style, storage chiavi, scadenza, gestione retry sicuri."
      - slug: rate-limiting-basics
        description: "Token bucket, fixed window, sliding window, perché serve (DoS, cost protection), dove implementare (edge/middleware/route)."
      - slug: middleware-pattern
        description: "Middleware Next.js, pattern withTenant (estrae tenant da JWT, mai dal body), composizione di middleware, ordering."
      - slug: api-versioning
        description: "URL versioning (/v1/) vs header versioning, quando serve, deprecation policy, comunicazione breaking change ai clienti."
      - slug: webhooks-pattern
        description: "Webhook in entrata (Stripe, etc.): validazione signature, idempotenza, retry policy lato fornitore, dead letter queue."
      - slug: edge-vs-server-runtime
        description: "Next.js edge runtime (V8 isolates, no Node API, latenza minima) vs Node runtime (full stdlib), cosa funziona dove, trade-off latenza vs capability."
      - slug: input-sanitization
        description: "Sanitize HTML (DOMPurify quando serve), escape per SQL (già coperto da prepared statements), shell/eval (mai), dove serve davvero e dove è teatro."
      - slug: pagination-patterns
        description: "Offset-based (LIMIT/OFFSET) vs cursor-based (keyset pagination): quando uno, quando l'altro, performance implications su tabelle grandi, infinite scroll vs paginated UI, stabilità dei risultati con inserimenti concorrenti."
      - slug: file-upload-multipart-supabase-storage
        description: "Upload file: multipart vs presigned URL, Supabase Storage policies (RLS analogo), validazione lato server (size, mime, magic bytes vs extension), gestione progress, idempotenza upload, scansione antivirus (quando serve)."
    learning_outcomes:
      - "Analizza API route AI-generated e identifica errori di design (logica spostata lato client, status code sbagliati, validation mancante, side effect non-idempotenti su POST)."
      - "Progetta API completa per nuovo dominio (es. endpoint prenotazioni palestra) con validazione Zod end-to-end, error handling, RLS-aware, idempotenza dove serve."
      - "Sa decidere quando POST vs PUT vs PATCH e difende la scelta in caso concreto."
      - "Sa decidere edge runtime vs Node runtime per ogni route con motivazione (es. AI streaming = edge, query DB pesante = node)."
    layer: L2
    mini_project: |
      (a) Analizzare le 5 API route più importanti del menu-qr-saas (es. place-order, get-menu, login,
          update-order-status, webhook esterno se esiste). Per ognuna produrre report: errori di design
          trovati + versione corretta motivata + diff applicabile.
      (b) Progettare da zero le routes per un nuovo dominio (prenotazioni palestra) con validazione Zod end-to-end,
          error handling, idempotency keys dove serve, rate limiting motivato.
    completion_criteria: |
      Report di review su 5 route reali con almeno 2 problemi per route documentati e fix in codice.
      Set completo di route per nuovo dominio con validazione end-to-end e ADR di motivazione per ogni scelta
      non ovvia (es. perché PATCH e non PUT, perché edge e non node).
    resources:
      - "REST API Design Rulebook (Mark Massé) — capitoli 1-4"
      - "Next.js docs — Route Handlers, Middleware, Edge runtime"
      - "Stripe API docs — sezione Idempotency, sezione Webhooks"
      - "Zod docs — schemi, inferenza tipo, transform/refine"
      - "Use The Index, Luke! — capitolo su pagination performance"
      - "Supabase Storage docs — RLS su bucket, signed URL"
    estimated_hours: 100
    status: pending
    started: null
    completed: null
    examination_level: null

  - id: 4
    name: "Next.js App Router & TypeScript profondo"
    why: |
      Stack target. App Router (server vs client components, SSR/ISR, data fetching, caching multi-layer)
      e TypeScript reale (generics, narrowing, discriminated unions) sono i due punti dove AI confonde
      server/client e produce type-cast bugiardi (`as any`, `as Type` mascherano i bug invece di esporli).
      Type safety è il principale strumento anti-hallucination che hai durante review.
    prerequisites: [rest-principles, request-validation, middleware-pattern]
    concepts:
      - slug: app-router-fundamentals
        description: "File-based routing, layout.tsx vs page.tsx, route groups, parallel routes, intercepting routes, naming conventions."
      - slug: server-vs-client-components
        description: "Differenza concettuale, directive 'use client', boundary tra i due, quando convertire, costi di hydration."
      - slug: data-fetching-patterns
        description: "Fetch in server components (no useEffect per data!), mutations via Server Actions o Route Handlers, cache deduplication."
      - slug: caching-strategies-nextjs
        description: "Full route cache, data cache, router cache, request memoization: 4 layer distinti. revalidatePath/revalidateTag, on-demand vs time-based."
      - slug: ssr-ssg-isr
        description: "Rendering modes (static, dynamic, ISR), generateStaticParams, dynamic = 'force-static' / 'force-dynamic', come scegliere."
      - slug: server-actions
        description: "Cosa sono, sicurezza (auth + validation obbligatori, sono endpoint pubblici!), quando preferirli a Route Handlers."
      - slug: streaming-suspense
        description: "React Streaming + Suspense, loading.tsx, error.tsx, granularità dello streaming, UX dei loading state."
      - slug: hydration-mismatches
        description: "Cosa è l'hydration, perché fallisce (Date.now(), random, localStorage in render), come debuggare, come prevenire."
      - slug: typescript-generics
        description: "Generics, constraint con extends, inference, quando servono davvero vs over-engineering, generics con default."
      - slug: discriminated-unions
        description: "Union types con discriminator (es. {status: 'success'} | {status: 'error'}), exhaustive pattern matching con never."
      - slug: type-narrowing
        description: "Type guards (typeof, instanceof, in), custom user-defined type guards, assertion functions, control flow analysis."
      - slug: utility-types-typescript
        description: "Partial, Pick, Omit, Record, ReturnType, Awaited, Parameters: quando usarli e quando creare un type apposta."
      - slug: type-safety-boundary
        description: "Tipi al confine API/DB: schema Zod → tipo TS inferito (singola fonte di verità), no any, never type come segnale, branded types per ID."
      - slug: forms-react-hook-form-zod
        description: "Pattern dominante per form in Next.js: react-hook-form + Zod resolver, singolo schema Zod condiviso tra client (validation) e server (Server Action / Route Handler), error mapping field-level, integrazione con Server Actions, accessibilità di base (label, aria-invalid, aria-describedby)."
    learning_outcomes:
      - "Spiega senza aiuto la differenza tra server e client component e identifica errori in codice AI-generated (es. 'use client' messo a un componente che non ne ha bisogno, fetch in useEffect, props non serializzabili)."
      - "Progetta struttura layout/route di una nuova app Next.js da zero motivando scelte (boundary client, struttura cache, dove streamare)."
      - "Identifica TypeScript any/cast pericolosi in codice AI e propone fix con generics o discriminated unions."
      - "Sa scegliere tra Server Action e Route Handler per ogni mutation con motivazione."
    layer: L2-L3
    mini_project: |
      (a) Review architetturale Next.js del menu-qr-saas: layout structure, server/client boundary, caching
          strategy across 4 layer. Report con almeno 5 issue (es. 'use client' over-used, hydration risk in
          componente X, caching mal configurato che invalida troppo o troppo poco).
      (b) Refactor di 3 componenti AI-generated rendendoli type-safe con discriminated unions, eliminando
          ogni `as any` e `as Type` non motivato. Diff con motivazione per ogni cambio.
    completion_criteria: |
      Report architettura con 5+ issue documentati. Refactor type-safe applicato. Spiegazione orale a Socrate
      del modello mentale completo della rendering pipeline App Router (request → match route → render server
      → stream client → hydrate → cache).
    resources:
      - "Next.js docs — App Router (sezioni Fundamentals, Caching, Rendering, Server Actions)"
      - "TypeScript Handbook — Generics, Narrowing, Advanced Types"
      - "Total TypeScript (Matt Pocock) — sezioni gratuite, in particolare narrowing e generics"
      - "React docs — Server Components RFC"
      - "react-hook-form docs + Zod resolver docs"
    estimated_hours: 106
    status: pending
    started: null
    completed: null
    examination_level: null

  - id: 5
    name: "Testing Strategy per AI-generated code"
    why: |
      AI scrive test scenografici che mascherano i bug invece di esporli (test che testano l'implementation
      invece del behavior, mock cascade che non valgono niente, copertura senza assert significativi).
      Devi sapere COSA testare (confini, autorizzazione, mutazioni di stato), QUALE livello (unit/integration/E2E),
      e leggere un test sapendo se è un test vero o un test teatro.
    prerequisites: [request-validation, error-handling-api, server-actions]
    concepts:
      - slug: test-pyramid
        description: "Unit/integration/E2E (e oggi 'testing trophy'), costi (velocità, fragilità) e benefici (confidenza), quando ognuno paga."
      - slug: what-to-test
        description: "Confini (input limite, off-by-one), error paths, autorizzazione (cross-tenant attempts), mutazioni di stato, NON happy path solo."
      - slug: unit-test-discipline
        description: "Arrange-Act-Assert, un test = una proprietà, no mock cascade che simulano l'intero universo."
      - slug: integration-tests-real-db
        description: "Integration test contro DB reale (testcontainers, Supabase local, schema branching) — perché mock-DB mente sui constraint e sulle RLS policy."
      - slug: e2e-playwright
        description: "Playwright basics, page object pattern, fixtures, parallel execution, gestione auth state."
      - slug: test-coverage-lies
        description: "Coverage % non significa qualità (puoi avere 100% senza un assert decente). Mutation testing come segnale reale di test che catchano bug."
      - slug: mocking-when-and-why
        description: "Mock di esterni inevitabili (Stripe, OpenAI, email), MAI mock del proprio codice (segnale che il design è sbagliato)."
      - slug: flaky-tests
        description: "Cause comuni (timing, ordering, shared state, randomness), come stabilizzare, perché un test flaky è peggio di nessun test."
      - slug: test-data-strategy
        description: "Factory pattern, builder, no fixtures-as-strings, named test data che racconta l'intent."
      - slug: ai-generated-test-smells
        description: "5+ pattern di test scenografici che AI scrive: test che ricalcano l'implementation, mock che si auto-confermano, assert tautologici, test commentati come 'TODO'."
      - slug: regression-tests-bug-driven
        description: "Ogni bug → test che riproduce il bug, quel test deve fallire prima del fix e passare dopo (red→green disciplina)."
      - slug: test-readability
        description: "Un test deve essere leggibile come specifica del comportamento, no setup fumoso, no condizionali, no loop nei test."
    learning_outcomes:
      - "Riconosce 5+ pattern di test AI-generated che non valgono niente e li riscrive con assert reali."
      - "Decide il livello (unit/integration/E2E) per ogni feature motivando il tradeoff."
      - "Progetta test strategy completa per nuovo dominio (prenotazioni palestra): cosa testare, dove, con che livello, con che strumenti."
      - "Sa scrivere E2E Playwright per flusso critico end-to-end (es. place-order da catalogo a conferma)."
    layer: L2
    mini_project: |
      (a) Review della test suite esistente del menu-qr-saas: identificare 5+ test che valgono poco
          (assert tautologici, mock self-confirming, copertura senza catch), riscrivere 3 di essi.
      (b) Scrivere E2E Playwright per flusso critico place-order (catalogo → carrello → checkout → conferma).
      (c) Scrivere integration test per place-order API contro DB Supabase locale (no mock DB) che verifichi
          RLS, idempotenza, error path.
    completion_criteria: |
      Test E2E place-order verde e stabile. Test integration place-order verde con DB reale (verifica RLS,
      idempotency, errori). Review test esistenti con 5+ issue documentati e 3 test riscritti.
    resources:
      - "Playwright docs — Best Practices, Page Object, Fixtures"
      - "Testing JavaScript (Kent C. Dodds) — sezione testing trophy gratuita"
      - "Vitest docs"
      - "Supabase docs — Local Development con CLI"
    estimated_hours: 90
    status: pending
    started: null
    completed: null
    examination_level: null

  - id: 6
    name: "AI Integration: LLM APIs, RAG, Prompt Engineering"
    why: |
      La parte di "specializzazione AI" del goal. Chiamare OpenAI/Anthropic/Gemini correttamente, structured
      outputs validati, RAG (chunking, embedding, vector DB), prompt engineering ingegneristico, gestione
      costi/latenza, fallback e guardrails. Questo è ciò che ti distingue da un Solutions Architect generico
      e da un developer che chiama un'API senza capirla.
    prerequisites: [rest-principles, error-handling-api, data-types-postgres, postgres-extensions]
    concepts:
      - slug: llm-api-basics
        description: "OpenAI/Anthropic/Gemini SDK, chat completion, ruoli system/user/assistant, temperature, top_p, stop sequences."
      - slug: structured-outputs
        description: "JSON mode, function calling, tool use, schema enforcement (Zod-validated post-output), come trattare l'output sempre come untrusted."
      - slug: streaming-responses
        description: "Streaming SSE, parsing incrementale, gestione interruzioni client-side, UX dello streaming."
      - slug: context-window-management
        description: "Token counting per ogni provider, context window limits, sliding window, summarization automatica della conversazione lunga."
      - slug: prompt-engineering-engineering
        description: "Prompt come codice: instruction following, few-shot examples, chain-of-thought esplicito, role priming, versioning del prompt."
      - slug: prompt-injection-defense
        description: "Cosa è prompt injection (OWASP Top 10 LLM #1), sanitization input utente, instruction hierarchies, delimiters, perché 'ignore previous instructions' funziona ancora."
      - slug: rag-fundamentals
        description: "Cosa è RAG, quando serve davvero (KB grande, contenuti che cambiano) e quando NON serve (contesto piccolo che entra nel prompt)."
      - slug: embeddings-basics
        description: "Cosa è un embedding (vettore N-dimensionale), come si genera (modello embedding), cosa misura la similarità (cosine, dot product)."
      - slug: vector-db-comparison
        description: "pgvector (in Postgres) vs Pinecone vs Weaviate vs Qdrant: costi, performance, operability, quando ognuno conviene per cliente PMI."
      - slug: chunking-strategies
        description: "Fixed-size, semantic chunking, sliding window con overlap, perché chunking fa o rompe la qualità del RAG, parent-child chunks."
      - slug: retrieval-quality
        description: "Hybrid search (vector + BM25/full-text), re-ranking con cross-encoder, evaluation metric (precision@k, recall@k, MRR)."
      - slug: llm-pricing-and-token-estimation
        description: "Base economica: pricing per token I/O di ogni provider (OpenAI/Anthropic/Gemini), differenza prezzo input vs output, come si conta un token (tokenizer per modello), stima costi per query tipo e per workload mensile."
      - slug: llm-cost-optimization
        description: "Avanzato: model routing (modello potente per hard tasks, economico per easy), prompt caching (Anthropic prompt caching, OpenAI), batching, prompt compression, semantic caching delle risposte."
      - slug: llm-latency-strategies
        description: "Streaming per perceived latency, parallel calls quando indipendenti, response caching deterministico, edge deployment."
      - slug: llm-fallback-and-guardrails
        description: "Timeout, retry policy con backoff, fallback su modello secondario, output validation (Zod), refusal/moderation handling."
      - slug: llm-evaluation
        description: "Golden set (input → output atteso), LLM-as-judge per scoring, human eval su sample, regression testing del prompt come del codice."
      - slug: agentic-patterns
        description: "ReAct (reasoning + acting), tool use, multi-step agents, when vale la complessità e quando è over-engineering per task semplici."
    learning_outcomes:
      - "Implementa da zero un endpoint RAG sul menu-qr-saas che risponde a domande sui prodotti in chat, con structured output validato, streaming, fallback model."
      - "Sa scegliere tra pgvector e vector DB esterno per nuovo cliente motivando in 5 punti (volume, query rate, operability, costo, latency)."
      - "Identifica prompt injection vulnerabilities in feature AI esistente o AI-generated e propone fix."
      - "Progetta strategia di cost optimization per workload AI di un cliente con stima costi mensili documentata."
    layer: L2-L3
    mini_project: |
      Aggiungere al menu-qr-saas una feature "AI Sommelier": chat che risponde a domande sui vini in catalogo.
      Requisiti:
      - RAG con pgvector sui dati vini reali del DB
      - Structured outputs validati con Zod
      - Streaming response
      - Prompt injection defense (sanitization input + instruction hierarchy)
      - Fallback model (es. Sonnet → Haiku in caso di errore/quota)
      - Cost tracking per query (token in/out + costo €)
      - ADR scritto con scelta del modello e stima costi mensili a 1000 query/giorno.
    completion_criteria: |
      Feature AI Sommelier funzionante con tutti i requisiti. ADR completo. Stima costi documentata.
      Test di prompt injection passato (almeno 5 attacchi tipici neutralizzati). Discussione orale con
      Socrate sulle decisioni prese in ogni layer (perché RAG e non solo prompt, perché pgvector e non
      Pinecone, perché quel modello, perché quel chunking).
    resources:
      - "OpenAI API docs / Anthropic API docs / Vercel AI SDK docs"
      - "OWASP Top 10 for LLM Applications (release più recente)"
      - "Anthropic Prompt Engineering guide (docs.anthropic.com)"
      - "pgvector docs + Supabase docs su pgvector"
      - "Documentazione pricing OpenAI / Anthropic / Gemini (aggiornata, cambia spesso)"
    estimated_hours: 116
    status: pending
    started: null
    completed: null
    examination_level: null

  - id: 7
    name: "Observability, Cost & Performance (scala PMI)"
    why: |
      Per clienti PMI la metrica che conta non è "scaliamo a 1M utenti", è "non ci facciamo svenare dai costi
      Supabase/Vercel/OpenAI e quando il sito va giù lo sappiamo prima del cliente". Logging strutturato,
      error tracking, cost analysis, query profiling, performance audit. Senza observability nessun feedback
      dalla produzione → non sai mai se la tua review è stata buona davvero.
    prerequisites: [query-performance, edge-vs-server-runtime, llm-cost-optimization]
    concepts:
      - slug: structured-logging
        description: "Log come JSON, correlazione (request id propagato), levels (debug/info/warn/error), MAI console.log in produzione."
      - slug: error-tracking
        description: "Sentry o equivalente, integrazione Next.js (server + client), upload sourcemap, alert rules, release tracking."
      - slug: monitoring-basics-paas
        description: "Vercel Analytics, Supabase logs (postgres + auth + edge), Cloudflare logs/analytics, come leggere le dashboard e cosa cercare."
      - slug: alerting-strategy
        description: "Cosa alerta davvero (sito down, errors > soglia, slow queries persistenti) vs cosa no (warning informativi), alert fatigue."
      - slug: incident-response-basics
        description: "Runbook per scenari comuni, postmortem blameless format, MTTR (mean time to recover), comunicazione cliente durante incident."
      - slug: backup-restore-supabase
        description: "Backup automatici Supabase (daily), Point-in-time recovery, restore drill (testare il restore!), concetti RTO/RPO."
      - slug: cost-analysis-paas
        description: "Leggere fattura Vercel/Supabase/OpenAI, identificare hot spots (top 5 cost drivers per app tipo), forecast costi."
      - slug: bundle-size-optimization
        description: "Bundle analyzer Next.js, dynamic import per code-splitting, tree shaking, ottimizzazione font (next/font), import barrel files."
      - slug: image-optimization-next
        description: "next/image, formats automatici (AVIF/WebP), sizes prop, priority per LCP, lazy loading di default."
      - slug: caching-layers
        description: "CDN (Cloudflare/Vercel edge), Next data cache, browser cache, headers HTTP (Cache-Control, ETag, Last-Modified)."
      - slug: database-query-monitoring
        description: "pg_stat_statements, Supabase query insights, slow query log, identificare query che costano e prioritizzare ottimizzazione."
      - slug: performance-budget
        description: "Come definirlo (es. LCP < 2.5s su 4G), come misurarlo (Web Vitals: LCP, INP, CLS), enforcement automatico nel CI."
      - slug: real-user-monitoring
        description: "RUM (dati utenti veri) vs synthetic (lab), Vercel Speed Insights, perché ti serve l'unione dei due."
    learning_outcomes:
      - "Configura observability completa per il menu-qr-saas: logging strutturato + Sentry + ≥3 alert critici + dashboard cost."
      - "Identifica top 5 cost drivers di un'app Next.js + Supabase + AI tipo e propone ottimizzazioni misurabili."
      - "Sa scrivere postmortem completo di un incidente (fittizio o reale) con timeline, root cause, action items."
      - "Conduce performance audit del menu-qr-saas con report Web Vitals (LCP/INP/CLS) e fix prioritizzati per impact."
    layer: L1-L2
    mini_project: |
      (a) Setup observability completa sul menu-qr-saas: structured logging (server + edge), Sentry attivo
          con sourcemap, almeno 3 alert configurati e testati (sito down, error rate spike, slow query),
          dashboard cost mensile leggibile.
      (b) Performance audit completo: report Web Vitals prima, top 5 issue identificati, fix applicato,
          report Web Vitals dopo con miglioramento misurato.
      (c) Postmortem fittizio di incidente "RLS policy regredita ha esposto ordini cross-tenant per 2 ore":
          timeline ricostruita, root cause, fix applicato, action items (test che avrebbe catchato, alert
          che avrebbe avvisato).
    completion_criteria: |
      Observability operativa con alert testati (trigger manuale ognuno). Performance audit con prima/dopo
      documentato. Postmortem completo discusso con Socrate (che farà obiezioni stile "manager preoccupato").
    resources:
      - "Sentry docs — Next.js integration, performance monitoring"
      - "Vercel docs — Analytics, Speed Insights, Logs"
      - "Supabase docs — Logs, Backups, Point-in-time Recovery"
      - "Web.dev — Core Web Vitals (LCP, INP, CLS)"
    estimated_hours: 70
    status: pending
    started: null
    completed: null
    examination_level: null

  - id: 8
    name: "Architectural Decision Making & Communication"
    why: |
      Meta-skill che chiude il plan. ADR (Architecture Decision Record), tradeoff analysis strutturata,
      scrivere RFC, presentare scelta tecnica al cliente PMI in 3 frasi senza diventare opaco. Senza questo
      nodo, i 7 precedenti sono inutilizzabili in commessa: sai le cose ma non sai venderle né documentarle
      per il futuro te o per altri.
    prerequisites: [rls-multi-tenant, soft-delete-pattern, rest-principles, server-vs-client-components, e2e-playwright, vector-db-comparison, structured-logging]
    concepts:
      - slug: adr-format
        description: "Architecture Decision Record nel formato Michael Nygard: titolo, status, context, decision, consequences. Quando scriverne uno."
      - slug: tradeoff-analysis-structured
        description: "Identificare opzioni reali (almeno 2-3), criteri di scelta espliciti, peso dei criteri, motivazione della scelta finale."
      - slug: rfc-writing
        description: "RFC come strumento di pre-decisione (prima della decisione, non dopo), audience interno/esterno, struttura, come gestire i commenti."
      - slug: decision-vs-implementation
        description: "La decisione è prima del codice, non un commento dentro al codice. Separare il livello 'che scegliamo' dal livello 'come lo facciamo'."
      - slug: cost-of-options-thinking
        description: "Ogni opzione ha un costo nascosto (skill team, debito tecnico futuro, lock-in fornitore, costo operativo). Renderlo esplicito."
      - slug: reversible-vs-irreversible
        description: "Decisioni reversibili (cambio facile) vs one-way doors (lock-in, dati migrati). Paranoia proporzionata al tipo."
      - slug: communication-non-tech
        description: "Spiegare scelta tecnica a cliente non-tech in 3 frasi senza opacità (no jargon gratuito) ma senza semplificazione menzognera."
      - slug: written-vs-verbal-decisions
        description: "Cosa va scritto (decisioni irreversibili, decisioni con disaccordo, decisioni che ti dimenticherai), cosa va detto, quando entrambi."
      - slug: decision-log-vs-changelog
        description: "Decision log = perché abbiamo fatto le scelte; changelog = cosa è cambiato. Audience diverso, granularità diversa."
      - slug: estimation-honest
        description: "Stima onesta = range con livello di confidenza, buffer giustificato, comunicare il rischio al cliente. Mai 'è facile, 2 giorni'."
      - slug: saying-no-tech-debt
        description: "Dire no a una feature richiesta perché il debito esistente la rende rischiosa o esplosiva. Motivarlo in modo che il cliente lo capisca e accetti."
      - slug: scope-creep-detection
        description: "Identificare scope creep precocemente (richieste 'piccole' che cambiano il contratto), comunicarlo al cliente con costo aggiuntivo, mai 'gratis'."
    learning_outcomes:
      - "Scrive ADR completo per decisione architetturale reale presa nel menu-qr-saas (es. soft delete vs hard delete su menu_items, pgvector vs Pinecone per AI Sommelier)."
      - "Presenta tradeoff tecnico a cliente PMI fittizio (Socrate impersona il cliente) in 3 frasi e regge 5 contro-obiezioni."
      - "Identifica nel menu-qr-saas le decisioni one-way door vs reversibili e tratta la profondità di analisi in modo proporzionato."
      - "Scrive 5 ADR retroattivi per scelte già fatte ma NON documentate, ricostruendo il contesto e le alternative escluse."
    layer: L2-L3
    mini_project: |
      (a) Scrivere 5 ADR retroattivi per scelte storiche del menu-qr-saas che non erano documentate
          (es. architettura monorepo, scelta stack Next.js+Supabase, soft-delete pattern, RLS multi-tenant
          come pattern di isolamento, scelta di pgvector se attivato).
      (b) Scrivere 1 ADR per una decisione attuale (es. "usare pgvector o Pinecone per AI Sommelier") con
          tradeoff analysis strutturata, opzioni considerate, criteri, scelta motivata, consequenze attese.
      (c) Role-play con Socrate: presentare al "cliente PMI" la scelta tecnica del progetto AI Sommelier
          in 3 frasi e reggere 5 obiezioni ("perché non usiamo ChatGPT direttamente?", "quanto mi costa al mese?",
          "se cambia OpenAI prezzi?", "se dico una cosa stupida il bot?", "i miei dati dove finiscono?").
    completion_criteria: |
      5 ADR retroattivi + 1 ADR nuovo (tutti strutturati, con context/decision/consequences/alternatives).
      Role-play superato: Socrate giudica chiarezza, solidità delle risposte alle obiezioni, assenza di jargon
      gratuito o di opacità. Vince il role-play = il "cliente" è convinto e non spaventato.
    resources:
      - "Michael Nygard — Documenting Architecture Decisions (articolo originale, gratis)"
      - "ThoughtWorks Tech Radar — modello di valutazione tecnologie"
      - "Will Larson — Staff Engineer (capitoli su writing e decisions)"
      - "Camille Fournier — The Manager's Path (sezioni su tech leadership e comunicazione)"
    estimated_hours: 60
    status: pending
    started: null
    completed: null
    examination_level: null

esame_finale:
  fase_corrente: null
  domande_risposte: []
  last_attempt: null
  outcome: null
---

## Goal narrativo

Alessio non vuole diventare un Solutions Architect dal nulla — in parte già lo è. Sa scoping, costing,
scelta dello stack ad alto livello, gestione cliente. Il vero gap che questo plan colma è la **profondità
tecnica necessaria per fare review affidabile di codice prodotto da AI**: identificare falle di sicurezza,
errori di architettura, problemi di performance, costi nascosti, type-safety bugiarda, test scenografici.

Non è "imparo a essere architetto". È "**imparo a essere il senior reviewer di un team in cui i developer
sono AI**". Diverso. Molto più mirato.

Lo stack è ancorato (Next.js + Supabase + AI APIs), il segmento cliente è ancorato (PMI italiane su commessa
custom), il modus operandi è ancorato (AI scrive, lui governa). Niente dispersione su Spring Boot, Kubernetes,
training di modelli. Niente vetrina enterprise. Tutto convergente sul tipo di progetto reale che sta facendo
(menu-qr-saas) e farà.

## Non-goals espliciti

- **ML Engineering** (training modelli, PyTorch, fine-tuning, MLOps) → si usano modelli via API.
- **DevOps/SRE profondo** (Kubernetes, Terraform, cluster Postgres HA, infra on-prem) → si usa PaaS managed.
- **Marketing / pre-sales / consulting puro** → il plan è tech-deep.
- **"Developer che fa fare tutto all'AI"** senza review → il plan è esattamente l'opposto.
- **Mobile native** (iOS/Android, React Native).
- **Microservizi / event-driven distribuiti** (Kafka, RabbitMQ) → over-engineering per PMI.
- **Design Patterns OOP classici (GoF)** → stack target è funzionale React.
- **Project management / Agile / Scrum** → non tech-deep, escluso.

## Mappa nodi

| # | Nodo | Perché |
|---|---|---|
| 1 | Security & Authentication | Fondamenta. AI sbaglia routinariamente qui e i danni sono i più gravi. |
| 2 | Database Design & SQL Mastery | Lo schema decide 12 mesi avanti. Prerequisito per API non-ingenue. |
| 3 | API Design & Backend Architecture | AI mette logica nei posti sbagliati. Senza profondità qui ogni route passa "perché sembra giusta". |
| 4 | Next.js App Router & TypeScript profondo | Stack target. Type safety = anti-hallucination tool. |
| 5 | Testing Strategy per AI-generated code | AI scrive test scenografici. Devi sapere cosa testare davvero. |
| 6 | AI Integration: LLM APIs, RAG, Prompt Engineering | La specializzazione AI del titolo. Distingue da SA generico. |
| 7 | Observability, Cost & Performance (scala PMI) | Per PMI la metrica è "non ci svenano i costi" e "quando va giù lo sappiamo". |
| 8 | Architectural Decision Making & Communication | Meta-skill. Senza questo, i precedenti 7 sono invenduti. |

## Dipendenze tra nodi

```
1 Security ─┐
            ├─→ 3 API ─→ 4 Next.js+TS ─→ 5 Testing ─→ 6 AI ─→ 7 Observability ─→ 8 Decision
2 Database ─┘
```

- 1 e 2 in parallelo (fondamenta indipendenti, ma 2 si lega a 1 via RLS).
- 3 richiede entrambi (auth dal 1, FK/transactions dal 2).
- 6 (AI) richiede 3 (API design) e tocca 2 (pgvector lato schema).
- 8 chiude: meta-livello che astrae da tutti i precedenti.

## Knowledge baseline (calibrato 2026-05-18)

**Totale 109 concept.** 10 L1 + 99 L0. Zero L2+. Tutti i 109 concept → path: **study** (target L2-L3, L1 non basta).

### Nodo 1 — Security & Authentication (13 concept)

| Topic | Baseline | Path |
|---|---|---|
| jwt-structure | L1 | study |
| rls-multi-tenant | L1 | study |
| env-variables | L1 | study |
| app-metadata | L1 | study |
| supabase-client-typescript | L0 | study |
| oauth-flow-basics | L0 | study |
| session-management | L0 | study |
| cors-policy | L1 (marginale) | study |
| owasp-top10-web | L0 | study |
| csrf-protection | L0 | study |
| sql-injection-prevention | L0 | study |
| secrets-rotation | L1 | study |
| gdpr-essentials-for-developers | L1 | study |

### Nodo 2 — Database Design & SQL Mastery (14 concept)

| Topic | Baseline | Path |
|---|---|---|
| schema-normalization | L1+ | study |
| foreign-keys-constraints | L0 | study |
| migrations-versioning | **L0 (antipattern interiorizzato — da correggere prima)** | study |
| indexing-strategies | L0 | study |
| soft-delete-pattern | L0 | study |
| transactions-acid | L0 | study |
| sql-joins-advanced | L0 | study |
| data-types-postgres | L0 | study |
| query-performance | L0 | study |
| postgres-extensions | L0 | study |
| constraints-checks | L0 | study |
| n-plus-one-problem | L0 | study |
| audit-log-patterns | L1 (marginale) | study |
| adr-format-basics | L0 | study |

### Nodo 3 — API Design & Backend Architecture (14 concept)

Tutti i 14 concept → **L0 / study**. Auto-dichiarazione batch ("non so nulla di questo nodo").

### Nodo 4 — Next.js App Router & TypeScript profondo (14 concept)

Tutti i 14 concept → **L0 / study**. Auto-dichiarazione batch.

### Nodo 5 — Testing Strategy per AI-generated code (12 concept)

Tutti i 12 concept → **L0 / study**. Auto-dichiarazione batch.

### Nodo 6 — AI Integration: LLM APIs, RAG, Prompt Engineering (17 concept)

| Topic | Baseline | Path |
|---|---|---|
| llm-pricing-and-token-estimation | L1 | study |
| Altri 16 concept (llm-api-basics, structured-outputs, streaming-responses, context-window-management, prompt-engineering-engineering, prompt-injection-defense, rag-fundamentals, embeddings-basics, vector-db-comparison, chunking-strategies, retrieval-quality, llm-cost-optimization, llm-latency-strategies, llm-fallback-and-guardrails, llm-evaluation, agentic-patterns) | L0 | study |

### Nodo 7 — Observability, Cost & Performance (13 concept)

Tutti i 13 concept → **L0 / study**. Auto-dichiarazione batch.

### Nodo 8 — Architectural Decision Making (12 concept)

Tutti i 12 concept → **L0 / study** con flag `pending_verification`.

**Nota:** alcuni concept di questo nodo (`communication-non-tech`, `estimation-honest`, `saying-no-tech-debt`, `scope-creep-detection`) potrebbero salire rapidamente a L1 per esperienza pratica non sistematizzata di Alessio. Prima sessione del Nodo 8: verificare con prompt "raccontami l'ultimo preventivo che hai fatto a un cliente".

## Pattern emersi dalla calibrazione

1. **"Cosa" ≠ "Operativo".** Per i concept L1 di Nodo 1, Alessio sa definire ma non sa applicare (RLS sa cos'è, non sa scrivere; JWT rubato sa che dura un'ora, non sa che fare). Il plan deve spingere su applicazione, non definizioni.

2. **Punti ciechi pericolosi specifici:** localStorage per JWT considerato normale (antipattern), CORS percepito come protezione completa (non lo è), migration considerate modificabili (antipattern grave).

3. **Migration immutabili** = prima cosa da correggere nel Nodo 2 prima di qualsiasi altro studio.

4. **Profilo coerente con memoria utente** "principiante backend/SQL". Non è anomalia: l'AI ha scritto il menu-qr-saas mentre Alessio faceva product/design. Il plan colma esattamente questo gap.

## Note

- **Profondità target dichiarata: L2-L3 sui concetti core.** Alessio ha esplicitamente rifiutato la profondità superficiale ("non voglio concetti superficiali"). Questo non è un non-goal, è un parametro di calibrazione: in Phase 3 i baseline L1 senza dimostrazione vanno comunque a `study` (non `review-light`), perché L1 non è sufficiente per il goal.
- **2 ore/giorno = ~14h/settimana.** Stima `estimated_weeks: 55` calcolata su 680 ore totali / 14h. Stima ottimistica; realistico 60-65 settimane con review, errori, applicazione lenta.
- **Progetto di riferimento per applicazione obbligatoria: menu-qr-saas.** Quasi ogni mini-project lo usa come terreno di prova. Vantaggio: continuità di contesto. Rischio: se il progetto stagna, anche l'applicazione del plan stagna — da monitorare.
- **Plan precedente "ai-solutions-builder" archiviato** come `test-plan-discarded` il 2026-05-18. Era un draft di prova senza Phase 2/3/4 completate.
- **Overlap caching segnalato (Phase 2 gap G8):** `caching-strategies-nextjs` (Nodo 4, 4 layer Next.js: route/data/router/memoization) e `caching-layers` (Nodo 7, CDN/browser/HTTP headers) sono distinti ma vicini. Studiare uno NON copre l'altro.

## Advanced topics — futuro (post-plan)

Topic emersi in Phase 2 (gap review 2026-05-18) e deferiti a plan futuro o approfondimento on-demand:

- **Accessibility (WCAG 2.1 AA basics)** — EU European Accessibility Act 2025 lo rende obbligatorio per molte categorie commerciali. Non blocca il goal "review codice AI" ma diventa requisito cliente. Da affrontare come plan separato se/quando un cliente lo richiede esplicitamente. (Gap G9)
- **Postgres triggers (uso generale)** — necessari per `audit-log-patterns` (già incluso nel Nodo 2) ma l'uso esteso (constraint complessi, materialized view refresh, business logic in trigger) è niche. Da affrontare on-demand quando un caso reale lo richiede. (Gap G10)
