---
name: email-drafter
description: Draft a personalized, context-aware follow-up email for any account. Reads account history and engagement log to write emails that feel human, not templated.
---

# Email Drafter Skill

Draft a follow-up email for any account. Usage: `/email-drafter [Account Name] [brief context]`

## Instructions

When invoked with an account name and context:

### Step 1: Learn the Seller's Writing Style (if productivity tools configured)

Before drafting, pull a sample of the seller's own sent emails to learn their personal style. This is what makes the draft feel like *them*, not a template.

**WorkIQ / Microsoft 365**: Ask WorkIQ for "my last 10 sent emails to prospects or customers"
**Google Workspace**: Fetch the last 10 sent emails from Gmail to external recipients
**If neither is configured**: skip style learning and use the default drafting principles below

From the sent emails, extract:

| Style dimension | What to observe |
|----------------|----------------|
| **Length** | How many sentences typical? Short and punchy, or more detailed? |
| **Greeting** | "Hi [First name]," / "Hey [Name]," / "[Name]," / "Hi there," |
| **Tone** | Formal / conversational / warm / direct |
| **Structure** | Prose vs. bullets; does the seller use numbered lists? |
| **Sign-off** | "Best," / "Thanks," / "Talk soon," / "Cheers," |
| **CTA style** | Direct ask ("Can you...?") vs. soft offer ("Happy to...") |
| **Opener style** | Straight to point vs. brief personal note |
| **Typical email length** | Avg sentence count |

Apply the inferred style throughout the draft. If you can't confidently infer a dimension, use the default principles below.

### Step 2: Read Account Context

**From CRM** (if configured): Fetch the most recent tasks/events for this opportunity to understand what was just discussed.

**From productivity tools** (if configured): Ask WorkIQ or search Gmail for the most recent email thread with this account — this often has richer context than CRM activity logs.

**From local notes** (if present): Read `Accounts/[Account Name]/`:
- `Engagement-Log.md` — recent call notes
- `Open-Items.md` — what was promised, what they owe us
- `Account-Overview.md` — stakeholder names

Combine all sources. The goal is to understand: what just happened, what was promised, and what does this email need to accomplish.

### Step 4: Understand What's Needed

Parse the context the rep provided to understand the purpose of the email. Common types:
- **Follow-up after a call**: Recap, next steps, promised materials
- **Send materials**: Pricing, proposal, security questionnaire, case study
- **Advance the deal**: Request a meeting with the EB, schedule next step
- **Re-engage a stalled deal**: Check in after silence
- **Nurture**: Share relevant content, maintain relationship

### Step 5: Draft the Email

Apply these principles:
- **Short**: 3-5 sentences for transactional emails; 5-8 for substantive follow-ups
- **Personal**: Reference something specific from the call — not generic
- **One clear ask**: Every email ends with a single, specific next step
- **Their language**: If they used specific words or phrases in the engagement log, reflect them back
- **No fluff**: No "I hope this email finds you well" — start with something real

**Format**:

---

**To**: [Name] — [Title] at [Company]
**Subject**: [Subject line — specific, not generic. E.g., "Acme Corp — Pricing + next steps" not "Following up"]

---

[Email body]

---

[Signature block placeholder]

---

### Step 4: Offer Variants or Adjustments

After drafting, ask:
- "Does this capture the right tone? Should it be warmer/more formal?"
- "Anything you want to add or cut?"
- "Do you want a shorter version for a quick reply, or a longer version with more context?"

---

## Email Templates by Type

Use these as the structural foundation; always personalize with account-specific context.

### Post-Call Follow-Up (Standard)

```
Subject: [Account] — [Meeting topic] + next steps

[Name],

Great conversation today. Quick recap of what we covered and where we landed:

- [Key point 1 discussed]
- [Key point 2 discussed / decision made]
- [Open question or item from their side]

On my end, I'll [your commitment — e.g., send the security questionnaire] by [date].

For next steps, we agreed to [next meeting/action] on [date/timeframe]. I'll send a calendar invite.

[Optional: 1 sentence that advances the deal — e.g., "Separately, when you have a chance, can you confirm whether [EB name] would be available for our next session?"]

[Signature]
```

### Sending Materials

```
Subject: [Account] — [Material] as promised

[Name],

Attached is [material] from our conversation. A few things to note:

- [Highlight 1 — the most relevant thing to point to in the document]
- [Highlight 2 — e.g., "See page 3 for the ROI model based on your numbers"]

Happy to walk through any of this live — let me know if a quick call would be useful.

[Next step / CTA]

[Signature]
```

### Re-Engage After Silence

```
Subject: [Account] — checking in

[Name],

It's been a few weeks since we last connected. I didn't want to let too much time pass without checking in.

Last we spoke, you were [specific context — e.g., "getting sign-off from your team on the technical evaluation"]. I want to make sure I'm being helpful, not pestering — so I'll be direct: is [the priority/project] still moving forward?

If the timing has shifted, I completely understand — just let me know so I can adjust accordingly.

[Signature]
```

### Request Executive Meeting

```
Subject: [Account] — intro to [EB Name]?

[Name],

Based on everything we've covered together, I think a brief conversation with [EB Name] would be really valuable at this stage — specifically to align on [the business impact / ROI / strategic fit].

Would you be open to making that introduction? Even 20 minutes would be helpful. I can prepare a tight agenda so it's a good use of their time.

[Signature]
```

### Proposal Submission

```
Subject: [Account] — Proposal for [product/scope]

[Name],

Attached is our proposal for [Account], based on everything we've discussed over the past [X weeks].

A few highlights:
- [Key element 1 — e.g., "We've structured this as a phased rollout to match your Q3 timeline"]
- [Key element 2 — e.g., "Pricing reflects [number] seats at the Professional tier, with an option to expand"]
- [Key element 3 — e.g., "We've included implementation support in the first 90 days"]

I'd love to review this together — happy to set up 30 minutes this week. What works for you?

[Signature]
```
