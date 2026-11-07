---
name: linkdr
description: >-
  Use the LinkDR MCP server (https://linkdr.com/mcp) to research Domain Rating (DR),
  organic traffic, keyword SERPs, and the LinkDR publisher catalog; inspect your LinkDR
  projects, outreach progress, live backlinks, and AI search answers (ChatGPT and Google
  AI Overview); approve or decline pending publisher sites; and quote or order DR-tiered
  backlinks. Trigger when the user asks about backlinks, domain authority, link building,
  SERP rankings, AI search citations, or their LinkDR account.
version: 1.0.0
homepage: https://linkdr.com/docs/mcp
metadata:
  site: https://linkdr.com
  mcp: https://linkdr.com/mcp
  docs: https://linkdr.com/docs/mcp
  pricing: https://linkdr.com/pricing
  contact: https://linkdr.com/contact
---

# LinkDR MCP Server Skill

LinkDR (`https://linkdr.com/mcp`) is a remote Streamable HTTP MCP server with browser OAuth 2.1. It gives agents access to a user's LinkDR projects, orders, outreach progress, placed backlinks, AI search answers, site approvals, backlink ordering, and domain/SERP research.

## Step 1. Check that the server is connected

Look for the LinkDR MCP tools (your client may prefix them, e.g. `mcp__linkdr__list_my_projects`):

- **Projects & strategy:** `list_my_projects`, `get_my_project`, `create_my_project`, `update_my_project_strategy`
- **Orders & approvals:** `list_my_orders`, `list_my_pending_approvals`, `decide_my_site_approval`, `order_backlinks`
- **AI search visibility:** `get_my_ai_answers`
- **Domain & SERP research:** `get_domains`, `get_keyword_serp`, `search_keywords`, `search_domains`
- **Support:** `submit_feedback`

If the tools are not connected, tell the user how to add the server:

- **Claude Code:** `claude mcp add --transport http linkdr https://linkdr.com/mcp`
- **Cursor / Claude Desktop:** Add a remote MCP server with URL `https://linkdr.com/mcp` (OAuth 2.1 opens in the browser on first use; no static API key is needed).

## Step 2. Recommended workflow

1. **Start with projects:** Call `list_my_projects` to discover the user's `slug` values. If the user does not have a project for their site yet, call `create_my_project` with `url` (and optional `competitors` up to 10 domains and `keywords` up to 25 searches).
2. **Inspect outreach & live links:** Call `get_my_project` with `slug` to read saved keywords, publisher outreach counts (`discovered`, `contacted`, `in_discussion`, `live`), every placed link with its latest crawler check (`live`, `removed`, `nofollow`), and project orders.
3. **Check AI search visibility:** Call `get_my_ai_answers` with `projectSlug` to see how ChatGPT and Google AI Overview answer the project's keywords, whether the user's brand is named, which of their pages are cited, and which competitor pages are cited.
4. **Research domains & SERPs:**
   - Call `get_domains` with up to 25 domains to compare Domain Rating (DR), organic traffic, keyword counts, and industry.
   - Call `get_keyword_serp` to inspect the top organic Google rankings for a keyword (`locationCode` defaults to `2840` US; pass `2826` for UK). Pass `refresh: true` only when a fresh SERP snapshot is needed (capped at 10 refreshes/hour).
   - Call `search_domains` to filter the cached publisher marketplace by `keywords`, `industry`, `country`, `minDR`/`maxDR`, and `minTraffic`/`maxTraffic`.

## Step 3. Reviewing and approving publisher sites

When an order uses the review-every-site plan (`siteApproval: "review"`):

1. Call `list_my_pending_approvals` (optionally filtered by `projectSlug` or `orderId`).
2. Present each pending site to the user: `publisherDomain`, `domainRating`, `traffic`, `pageUrl`, rationale, `priceNote`, and `approvalUrl`.
3. **Wait for explicit user confirmation** before calling `decide_my_site_approval`. Approvals cannot be undone via MCP.

## Step 4. Quoting and ordering backlinks

When the user wants to price or order backlinks:

1. **Always quote first:** Call `order_backlinks` with `targetUrl`, `links` (`dr30`, `dr40`, `dr50`, `dr60`), `anchorText` (`""` for natural variation), `contentType` (`"guide"`, `"listicle"`, or `"product-review"`), `siteApproval` (`"auto"` or `"review"`), and `billing` (`"once"` or `"monthly"`), leaving `confirmPaymentSpend` omitted or `false`.
2. **Show the preview to the user:** Share the exact `totalUsd`, tier breakdown, `savedCard` (if on file), and the prefilled `checkoutUrl`.
3. **Confirm only after an explicit yes:** Only after the user explicitly approves the quote, call `order_backlinks` again with `confirmPaymentSpend: true` and `expectedTotalUsd` set to the quoted total. If no card is on file, billing is monthly, or the bank requires 3-D Secure, give the user the `checkoutUrl` to complete payment in their browser.

## Reference

- Documentation: https://linkdr.com/docs/mcp
- Pricing: https://linkdr.com/pricing
- Support: https://linkdr.com/contact
