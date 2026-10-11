# João Vargas

Full-stack developer building **Nivela** — a multi-tenant SaaS/ERP platform for service businesses in Brazil.

I build practical software for businesses that run on appointments, customer relationships, teams and cash flow. My current work is centered on turning operational and financial complexity into a product that is clear, reliable and pleasant to use.

## Building Nivela

Nivela is designed for appointment-driven businesses such as barbershops, salons, beauty clinics, pet care and fitness studios. The product combines day-to-day operations, finance and SaaS infrastructure in one platform.

### Product capabilities

- **Scheduling and service operations** — public booking pages per business, calendar, customer records, services, professionals, attendance flow, orders and walk-in queue.
- **Products and inventory** — product and service catalog, stock support and operational visibility.
- **Finance and cash flow** — receivables/payables routines, daily cash box, categories with configurable accounting mappings, reporting and business-oriented financial controls.
- **Customer support** — an in-app support inbox with ticket protocols, categories, sectors, priorities, deadlines, assignments, email replies, attachments, customer notifications and full conversation history.
- **SaaS foundation** — tenant isolation, role-based permissions, plan and billing flows, internal administration, fiscal workflows and document tracking.
- **Mobile and platform security** — ongoing work on mobile API security, authentication, automated regression coverage and database contracts.

### Nivela AI

Nivela AI is part of the product, not a separate side project. It lets users describe an operational request in natural language — for example, scheduling a service for a customer with a specific professional and time.

The AI can look up customers, services and availability, then returns one of three outcomes: a validated proposed action, a clarifying question or a clear refusal when the request cannot be completed safely.

It never writes business data directly. Every action requires explicit user confirmation, is validated again by the server, is scoped to the correct tenant and user, and is executed only once. This keeps AI useful for real operations without treating it as an unchecked automation layer.

## AI-assisted development, with guardrails

I use AI coding agents to investigate, plan, implement and review work — but I do not delegate engineering judgment to them.

- **Versioned context before changes** — agents start from the repository’s permanent rules in `AGENTS.md`, the delivery workflow, architecture and module documentation, plus a dedicated Obsidian vault for product decisions, ADRs, roadmap and domain knowledge.
- **Scoped, isolated work** — work begins from `staging` in an isolated branch/worktree. Agents do not merge into the release branch or deploy production on their own.
- **Rules that protect the product** — instructions make tenant isolation, authorization, migration discipline, financial correctness and regression coverage non-negotiable.
- **Evidence over summaries** — I inspect the actual diff and validate claims against the current codebase; an agent saying that a task is done is never the acceptance criterion.
- **Quality gates** — changes go through focused tests and regressions, broader test suites when appropriate, `compileall`, `git diff --check`, documentation updates and a PR quality gate before review.
- **Human decision points** — merge, release and production validation remain deliberate human decisions, followed by smoke tests and operational monitoring.

The result is a workflow where AI increases speed and coverage without being allowed to silently introduce risky business, financial or multi-tenant changes.

## Engineering approach

- Start from the real operational workflow before writing code.
- Treat financial accuracy, authorization and tenant isolation as first-class concerns.
- Ship in small, testable increments with migration discipline.
- Use documentation and automated tests to make complex systems safer to change.
- Apply AI to improve product workflows and engineering quality, with human control at the decision point.

## Current stack

- Python, Flask, SQLAlchemy and Alembic
- PostgreSQL
- Jinja2, Bootstrap 5 and vanilla JavaScript
- Docker, GitHub and Railway
- Stripe and Asaas billing integrations

## Contact

- Email: [joaovargas1108@gmail.com](mailto:joaovargas1108@gmail.com)
- GitHub: [@jrasch7](https://github.com/jrasch7)

> Building software that makes real business operations easier to run.
