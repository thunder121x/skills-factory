---
name: sec-apis
description: |
  Integrate with Thailand SEC OpenAPI for mutual funds, fund daily data, bonds, license checks, digital assets, PVD factsheets, One Report, and reference data. Use when calling api.sec.or.th, working with SEC subscription keys, retrieving Thai capital-market datasets, or building Python clients for SEC API products.
compatibility: Requires network access and a valid SEC API Portal subscription key for each API product used.
license: MIT
homepage: https://api-portal.sec.or.th
metadata:
  author: skills-factory
  version: "1.0"
  categories: finance, thailand, api
---

# SEC APIs

Thailand SEC OpenAPI publishes machine-readable datasets from the Securities and Exchange Commission, Thailand. The APIs require product-specific subscription keys from the SEC API Portal and use `Ocp-Apim-Subscription-Key` authentication.

## Authentication

Use the SEC API Portal to subscribe to products, get keys, inspect schemas, and confirm parameter formats. Send the product key in every request:

```python
headers = {
    "Content-Type": "application/json",
    "Accept": "application/json",
    "Cache-Control": "no-cache",
    "Ocp-Apim-Subscription-Key": SEC_API_KEY,
}
```

Use separate environment variables for separate products, such as `SEC_FUND_FACTSHEET_KEY`, `SEC_FUND_DAILY_INFO_KEY`, and `SEC_COMMON_KEY`. Never hard-code keys, commit `.env` files, or print subscription keys in logs.

## Rate Limits

SEC APIs are rate limited. Use a practical ceiling of about 5 calls per second unless the API Portal specifies otherwise. Add throttling around loops, especially when iterating all AMCs or all funds.

```python
import time
import requests

def sec_get(url: str, api_key: str):
    headers = {
        "Accept": "application/json",
        "Ocp-Apim-Subscription-Key": api_key,
    }
    response = requests.get(url, headers=headers, timeout=30)
    response.raise_for_status()
    time.sleep(0.25)
    return response.json()
```

## Common Workflows

Before writing custom code, identify the SEC product family, required subscription key, endpoint path, and identifier format (`unique_id`, `proj_id`, `period`, `trade_date`, or `issued_ref_id`).

### Mutual Fund Discovery

1. Call `GET /FundFactsheet/fund/amc` to list AMCs and get `unique_id`.
2. For each `unique_id`, call `GET /FundFactsheet/fund/amc/{unique_id}`.
3. Normalize the results with `pandas.DataFrame` and filter by fields like `fund_status`, `regis_date`, `proj_id`, or `proj_abbr_name`.

Build `amc = DataFrame(GET /fund/amc)`, loop through `amc["unique_id"]`, then concatenate each `GET /fund/amc/{unique_id}` response.

### Fund Details

Use `POST /FundFactsheet/fund` with `{"name": "..."}` when the input is a fund abbreviation or partial name rather than an AMC `unique_id`.
Use the fund `proj_id` for factsheet detail endpoints such as `URLs`, `policy`, `asset`, `performance`, `fee`, `FundPort/{period}`, `FundFullPort/{period}`, and `FundTop5/{period}`.

### Get Daily NAV

Use `GET /FundDailyInfo/{proj_id}/dailynav/{nav_date}`. Confirm the expected `nav_date` format from the API Portal for the subscribed product before implementing production code.

## Product Areas

See [API catalog](references/api-catalog.md) for endpoint families and common use cases. Product families covered: Fund Factsheet, Fund Daily Info, Common references, License Check, Bond, Digital Asset, PVD Factsheet, and One Report references.

## Best Practices

- Use `response.raise_for_status()` and log the URL/status for non-200 responses.
- Use `pd.concat`, not deprecated `DataFrame.append`, when accumulating records.
- Cache reference data and AMC/fund lists when possible to reduce API calls.
- Discover in the API Portal first; endpoint schemas and date formats can differ by product.
- Treat subscription keys as secrets and keep them in environment variables or a secret manager.
- Add retry/backoff for transient `429` and `5xx` responses, but do not bypass rate limits.
- Validate required identifiers before calling detail endpoints to avoid noisy API failures.
- Persist raw responses for auditability when building financial data pipelines.
