# EuroFlow — Modular Personal Budget & Savings Tracker (Germany)

A clean, privacy-first personal finance and budget tracker tailored for living in Germany. Built incrementally using strict engineering standards, space-efficient database design, and a local load-balanced architecture to double as a hands-on DevOps and system design lab.

---

## Why This Project Exists
1. **Practical Daily Budgeting in Germany:** Track fixed commitments (*Fixkosten* like *Miete*, *Deutschlandticket*, *Rundfunkbeitrag*), variable spending, and multi-month savings goals in EUR (`€`)—with support for switching display currencies.
2. **Privacy & Storage Discipline:** Built with GDPR/DSGVO principles in mind—strict user isolation, minimal data retention, and an export-first monthly archive flow that removes old expense rows from the database once downloaded.
3. **Hands-On System Design & DevOps Practice:** Designed with stateless app instances behind a local reverse proxy / load balancer so horizontal scaling, health checks, and failover can be tested live.

---

## Core Features
- **Secure Email & Password Auth + Smart Onboarding:** First-time logins launch a modern setup questionnaire to configure income, fixed German bills, initial budgets, and goals. Returning logins skip setup and go straight to the live dashboard.
- **Budget vs. Actual & Over-Budget Alerts:** Shows exact budget allocations ("what was assigned to what"), remaining *Safe-to-Spend* balance, and clear alerts when categories go over budget.
- **Dedicated Savings & Multi-Month Goals:** Track goals (like saving for a bike over multiple months) with progress graphs and mid-month contribution recalculations.
- **Collapsible Sidebar & Modular UI:** Dedicated pages (`/dashboard`, `/expenses`, `/goals`, `/downloads`, `/settings`) where users can toggle optional modules on/off, switch display currencies (`EUR`, `USD`, `GBP`, `INR`), and toggle Dark/Light mode.
- **Safe Archive-Then-Purge Downloads:** Export completed past months as CSV/JSON files, followed by an atomic database cleanup (with clear post-download removal notices) while preserving multi-month goals.

---

## Tech Stack & Why Each Was Chosen

| Layer | Technology | Why It Was Chosen |
| :--- | :--- | :--- |
| **Frontend & Routing** | **Next.js (App Router) + TypeScript** | Server-side route protection for first-time vs. returning users, fast page transitions, and strict compile-time type safety. |
| **UI & Charts** | **Tailwind CSS, shadcn/ui, Lucide, Recharts** | Lightweight, accessible copy-paste components, effortless Dark/Light mode, and clean responsive charts without heavy bundle bloat. |
| **Validation** | **Zod** | Enforces strict runtime validation at every form and API boundary so bad inputs never reach the database. |
| **Backend & Database** | **Supabase (PostgreSQL + Auth)** | Relational integrity, integer minor-unit (`BIGINT` cents) storage to prevent floating-point math bugs, and Row-Level Security (RLS) for strict data isolation. |
| **Testing** | **Vitest + React Testing Library** | Fast automated unit and component verification after every single feature before merging to `main`. |
| **Load Balancing & Sim** | **Nginx + Grafana k6** | Runs multiple stateless Next.js instances behind a local reverse proxy to test routing, health checks, and failover under 10–15 concurrent virtual users. |

---

## Engineering & Git Workflow
- **Incremental Build:** Built one module/phase at a time with passing tests before moving forward.
- **Branch Strategy:** Feature work happens on `feat/*` branches with regular commits; only tested, verified code merges into `main`.
- **Two-Tier Data Lifecycle:** Monthly expenses (`Tier 1`) can be archived and purged to keep database storage lean, while savings goals and compact monthly trend snapshots (`Tier 2`) persist across months.
