# Levirge Search — Claude plugin

Slash commands and a skill for the [Levirge Search](https://search.levirge.com) MCP server:
web search via SearXNG, and a routed fetch that falls back to a stealth browser for sites
that block ordinary crawling.

## Install

```sh
claude plugin marketplace add levirge/search
claude plugin install search@levirge-search
```

Then restart the client. The desktop "Add marketplace" dialog accepts `levirge/search` too —
it only allows github.com, gitlab.com, bitbucket.org and GitHub Enterprise hosts, which is
why this repo exists rather than the self-hosted marketplace the service also serves.

### Update

`claude plugin install` does **not** upgrade an already-installed plugin — it prints
"already installed" and exits. To move to a newer version:

```sh
claude plugin marketplace update levirge-search
claude plugin update search@levirge-search
```

Restart afterwards. The restart is also what re-pulls the MCP tool list: `.mcp.json` is only
a pointer at the server, tool schemas are fetched at connect time and cached for the life of
the connection, so a server-side tool change stays invisible until the client reconnects.

## What's in it

| | |
| --- | --- |
| `/search:research` | multi-query search, fetch top hits, synthesise with sources |
| `/search:fetch` | routed fetch of one URL (auto-stealth) and summarise |
| `/search:stealth` | list or register stealth-required domains |
| `web-fetch-fallback` skill | when a URL fetch is refused, escalate to the stealth fetch instead of falling back to search |

## Auth

`.mcp.json` points at `https://search.levirge.com/mcp` and passes an optional bearer from the
plugin's `token` user-config field. Mint one in the app under **Profile → tokens**; leave the
field blank for anonymous access. The token is shown once and cannot be recovered.

## Source

This repo is the published marketplace. The plugin is developed in the `agent-search`
repository under `plugin/` and copied here — **edit it there**, not here, or the next sync
will overwrite your change.
