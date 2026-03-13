# CLAUDE.md — B2B Sales Workspace

This file loads automatically every session. It defines who we are, what success looks like, and how to work effectively. Fill in the `[FILL IN]` sections before using this workspace.

---

## Company & Product Context

- **Company**: [FILL IN — e.g., Acme Corp]
- **Product**: [FILL IN — e.g., Acme Sales Intelligence Platform]
- **What we sell**: [FILL IN — 1-2 sentence description of what the product does and who it's for]
- **Ideal Customer Profile (ICP)**: [FILL IN — company size, industry, buying signals, e.g., "Mid-market B2B SaaS, 200-2000 employees, VP Sales or RevOps as economic buyer"]
- **Average deal size**: [FILL IN — e.g., $50K-$250K ARR]
- **Sales cycle length**: [FILL IN — e.g., 3-6 months]
- **Primary competitors**: [FILL IN — e.g., Competitor A, Competitor B, Competitor C]
- **CRM**: [FILL IN — e.g., Salesforce, HubSpot]

---

## Team Context

- **Team**: [FILL IN — e.g., Enterprise Sales, North America]
- **Manager**: [FILL IN]
- **Territory/Segment**: [FILL IN — e.g., Enterprise accounts >$1B revenue, AMER West]
- **Fiscal year end**: [FILL IN — e.g., June 30]

---

## Sales OKRs

*Update these quarterly. See `Knowledge/metrics/Sales-OKRs.md` for full details.*

| Objective | Key Result | Target | Current |
|-----------|------------|--------|---------|
| Hit quota | Closed ARR | [FILL IN] | [CURRENT] |
| Build pipeline | Pipeline coverage (3x quota) | [FILL IN] | [CURRENT] |
| Expand accounts | NRR | [FILL IN]% | [CURRENT]% |
| Win rate | % deals won | [FILL IN]% | [CURRENT]% |

---

## Key Documents

- **`Knowledge/product/Product-Overview.md`** — What we sell, key differentiators, ICP
- **`Knowledge/plays/Discovery-Framework.md`** — MEDDIC discovery questions and qualification criteria
- **`Knowledge/plays/Objection-Handling.md`** — Common objections and responses
- **`Knowledge/compete/Competitive-Landscape.md`** — Overview of competitive set
- **`Knowledge/metrics/Sales-OKRs.md`** — Full OKR detail with progress

---

## Priority Framework

- **P0**: Customer commitments, active deal decisions, manager requests with deadlines
- **P1**: Pipeline-building activities, active opportunities in late stages, QBR prep
- **P2**: Prospecting, relationship maintenance, competitive research
- **P3**: Administrative work, internal alignment, non-urgent learning

**Decision filter**: Does this move a deal forward? Does it build pipeline? Will it help me hit quota?

---

## Working Principles

- **"Understand before proposing"** — Learn the prospect's business before pitching. Ask, don't tell.
- **"Champion or no deal"** — If you don't have an internal champion, you don't have a deal.
- **"Pipeline is a lagging indicator"** — The leading indicators are activities: calls, meetings, demos, proposals.
- **"3-bullet rule"** — If you can't explain the customer's pain in 3 bullets, keep asking questions.
- **Always know the next step** — Every interaction ends with a confirmed next meeting or action.

---

## Sales Methodology

- **Qualification framework**: [FILL IN — e.g., MEDDIC, BANT, SPICED, Challenger]
- **Sales motion**: [FILL IN — e.g., Solution selling / Consultative / Product-led]
- **Typical buying committee**: [FILL IN — e.g., Economic Buyer (VP Sales), Champion (Sales Ops), Technical (IT/Security)]

---

## Available Skills

| Skill | Invocation | What it does |
|-------|------------|-------------|
| **Hydrate** | `/hydrate [source] [location]` | Bootstrap Knowledge/ from website, docs, folder, or Seismic — run this first |
| Call Prep | `/call-prep [Account Name]` | 1-page briefing before any call |
| Pipeline Review | `/pipeline-review [date]` | Weekly pipeline snapshot with risk flags |
| Deal Health | `/deal-health [Account Name]` | MEDDIC scoring + next actions |
| Account Research | `/account-research [Company Name]` | Public research → pain points + outreach hooks |
| Competitive Intel | `/competitive-intel [Competitor]` | Battle card retrieval |
| QBR Prep | `/qbr-prep [Account Name]` | Customer QBR narrative |
| Email Drafter | `/email-drafter [Account Name] [context]` | Context-aware follow-up email |

---

## Recurring Commitments

- **Weekly**: Pipeline review with manager (use `/pipeline-review`)
- **Monthly**: Forecast call — commit, upside, best case
- **Quarterly**: QBRs with top accounts (use `/qbr-prep`), personal deal retrospective
- **Ongoing**: Update `Accounts/[Name]/Engagement-Log.md` within 24 hours of every call

---

## Data Architecture

**Your CRM is the system of record for deals.** This workspace does not sync or store CRM data — it queries your CRM live via MCP so your org's sharing rules and RBAC are always respected. Each seller connects with their own credentials and only sees what they have access to in the CRM.

| What lives here (git) | What lives in your CRM | Queried live (productivity tools) | What stays local only |
|----------------------|----------------------|----------------------------------|----------------------|
| Shared knowledge (product, compete, plays) | Opportunities, contacts, activities | Recent emails + meetings (not in CRM) | Account qualitative notes (`Accounts/`) |
| Skills and automation | Pipeline and forecast data | Calendar events and upcoming calls | Generated pipeline reports |
| Win/loss retrospectives | Deal history | Slack threads mentioning an account | |

**`Accounts/` folders are for qualitative context that doesn't belong in your CRM** — political dynamics, relationship nuance, stakeholder motivations. They are gitignored and never shared. Skills use CRM data as the primary source, supplement with productivity tool context (email/calendar/Slack), and layer in local notes when present.

See `SALESFORCE-SETUP.md` to connect your CRM and optionally configure WorkIQ/M365, Google Workspace, or Slack.

---

## Document Naming Convention

Use YYYYMMDD timestamps for strategic docs: `document-name-20260120.md`
