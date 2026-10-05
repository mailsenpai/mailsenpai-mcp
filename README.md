<p align="center">
  <img src="assets/logo.png" alt="MailSenpai logo" width="128" height="128">
</p>

<h1 align="center">MailSenpai MCP server</h1>

<p align="center">
  Manage MailSenpai email marketing from your AI assistant: lists, subscribers, segments, templates, draft campaigns and campaign stats.
</p>

<p align="center">
  <a href="https://glama.ai/mcp/connectors/com.mailsenpai/mcp"><img src="https://glama.ai/mcp/connectors/com.mailsenpai/mcp/badges/score.svg" alt="MailSenpai MCP connector on Glama"></a>
</p>

This is the official MCP server of [MailSenpai](https://www.mailsenpai.com/), an email marketing platform made in Italy with servers and data in the EU. It is a **hosted, remote MCP server**: there is nothing to install or run locally.

> **About this repository.** It contains only documentation and client manifests (the official MCP Registry entry and configuration files for Claude, Cursor, Gemini CLI and other MCP clients). The server source code is not published here. The server runs at the URL below and is operated by MailSenpai.

| | |
|---|---|
| **Endpoint** | `https://mcp.mailsenpai.com/mcp` |
| **Transport** | Streamable HTTP |
| **Authentication** | OAuth 2.1 (authorization code with PKCE S256, dynamic client registration). No API keys. |
| **Official MCP Registry** | [`com.mailsenpai/mcp`](https://registry.modelcontextprotocol.io/v0/servers?search=com.mailsenpai/mcp) |
| **Docs** | https://mcp.mailsenpai.com/docs |
| **Privacy policy** | https://www.mailsenpai.com/privacy-policy/ (MCP-specific notice: https://mcp.mailsenpai.com/privacy) |
| **Terms** | https://www.mailsenpai.com/tc/ (MCP terms of use: https://mcp.mailsenpai.com/terms) |
| **Support** | https://mcp.mailsenpai.com/support |

## What you need

- A MailSenpai account with AI access enabled. You can start with a free demo account at [www.mailsenpai.com](https://www.mailsenpai.com/) (English: [en.mailsenpai.com](https://en.mailsenpai.com/)).
- An MCP client that supports remote servers with OAuth.

On first use, the client opens the MailSenpai sign-in page. Sign in with your MailSenpai email and password, review the requested access, accept the terms and you are sent back to the client. Your password is never shared with the AI client.

## What the tools can do

The server exposes 70 tools, each annotated as read-only, write or destructive.

| Area | What the assistant can do | Type |
|---|---|---|
| Account | See permissions, plan limits and visible lists | Read |
| Lists and custom fields | List, view, create, update, copy and delete lists and fields | Read and write |
| Segments | Create segments with conditions, update them and read their subscribers | Read and write |
| Subscribers | Search by email across lists, add, update, confirm, unsubscribe or delete; imports within plan limits | Read and write |
| Email templates | List, search, view, create, update and delete templates | Read and write |
| Campaigns | Create drafts, copy, schedule, pause or cancel | Read and write |
| Reports and bounces | Campaign statistics and recorded bounces | Read |
| Transactional emails | List and view them, and create them when the plan allows it | Read and write |
| Reference data | Countries and zones, field types and segment operators | Read |

Example prompts:

- "List my lists and how many confirmed subscribers they have."
- "Create a draft campaign for the Customers list using the Welcome template."
- "Summarize the statistics of my last five campaigns."
- "Create a segment of subscribers who joined in the last 30 days."

### Permissions and confirmations

- The access token only receives the permissions of your MailSenpai plan and role, even if the client asks for more.
- Deleting requires `confirm_delete`, sending requires `confirm_send`. Sending, scheduling and deleting are available only if your plan includes them.
- Calls are logged and rate-limited. Details: https://mcp.mailsenpai.com/rate-limits and https://mcp.mailsenpai.com/security

## Connect it

### Claude (claude.ai and Claude Desktop)

Settings → Connectors → **Add custom connector**. Name: `MailSenpai`, URL: `https://mcp.mailsenpai.com/mcp`. Select **Connect** and sign in with your MailSenpai account.

### Claude Code

```bash
claude mcp add --transport http mailsenpai https://mcp.mailsenpai.com/mcp
```

Then run `/mcp` inside Claude Code and choose **Authenticate** for `mailsenpai`.

This repository is also a Claude Code plugin (`.claude-plugin/plugin.json` + `.mcp.json`).

### ChatGPT

In ChatGPT settings, add a custom connector (app) with the URL `https://mcp.mailsenpai.com/mcp` and OAuth authentication, then sign in with your MailSenpai account. Depending on your ChatGPT plan, developer mode may be required.

### Cursor

One click: [Add MailSenpai to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=mailsenpai&config=eyJ1cmwiOiJodHRwczovL21jcC5tYWlsc2VucGFpLmNvbS9tY3AifQ==)

Or add it to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "mailsenpai": {
      "url": "https://mcp.mailsenpai.com/mcp"
    }
  }
}
```

Cursor shows a **Needs login** button next to the server: click it to sign in.

### Cline

Open the MCP Servers panel → **Remote Servers**, name `mailsenpai`, URL `https://mcp.mailsenpai.com/mcp`, transport Streamable HTTP. Or edit `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "mailsenpai": {
      "type": "streamableHttp",
      "url": "https://mcp.mailsenpai.com/mcp"
    }
  }
}
```

When Cline asks you to authenticate, sign in with your MailSenpai account.

### VS Code (GitHub Copilot)

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "mailsenpai": {
      "type": "http",
      "url": "https://mcp.mailsenpai.com/mcp"
    }
  }
}
```

### Gemini CLI

```bash
gemini extensions install https://github.com/mailsenpai/mailsenpai-mcp
```

Then run `/mcp auth mailsenpai` to sign in.

### Other MCP clients

Add a remote MCP server with the URL `https://mcp.mailsenpai.com/mcp`. Clients that only support local (stdio) servers can use a bridge such as [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "mailsenpai": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.mailsenpai.com/mcp"]
    }
  }
}
```

## Disconnect

Remove the connector from your AI client's settings. Issued access expires on its own; to revoke it right away, contact support from https://mcp.mailsenpai.com/support.

## Files in this repository

| File | Purpose |
|---|---|
| `server.json` | Entry of the official MCP Registry (`com.mailsenpai/mcp`) |
| `.mcp.json`, `mcp.json` | Generic MCP client configuration (remote HTTP server) |
| `.claude-plugin/plugin.json` | Claude Code plugin manifest |
| `.cursor-plugin/plugin.json` | Cursor plugin manifest |
| `plugin.json` | Agent plugin manifest (Kiro powers) |
| `gemini-extension.json` | Gemini CLI extension manifest |
| `assets/logo.png`, `assets/logo-400.png` | Logo (512×512 and 400×400) |

## Also listed on

[Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.mailsenpai/mcp) · [Smithery](https://smithery.ai/servers/infomailsenpai/mailsenpai) · [Glama](https://glama.ai/mcp/connectors/com.mailsenpai/mcp)

## License

The files in this repository (README, manifests and configuration examples) are released under the [MIT License](LICENSE). The license does not cover the MailSenpai service or the hosted MCP server, whose source code is not included here.
