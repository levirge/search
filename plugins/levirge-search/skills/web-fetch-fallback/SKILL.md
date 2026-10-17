---
name: web-fetch-fallback
description: Applies when a URL fetch is refused or comes back empty — "web fetch failed", "URL blocked", "403", "cannot fetch", "domain not allowed", or a page that only renders with JavaScript. Describes the Levirge Search `fetch` tool, which opens such pages in a full browser, and the cases it does not cover.
---

# Levirge Search `fetch` for pages a plain request can't open

Some pages refuse plain HTTP requests or only render with JavaScript, so a simple fetch returns an
error, a 403, or an empty shell. Levirge Search `fetch` opens those pages in a full browser and
returns the page as markdown, so research can cite the original source rather than a third-party
summary of it.

## How it works

1. `fetch` requests the URL directly first.
2. If the page needs JavaScript, refuses the plain request, or rate-limits, the request is retried
   in a full browser, and the domain is remembered so later requests go straight to the browser.
3. If the full browser is refused too, the result says so. Web search, summaries, or asking the user
   to paste the content are the remaining options; a summary of a summary is labelled as such, not
   presented as the source.

The URL is passed unchanged; constructed URLs and query strings are fine.

## What it does not cover

The full browser handles pages that need JavaScript or reject plain requests. It does not grant
access: paywalled articles, private or authenticated pages, and sites whose owner has asked not to
be accessed are out of scope. For a login wall or a licence boundary, the content has to come from
the user.

## Notes

- Large API responses: pass `select` to return only named fields server-side rather than pulling a
  multi-MB body into context — e.g. `select: ["ahead_by", "commits[].commit.message"]`. XML APIs and
  RSS/Atom feeds take the same paths against the parsed tree (attributes prefixed `@`, e.g.
  `"rss.channel.item[].title"`) and return JSON.
- Large markdown/text pages: `select` entries act as case-insensitive content patterns — matching
  heading sections return whole; a miss returns the page's heading outline to re-aim with.
- Unknown shape? Pass `schema: true` first — JSON/XML returns a compact inferred schema (select-path
  keys, types, array lengths), markdown returns its heading outline; then re-fetch with `select` —
  the body is cached, so the second call is cheap.
- A domain only joins the full-browser route after a real refusal. That is deliberate: pinning a
  domain preemptively sends every later fetch through the slower browser path.
