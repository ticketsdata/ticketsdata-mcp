# TicketsData MCP Server

Live ticket listings, prices and fees inside your AI assistant.

The TicketsData MCP server connects Claude, Cursor, VS Code and any other
[Model Context Protocol](https://modelcontextprotocol.io) client to the
[TicketsData](https://ticketsdata.com) real-time API. Ask a question in plain
language and the assistant pulls live inventory from the major primary and
resale ticket marketplaces, with responses in about 1 to 2 seconds and no cache
in between.

> "What are the cheapest two seats for this event right now, all fees included?"
>
> "Compare this event across every marketplace and show me where the section spreads are widest."
>
> "List this artist's upcoming shows and tell me which ones have the thinnest inventory."

It is a hosted server: nothing to install.

- **Server URL:** `https://mcp.ticketsdata.com/mcp`
- **Transport:** Streamable HTTP
- **Auth:** `Authorization: Bearer YOUR_MCP_API_KEY`

## Get a key

1. Create an account or start a trial at [ticketsdata.com](https://ticketsdata.com).
2. Open **Dashboard, Settings, MCP Access** and copy your MCP API key.

You can regenerate the key there at any time.

## Connect

**Claude Code**

```bash
claude mcp add --transport http ticketsdata https://mcp.ticketsdata.com/mcp \
  --header "Authorization: Bearer YOUR_MCP_API_KEY"
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "ticketsdata": {
      "url": "https://mcp.ticketsdata.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_MCP_API_KEY"
      }
    }
  }
}
```

**VS Code** (`.vscode/mcp.json`)

```json
{
  "servers": {
    "ticketsdata": {
      "type": "http",
      "url": "https://mcp.ticketsdata.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_MCP_API_KEY"
      }
    }
  }
}
```

**Claude Desktop** (`claude_desktop_config.json`, uses the `mcp-remote` bridge)

```json
{
  "mcpServers": {
    "ticketsdata": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote", "https://mcp.ticketsdata.com/mcp",
        "--header", "Authorization:${AUTH_HEADER}"
      ],
      "env": {
        "AUTH_HEADER": "Bearer YOUR_MCP_API_KEY"
      }
    }
  }
}
```

## Tools

| Tool | What it does | Cost |
| --- | --- | --- |
| `list_marketplaces` | Supported marketplaces and the URL format each expects | Free |
| `get_event_listings` | Live listings for one event on one marketplace: section, row, quantity, price, fees | 1 credit |
| `find_events` | Upcoming events for a performer, team, organizer or venue page | 1 credit |
| `compare_marketplaces` | One event matched and priced across every marketplace, with per-section spreads | 1 report + up to 12 credits (Pro and above) |

All tools are read-only and return the complete REST API response, unchanged.
Supported marketplaces: [ticketsdata.com/marketplaces](https://ticketsdata.com/marketplaces).

## Credits and limits

Every call is a live fetch and spends credits from your plan, exactly like the
REST API. To keep an agent stuck in a loop from draining your plan, MCP access
is capped at 30 calls per minute and 1,000 calls per 24 hours per account, and
one cross-market report at a time. For bulk or scheduled work use the
[REST API](https://ticketsdata.com/docs).

## Docs and support

- Setup guide: [ticketsdata.com/docs#mcp](https://ticketsdata.com/docs#mcp)
- Support: [ticketsdata.com/contact](https://ticketsdata.com/contact)
