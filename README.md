# Heidi API and MCP server for Claude and ChatGPT

**Yes, ChatGPT, Claude and other AI agents can work with your Heidi account.** Cotlet.dev connects your own signed-in Heidi account so an agent can read sessions, draft and edit notes, create consultation documents, work with billing codes, search Heidi Evidence and customise note templates.

## What you can ask

> Go through today's sessions and notes.

> Write a session up as a SOAP note.

> Draft a referral letter or certificate.

> Add ICD-10 and SNOMED codes before claims.

> Ask Heidi Evidence a clinical question.

> Keep snippets and terms consistent.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Heidi tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"heidi":{"command":"uvx","args":["heidi-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t heidi-mcp .
docker run -i --rm -e COTLET_API_KEY heidi-mcp
```

Tools: `heidi_guide`, `heidi_status`, `heidi_open`, `heidi_read`, `heidi_click`, `heidi_type`, `heidi_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install heidi-mcp        # or run it once: uvx heidi-mcp connect
heidi-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 heidi_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 heidi_agent.py status    # is Heidi ready in your Cotlet.dev account
python3 heidi_agent.py guide     # Heidi screens and sign-in link
python3 heidi_agent.py read      # open Heidi, read the page, close the browser
python3 heidi_agent.py tools     # list Cotlet.dev tools
python3 heidi_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Heidi users

**How do I get access?**

Sign up free at https://cotlet.dev/signup?app=heidi, turn on Heidi under Integrations in the console and sign in to Heidi in your Cotlet.dev Cloud Browser.

**Does Heidi have an API?**

Heidi's API is for partners: its developer documentation describes integrations for EHRs, practice systems and telehealth platforms, with access through the Heidi developer programme.

**Does Cotlet sign a BAA or cover HIPAA?**

No. Cotlet signs no BAA and makes no HIPAA claim. Heidi's own compliance covers Heidi; check your obligations before allowing an AI agent to work with clinical notes.

**How is this different from Heidi Connectors?**

Heidi Connectors work the other way: your EMR or practice system exposes a small MCP server and Heidi works inside it. Cotlet lets ChatGPT, Claude or another agent work with your Heidi account.

**Can the agent record a consultation?**

No. Recording stays with you in Heidi, on your own device. The agent works with sessions and notes that are already in your Heidi account.

**Is the MCP server different from the REST API?**

Only in how it is called. Both carry the same 7 tools, inputs, answers, API key and price. Agents use MCP; scripts and n8n HTTP steps can use the API.

**Is Cotlet affiliated with Heidi?**

No. Cotlet is independent and not affiliated with Heidi or Heidi Health.

## Find it on

- [Heidi on Cotlet.dev](https://cotlet.dev/integrations/heidi-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=heidi&utm_content=find)
- [PyPI package heidi-mcp](https://pypi.org/project/heidi-mcp/)
- [Odel](https://odel.app/modules/cotlet/heidi)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/heidi/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-heidi-to-ai-agents-guide)

## Also works with

- [simplepractice MCP](https://github.com/cotlet-connect/simplepractice-mcp)
- [jane-app MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [clinicsense MCP](https://github.com/cotlet-connect/clinicsense-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/heidi-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=heidi

Start free: https://cotlet.dev/signup?app=heidi&utm_source=github&utm_medium=readme&utm_campaign=heidi
