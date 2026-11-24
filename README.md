## Hi, I'm Iván Padrón 👋

**Senior backend engineer, 8+ years in C#, .NET, SQL Server and Azure. Specialized in APIs, integrations and event-driven systems.**

📍 Dominican Republic · Remote · English (B2 - intermediate) · Spanish (native)
🌐 CV: [EN](https://github.com/Dustman001/Dustman001/blob/main/EN.pdf) / [ES](https://github.com/Dustman001/Dustman001/blob/main/ES.pdf) · 🌐 [Portfolio](https://dustman001.github.io) · 💼 [LinkedIn](https://www.linkedin.com/in/ivan-amado-padron-perez-39474574) · ✉️ dustman01@gmail.com

---

**2022–2026, one property-tech client:** 1,600+ commits · 30+ repositories · ~450 work items · top contributor to its main web app.

### What I build

I connect systems that were never designed to talk to each other: canonical data models, typed APIs, change-data-capture pipelines and resilient messaging on Azure, from the SQL schema all the way to production.

### Highlights

**Universal Data Model.** I designed the canonical SQL Server model that unifies three platforms (property management, community and suite management) for **30+ management companies and ~500 US communities**. It is fed by CDC events and exposed through typed Azure Functions endpoints.

```mermaid
flowchart LR
  A[Property management] -- CDC events --> U[(Universal Data Model<br/>SQL Server)]
  B[Community platform] -- CDC events --> U
  C[Suite management] -- CDC events --> U
  U --> F[Typed API<br/>Azure Functions]
  K[API-key service] --> F
  F --> R[React consumer]
```

**CDC event system.** A timer-triggered Azure Functions listener reads SQL Server CDC tables in every client database and publishes "entity changed" events to Event Hub. LSN checkpoints stop duplicate processing, batch sizes are configurable, and Kubernetes health probes restart a stuck listener.

On the consumer side I made delivery loss-proof. Fast exponential-backoff retries are backed by a retry ceiling stored in SQL that survives instance restarts. Events that keep failing are dead-lettered with replay context, every write is deduplicated by LSN, and a transactional outbox commits the processed record and its outgoing event together. All per-event state is scoped by tenant, since two client databases can emit the same LSN. Testing end to end with real QA data then exposed six defects that had all looked like success, and I fixed them.

```mermaid
flowchart LR
  D[(Client DBs<br/>CDC tables)] -- poll --> L[CDC listener<br/>LSN checkpoint]
  L -- entity changed --> H{{Event Hub}}
  H --> C[Consumer<br/>retries · LSN dedup<br/>outbox · per tenant]
  C --> U[(Data model)]
  C -. after N tries .-> X[Dead-letter]
  H --> N[Notifications]
```

**Notification service.** It resolves recipients by community, role and subscription rules. Messages are queued on Azure Service Bus and delivered by email (SendGrid), SMS (Twilio) and push, with Polly v8 for resilience and Cosmos DB for tracking.

```mermaid
flowchart LR
  E[Change event] --> R[Recipient resolver]
  R --> Q[[Service Bus<br/>queue + retries]]
  Q --> M[Email · SendGrid]
  Q --> S[SMS · Twilio]
  Q --> P[Push]
  Q --> T[(Cosmos DB<br/>tracking)]
```

**Community platform architecture.** I worked across nearly every part of a community-management platform: a .NET Framework web app, its REST API, gatehouse kiosks with offline sync, background services, device sync and an accounting integration, all sharing one tested business library.

```mermaid
flowchart LR
  W[Web app<br/>WebForms + MVC] --> L
  M[Mobile app] --> A[REST API] --> L
  G[Gatehouse kiosks<br/>offline sync] --> S[Gate service] --> L
  L[[Shared business library<br/>NuGet]] --> D[(SQL Server)]
  S -. real-time .-> P{{PubNub}} -. push .-> G
  B[Background services] --> L
  X[Accounting platform] -- Service Bus --> A
```

**In-app notifications across three products.** A new in-app channel in the shared messaging hub (OAuth client-credentials tokens), hub-first delivery with a database fallback, role-aware deep links that open the exact record, and one config-driven host rule validated per environment.

**Also shipped:** an asynchronous reporting platform (scheduled reports, 300k+ row Excel exports), a legacy WebForms refactor into a tested service layer, bidirectional sync pipelines (ADF + Functions), an access-control and license-plate-recognition system (controllers scaled to 100,000 cards), a community web platform taken to GA, payroll modernization from batch to event-driven sync, and utility KPI apps that cut human errors by 80%.

### Toolbox

![C#](https://img.shields.io/badge/C%23-512BD4?style=flat&logo=dotnet&logoColor=white)
![.NET](https://img.shields.io/badge/.NET_8%2F10-512BD4?style=flat&logo=dotnet&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=flat&logo=microsoftsqlserver&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![Kubernetes](https://img.shields.io/badge/AKS_%2F_Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-844FBA?style=flat&logo=terraform&logoColor=white)
![React](https://img.shields.io/badge/React-149ECA?style=flat&logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)

- **Backend:** ASP.NET Core Web API, EF Core, Dapper, Polly, REST, SOAP, webhooks
- **Azure:** Functions, Service Bus, Event Hub, Data Factory, Key Vault, Application Insights, Azure DevOps
- **Data:** SQL Server, CDC, Cosmos DB, PostgreSQL, Redis
- **Architecture:** event-driven, microservices, serverless, transactional outbox, idempotent messaging, CQRS, DDD, TDD
- **AI tooling:** Claude Code, RAG, MCP
