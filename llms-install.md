# Installing Levirge Search in Cline

Levirge Search is a hosted remote MCP server for web search and page reading. There is nothing to
install locally: add one remote server entry and sign in.

- **Server URL:** `https://search.levirge.com/mcp`
- **Transport:** Streamable HTTP (`"type": "streamableHttp"` in Cline's config)
- **Docs:** https://levirge.com/search/docs

## Prerequisites

A Levirge Search account. Sign up at https://search.levirge.com/signup.

## Steps

1. Open Cline's MCP settings file (`cline_mcp_settings.json`: **MCP Servers** icon → **Edit
   Configuration**) and add this entry under `mcpServers`:

```json
{
    "mcpServers": {
        "levirge-search": {
            "type": "streamableHttp",
            "url": "https://search.levirge.com/mcp"
        }
    }
}
```

Keep `"type": "streamableHttp"`. Without it, Cline assumes the older SSE transport and the
connection fails.

2. Save the file. The server answers `401` with OAuth discovery metadata, and Cline offers to sign
   in: follow its prompt to open the Levirge sign-in page in your browser, approve access, and
   return to Cline.

3. **If your Cline version does not offer a browser sign-in**, use a personal token instead: sign in
   at https://search.levirge.com, open **Profile → Your tokens**
   (https://search.levirge.com/profile), create a token, and add it as a header:

```json
{
    "mcpServers": {
        "levirge-search": {
            "type": "streamableHttp",
            "url": "https://search.levirge.com/mcp",
            "headers": { "Authorization": "Bearer <your-token>" }
        }
    }
}
```

4. Toggle the server off and on in **MCP Servers**. These tools should appear: `search`, `fetch`,
   `fetch_async`, `fetch_results`, `stealth_list`, `stealth_failures`, `stealth_add`,
   `stealth_remove` and `feedback`.

## Verify

Run a read-only call: use the `search` tool with the query `Model Context Protocol`. A JSON list of
results, each with `title`, `url` and `content`, means the install worked.

## Troubleshooting

- **`401 Unauthorized`:** the sign-in did not finish, or the token header is missing or revoked.
  Check the `Authorization: Bearer <token>` line and the token list in Profile.
- **Server shows disconnected:** check that the entry has `"type": "streamableHttp"`, then toggle
  the server off and on.
