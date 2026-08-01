---
name: Export Sprig study results
description: Pull a study's configuration, its responses, and the AI-identified themes out of Sprig for analysis or downstream storage.
api: openapi/sprig-api-openapi-original.json
operations: [get-v1-surveys, get-v1-responses, get-v1-themes]
---

# Export Sprig study results

Use this to move Sprig study data into another store or reporting tool.

## Auth
- Send `Authorization: Bearer YOUR_API_KEY` (v1 export endpoints use the Bearer form).
- All requests must be HTTPS. Base URL: `https://api.sprig.com`.

## Steps
1. **List studies** — `GET /v1/surveys` (`get-v1-surveys`). Returns up to 1000 survey/study configs. Note each study's `sid`.
2. **Pull responses** — `GET /v1/responses?sid={sid}` (`get-v1-responses`). Returns up to 1000 responses per page.
3. **Pull themes** — `GET /v1/themes?sid={sid}` (`get-v1-themes`) to get AI-identified themes with their associated responses.

## Conventions
- **Pagination:** page with the opaque `cursor` param; set `limit` (1..1000). Keep following the returned cursor until exhausted.
- **Date filtering:** narrow with `start` / `end` (epoch milliseconds) to avoid large-request timeouts.
- **Errors:** `403` invalid key, `404` unknown study, `429` rate limited (Enterprise 1,000 QPS) — back off and retry. See `errors/sprig-problem-types.yml`.
