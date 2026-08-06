<p align="left">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/levirge-search-logo-dark.svg">
    <img src="assets/levirge-search-logo-light.svg" alt="Levirge Search" width="420">
  </picture>
</p>

# Levirge Search — web research your agent can actually reach

**Your agent's built-in fetch has a blocklist. This one doesn't.**

Levirge Search is a hosted [MCP](https://modelcontextprotocol.io) server giving agents
two things their host tools usually can't: unfiltered web search, and a fetch that
escalates to a stealth browser when a site blocks ordinary crawling. Reddit, X,
LinkedIn, Medium, Cloudflare-fronted docs — pages your agent currently answers about
from third-party summaries, read directly instead.

## Install (2 minutes)

Claude Code:

```bash
claude plugin marketplace add levirge/search
claude plugin install search@levirge-search
```

Then run `/mcp` in a session and sign in.

Codex CLI:

```bash
codex plugin add levirge/search
```

Any other MCP client — point it at `https://search.levirge.com/mcp`. It answers a
standard OAuth challenge, so clients that speak MCP authorization discover sign-in on
their own; no token to paste.

## What you get

- **Search that isn't a summary** — `search` runs SearXNG across engines and returns
  real result sets, not one provider's ranking.
- **Fetch that gets through** — `fetch` takes any URL, including agent-constructed API
  endpoints, and auto-escalates to a stealth browser on a bot-wall. One tool, both routes.
- **Background scans** — `fetch_async` queues up to 200 URLs through the same routing and
  paces them per-domain; collect with `fetch_results` instead of blocking.
- **A stealth registry that learns** — domains that needed escalation are remembered per
  tenant, so the second visit skips the retry (`/search:stealth`).
- **A failure queue worth reading** — `stealth_failures` lists domains where the stealth
  browser itself got blocked, including regressions on domains that used to work.
- **A skill that fires on refusal** — `web-fetch-fallback` teaches the agent to reach for
  this when its primary fetch says "blocked", instead of silently falling back to search.

## Commands

| Command             | Does                                                     |
| ------------------- | -------------------------------------------------------- |
| `/search:research`  | Multi-query search, fetch the top hits, synthesize with sources |
| `/search:fetch`     | Fetch one URL via routed fetch and summarize it           |
| `/search:stealth`   | List stealth-required domains, or register one            |

## Links

- **App / sign in:** <https://search.levirge.com>
- **Levirge:** <https://levirge.com>
- **Plugin reference:** [plugins/search/README.md](plugins/search/README.md)
