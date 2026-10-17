<p align="left">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/levirge-search-logo-dark.svg">
    <img src="assets/levirge-search-logo-light.svg" alt="Levirge Search" width="420">
  </picture>
</p>

[![version](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2Flevirge%2Fsearch%2Fmain%2Fplugins%2Flevirge-search%2F.codex-plugin%2Fplugin.json&query=%24.version&label=version&color=d99a3a)](plugins/levirge-search/.codex-plugin/plugin.json)

# Levirge Search — web research your agent can actually reach

**Search the web and read the sources, with citations.**

Levirge Search is a hosted [MCP](https://modelcontextprotocol.io) server for research: unfiltered
web search, and a URL fetch where pages that need JavaScript or refuse plain requests are opened in
a full browser — so your agent can open the original source and cite it.

## Install (2 minutes)

Claude Code:

```bash
claude plugin marketplace add levirge/search
claude plugin install levirge-search@levirge-search
```

Then run `/mcp` in a session and sign in.

Codex CLI:

```bash
codex plugin add levirge/search
```

Any other MCP client — point it at `https://search.levirge.com/mcp`. It answers a standard OAuth
challenge, so clients that speak MCP authorization discover sign-in on their own; no token to paste.

## Update

`plugin install` won't upgrade, and the desktop "Update" button stays greyed out until the app's
marketplace cache is refreshed (the dialog has no refresh button). From the CLI — it shares the same
store as the desktop app:

```bash
claude plugin marketplace update levirge-search
claude plugin update levirge-search@levirge-search
```

Then restart the client — the plugin registry and MCP tool schemas are cached at startup, so new
commands and tool params don't appear until then.

## What you get

- **Search that isn't a summary** — `search` runs SearXNG across engines and returns real result
  sets, not one provider's ranking.
- **Fetch that opens the page** — `fetch` takes any URL, including agent-constructed API endpoints;
  pages that need JavaScript or refuse plain requests are opened in a full browser. One tool, both
  routes.
- **Background scans** — `fetch_async` queues up to 200 URLs through the same routing and paces them
  per-domain; collect with `fetch_results` instead of blocking.
- **A stealth registry that learns** — domains that needed the full browser are remembered per
  tenant, so the second visit skips the retry (`/levirge-search:stealth`).
- **A failure queue worth reading** — `stealth_failures` lists domains where the stealth browser
  itself got blocked, including regressions on domains that used to work.
- **A skill that fires on refusal** — `web-fetch-fallback` teaches the agent to reach for this when
  its primary fetch says "blocked", instead of silently falling back to search.

## Commands

| Command                    | Does                                                            |
| -------------------------- | --------------------------------------------------------------- |
| `/levirge-search:research` | Multi-query search, fetch the top hits, synthesize with sources |
| `/levirge-search:fetch`    | Fetch one URL via routed fetch and summarize it                 |
| `/levirge-search:stealth`  | List stealth-required domains, or register one                  |

## Links

- **App / sign in:** <https://search.levirge.com>
- **Levirge:** <https://levirge.com>
- **Plugin reference:** [plugins/levirge-search/README.md](plugins/levirge-search/README.md)
