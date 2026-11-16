# heyllave MCP marketplace

Connect AI coding agents (Claude Code, Codex, or any MCP-capable client) to your [heyllave](https://heyllave.co) workspace: work orders, customers and catalog, straight from your tenant.

The MCP server lives at:

```
https://mcp.heyllave.co/api/v1/agent/mcp
```

Authentication is OAuth: the first time your client connects, it opens a browser window to log in to your heyllave account. No API keys or secrets are stored in config.

## Claude Code

Add this repo as a plugin marketplace, then install the plugin:

```sh
claude plugin marketplace add heyllave/heyllave-mcp
claude plugin install heyllave-mcp@heyllave
```

Or interactively: run `/plugin` inside Claude Code, add the `heyllave/heyllave-mcp` marketplace, and install **heyllave-mcp**. On first use, run `/mcp` and select **heyllave** to complete the OAuth login.

### Without the plugin system

You can also register the server directly:

```sh
claude mcp add --transport http heyllave https://mcp.heyllave.co/api/v1/agent/mcp
```

## Codex

This repo is also a [Codex plugin marketplace](https://developers.openai.com/plugins/build/plugins). Add it, then install the plugin:

```sh
codex plugin marketplace add heyllave/heyllave-mcp
codex plugin add heyllave-mcp@heyllave
```

The plugin declares the server as `streamable-http` with OAuth via dynamic client registration, so Codex walks you through the browser login on first use.

### Without the plugin system

For older Codex versions, the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) stdio wrapper handles the OAuth flow itself. Add to `~/.codex/config.toml`:

```toml
[mcp_servers.heyllave]
command = "npx"
args = ["-y", "mcp-remote", "https://mcp.heyllave.co/api/v1/agent/mcp"]
```

## Cursor

One-click install:

[![Add to Cursor](https://img.shields.io/badge/Add_to-Cursor-black)](https://cursor.com/install-mcp?name=heyllave&config=eyJ1cmwiOiJodHRwczovL21jcC5oZXlsbGF2ZS5jby9hcGkvdjEvYWdlbnQvbWNwIn0%3D)

Or add to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (per project):

```json
{
  "mcpServers": {
    "heyllave": {
      "url": "https://mcp.heyllave.co/api/v1/agent/mcp"
    }
  }
}
```

## VS Code (GitHub Copilot)

```sh
code --add-mcp '{"name":"heyllave","type":"http","url":"https://mcp.heyllave.co/api/v1/agent/mcp"}'
```

Or add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "heyllave": {
      "type": "http",
      "url": "https://mcp.heyllave.co/api/v1/agent/mcp"
    }
  }
}
```

## Gemini CLI

```sh
gemini mcp add --transport http heyllave https://mcp.heyllave.co/api/v1/agent/mcp
```

## Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "heyllave": {
      "serverUrl": "https://mcp.heyllave.co/api/v1/agent/mcp"
    }
  }
}
```

## claude.ai (web and desktop)

Add the server as a custom connector: **Settings → Connectors → Add custom connector**, paste the server URL, and complete the browser login. Custom connectors are available on paid Claude plans.

## ChatGPT

ChatGPT cannot use heyllave's automatic registration (that is reserved for the Claude Code and Codex loopback profiles), so it needs a tenant-approved OAuth client first:

1. A heyllave administrator (with the `oauth_clients.manage` permission) registers the client once:

   ```
   POST /api/v1/auth/oauth2/clients
   { "client_id": "heyllave-chatgpt", "name": "ChatGPT",
     "redirect_uris": ["https://chatgpt.com/connector_platform_oauth_redirect"] }
   ```

2. In ChatGPT: **Settings → Apps & Connectors**, enable **Developer mode** (advanced settings; Plus/Pro/Business plans), create a connector with the server URL, and set the **OAuth Client ID** to `heyllave-chatgpt` in the connector settings. Leave the client secret empty — heyllave uses public clients with PKCE only.

For listing in the official ChatGPT app directory, the server must additionally pass OpenAI's domain verification and app review — see the [Apps SDK deployment docs](https://developers.openai.com/apps-sdk/deploy/).

## Other MCP clients

Any client that speaks streamable HTTP MCP with OAuth (dynamic client registration, RFC 7591) can connect to the URL above. For stdio-only clients, use the `mcp-remote` wrapper shown in the Codex section.

## What's in this repo

```
.claude-plugin/marketplace.json      # Claude Code marketplace manifest
.agents/plugins/marketplace.json     # Codex marketplace catalog
plugins/heyllave/
  .claude-plugin/plugin.json         # Claude Code plugin metadata
  .mcp.json                          # MCP server definition (Claude Code)
  plugin.json                        # portable plugin manifest (Codex)
  mcp.json                           # MCP server definition (Codex, with OAuth/DCR config)
  skills/
    setup-catalog/SKILL.md           # migrate data into heyllave, set up the catalog
    work-orders-report/SKILL.md      # build reports from work orders
    create-dashboard/SKILL.md        # build KPI dashboards from heyllave data
```
