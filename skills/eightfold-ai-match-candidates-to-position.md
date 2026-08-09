---
name: Match candidates to an open position
description: Find and review the best-matching candidates for an Eightfold position.
api: https://apiv2.eightfold.ai/api/v2/
operations: [list_position, get_position_by_id, list_profile_matched_candidates, get_profile_by_id]
generated: '2026-07-19'
method: generated
source: https://apidocs.eightfold.ai/reference
---

# Match candidates to an open position

Use the Eightfold Talent Intelligence API v2 to surface the strongest candidates for a
position and inspect their profiles.

## Auth

1. Generate an API key in the Admin Console (`Integration > Eightfold API > Authentication`).
2. Mint a bearer token: `POST https://apiv2.eightfold.ai/oauth/v1/authenticate` with
   Basic-auth region credentials and body `{ "grantType": "password", "username": "<email>", "password": "<api key>" }`.
3. Send `Authorization: Bearer <access_token>` on every call. The key needs `position:READ`
   and `profile:READ` permission scopes.

## Steps

1. **Find the position.** Call `list_position` with search query params to locate the
   position, or `get_position_by_id` if you already hold the position id.
2. **List matched candidates.** Call `list_profile_matched_candidates` for the position id.
   It returns candidates ranked by match score; pass filter query params to narrow, and the
   `leads` parameter to request up to the top 1000 leads. Note: results can differ from the
   UI, which may apply additional configured settings.
3. **Inspect a candidate.** For any returned profile id, call `get_profile_by_id` to pull
   the full profile object.

## Conventions

- List/search endpoints combine filters as AND across different filters, OR within one filter.
- Respect per-key READ/WRITE scopes; a missing scope yields access denied.
- See `conventions/eightfold-conventions.yml` for pagination and async semantics.
