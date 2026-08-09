---
name: Create a candidate profile and advance their application stage
description: Add a candidate to Eightfold and move them through an application stage for a position.
api: https://apiv2.eightfold.ai/api/v2/
operations: [create_profile, create_profile_stages, get_async_api_status_transactions_by_id]
generated: '2026-07-19'
method: generated
source: https://apidocs.eightfold.ai/reference
---

# Create a candidate profile and advance their application stage

## Auth

Obtain a bearer token as described in `authentication/eightfold-authentication.yml`. The key
needs `profile:WRITE` (and the relevant application scope).

## Steps

1. **Create the profile.** `POST` via `create_profile` with the candidate data in the
   payload. The response returns the new profile id.
2. **Upsert the application stage.** Call `create_profile_stages` (Upsert Application Stage)
   with the profile id and target position to create or advance the candidate's application
   stage.
3. **Confirm async writes.** If the write is asynchronous, the response returns a transaction
   id. Poll `get_async_api_status_transactions_by_id` with that id until status is `PASS`
   (or handle `FAIL`). A 2xx alone does not guarantee the write committed — transaction
   status is authoritative for up to 3 days.

## Conventions

- `PATCH` updates only fields present in the body; `PUT` (Update) overwrites and wipes
  omitted fields — prefer PATCH for partial edits.
- Indexing can lag: a newly created entity may not appear in `list` immediately but is
  fetchable by id.
- See `conventions/eightfold-conventions.yml` for async and upsert details.
