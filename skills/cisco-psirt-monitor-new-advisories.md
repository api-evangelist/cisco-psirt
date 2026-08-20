---
name: Incrementally sync new and updated Cisco advisories
description: >-
  Poll the Cisco PSIRT openVuln API for advisories first published or last updated
  since your previous run, page through them within the API's hard ceilings, and
  keep a local cache. This is the only programmatic freshness mechanism this API
  offers — there are no webhooks.
api: openapi/cisco-psirt-current-endpoints-api-openapi.yml
base_url: https://apix.cisco.com/security/advisories/v2
operations:
  - 'GET /all/lastpublished'
  - 'GET /all/firstpublished'
  - 'GET /latest/{number}'
---

# Incrementally sync new and updated Cisco advisories

> **There is no event surface.** No webhooks, no AsyncAPI, no streaming. Cisco's
> only push channel is a human mailing list (`openvuln-announce-join@cisco.com`).
> Polling a date range is the mechanism.

## Steps

1. **Authenticate.** Client-credentials exchange at
   `https://id.cisco.com/oauth2/default/v1/token`; tokens last 3600 seconds. Mint a
   fresh one per run rather than caching across runs.

2. **Choose the window.** Use the last-updated feed, not the first-published feed,
   if you care about revisions to existing advisories — Cisco re-versions advisories
   (`version` and `status` fields) after publication.

   `GET /all/lastpublished` with `startDate` and `endDate`, **both required**, both
   `YYYY-MM-DD`. Use `GET /all/firstpublished` for genuinely new advisories only.

   ```
   GET https://apix.cisco.com/security/advisories/v2/all/lastpublished?startDate=2026-08-01&endDate=2026-08-19&pageIndex=1&pageSize=100
   ```

3. **Page until empty.** Increment `pageIndex` until you receive `404` /
   `NO_DATA_FOUND`. **Nothing in the response tells you how many pages exist** —
   there is no total count, no page count, no `Link` header. The empty-set 404 is
   your terminator, and it is a normal answer, not an error.

4. **Respect the ceiling.** `pageIndex` maxes at 100 and `pageSize` maxes at 100, so
   a single query can reach at most 10,000 records. If a window would exceed that,
   split it into narrower date ranges rather than raising the page size.

5. **Diff against your cache.** Key on `advisoryId`; treat a changed `lastUpdated`
   or `version` as an update. Store `csafUrl` so you can fetch the full document
   lazily.

6. **Backfill or spot-check with `GET /latest/{number}`** (proposed id
   `getLatestByNumber`), `number` between 1 and 100 — useful for a cheap
   "anything new?" probe between full windows.

## Rules

- **Pace yourself.** 5 requests/second, 30/minute, 5000/day per application. A
  30-per-minute ceiling means a 100-page walk takes at least 3.3 minutes. Plan the
  window size around the quota, not the other way round.
- **No runtime quota signal exists.** No `RateLimit-*` headers, no documented 429,
  no `Retry-After`. Back off on any unexpected status.
- **All dates are UTC.** `firstPublished` and `lastUpdated` are ISO 8601 without an
  offset suffix.
- **Date errors are not retryable:** `INVALID_DATE_FORMAT`, `START_DATE_GREATER`,
  `START_DATE_AND_END_DATE_MANDATORY`. Fix the request.
- **For a whole-corpus load, do not use this API.** Take the bulk CSAF distribution
  at `https://www.cisco.com/.well-known/csaf/` instead — same content, no quota.
  Use the API for the incremental delta.
