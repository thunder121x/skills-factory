# SEC Open API Migration Notes

## Major Changes

- Fund Factsheet API and Fund Daily Info API were merged into `Fund API` under `/v2/fund/*`.
- Bond API was restructured under `/v2/bond/*` with standardized metadata and pagination.
- Some products remain documented under `/v1`, including Digital Asset, License Check, One Report, and PVD. Use the documented versioned path instead of forcing every product to `/v2`.
- Common API was removed; reference-code data moved into related product APIs.
- Legacy pagination fields such as `current_page`, `total_pages`, and `total_items` were replaced by `next_cursor`, `page_size`, and `items`.
- Legacy generic search parameters were split into clearer product-specific parameters such as `fund_class_name`, `fund_status`, `project_info`, and `company_info`.

## Cursor Pagination

Use `next_cursor` from the previous response as the next request's query parameter. Stop when `next_cursor` is an empty string.

```python
params = {"page_size": 100}
while True:
    data = sec_request("GET", "/v2/fund/general-info/profiles", api_key, params=params)
    process(data["items"])
    if not data.get("next_cursor"):
        break
    params["next_cursor"] = data["next_cursor"]
```

## Latest Factsheet Parameter

Fund factsheet endpoints 7-17 support `latest=true`. When enabled, the API returns the latest effective factsheet period and ignores `start_date` and `end_date`.

Use this when the application needs current factsheet data only. Do not combine it with historical date-range logic.

## Rate Limit

- Current policy: `5,000` calls per `300` seconds.
- If calls are faster than `16 ms`, add at least `16 ms` delay between requests.
- On HTTP `421`, read `Retry-After` and retry only after the specified wait.
- For higher-volume usage, contact the SEC support channel described in SEC Open Data.

## Common API Migration

Common reference meanings now live in product-specific APIs:

| Deprecated Common area | New product area |
| --- | --- |
| Company and person license types | License Check |
| Fund portfolio asset types | Fund |
| Securities, offering, currency, coupon, redemption, secured, bond party types | Bond |
| Digital asset customer and asset types | Digital Asset |
| PVD policy codes | PVD |
| One Report financial, social, risk, export, and environment codes | One Report |

## Fund Workflow

Fund data has three levels:

| Level | Meaning | Key identifiers |
| --- | --- | --- |
| AMC | Asset management company | `unique_id` |
| Fund | Fund/project-level data | `proj_id` |
| Share Class | Unit-class-level data | `fund_class_name`, class abbreviations |

Start with `/v2/fund/general-info/amcs`, pass `unique_id` as `company_info` to `/v2/fund/general-info/profiles`, then loop over `proj_id` and/or `fund_class_name` for detailed endpoints. If no parameters are supplied, many Fund endpoints return all available data; use filters for selective retrieval.

## Response Shapes

Do not assume one response shape across all products:

| Shape | Products/paths | Handling |
| --- | --- | --- |
| Cursor object with `items` | Most `/v2/bond/*` and `/v2/fund/*` list endpoints | Iterate `items`, pass `next_cursor` until empty |
| JSON array | Many `/v1/license-check/*`, `/v1/pvd/*`, and similar endpoints | Treat the whole response as records |
| JSON object | Some detail endpoints | Persist object directly or normalize nested fields carefully |
