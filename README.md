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

The most portable path is the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) stdio wrapper, which handles the OAuth flow itself. Add to `~/.codex/config.toml`:

```toml
[mcp_servers.heyllave]
command = "npx"
args = ["-y", "mcp-remote", "https://mcp.heyllave.co/api/v1/agent/mcp"]
```

Recent Codex versions also support streamable HTTP servers natively; if yours does, you can instead use:

```toml
[mcp_servers.heyllave]
url = "https://mcp.heyllave.co/api/v1/agent/mcp"
```

Check your Codex version's MCP documentation for the exact keys if the native form doesn't connect.

## Other MCP clients

Any client that speaks streamable HTTP MCP with OAuth (dynamic client registration, RFC 7591) can connect to the URL above. For stdio-only clients, use the same `mcp-remote` wrapper shown for Codex.

## What's in this repo

```
.claude-plugin/marketplace.json      # Claude Code marketplace manifest
plugins/heyllave/
  .claude-plugin/plugin.json         # plugin metadata
  .mcp.json                          # MCP server definition
```
