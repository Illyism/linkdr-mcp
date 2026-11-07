# LinkDR: MCP server for backlink research, SERP analysis, AI search visibility, and managed link building

Connect Claude Code, Cursor, Claude Desktop, VS Code, Codex, or any MCP client to [LinkDR](https://linkdr.com): inspect Domain Rating (DR) and organic traffic across domains, check who ranks in Google SERPs, review how ChatGPT and Google AI Overview cite your site and competitors, approve or decline publisher placements, and order DR-tiered backlinks.

```bash
claude mcp add --transport http linkdr https://linkdr.com/mcp
```

This repository contains the client configuration, plugin manifests, and agent skill for the hosted LinkDR MCP server at `https://linkdr.com/mcp`. Full documentation is at [linkdr.com/docs/mcp](https://linkdr.com/docs/mcp).

## What your agent can do with LinkDR

- **Domain & publisher research:** Look up Domain Rating (DR), organic traffic, ranking keyword counts, and industry for up to 25 domains in one call (`get_domains`), or filter the publisher marketplace by keyword, industry, country, DR, and traffic (`search_domains`).
- **SERP & keyword discovery:** Inspect the top organic rankings for any keyword (`get_keyword_serp`) or search cached keyword analyses (`search_keywords`).
- **AI search visibility:** Check whether ChatGPT and Google AI Overview mention and cite your site or your competitors for your project keywords (`get_my_ai_answers`).
- **Projects & outreach tracking:** List your LinkDR projects (`list_my_projects`), inspect outreach progress across Discovered, Contacted, In discussion, and Live stages (`get_my_project`), add new sites (`create_my_project`), and expand tracked keywords and competitors (`update_my_project_strategy`).
- **Site approvals:** Review pending publisher proposals and site lists (`list_my_pending_approvals`) and approve or decline placements (`decide_my_site_approval`).
- **Backlink ordering:** Get an exact price quote and prefilled browser checkout URL for DR 30+, DR 40+, DR 50+, and DR 60+ backlinks (`order_backlinks`), or charge a saved card on the project after explicit confirmation.

## Setup

The server runs over Streamable HTTP at `https://linkdr.com/mcp` and authenticates with OAuth 2.1 in your browser. No static API keys are required.

### Claude Code

```bash
claude mcp add --transport http linkdr https://linkdr.com/mcp
```

Run `/mcp` inside Claude Code to sign in with your LinkDR account and view the available tools.

### Cursor

Add this to `~/.cursor/mcp.json` or `.cursor/mcp.json` in your project:

```json
{
  "mcpServers": {
    "linkdr": {
      "url": "https://linkdr.com/mcp"
    }
  }
}
```

### Claude Desktop and claude.ai

Open **Settings → Connectors → Add custom connector** and enter:

- **Name:** `LinkDR`
- **URL:** `https://linkdr.com/mcp`

When prompted, sign in to your LinkDR account and click **Allow**.

### VS Code

Add this to `.vscode/mcp.json`:

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

### Codex

Add this to `~/.codex/config.toml`:

```toml
[mcp_servers.linkdr]
url = "https://linkdr.com/mcp"
```

### Claude Code plugin & Agent Skill

Install the plugin and skill together in Claude Code:

```bash
claude plugin marketplace add Illyism/linkdr-mcp
claude plugin install linkdr@linkdr-mcp
```

Or install the skill into any coding agent with the Skills CLI:

```bash
npx skills add Illyism/linkdr-mcp
```

## Tools

| Tool | Description |
| --- | --- |
| `list_my_projects` | List the LinkDR projects you own or belong to: slug, domain, your role, keyword count, and order count. Use the slug with `get_my_project`. |
| `get_my_project` | One of your projects by slug: saved keywords, publisher outreach (discovered, contacted, in discussion, live), every link LinkDR placed for it with its latest check (live, removed, nofollow), and your orders. |
| `create_my_project` | Add a site to your LinkDR account: creates the project (or returns the one you already have on that domain), profiles the site, and seeds keywords and competitors. |
| `update_my_project_strategy` | Add competitor domains (up to 10) and keywords (up to 25) to one of your projects. Adds only; never removes, and never changes an order you paid. |
| `list_my_orders` | Your LinkDR orders, newest first: order id, status, link count, total, target URL, and project. Optional `projectSlug` limits it to one project. |
| `list_my_pending_approvals` | Sites waiting for your yes or no on review-plan orders (`site_list` rows or single-item `proposal` entries with DR, traffic, page URL, rationale, and price note). |
| `decide_my_site_approval` | Approve or decline pending sites from `list_my_pending_approvals` by `listId` or `orderItemId`. Always asks the user before submitting. |
| `get_my_ai_answers` | How ChatGPT and Google AI Overview answer your project keywords: whether the answer names you, your cited pages, and competitor citations. |
| `order_backlinks` | Quote or order backlinks by DR tier (`dr30`, `dr40`, `dr50`, `dr60`). Without `confirmPaymentSpend`, returns a price preview and prefilled `checkoutUrl`. Only charges a saved card after explicit user confirmation. |
| `submit_feedback` | Send a bug report, missing feature request, or question directly to the LinkDR team. |
| `get_domains` | Read LinkDR metrics for up to 25 domains in one call (Domain Rating, organic traffic, keywords, industry). |
| `search_keywords` | Search cached `KeywordAnalysis` rows by substring (cache-only). |
| `get_keyword_serp` | Inspect who ranks in Google SERPs for a keyword: rank, title, URL, website, domain, and description (up to 100 items; optional live refresh). |
| `search_domains` | Search the cached publisher marketplace by keywords, industry, country, min/max DR, and min/max traffic. |

## Try it

Ask your agent:

```text
Use the LinkDR MCP. List my projects, check outreach progress and live links for the first project, and check the Domain Rating of my top 5 competitors.
```

## Security & billing boundaries

- **Project-scoped access:** You only see and act on projects you own or belong to.
- **No unsolicited outreach:** No tool on `https://linkdr.com/mcp` sends emails to publishers.
- **Explicit quote confirmation:** `order_backlinks` always returns a quote (`status: "preview"`) and a browser `checkoutUrl` first. It never charges a saved card unless you explicitly confirm the exact quoted amount (`confirmPaymentSpend: true` and `expectedTotalUsd`).

## Links

- **Documentation:** [https://linkdr.com/docs/mcp](https://linkdr.com/docs/mcp)
- **Agent checkout skill:** [https://linkdr.com/SKILL.md](https://linkdr.com/SKILL.md)
- **Pricing:** [https://linkdr.com/pricing](https://linkdr.com/pricing)
- **Support:** [https://linkdr.com/contact](https://linkdr.com/contact)
- **Privacy & Terms:** [https://linkdr.com/privacy](https://linkdr.com/privacy) · [https://linkdr.com/terms](https://linkdr.com/terms)

Built by [LinkDR](https://linkdr.com).
