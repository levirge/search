---
description: Fetch a URL via routed fetch (auto-stealth) and summarize it
argument-hint: <url>
---

Call the levirge-search MCP `fetch` tool with url="$ARGUMENTS".

Report back:

- A concise summary of the page (a few bullets; keep code/config snippets verbatim if the page is
  technical).
- Which route served it (`direct` or `stealth`) — for a page, the tool result's first content block
  reads `[via …]`. A JSON body is returned alone with no route block, so there is nothing to report
  in that case. If the domain was newly registered as stealth, mention that in one line.

For a JSON API, pass `select` to pull only the fields you need rather than reading the whole body,
e.g. `select=["ahead_by", "commits[].sha", "commits[].commit.message"]` — dot paths with `[]` array
wildcards. `max_bytes` caps a large text body. A JSON body is returned verbatim in its own content
block, so it parses directly.

If the fetch fails outright, report the error as-is — don't retry with made-up URL variants.
