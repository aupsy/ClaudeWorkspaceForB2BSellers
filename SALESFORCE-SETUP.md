# CRM Setup Guide

How to connect your CRM to this workspace. Each seller does this once on their own machine. Credentials are stored locally and never committed to git — your CRM data stays yours, and your org's sharing rules are always respected.

---

## How It Works

This workspace uses an **MCP (Model Context Protocol) server** to query your CRM live during Claude sessions. When you run `/call-prep Acme Corp`, Claude queries your CRM as *you* — so you only see accounts and opportunities you already have access to in your CRM. No data is synced, stored in the repo, or visible to other sellers.

```
You → Claude → MCP Server → CRM API (as you) → live data back to Claude
```

---

## Salesforce Setup (~15 minutes)

### Prerequisites
- Python 3.10+ installed (`python --version`)
- `uv` package manager installed (`pip install uv` or see uv.astral.sh)
- A Salesforce account with API access enabled

### Step 1 — Install the MCP server

```bash
uv tool install mcp-server-salesforce
```

### Step 2 — Get your Security Token

Your Salesforce security token is required for API authentication.

1. Log into Salesforce
2. Click your avatar (top right) → **Settings**
3. In the left sidebar: **Personal** → **Reset My Security Token**
4. Click **Reset Security Token** — Salesforce will email it to you
5. Save it somewhere safe

> If your org uses IP allowlisting, you may not need a security token — ask your Salesforce admin.

### Step 3 — Configure your credentials

Copy `.env.example` to `.env` in the workspace root:

```bash
cp .env.example .env
```

Edit `.env` and fill in your values:

```
SALESFORCE_USERNAME=your.email@company.com
SALESFORCE_PASSWORD=yourSalesforcePassword
SALESFORCE_SECURITY_TOKEN=theTokenFromYourEmail
SALESFORCE_INSTANCE_URL=https://yourcompany.my.salesforce.com
```

**Finding your instance URL**: It's the URL in your browser when you're logged into Salesforce (e.g., `https://acme.my.salesforce.com`).

### Step 4 — Verify the connection

Open Claude Code in this workspace and ask:

> "Query Salesforce for my open opportunities"

If you see your pipeline, you're connected. If you get an auth error, double-check your security token — it resets every time you change your Salesforce password.

### Step 5 — Update CLAUDE.md

Open `CLAUDE.md` and set the CRM field:

```
CRM: Salesforce
```

---

## HubSpot Setup (~10 minutes)

### Step 1 — Create a Private App token

1. In HubSpot: **Settings** → **Integrations** → **Private Apps**
2. Click **Create a private app**
3. Name it (e.g., "Claude Workspace")
4. Under **Scopes**, select:
   - `crm.objects.contacts.read`
   - `crm.objects.deals.read`
   - `crm.objects.companies.read`
   - `crm.objects.notes.read`
   - `crm.objects.tasks.read`
5. Click **Create app** → copy the access token

### Step 2 — Update `.mcp.json`

Replace the `salesforce` block (or add alongside it) in `.mcp.json`:

```json
{
  "mcpServers": {
    "hubspot": {
      "command": "uvx",
      "args": ["mcp-server-hubspot"],
      "env": {
        "HUBSPOT_ACCESS_TOKEN": "${HUBSPOT_ACCESS_TOKEN}"
      }
    }
  }
}
```

### Step 3 — Configure your credentials

In your `.env`:

```
HUBSPOT_ACCESS_TOKEN=pat-na1-xxxxxxxxxxxx
```

### Step 4 — Update CLAUDE.md

```
CRM: HubSpot
```

---

## Microsoft Dynamics 365 Setup (~20 minutes)

Requires an Azure AD App Registration. Ask your IT admin if you don't have permissions to create one.

### Step 1 — Register an Azure AD app

