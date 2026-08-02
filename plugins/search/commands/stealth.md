---
description: List stealth-required domains, or register one (arg: [domain])
argument-hint: [domain]
---

If `$ARGUMENTS` is empty: call the Levirge Search MCP `stealth_list` tool and show the domains as a
compact list (one per line). If empty, say so in one line.

If `$ARGUMENTS` contains a domain or URL: call `stealth_add` with domain="$ARGUMENTS" and confirm
what was registered (the tool normalizes URLs to their host).
