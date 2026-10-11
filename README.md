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