1. Go to [portal.azure.com](https://portal.azure.com) → **Azure Active Directory** → **App registrations**
2. Click **New registration** → name it "Claude Sales Workspace"
3. Set redirect URI to `http://localhost` (type: Web)
4. After creation, note your **Application (client) ID** and **Directory (tenant) ID**
5. Go to **Certificates & secrets** → **New client secret** → copy the value immediately

### Step 2 — Grant API permissions

In your app registration → **API permissions**:
- Add **Dynamics CRM** → `user_impersonation`
- Click **Grant admin consent**

### Step 3 — Update `.mcp.json`

```json
{
  "mcpServers": {
    "dynamics": {
      "command": "uvx",
      "args": ["mcp-server-dynamics"],
      "env": {
        "DYNAMICS_TENANT_ID": "${DYNAMICS_TENANT_ID}",
        "DYNAMICS_CLIENT_ID": "${DYNAMICS_CLIENT_ID}",
        "DYNAMICS_CLIENT_SECRET": "${DYNAMICS_CLIENT_SECRET}",
        "DYNAMICS_INSTANCE_URL": "${DYNAMICS_INSTANCE_URL}"
      }
    }
  }
}
```

### Step 4 — Configure your credentials

In your `.env`:

```
DYNAMICS_TENANT_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
DYNAMICS_CLIENT_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
DYNAMICS_CLIENT_SECRET=your_secret_value
DYNAMICS_INSTANCE_URL=https://yourorg.crm.dynamics.com
```

### Step 5 — Update CLAUDE.md

```
CRM: Microsoft Dynamics 365
```

---

## Seismic Setup (~5 minutes)

Used by the `/hydrate seismic` command to pull pitch decks, battle cards, and playbooks from your Seismic library into the Knowledge/ base.

### Step 1 — Get your API token

1. Log into Seismic
2. Click your avatar (top right) → **Settings** → **Integrations** → **API Access**
3. Generate a new token (or use an existing personal access token)
4. Copy the token value

> If you don't see API Access in settings, ask your Seismic admin to enable API access for your account.

### Step 2 — Find your tenant URL

Your Seismic tenant URL is the domain you use to log in — e.g., `yourcompany.seismic.com`. Do not include `https://`.

### Step 3 — Configure your credentials

In your `.env`:

```
SEISMIC_TENANT=yourcompany.seismic.com
SEISMIC_TOKEN=your_bearer_token
```

### Step 4 — Find the folder path to hydrate from

In Seismic, navigate to the folder containing your pitch decks / battle cards. The folder path is the breadcrumb trail at the top of the page, e.g., `Enterprise Sales/Pitch Decks` or `Competitive/Battle Cards`.

### Step 5 — Run hydration

```
/hydrate seismic "Enterprise Sales/Pitch Decks"
/hydrate seismic "Competitive/Battle Cards"
```

Run multiple times for different folders. Each run adds to (not overwrites) what's already in Knowledge/.

---

## Productivity Tools Setup (Optional)

Connect one or more productivity tools to enrich call prep, deal health, and email drafting with context from your recent emails, meetings, and messages. Skills use whichever tools are configured and skip the rest — none are required.

---

### WorkIQ / Microsoft 365 (~5 minutes)

Best for: teams on Microsoft 365 (Outlook, Teams, Calendar).

Enables: recent email threads by account, meeting notes, calendar events, seller email style learning for `/email-drafter`.

#### Step 1 — Get your WorkIQ API key

1. Go to [workiq.com](https://workiq.com) and sign in with your Microsoft 365 account
2. Navigate to **Settings** → **API** → **Generate API Key**
3. Copy the key

> If your organization hasn't set up WorkIQ, ask your IT admin or use a personal trial at workiq.com. Alternatively, configure Google Workspace or Slack below.

#### Step 2 — Add to `.env`

```
WORKIQ_API_KEY=your_workiq_api_key
```

#### Step 3 — Uncomment in `.mcp.json`

The `workiq` MCP server block is already in `.mcp.json` but disabled (commented out). Remove the comments to activate it:

```json
"workiq": {
  "command": "npx",
  "args": ["-y", "@workiq/mcp-server"],
  "env": {
    "WORKIQ_API_KEY": "${WORKIQ_API_KEY}"
  }
}
```

#### Verify

Ask Claude: `"What emails have I sent to contacts at Acme Corp in the last 30 days?"` — you should see real results from your Outlook.

---

### Slack (~5 minutes)

Best for: teams that coordinate deals and account context via Slack channels.

Enables: search Slack threads mentioning an account, surface context not in CRM.

#### Step 1 — Create a Slack Bot

1. Go to [api.slack.com/apps](https://api.slack.com/apps) → **Create New App** → **From scratch**
2. Name it "Claude Sales Assistant" and select your workspace
3. Under **OAuth & Permissions** → **Bot Token Scopes**, add:
   - `channels:history` — read public channel messages
   - `channels:read` — list channels
   - `groups:history` — read private channels the bot is in
   - `search:read` — search messages
4. Click **Install to Workspace** → copy the **Bot User OAuth Token** (starts with `xoxb-`)
5. Your **Team ID** is in your Slack workspace URL: `https://app.slack.com/client/T0123456789/...` — it's the `T...` part

#### Step 2 — Invite the bot to relevant channels

In each Slack channel with deal/account discussions: `/invite @Claude Sales Assistant`

#### Step 3 — Add to `.env`

```
SLACK_BOT_TOKEN=xoxb-your-bot-token
SLACK_TEAM_ID=T0123456789
```

#### Step 4 — Uncomment in `.mcp.json`

```json
"slack": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-slack"],
  "env": {
    "SLACK_BOT_TOKEN": "${SLACK_BOT_TOKEN}",
    "SLACK_TEAM_ID": "${SLACK_TEAM_ID}"
  }
}
```

#### Verify

Ask Claude: `"Search Slack for recent messages mentioning Acme Corp"` — you should see threads from channels the bot was invited to.

---

### Google Workspace (Gmail + Calendar) (~15 minutes)

Best for: teams using Google Workspace (Gmail, Google Calendar).

Enables: recent email threads by account, calendar events, seller email style learning for `/email-drafter`.

#### Step 1 — Create a Google OAuth app

1. Go to [console.cloud.google.com](https://console.cloud.google.com) → **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **OAuth client ID**
3. Application type: **Desktop app** → name it "Claude Sales Workspace"
4. Download the JSON file — you'll need the `client_id` and `client_secret`

#### Step 2 — Enable required APIs

In **APIs & Services** → **Library**, enable:
- **Gmail API**
- **Google Calendar API**
- **People API** (for contact names)

#### Step 3 — Get a refresh token

Run this once to authorize and get a long-lived refresh token:

```bash
# Install the Google auth helper
pip install google-auth-oauthlib

# Run the authorization flow (opens a browser)
python3 -c "
from google_auth_oauthlib.flow import InstalledAppFlow
flow = InstalledAppFlow.from_client_secrets_file('client_secret.json',
  scopes=['https://www.googleapis.com/auth/gmail.readonly',
          'https://www.googleapis.com/auth/calendar.readonly'])
creds = flow.run_local_server(port=0)
print('Refresh token:', creds.refresh_token)
"
```

Copy the refresh token printed at the end.

#### Step 4 — Add to `.env`

```
GOOGLE_CLIENT_ID=your_client_id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your_client_secret
GOOGLE_REFRESH_TOKEN=your_refresh_token
```

#### Step 5 — Uncomment in `.mcp.json`

```json
"google-workspace": {
  "command": "uvx",
  "args": ["mcp-server-google-workspace"],
  "env": {
    "GOOGLE_CLIENT_ID": "${GOOGLE_CLIENT_ID}",
    "GOOGLE_CLIENT_SECRET": "${GOOGLE_CLIENT_SECRET}",
    "GOOGLE_REFRESH_TOKEN": "${GOOGLE_REFRESH_TOKEN}"
  }
}
```

#### Verify

Ask Claude: `"Show me recent emails to or from contacts at Acme Corp"` — you should see Gmail results.

---

## Troubleshooting

**"Authentication failed" (Salesforce)**
- Your security token resets every time you change your password. Get a new one via Settings → Reset My Security Token.
- Check that `SALESFORCE_INSTANCE_URL` ends with `.salesforce.com` and has no trailing slash.

**"No data returned" (any CRM)**
- Confirm your CRM user has API access enabled (ask your admin).
- Confirm the MCP server is installed: run `uvx mcp-server-salesforce --version`.

**"MCP server not found"**
- Make sure `uv` is installed and on your PATH: `uv --version`.
- Re-run `uv tool install mcp-server-salesforce`.

**"I can see other sellers' data"**
- This should not happen — the MCP server authenticates as you, so your org's sharing rules apply. If you're seeing records you shouldn't, contact your CRM admin — it indicates a sharing model issue in the CRM itself, not in this workspace.
