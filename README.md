# Inrō MCP server for Instagram

Connect your Instagram account to ChatGPT, Claude, Cursor or any MCP client, and run DMs, comments, automations and analytics by describing what you want.

Hosted, nothing to install. Built on Meta's official Instagram API (Inrō is a Meta Tech Provider).

|               |                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------- |
| Server URL    | `https://api.inro.social/mcp`                                                            |
| Transport     | Streamable HTTP, MCP protocol 2025-11-25                                                 |
| Auth          | OAuth 2.1 (PKCE, dynamic client registration), or a private API token from Inrō settings |
| Requires      | An Inrō Pro plan or free trial, and an Instagram Business or Creator account             |
| Tools         | 44, each covering a group of related actions                                             |

## What your assistant can do

- Read and reply to DMs and comments
- Build comment-to-DM automations from a sentence ("DM the guide to everyone who comments GUIDE on my last Reel")
- Create and send DM campaigns to a segment of contacts
- Manage contacts, folders, segments and contact properties (CRM)
- Configure the Inrō AI agent that answers DMs
- Publish posts and stories
- Ask what's working: post, automation and campaign results

## Connect

**Claude (claude.ai or Claude Desktop):** Settings, Connectors, Add custom connector, then paste `https://api.inro.social/mcp` and sign in to Inrō.

**ChatGPT:** add a custom connector (MCP) with the URL `https://api.inro.social/mcp` and sign in to Inrō. Custom connectors may require developer mode, depending on your ChatGPT plan.

**Cursor**, in `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "inro": { "url": "https://api.inro.social/mcp" }
  }
}
```

**Claude Code:**

```bash
claude mcp add --transport http inro https://api.inro.social/mcp
```

**Clients without OAuth:** send your private API token as `Authorization: Bearer <token>`. You can find it in your Inrō organization settings.

**Several Instagram accounts in one client:** some clients allow one connector per URL. Add any query string to make each URL unique, for example `https://api.inro.social/mcp?username=my_second_account`. It is ignored by the server.

## Tools

Related actions are grouped into one tool, and reading and changing are separate tools (`contacts` reads, `contacts_manage` writes). Your client can therefore ask for approval only on the tools that change something, and sending a campaign, triggering a scenario or deleting data is flagged as destructive.

| Area                | Tools                                                                  |
| ------------------- | ---------------------------------------------------------------------- |
| Contacts and CRM    | `contacts`, `folders`, `segments`, `contact_properties`                |
| Automations         | `scenarios`, `scenario_folders`, `executions`, `get_trigger_types`, `get_action_types`, `show_scenario` |
| Campaigns           | `campaigns`, `campaign_folders`, `show_campaign`, `get_contact_filter_types` |
| Messaging           | `send_message`, `conversion_links`                                     |
| Posts and stories   | `medias` (including comment-to-DM), `stories`, `media_files`           |
| AI agent            | `agent`                                                                |
| Profile settings    | `settings`, `icebreakers`, `persistent_menu`                           |
| Account and help    | `get_organization_info`, `search_templates`, `search_product_knowledge`, `batch_tasks` |

Everything except `*_manage` tools only reads. Publishing requires Instagram publishing permission on the connected account.

## Agencies

Manage several client accounts from one connection with the partner endpoint `https://api.inro.social/partner/mcp` and a partner token. Every tool then takes an `organization` argument (an id or an Instagram username), and four extra tools list, create and remove client accounts. The partner endpoint uses a bearer token, not OAuth.

## Safety

- Official Meta API only. No passwords, no unofficial access.
- Every request is logged in your Inrō account, under Profile, Platform logs.
- Tools that send messages, publish or delete are flagged as destructive, so MCP clients that support approvals ask you first.
- Contacts who opt out are skipped by every automation and campaign.

## Pricing

Inrō has a free plan. The MCP server needs Inrō Pro (from EUR 12.99 per month, with a 14-day free trial). Free accounts can connect and are shown how to upgrade.

## Links

- Product page: https://www.inro.social/product/mcp
- Guide: https://www.inro.social/blog/mcp-for-instagram
- Pricing: https://www.inro.social/pricing
- Help: https://help.inro.social

## License

The contents of this repository are MIT licensed. The Inrō service itself is proprietary.
