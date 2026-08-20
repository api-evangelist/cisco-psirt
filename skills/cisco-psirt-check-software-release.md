---
name: Check whether a specific Cisco software release is affected
description: >-
  Use the Cisco Software Checker integration in the openVuln API to find the
  advisories that affect one exact software release of ASA, FMC, FTD, FXOS, IOS,
  IOS XE, NX-OS or NX-OS in ACI mode.
api: openapi/cisco-psirt-current-endpoints-api-openapi.yml
base_url: https://apix.cisco.com/security/advisories/v2
operations:
  - 'GET /OSType/{OSType}'
  - 'GET /OS_version/OS_data'
  - 'GET /platforms'
---

# Check whether a specific Cisco software release is affected

This is the highest-value question the openVuln API answers, and the one most
integrations miss: not "what advisories exist" but "does *my* running release have
a problem".

## Steps

1. **Authenticate** (client credentials → Bearer JWT; see the CVE-exposure skill).

2. **Confirm the OS type is supported.** `GET /OSType/{OSType}` (proposed id
   `getOSTypeByOSType`). The spec's enum is exact and closed:
   `aci`, `ios`, `iosxe`, `nxos`, `asa`, `ftd`, `fmc`, `fxos`. Anything else
   returns `406` / `INVALID_OS_TYPE`.

3. **Resolve valid versions if you are unsure.** `GET /OS_version/OS_data`
   (proposed id `getOSVersionOSData`) returns `nos_type`, `nos_version` and
   `platform_name`. Cisco's own error text points here: *"Use endpoint
   /OS_version/OS_data to retrieve valid OS versions."* `version` is a **required**
   query parameter on the version lookups — omitting it returns
   `VERSION_ID_IS_MANDATORY`.

4. **Narrow by platform where the OS supports it.** `GET /platforms` (proposed id
   `getPlatforms`) returns platform aliases. Only four operating systems accept a
   `platformAlias`: `asa`, `ftd`, `fxos`, `nxos`. Aliases are a closed set —
   `ASA5500`, `ASA5500X`, `ASASM`, `ASAV`, `FPR1000`, `FPR2100`, `FPR4100`,
   `FPR9000`, `FPRNGFW`, `FWL3100`, `ISA3000`, `MDS9000`, `NEXUS1000V`,
   `NEXUS3000`, `NEXUS5000`, `NEXUS6000`, `NEXUS7000`, `NEXUS9000`.

5. **Read the result.** A match returns advisory records; no match returns the
   `AdvisoriesErrors` shape with `NO_IOS_AFFECTING_ADVISORIES_FOUND` (nothing
   affects that release — good news) or `INVALID_IOS_VERSION` (your version string
   is wrong — fix it and re-ask). **These two mean opposite things. Do not collapse
   them into one "no result".**

6. **Escalate to CSAF** for the exact affected version ranges and fixed-release
   guidance, via each advisory's `csafUrl`.

## Rules

- **Version strings are literal and fussy.** Cisco's own CLI documents escaping
  brackets (`15.6\(2\)SP`). Send the release string exactly as the device reports it.
- **End-of-Support releases disappear.** Cisco states it "reserves the right to
  remove End-of-Support releases from the Cisco Software Checker (subsequently
  reflected in this API)". An unknown version may mean unsupported, not invalid.
- **Do not use the sunset endpoints.** `/ios`, `/iosxe`, `/aci`, `/nxos` and
  `/nos_version/nos_data` only work against the legacy `security/advisories`
  basepath and are tagged "Sunset Endpoints" by Cisco. Use `/OSType/{OSType}` and
  `/OS_version/OS_data` on `security/advisories/v2`.
- **Never call `/cvrf/*`.** All 10 are `deprecated: true` and return `410 Gone`
  with `URI_CONTAINING(/CVRF/)_IS_NO_LONGER_AVAILABLE`.
