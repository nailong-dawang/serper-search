---
name: "serper"
description: "Google search via Serper API. Use when the user asks for a web search, for source discovery (finding blogs/feeds), or when browser.search results are insufficient."
---

# Serper

## Purpose
Google search as an API via the Serper API (https://serper.dev). Best for programmatic discovery (e.g. finding security blogs with RSS feeds) and for queries where curated search results beat a general web search.

## Tooling
CLI: `./bin/serper-search`

```
serper-search "query" [--num 10] [--endpoint search|news|images|videos|places|scholar|autocomplete]
serper-search --endpoint scrape --url "https://example.com/page"
```
- `search` / `news`: JSON array of `{title, url, snippet}` (+`date`,`source` for news).
- `images`: `{title, image_url, page_url, thumbnail}`.
- `videos`: `{title, url, thumbnail, duration}`.
- `places`: `{title, address, rating, phone, website}`.
- `scholar`: like search, for academic papers.
- `autocomplete`: `{value}` suggestions.
- `scrape`: `{url, title, text}` (first 8000 chars). **Requires Serper scrape plan — current keys return 403.**
Keep `--num` modest (5-20); each call costs one Serper credit.

## Multi-key failover
Set `SERPER_API_KEY` env var (comma-separated for multiple keys), or put one key per line in `connectors.txt`. Keys are tried in order — on 401/402/403/429 it silently moves to the next one.

## Auth
Get a free key at https://serper.dev (2500 free queries). Set `SERPER_API_KEY` env var. Never commit keys to git.

## Operating Rules
1. Use this skill for web/API search tasks and source discovery.
2. Restrict authenticated requests to: google.serper.dev.
3. Do not print, log, or persist raw credentials.
4. If auth is missing or rejected, follow the Auth section rather than asking for a key.
