---
name: deal-health
description: Score a deal's health using the MEDDIC framework. Identifies gaps, rates risk, and recommends next 3 actions to advance the deal.
---

# Deal Health Skill

Score a deal's MEDDIC health and get specific next actions. Usage: `/deal-health [Account Name]`

## Instructions

When invoked with an account name:

### Step 1: Gather Information

**Primary — query CRM live:**
```soql
SELECT Id, Name, StageName, Amount, CloseDate, Probability,
       NextStep, LastActivityDate, LastStageChangeDate,
       Account.Name, Account.Industry,
       (SELECT Id, Name, Title, Email, Role FROM OpportunityContactRoles),
       (SELECT Id, Subject, ActivityDate, Description FROM Tasks
        ORDER BY ActivityDate DESC LIMIT 10),
       (SELECT Id, Subject, ActivityDateTime FROM Events
        ORDER BY ActivityDateTime DESC LIMIT 5)
FROM Opportunity
WHERE Account.Name LIKE '%{account_name}%'
  AND IsClosed = false
LIMIT 1
```

Also check for any MEDDIC-related custom fields your org may have set up (e.g., `MEDDIC_Champion__c`, `Economic_Buyer__c`, `Decision_Criteria__c`).

**If your CRM is not Salesforce**, query the equivalent objects via the configured MCP server.

**Supplement — check local qualitative notes:**
Look for `Accounts/[Account Name]/` in the repo. If found, read:
- `Account-Overview.md` — stakeholder context, political notes, anything not in the CRM
- `Engagement-Log.md` — qualitative notes not captured in CRM activities
- `Open-Items.md` — commitments and open questions

Local notes supplement CRM data — they capture what sellers know but don't put in the CRM.

**Also query productivity tools (if configured):**

Use whichever are available — WorkIQ/M365, Google Workspace, or Slack:
- "Show me all emails and meetings with [account name] in the last 60 days"
- "How frequently have I been in contact with [account name] recently?"
- "Any communications with executive-level contacts at [account name]?"

Extract these signals for MEDDIC scoring:
- **Email/meeting frequency**: High = engaged; dropping off = risk signal
- **Who's on the emails**: Champion only = no EB access; execs included = strong sign
- **Tone trend**: Emails getting shorter and slower? Enthusiasm fading? = yellow flag
- **Topics discussed**: Metrics, ROI, procurement, legal = late-stage progress signals
- **Silence gaps**: If no comms in 3+ weeks despite active stage = red flag

**If neither CRM, local notes, nor productivity tools are available**, ask the rep to describe the deal before scoring.

### Step 2: Score Each MEDDIC Dimension

For each dimension, rate: **Strong (Green)** / **Partial (Yellow)** / **Missing (Red)**

**Scoring criteria**:

**M — Metrics**
- Green: Specific, quantified success metrics agreed upon (e.g., "reduce research time by 50%, save $200K annually")
- Yellow: General goals stated but not quantified
- Red: No discussion of measurable outcomes

**E — Economic Buyer**
- Green: Met with EB, they're engaged and supportive
- Yellow: Know who they are but haven't met them
- Red: Don't know who controls the budget

**D — Decision Criteria**
- Green: Documented their evaluation scorecard; we influence the criteria
- Yellow: Know some criteria informally
- Red: Don't know how they're making the decision

**D — Decision Process**
- Green: Mapped every step to contract; have a mutual close plan with dates
- Yellow: Know the rough process but not specifics
- Red: Unknown — no visibility into their buying process

**I — Identify Pain**
- Green: Clear, quantified, urgent pain; executive-level acknowledgment
- Yellow: Pain described but not quantified or not urgent
- Red: Pain unclear or too vague to build urgency around

**C — Champion**
- Green: Identified champion who is actively selling for us internally, has EB access
- Yellow: Someone who likes us but hasn't been tested
- Red: No internal advocate

### Step 3: Generate the Deal Health Report

---

## Deal Health: [Account Name]
*Scored: [Today's date]*

### Overall Health: [🟢 Strong / 🟡 At Risk / 🔴 Critical]

**Deal snapshot**: [ARR] | [Stage] | [Close date] | [Days in current stage]

### MEDDIC Scorecard

| Dimension | Score | Evidence | Gap |
|-----------|-------|---------|-----|
| M — Metrics | 🟢/🟡/🔴 | [What we know] | [What's missing] |
| E — Economic Buyer | 🟢/🟡/🔴 | [What we know] | [What's missing] |
| D — Decision Criteria | 🟢/🟡/🔴 | [What we know] | [What's missing] |
| D — Decision Process | 🟢/🟡/🔴 | [What we know] | [What's missing] |
| I — Identify Pain | 🟢/🟡/🔴 | [What we know] | [What's missing] |
| C — Champion | 🟢/🟡/🔴 | [What we know] | [What's missing] |

**MEDDIC score**: [X/6 dimensions Green]

### Communication Patterns *(from productivity tools — omit if not configured)*

| Signal | Status | Detail |
|--------|--------|--------|
| Contact frequency (last 30 days) | [Active / Slowing / Silent] | [# emails, # meetings] |
| Most recent outreach | [Date] | [Who initiated — us or them] |
| Seniority of contacts engaged | [Exec level / Mid-level / Practitioner only] | [Names/titles seen in emails] |
| Tone trend | [Positive / Neutral / Cooling] | [Evidence] |
| CRM logging gap | [Up to date / Behind] | [Events in email not yet in CRM] |

**Communication health**: [🟢 Strong / 🟡 Slowing — act now / 🔴 Gone dark]

### Key Risks

[List the 2-3 biggest risks to this deal closing — be specific, not generic]

1. [Risk 1 — e.g., "No EB access — decision could go above our champion's head without us"]
2. [Risk 2]
3. [Risk 3]

### Recommended Next Actions

The 3 most impactful things to do in the next 2 weeks:

1. **[Action 1]** — Why: [reasoning tied to a specific gap] — How: [specific approach]
2. **[Action 2]** — Why: — How:
3. **[Action 3]** — Why: — How:

### Forecast Recommendation

Based on MEDDIC health: **[Commit / Upside / Best Case / Qualify Out]**

[1-2 sentence rationale]

---

### Step 4: Follow Up

"Based on this assessment, the biggest risk is [top gap]. Want to talk through how to address it, or should I help you draft the outreach for the Economic Buyer?"
