# ZAPIER MCP + CLAUDE CODE

## Open Zapier MCP

1. Go to [https://mcp.zapier.com/](https://mcp.zapier.com/)
2. Sign in with your Zapier account

## Create MCP Server

1. Click **+ New MCP Server**
2. Client: **Claude Code**
3. Open **Connect** tab

## Connect MCP to Claude (PowerShell)

```powershell
claude mcp add --transport http zapier https://mcp.zapier.com/api/v1/connect
```

![Zapier MCP screenshot](https://raw.githubusercontent.com/MochithaCodeBasics/Test/main/Picture1.png)

In Powershell:

!\[PowerShell MCP Add Command](Invoice\_ZapierMCP\_Claudecode\_images/image\_1.png)

!\[MCP Connection Result](Invoice\_ZapierMCP\_Claudecode\_images/image\_10.png)

**(OR)**

### Authenticate in Claude Code UI

1. Open Claude Code UI
2. Type `/mcp`
3. Complete auth/connect flow for zapier

!\[Claude Code MCP Auth 1](Invoice\_ZapierMCP\_Claudecode\_images/image\_9.png)

!\[Claude Code MCP Auth 2](Invoice\_ZapierMCP\_Claudecode\_images/image\_7.png)

!\[Slack App Config](Invoice\_ZapierMCP\_Claudecode\_images/image\_5.png)

\---

## Add Tools in Zapier MCP (Configure Tab)

1. Add Gmail, Slack, Google Sheets, QuickBooks Online tools
2. Connect each account and complete OAuth
3. Save each tool

### Enable Actions for Each Tool

**Gmail:**

* Send Email (required)
* Find Email (recommended)
* Find Attachment / Get Attachment (recommended)

!\[alt text](image-1.png)

**Slack:**

* Send Channel Message (required)
* Send Direct Message (optional)
* Upload File to Channel (optional)
!\[alt text](image-2.png)

**Google Sheets:**

* Create Spreadsheet Row (required)
* Update Spreadsheet Row (optional)
* Find Spreadsheet Row (optional)

**QuickBooks Online:**

* Create Bill (required)
* Find Vendor (recommended)
* Find Bill (optional)
!\[alt text](image-3.png)

!\[Zapier MCP Tools Configuration](Invoice\_ZapierMCP\_Claudecode\_images/image\_4.png)

\---

## Verify Tool Auth

In Zapier MCP tools list, fix any warning with **Update auth**.

\---

## Configure Backend `.env`

Edit `Invoice\_Automator\_ClaudeCode\_ZapierMCP/backend/.env`:

```env
INTEGRATION\_MODE=mcp
ANTHROPIC\_API\_KEY=

APP\_BASE\_URL=http://localhost:8000
APPROVAL\_EMAIL\_RECIPIENTS=
REPORT\_EMAIL\_RECIPIENTS=
SLACK\_DEFAULT\_CHANNEL=finance-alerts

JWT\_PRIVATE\_KEY=
JWT\_PUBLIC\_KEY=
```

> \[!NOTE]
> If using localtunnel/ngrok, set `APP\_BASE\_URL` to current public HTTPS URL.

!\[Localtunnel Setup](Invoice\_ZapierMCP\_Claudecode\_images/image\_2.png)

Let localtunnel run in another terminal. Copy the URL and paste in `.env` as `APP\_BASE\_URL`.

\---

## Run Backend

### Initial setup

```powershell
cd Invoice\_Automator\_ClaudeCode\_ZapierMCP\\backend
py -3.12 -m venv .venv
.venv\\Scripts\\Activate.ps1
pip install -r requirements.txt
python -m uvicorn app.main:app --reload --port 8000
```

### Re-run (after initial setup)

```powershell
cd Invoice\_Automator\_ClaudeCode\_ZapierMCP\\backend
.venv\\Scripts\\Activate.ps1
python -m uvicorn app.main:app --reload --port 8000
```

### Health check

```powershell
Invoke-RestMethod http://localhost:8000/health
```

\---

## Run End-to-End MCP Tests from Claude

* Gmail send test mail
* Slack send test message
* Sheets create test row
* QBO create test bill (sandbox)

