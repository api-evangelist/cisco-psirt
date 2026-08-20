---
name: Triage Cisco advisories by severity
description: >-
  Pull Cisco security advisories at a given Security Impact Rating, optionally
  bounded by a publication date range, and rank them for remediation using CVSS,
  affected products and CWE.
api: openapi/cisco-psirt-current-endpoints-api-openapi.yml
base_url: https://apix.cisco.com/security/advisories/v2
operations:
  - 'GET /severity/{severity}'
  - 'GET /severity/{severity}/firstpublished'
  - 'GET /severity/{severity}/lastpublished'
  - 'GET /product'
---

# Triage Cisco advisories by severity

## Steps

1. **Authenticate** (client credentials → Bearer JWT).

2. **Pull the severity band.** `GET /severity/{severity}` (proposed id
   `getSeverityBySeverity`). The path enum is
   `critical | high | medium | low | informational`. Note that Cisco's own
   `INVALID_SEVERITY` message lists `informational` while some documentation pages
   omit it — the spec enum is the authority.

   ```
   GET https://apix.cisco.com/security/advisories/v2/severity/critical?summaryDetails=true&productNames=true&pageIndex=1&pageSize=100
   ```

3. **Bound it by time for a working queue.** `GET /severity/{severity}/firstpublished`
   or `/lastpublished`, with `startDate` and `endDate` (both required, `YYYY-MM-DD`).
   Use `lastpublished` if you want advisories that were *revised* in the window.

4. **Page to exhaustion.** `pageIndex` and `pageSize` are each capped at 100 and
   nothing reports the total; page until `404` / `NO_DATA_FOUND`, which is the
   normal empty-set terminator.

5. **Rank.** Sort on `cvssBaseScore` (number) within `sir` band, then weight by
   whether `productNames` intersects your estate. `cwe` gives you the weakness
   class for pattern-level remediation.

6. **Cross-check your inventory.** For each candidate advisory, confirm exposure
   against your actual running releases using the software-release skill
   (`GET /OSType/{OSType}`, `GET /OS_version/OS_data`) — a Critical advisory that
   does not touch your release is not your Critical.

7. **Optionally pivot by product.** `GET /product` (proposed id `getProduct`) takes
   a required `product` query parameter. Special characters
   (`& ! $ ' @ # < ?`) are rejected with `INVALID_PRODUCT_NAME_FORMAT`; wildcards
   like `cisco-xe*` are accepted.

## Rules

- **`sir` is Cisco's rating, `cvssBaseScore` is the industry one.** They can
  disagree. Report both; do not silently substitute one for the other.
- **Ask for what you need up front.** `summaryDetails` and `productNames` default
  to off. Omitting them and then re-fetching per advisory will burn the 5000/day
  quota fast.
- **Cache aggressively.** Advisory content is stable between `lastUpdated` changes,
  and Cisco explicitly asks integrators to cache rather than re-poll.
- **Empty is not an error.** `404` / `NO_DATA_FOUND` on a severity+date query means
  nothing was published in that band and window.
