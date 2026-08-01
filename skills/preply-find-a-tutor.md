---
name: Find a Preply tutor
description: Search the Preply marketplace for language tutors matching a learner's subject, budget and availability, then fetch full details for the best match.
api: openapi/preply-openapi-original.json
operations:
- searching-tutors
- tutor-by-id
---

# Find a Preply tutor

Use the public Preply plugin API (`https://preply.com/api/chatgpt_openapi/`) to
find language tutors. The API is **unauthenticated** and **read-only** — no API
key or OAuth token is required.

## Steps

1. **Search for tutors** — call `searching-tutors`
   (`GET /api/chatgpt_openapi/tutors/`). The only required parameter is
   `subject` (e.g. `english`, `spanish`, `ukrainian`). Narrow with optional
   filters: `priceRange` (`"10-20"`), `tl` (spoken-language code), `tutorIsFrom`
   (country codes), `native` (`true`/`false`), `certified`, `day`, `time`,
   `searchtext`. Set `sort=rating` to surface the best-rated tutors first.
2. **Page through results** — results are fixed at **10 per page**. Read
   `data.allTutors.totalResults` for the total, and pass `page=2`, `page=3`, …
   to retrieve more. If zero tutors are returned, relax or remove filters.
3. **Fetch full details** — for a chosen tutor, call `tutor-by-id`
   (`GET /api/chatgpt_openapi/tutors/{id}`) with the numeric tutor `id` from the
   search result to get the full profile.

## Conventions & error handling

- **Errors** are not HTTP problem codes — every 200 response carries an `errors`
  array of human-readable strings alongside `data`. Always check `errors` before
  trusting `data`.
- **Price** is nested at `price.value` with `price.currency.code`.
- **Profile link** for a tutor is `publicUrl`; rating is `averageScore`.
- No idempotency key is needed — all operations are safe GETs.
