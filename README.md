<h1 align="center">João Vargas</h1>

<p align="center">
  Full-stack engineer building <strong>Nivela</strong>, a multi-tenant SaaS/ERP for service businesses in Brazil.<br>
  Financial correctness, tenant isolation and AI that asks before it acts.
</p>

<p align="center">
  <a href="https://jrasch7.github.io/portfolio/"><img alt="Interactive portfolio" src="https://img.shields.io/badge/portfolio-interactive-8b5cf6?style=for-the-badge"></a>
  <a href="https://www.linkedin.com/in/jo%C3%A3o-vargas/"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-João%20Vargas-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"></a>
  <a href="mailto:joaovargas1108@gmail.com"><img alt="Email" src="https://img.shields.io/badge/contact-email-18181b?style=for-the-badge"></a>
</p>

<p align="center">
  <img alt="Python" src="https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white">
  <img alt="Flask" src="https://img.shields.io/badge/Flask-3-000000?logo=flask&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-RLS-4169E1?logo=postgresql&logoColor=white">
  <img alt="Redis" src="https://img.shields.io/badge/Redis-RQ-DC382D?logo=redis&logoColor=white">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white">
  <img alt="Next.js" src="https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white">
  <img alt="Expo" src="https://img.shields.io/badge/Expo-SDK%2057-000020?logo=expo&logoColor=white">
  <img alt="Railway" src="https://img.shields.io/badge/Railway-deploy-0B0D0E?logo=railway&logoColor=white">
</p>

---

## Nivela at a glance

Nivela is for appointment-driven businesses: barbershops, salons, clinics, pet care, fitness studios. One platform for scheduling, customers, cash flow, fiscal documents, subscriptions and an in-app AI assistant.

| | |
|---|---|
| **290+** | services holding the business rules |
| **560+** | HTTP routes across 70+ blueprints |
| **200+** | Alembic migrations |
| **390+** | test files |

<sub>Counted in the codebase in November 2026 and rounded down.</sub>

> **Explore it interactively →** the [portfolio](https://jrasch7.github.io/portfolio/) has a clickable architecture diagram, a tenant-isolation simulator and a walkthrough of the AI confirmation flow.

## How it is built

```mermaid
flowchart LR
  subgraph Clients
    W[Browser<br/>Jinja2 + vanilla JS]
    M[Nivela Pay app<br/>Expo / React Native]
    P[Partner apps<br/>versioned API + webhooks]
  end
  subgraph App["Flask monolith (modular)"]
    B[Blueprints<br/>HTTP only]
    S[Services<br/>business rules]
    Q[Worker<br/>RQ + Redis, retries]
  end
  subgraph Data
    O[SQLAlchemy 2 + Alembic]
    D[(PostgreSQL<br/>tenant_id + RLS)]
  end
  subgraph Integrations
    X[Stripe / Asaas]
    F[NFS-e Nacional]
    R[Resend]
    A[Claude API]
  end
  W --> B
  M --> B
  P --> B
  B --> S
  S --> O --> D
  S --> Q
  Q --> D
  S --> X
  S --> F
  Q --> R
  S --> A
```

<details>
<summary><b>Why a modular monolith</b></summary>

One deploy, one codebase, clear layers. Blueprints only authenticate, read parameters, call a service and respond. Every new rule (money, stock, fiscal, commission, payment, integration) is born in a service, so the same rule serves the web app, the mobile app and the AI. Microservices only when there is real pain that justifies them.
</details>

<details>
<summary><b>Multi-tenancy with a second barrier</b></summary>

Every business query filters by `tenant_id`. PostgreSQL Row Level Security backs that up, so a forgotten `WHERE` in application code does not turn into a cross-customer leak. New tables are born with RLS in the same migration, and migrations that touch the schema are validated on a throwaway PostgreSQL (upgrade, downgrade, upgrade).
</details>

<details>
<summary><b>Money and fiscal</b></summary>

A title-based finance engine separates realized cash, contractual projection and overdue amounts. Bank reconciliation (OFX/CSV) is auditable and never changes cash on its own. A full review of the finance module found and fixed real bugs (interest applied per installment instead of on the total, refunds that were not symmetric, installment sums that diverged from the title), each now covered by regression tests. Fiscal documents are issued directly through the national NFS-e system and SEFAZ, with no paid middleman (recorded as an ADR).
</details>

<details>
<summary><b>Background work that survives failure</b></summary>

Emails, landing-page leads and heavy routines leave the request and run in an RQ worker. Each task gets its own app context, database session and tenant, and retries up to three times with growing waits (30 s, 2 min, 10 min).
</details>

## Nivela AI: useful without being unchecked

```mermaid
sequenceDiagram
  actor U as Operator
  participant AI as Nivela AI
  participant T as Tools (read-only)
  participant S as Server
  U->>AI: "Book a haircut for Carlos tomorrow 3pm with Rafa"
  AI->>T: look up customer, service, availability
  T-->>AI: facts for this tenant
  AI-->>U: proposed action (validated)
  U->>S: explicit confirmation
  S->>S: re-validate tenant, user, permission, slot
  S-->>U: executed exactly once
```

Three possible outcomes: a validated proposal, a clarifying question, or a clear refusal. The AI never writes business data directly. Every action needs explicit confirmation, is validated again on the server, is scoped to the right tenant and user, and runs only once (a repeated confirmation is ignored). Changes to prompts, tools or models are evaluated against a test set before they reach staging.

## Other work

| Project | What it is | Stack |
|---|---|---|
| **Nivela Pay** | iPhone and Android app with the business's payment account: balance, statement, transfers, receipts. No money rules in the app; amounts travel as integer cents. | Expo SDK 57, React Native, TypeScript strict |
| **[SulCore](https://github.com/sulcore/sulcore.github.io)** | Open, fully static company site built to show how we work: CI, CodeQL, OpenSSF Scorecard, Lighthouse CI, accessibility tests, numbered ADRs. | Next.js 16, TypeScript strict, Tailwind 4, Playwright |

## AI-assisted development, with guardrails

I use AI coding agents to investigate, plan, implement and review, and I do not delegate engineering judgment to them.

1. **Versioned context first.** Agents start from the repository's permanent rules (`AGENTS.md`), architecture and module docs, and a vault of product decisions and ADRs.
2. **Isolated work.** Every task starts from `staging` in its own branch or worktree. Agents never merge to the release branch or deploy production.
3. **Rules that protect the product.** Tenant isolation, authorization, migrations, financial correctness and regression coverage are non-negotiable.
4. **Evidence over summaries.** I read the actual diff and check additions and removals. An agent saying "done" is never the acceptance criterion.
5. **Quality gates.** Focused tests, broader suites when warranted, visual validation on real screens (desktop and phone), documentation updated in the same PR.
6. **Human decision points.** Merge, release and production validation stay deliberate human decisions, followed by smoke tests and monitoring.

## Engineering principles

- Start from the real operational workflow, then write code.
- Treat money, authorization and tenant isolation as first-class concerns.
- Ship in small, testable, additive increments with migration discipline.
- Keep documentation alive: outdated docs count as an unfinished delivery.

## Contact

[LinkedIn](https://www.linkedin.com/in/jo%C3%A3o-vargas/) · [joaovargas1108@gmail.com](mailto:joaovargas1108@gmail.com) · [Portfolio](https://jrasch7.github.io/portfolio/) · Novo Hamburgo, RS, Brazil
