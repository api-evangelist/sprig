---
name: Manage Sprig users and honor privacy requests
description: Upsert tracked users with attributes, look them up, and purge visitor data to satisfy GDPR/CCPA deletion requests.
api: openapi/sprig-api-openapi-original.json
operations: [post-user-v2, get-v2-users-:userId, post-v2-purge-visitors]
---

# Manage Sprig users and honor privacy requests

## Auth
- Send `Authorization: API-Key YOUR_API_KEY` (v2 endpoints use the API-Key form).
- HTTPS only. Base URL: `https://api.sprig.com`.

## Steps
1. **Upsert a user** — `POST /v2/users` (`post-user-v2`) with the `userId` and attributes. This is an upsert keyed on `userId`, so repeated calls converge on the same record (returns `202 Accepted`). A `422` means the payload failed validation.
2. **Retrieve a user** — `GET /v2/users/{userId}` (`get-v2-users-:userId`) to read the current attributes. `404` if unknown.
3. **Purge on request** — `POST /v2/purge/visitors` (`post-v2-purge-visitors`) to delete visitor data for a GDPR/CCPA erasure request.

## Notes
- Because the user endpoint is an upsert, no idempotency key is needed — see `conventions/sprig-conventions.yml`.
- Segment tracked (identified) visitors from unregistered visitors by whether they carry a `userId`.
- Errors: `403` invalid key, `404` not found, `429` rate limited. See `errors/sprig-problem-types.yml`.
