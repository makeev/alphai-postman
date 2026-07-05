# AlphaAI — Postman Collection

Official [Postman](https://www.postman.com/) collection for the **[AlphaAI](https://alphai.io/developers)** REST API — relevance-scored, ticker-linked financial news (plus SEC Form 4 insider data) for AI agents and trading bots.

Every article is enriched at ingest with per-ticker impact analysis, a category, and a **1–10 relevance score**. Consume it here over REST, or as an [MCP server](https://mcp.alphai.io) in Claude Desktop / Cursor / any agent.

- 🔑 **Free tier, no card** — 20 req/min · 100/day
- 📖 **Live docs & playground** — <https://alphai.io/developers>
- 🧭 **OpenAPI 3.1 spec** — <https://api.alphai.io/api/schema/> · try it in-browser via [**Swagger UI**](https://api.alphai.io/api/schema/swagger-ui/)
- 💳 **Pricing** — Free · Basic $2.99/mo · Pro $9.99/mo (<https://alphai.io/pricing>)

---

## Quickstart

1. **Get an API key** — sign up at [alphai.io](https://alphai.io) and create one at [`/account/api-keys`](https://alphai.io/account/api-keys). Keys look like `ak_live_…`.
2. **Import into Postman** — import both files from this repo:
   - `AlphaAI.postman_collection.json` (the requests)
   - `AlphaAI.postman_environment.json` (the `baseUrl` + `bearerToken` variables)
3. **Select the "AlphaAI — Production" environment**, paste your key into the `bearerToken` variable, and hit **Send** on any request.

That's it — auth is set at the collection level (`Authorization: Bearer {{bearerToken}}`) and `baseUrl` points at `https://api.alphai.io`, so every request just works.

```bash
# The same call outside Postman:
curl -H "Authorization: Bearer ak_live_…" "https://api.alphai.io/api/news/?symbol=NVDA&min_relevance=7"
```

## Endpoints

### News
| Request | Path |
|---|---|
| List news (cursor-paginated) | `GET /api/news/` |
| Trending news (last 48h) | `GET /api/news/trending/` |
| Insider-transaction news (SEC Form 4) | `GET /api/news/insider/` |
| Get a news article by UID | `GET /api/news/{uid}/` |
| Articles related to one article | `GET /api/news/{uid}/related/` |

### Symbols
| Request | Path |
|---|---|
| List active tickers | `GET /api/symbols/` |
| Symbol detail | `GET /api/symbols/{ticker}/` |
| 7-day AI sentiment rollup | `GET /api/symbols/{ticker}/sentiment-summary/` |
| 30-day insider rollup | `GET /api/symbols/{ticker}/insider-summary/` |
| Related tickers (peers) | `GET /api/symbols/{ticker}/peers/` |
| Stock directory | `GET /api/symbols/directory/` |
| Sector's most-covered tickers | `GET /api/symbols/sectors/{slug}/` |

## Auth & rate limits

Send `Authorization: Bearer ak_live_…` on **every** request to `api.alphai.io`. Traffic is metered per account on two layers — a per-minute burst cap and a per-day volume cap; a request passes only if both are under budget:

| Tier | Per minute | Per day | Archive depth |
|---|---|---|---|
| Free | 20 | 100 | 30 days |
| Basic | 60 | 10,000 | 90 days |
| Pro | 150 | 100,000 | full |

Every response carries `X-RateLimit-Limit` / `-Remaining` / `-Reset` (the daily layer). A `429` names your tier, its caps, and an `upgrade` hint. Full semantics live in the [OpenAPI spec](https://api.alphai.io/api/schema/) and on [`/developers`](https://alphai.io/developers).

## Keeping the collection in sync

The collection is generated from the canonical **OpenAPI 3.1** spec (`openapi.yaml`, mirrored here from the live `/api/schema/`) with Postman's official converter:

```bash
npx openapi-to-postmanv2 -s openapi.yaml -o AlphaAI.postman_collection.json -p \
  -O folderStrategy=Tags,requestParametersResolution=Example
```

## Links

- Developers / playground — <https://alphai.io/developers>
- Swagger UI (try endpoints in-browser) — <https://api.alphai.io/api/schema/swagger-ui/>
- OpenAPI 3.1 spec — <https://api.alphai.io/api/schema/>
- MCP server — <https://mcp.alphai.io>
- Changelog — <https://alphai.io/changelog>
- Contact — <https://alphai.io/contact>
