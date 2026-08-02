---
description: Research a topic — multi-query search, fetch top hits, synthesize with sources
argument-hint: <topic…>
---

Research **$ARGUMENTS** using the Levirge Search MCP server (tools `search` and `fetch` — e.g.
`mcp__search__search`; adjust the prefix to however the server is aliased in this session).

1. Run 2–3 `search` calls with distinct query phrasings of the topic (the literal phrase, a question
   form, and a narrower/technical variant).
2. Pick the 3–5 most promising distinct URLs across all result sets (skip duplicates and obvious SEO
   spam).
3. `fetch` each. If a fetch reports it went via stealth, just carry on — that's routing, not an
   error.
4. Synthesize: a tight answer to the topic first, then supporting detail. Every claim that came from
   a page gets its source URL. End with a "Sources" list of the URLs actually used.

If the searches disagree or the topic is contested, say so explicitly rather than averaging the
claims.
