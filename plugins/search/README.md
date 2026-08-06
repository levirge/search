# Levirge Search plugin

Slash commands and a skill over the [Levirge Search](https://search.levirge.com) MCP server: web
search via SearXNG, and a routed fetch that escalates to a stealth browser for sites that block
ordinary crawling. Commands install as `/search:research`, `/search:fetch`, `/search:stealth`.

## Install

```sh
claude plugin marketplace add levirge/search
claude plugin install search@levirge-search
```

Then restart the client. The desktop "Add marketplace" dialog accepts `levirge/search` too — it only
allows github.com, gitlab.com, bitbucket.org and GitHub Enterprise hosts, which is why this repo
exists rather than the self-hosted marketplace the service also serves.

### Update

`claude plugin install` does **not** upgrade an already-installed plugin — it prints "already
installed" and exits. To move to a newer version:

```sh
claude plugin marketplace update levirge-search
claude plugin update search@levirge-search
```

Restart afterwards. The restart is also what re-pulls the MCP tool list: `.mcp.json` is only a
pointer at the server; tool schemas are fetched at connect time and cached for the life of the
connection, so a server-side tool change stays invisible until the client reconnects.

On Codex, bump the version and reinstall, then open a **new** task — refreshed `interface` metadata
(name, logo, icon) does not appear in an already-running one.

## What's in it

|                            |                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| `/search:research`         | multi-query search, fetch top hits, synthesise with sources                                  |
| `/search:fetch`            | routed fetch of one URL (auto-stealth) and summarise                                         |
| `/search:stealth`          | list or register stealth-required domains                                                    |
| `web-fetch-fallback` skill | when a URL fetch is refused, escalate to the stealth fetch instead of falling back to search |

## Tools

| Tool               | Reads/writes          | Notes                                                              |
| ------------------ | --------------------- | ------------------------------------------------------------------ |
| `search`           | read-only, open-world | SearXNG across engines; returns `[{title, url, content}]`          |
| `fetch`            | read-only, open-world | any URL, including API endpoints; auto-escalates to stealth        |
| `fetch_async`      | queues work           | up to 200 URLs, paced per-domain                                   |
| `fetch_results`    | read-only             | collect what `fetch_async` queued                                  |
| `stealth_list`     | read-only             | domains known to need the stealth browser                          |
| `stealth_failures` | read-only             | domains where stealth itself was blocked — the investigation queue |
| `stealth_add`      | write, idempotent     | mark a domain as stealth-required                                  |
| `stealth_remove`   | destructive           | drop a domain so it routes direct again                            |

Every tool carries MCP annotations, so a host can auto-approve the read-only ones and gate
`stealth_remove` behind confirmation.

## Auth

The server requires authentication — **there is no anonymous access.** `/mcp` answers an RFC 9728
challenge pointing at `auth.levirge.com`, and clients that support MCP authorization run sign-in
automatically. Both MCP manifests use this OAuth flow and do not send an unconditional bearer
header.

## Manifests

The plugin ships four manifests so it installs cleanly across harnesses:

| File                                       | Read by                                                                |
| ------------------------------------------ | ---------------------------------------------------------------------- |
| `plugin.json` + `mcp.json` (root)          | [Agent Plugins 1.0.0](https://agent-plugins.org/specification) clients |
| `.claude-plugin/plugin.json` + `.mcp.json` | Claude Code                                                            |
| `.codex-plugin/plugin.json`                | Codex CLI (carries the `interface` branding block)                     |

`version` is duplicated across the three `plugin.json` files — bump all of them together.

## Source

This repo is the published marketplace. The plugin is developed in the `agent-search` repository
under `plugin/` and copied here — **edit it there**, not here, or the next sync will overwrite your
change.
