---
name: sec-apis-v2
description: |
  Integrate with Thailand SEC Open APIs using documented v1/v2 paths, cursor pagination, latest-period factsheet queries, product-level subscription keys, and migrated Common API mappings. Use when calling api.sec.or.th for SEC Open Data products, choosing endpoint paths, migrating legacy Fund/Bond/Common APIs, handling SEC rate limits, or retrieving Thai capital-market datasets.
compatibility: Requires network access and a valid SEC API Portal subscription key for each API product used.
license: MIT
homepage: https://secopendata.sec.or.th
metadata:
  author: skills-factory
  version: "1.0"
  categories: finance, thailand, api
---

# SEC Open APIs

Thailand SEC Open APIs publish capital-market datasets through documented `/v2/*` and `/v1/*` paths. Treat SEC Open Data developer docs as the source of truth for product subscriptions, schemas, path/query parameters, date formats, and response fields.

## Start Here

1. Identify the product and exact documented path, method, parameters, response shape, and subscription key.
2. Use cursor pagination for paged endpoints; use `latest=true` for supported Fund factsheet endpoints.

## API Model

Base URL: `https://api.sec.or.th`. Documented prefixes include `/v2/bond/*`, `/v2/fund/*`, `/v1/digital-asset/*`, `/v1/license-check/*`, `/v1/one-report/*`, and `/v1/pvd/*`.

Common API is deprecated in v2; reference-code meanings moved into related product APIs. Published data updates twice daily at about `09:30` and `17:30`.

## Path Client

Use a path-first client so any documented operation can be called without adding a function per endpoint:

```python
import time
import requests

BASE_URL = "https://api.sec.or.th"

def sec_request(method: str, path: str, api_key: str, *, json=None, params=None):
    url = f"{BASE_URL}/{path.lstrip('/')}"
    headers = {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Cache-Control": "no-cache",
        "Ocp-Apim-Subscription-Key": api_key,
    }
    response = requests.request(method, url, headers=headers, json=json, params=params, timeout=60)
    if response.status_code == 421:
        time.sleep(float(response.headers.get("Retry-After", "1")))
        response = requests.request(method, url, headers=headers, json=json, params=params, timeout=60)
    elif response.status_code in {429, 500, 502, 503, 504}:
        time.sleep(2.0)
        response = requests.request(method, url, headers=headers, json=json, params=params, timeout=30)
    response.raise_for_status()
    time.sleep(0.016)
    return response.json()
```

Preserve documented path casing exactly. Use `GET` for path/query reads and `POST` for body-based searches.

## Pagination

V2 responses use `{"message": "success", "page_size": 100, "next_cursor": "...", "items": []}`. `page_size` is `1` to `100`; `next_cursor == ""` means the final page.

```python
def sec_pages(path: str, api_key: str, **params):
    params = {"page_size": 100, **params}
    while True:
        data = sec_request("GET", path, api_key, params=params)
        yield from data.get("items", [])
        if not data.get("next_cursor"):
            break
        params["next_cursor"] = data["next_cursor"]
```

Some `/v1` endpoints return a JSON array directly or a non-paginated object. Inspect the response shape before assuming `items` exists.

## Fund Factsheet Latest

Fund factsheet endpoints 7-17 support boolean `latest`. When `latest=true`, the API returns the latest effective factsheet data and ignores `start_date` and `end_date`. Affected paths include `/v2/fund/factsheet/ipos`, `benchmarks`, `subscription-redemption-minimums`, `subscription-redemption-periods`, `risk-spectrum`, `statistics`, `dividend-policy`, `fees`, `performance`, `asset-allocation`, and `top5-holdings`.

## Rate Limits and Secrets

Current limit: up to `5,000` calls per `300` seconds across each API product policy. If calls are faster than `16 ms`, delay at least `16 ms`. On HTTP `421`, read `Retry-After` and retry after that delay.

Store keys in environment variables such as `SEC_FUND_KEY`, `SEC_BOND_KEY`, and `SEC_LICENSE_CHECK_KEY`. Never hard-code, commit, log, or place keys in URLs.

## Common Workflows
Fund: start with `/v2/fund/general-info/amcs`, use `unique_id` as `company_info` in `/v2/fund/general-info/profiles`, then use `proj_id` and `fund_class_name` for factsheet, outstanding, NAV, and dividend endpoints.

Bond: start with `/v2/bond/issuers`, then use `/v2/bond/features`, `credit-ratings`, `outstanding-values`, `involve-parties`, and `investor-holdings`. License Check, One Report, PVD, and Digital Asset currently use documented `/v1` paths; do not invent `/v2` paths unless the docs show them.

## Product Coverage

See [endpoint catalog](references/endpoint-catalog.md) for documented paths, [usage policy](references/usage-policy.md) for any-path request rules, and [migration notes](references/migration-notes.md) for legacy-to-current changes.

## Best Practices

- Keep one low-level path client and add thin helpers only for repeated workflows.
- Preserve exact documented path casing and API version.
- Validate required identifiers and date formats before calling detail endpoints.
- Cache low-churn data such as AMCs, issuers, profiles, mappings, and reference meanings.
- Persist raw JSON before transformation in financial data pipelines.
