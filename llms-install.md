# LinkDR MCP Server — Automated Installation Guide

This guide is written for AI coding agents (Cline, Cursor, Claude Code, Codex, Windsurf, etc.) to install and configure the LinkDR MCP server automatically. Follow the steps in order.

## What this server does

LinkDR (`https://linkdr.com/mcp`) is a remote Streamable HTTP MCP server for SEO, domain authority research, SERP analysis, AI search visibility (ChatGPT and Google AI Overview), publisher site approvals, and managed DR-tiered backlink ordering.

## Prerequisites

None for installation. The server runs over Streamable HTTP at `https://linkdr.com/mcp` and uses OAuth 2.1 in the browser when first called (no static API key is needed).

## Installation

Add the remote HTTP server to the client's MCP configuration:

### Cline / Cursor / Claude Desktop (`mcpServers` JSON)

```json
{
  "mcpServers": {
    "linkdr": {
      "url": "https://linkdr.com/mcp"
    }
  }
}
```

### Claude Code CLI

```bash
claude mcp add --transport http linkdr https://linkdr.com/mcp
```

### VS Code (`.vscode/mcp.json`)

```json
{
  "servers": {
    "linkdr": {
      "type": "http",
      "url": "https://linkdr.com/mcp"
    }
  }
}
```

### Codex (`~/.codex/config.toml`)

```toml
[mcp_servers.linkdr]
url = "https://linkdr.com/mcp"
```

## Verify the installation

Call `list_my_projects` (or `get_domains` with `domains: ["linkdr.com"]`). If the client is not signed in yet, it will open the LinkDR OAuth consent screen in the browser and then return the user's projects or domain metrics.

## Support

- Documentation: https://linkdr.com/docs/mcp
- Support: https://linkdr.com/contact
