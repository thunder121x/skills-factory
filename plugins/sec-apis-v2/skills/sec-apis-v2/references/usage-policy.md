# SEC API Usage Policy

Use this policy when calling any documented SEC Open API path.

## How to Start

1. Choose the product and data question first.
2. Look up the documented path, method, required subscription product, parameters, and response example.
3. Store the product key in an environment variable or secret manager.
4. Call the exact documented path under `https://api.sec.or.th`.
5. Handle the actual response shape: cursor object, JSON array, or JSON object.
6. Persist raw responses before transformation for auditability.

## Request Rules

- Preserve API version and path casing exactly.
- Use `GET` for query/path reads and `POST` for body-based searches.
- Send `Ocp-Apim-Subscription-Key` in headers.
- Send `Accept: application/json`; send `Content-Type: application/json` when a body is present.
- URL-encode path parameters, especially names and identifiers with spaces or non-ASCII text.
- Keep secrets out of URLs, logs, notebooks, and committed files.

## Pagination Rules

- Use `page_size` between `1` and `100` where supported.
- If the response contains `next_cursor`, pass it as the next request's `next_cursor` query parameter.
- Stop when `next_cursor` is empty or missing.
- Do not use legacy `current_page`, `total_pages`, or `total_items` for current v2 Fund/Bond endpoints.

## Rate and Retry Rules

- Current rate policy is up to `5,000` calls per `300` seconds.
- Add at least `16 ms` delay if requests complete faster than that.
- On HTTP `421`, respect the `Retry-After` header before retrying.
- Retry transient `429`, `500`, `502`, `503`, and `504` with bounded backoff.
- Do not retry `400`, `401`, `403`, or `404` without changing credentials, subscription, path, or parameters.

## Data Rules

- Cache low-churn master data such as AMCs, issuers, company lists, fund profiles, mappings, and reference meanings.
- For Fund factsheet latest-only use cases, send `latest=true` and omit historical date-range assumptions.
- For historical Fund factsheet use cases, use `start_date` and `end_date` only when `latest` is not true.
- Validate identifiers before detail calls: `unique_id`, `proj_id`, `fund_class_name`, `company_id`, `case_id`, `report_year`, `period`, and `nav_date`.
- Normalize nested fields only after storing raw JSON because schemas differ by product.
