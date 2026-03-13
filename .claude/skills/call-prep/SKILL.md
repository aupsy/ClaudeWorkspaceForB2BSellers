---
name: call-prep
description: Generate a 1-page pre-call briefing for any account. Queries your CRM live for opportunity data, contacts, and recent activity. Supplements with any local qualitative notes.
---

# Call Prep Skill

Generate a pre-call briefing for an account. Usage: `/call-prep [Account Name]`

## Instructions

### Step 1: Query Your CRM (Primary Source)

Use the CRM MCP server to fetch live data. Run these queries in order:

**Find the account and opportunity:**
```soql
SELECT Id, Name, StageName, Amount, CloseDate, Probability,
       NextStep, LastActivityDate,
       Account.Id, Account.Name, Account.Industry,
       Account.AnnualRevenue, Account.NumberOfEmployees,
       Account.Website, Account.BillingCity, Account.BillingCountry
FROM Opportunity
WHERE Account.Name LIKE '%{account_name}%'
  AND IsClosed = false
ORDER BY Amount DESC
LIMIT 1
```

**Fetch key contacts:**
```soql
SELECT Id, Name, Title, Email, Phone, Department
FROM Contact
WHERE AccountId = '{account_id}'
ORDER BY LastModifiedDate DESC
LIMIT 10
```

**Fetch recent activity (last 60 days):**
```soql
SELECT Id, Subject, ActivityDate, Description, Status,
       Who.Name, Who.Title
FROM Task
WHERE WhatId = '{opportunity_id}'
  AND ActivityDate >= LAST_N_DAYS:60
ORDER BY ActivityDate DESC
LIMIT 10
```

```soql
SELECT Id, Subject, ActivityDateTime, Description,
       Who.Name, Who.Title
FROM Event
WHERE WhatId = '{opportunity_id}'
  AND ActivityDateTime >= LAST_N_DAYS:60
ORDER BY ActivityDateTime DESC
LIMIT 5
```

**If your CRM is not Salesforce**, adapt the queries to the equivalent objects in your CRM (e.g., HubSpot Deals/Contacts/Activities, Dynamics Opportunities/Contacts/Activities).

**If MCP is not configured**, skip to Step 2 and rely on local notes, flagging that CRM data is unavailable.

### Step 2: Query Productivity Tools (if configured)

Check which productivity tool MCP servers are available and query all that are:

**WorkIQ / Microsoft 365** — use `ask_work_iq` to search for recent context:
- "Show me emails and meetings involving [account name] in the last 30 days"
- "What did I discuss with contacts at [account name] recently?"
- "Any calendar events coming up with [account name]?"

**Google Workspace** — search Gmail and Calendar:
- Search Gmail for threads where `to:` or `from:` matches any known contact email, or subject/body contains the account name, in the last 30 days
- Search Calendar for upcoming or recent events mentioning the account

**Slack** — search for account mentions:
- Search for messages containing the account name in the last 30 days
- Note the channel, participants, and key discussion points

**If none are configured**: skip this step — the briefing still works from CRM data and local notes.

**What to extract from productivity tools**:
- Most recent email exchange: date, subject, tone (positive/neutral/concerned), any open questions
- Most recent meeting: date, attendees, what was discussed (from notes or body)
- Anything discussed that isn't yet logged in CRM (the gap between what happened and what was recorded)
- Upcoming meeting or commitment visible in calendar

### Step 3: Check for Local Qualitative Notes (Supplement)

Look for `Accounts/[Account Name]/` in the repo. If found, read:
- `Account-Overview.md` — for stakeholder context, political notes, relationship nuance
- `Engagement-Log.md` — for qualitative call notes not captured in CRM activities
- `Open-Items.md` — for commitments and open questions

These supplement the CRM data — they capture what sellers know but don't (or shouldn't) put in the CRM.

### Step 5: Pull Relevant Knowledge

- If a competitor is mentioned in CRM, emails, or local notes, read `Knowledge/compete/[Competitor].md`
- For early-stage deals, read `Knowledge/plays/Discovery-Framework.md` for suggested questions
- Read `Knowledge/product/Product-Overview.md` only if needed to frame product context

### Step 6: Generate the Briefing

---

## Pre-Call Briefing: [Account Name]
*Generated: [Today's date] | Sources: [CRM ✓/✗] [M365/WorkIQ ✓/✗] [Google ✓/✗] [Slack ✓/✗]*

### Quick Context (30-second read)
[2-3 sentences: who they are, where we are in the deal, what this call needs to accomplish]

### Relationship & Deal Status
- **Stage**: [from CRM]
- **Deal size**: [Amount from CRM]
- **Expected close**: [CloseDate from CRM]
- **Last CRM activity**: [LastActivityDate — flag if >21 days ago]
- **Champion**: [from local notes or CRM contact roles]
- **Economic Buyer**: [from local notes or CRM — have we met them? Y/N]

### Recent Activity (from CRM)
[3-5 bullet summary of recent logged tasks/events — key interactions, what was discussed]

### Recent Communications (from productivity tools)
[Only if productivity tools are configured — what's happened in email/calendar/Slack that isn't yet in CRM]
- **Last email**: [Date | Subject | Tone — positive/neutral/concerned | Any open question]
- **Last meeting**: [Date | Attendees | What was discussed — from calendar notes or email recap]
- **Upcoming**: [Any calendar events with this account in the next 7 days]
- **Slack**: [Any notable Slack threads mentioning this account]
- **Gap vs. CRM**: [Flag anything discussed in email/meetings that hasn't been logged in CRM yet]

*If no productivity tools are configured, omit this section.*

### Qualitative Context (from local notes)
[Only if local account folder exists — political dynamics, relationship nuance, anything not in the CRM or email. If no local notes exist, omit this section.]

### Open Items Heading Into This Call
**We owe them** [from Open-Items.md or CRM tasks assigned to us]:
- [Item]

**They owe us** [from Open-Items.md or CRM tasks assigned to them]:
- [Item]

### Suggested Agenda
[3-5 point agenda based on deal stage and open items]

### Key Questions to Ask
[5-7 questions tailored to their situation — pull from Discovery Framework for early-stage deals]

### Competitive Watch
[If a competitor is in play: positioning notes, landmines, questions to surface differentiation]

### What a Good Outcome Looks Like
[Define success for this specific call]

---

### Step 7: Close with a Check-In

"Anything specific you want me to focus on, or any context I might have missed? [If productivity tools weren't configured: I didn't have access to your email or calendar — anything recent I should know about?]"
