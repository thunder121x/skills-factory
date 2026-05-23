# SEC Open API Endpoint Catalog

Confirm exact parameters, schemas, and subscription product names in SEC Open Data before production use.

## Product Prefixes

| Product | Prefix | Notes |
| --- | --- | --- |
| Bond | `/v2/bond/*` | Debt instrument issuers, features, ratings, outstanding values, related parties, holdings |
| Fund | `/v2/fund/*` | Combines legacy Fund Factsheet and Fund Daily Info into one product |
| Digital Asset | `/v1/digital-asset/*` | Digital asset intermediaries and trading data |
| License Check | `/v1/license-check/*` | Approved persons, licensed companies, investor alerts, audit firms |
| One Report | `/v1/one-report/*` | 56-1 One Report company, financial, ESG, executive, and risk data |
| PVD | `/v1/pvd/*` | Provident fund profile, policy, returns, fees, portfolio, NAV data |

## Bond v2

| Path | Use |
| --- | --- |
| `GET /v2/bond/issuers` | Debt instrument issuers |
| `GET /v2/bond/features` | General bond features; search by `thaibma_symbol`, `isin_code`, or `company_id` |
| `GET /v2/bond/credit-ratings` | Credit ratings |
| `GET /v2/bond/outstanding-values` | Outstanding values |
| `GET /v2/bond/involve-parties` | Related parties by period |
| `GET /v2/bond/investor-holdings` | Investor holdings |

## Fund v2

| Path | Use |
| --- | --- |
| `GET /v2/fund/general-info/amcs` | Asset management companies |
| `GET /v2/fund/general-info/profiles` | Fund profiles; supports `company_info` for AMC filtering |
| `GET /v2/fund/general-info/specifications` | Fund specification data |
| `GET /v2/fund/general-info/mutual-fund-fees` | Mutual fund fees |
| `GET /v2/fund/general-info/involve-parties` | Fund related parties |
| `GET /v2/fund/factsheet/urls` | Factsheet URLs |
| `GET /v2/fund/factsheet/ipos` | IPO and offering data; supports `latest` |
| `GET /v2/fund/factsheet/benchmarks` | Benchmarks; supports `latest` |
| `GET /v2/fund/factsheet/subscription-redemption-minimums` | Subscription/redemption minimums; supports `latest` |
| `GET /v2/fund/factsheet/subscription-redemption-periods` | Subscription/redemption periods; supports `latest` |
| `GET /v2/fund/factsheet/risk-spectrum` | Risk level; supports `latest` |
| `GET /v2/fund/factsheet/statistics` | Statistics; supports `latest` |
| `GET /v2/fund/factsheet/dividend-policy` | Dividend policy; supports `latest` |
| `GET /v2/fund/factsheet/fees` | Factsheet fees; supports `latest` |
| `GET /v2/fund/factsheet/performance` | Historical performance; supports `latest` |
| `GET /v2/fund/factsheet/asset-allocation` | Asset allocation; supports `latest` |
| `GET /v2/fund/factsheet/top5-holdings` | Top five holdings; supports `latest` |
| `GET /v2/fund/outstanding/portfolio` | Portfolio data |
| `GET /v2/fund/outstanding/portfolio-asset-type` | Portfolio asset type data |
| `GET /v2/fund/daily-info/nav` | Daily NAV |
| `GET /v2/fund/daily-info/dividend-history` | Dividend history |

## Digital Asset v1

| Path | Use |
| --- | --- |
| `GET /v1/digital-asset/profile/intermediary` | Digital asset intermediary profile |

## License Check v1

| Path | Use |
| --- | --- |
| `POST /v1/license-check/licensee/person` | Search approved persons by name or registration number |
| `GET /v1/license-check/licensee/company` | List licensed or registered companies |
| `POST /v1/license-check/licensee/company` | Search licensed or registered companies by name |
| `GET /v1/license-check/licensee/person/{unique_id}/license` | Person approval/license types |
| `GET /v1/license-check/licensee/company/{unique_id}/personnel` | Personnel under a company |
| `GET /v1/license-check/licensee/person/{unique_id}/work_info` | Person work information across companies |
| `GET /v1/license-check/licensee/company/{unique_id}/license` | Company license or registration types |
| `GET /v1/license-check/licensee/company/{unique_id}/business_act` | Current company business activities |
| `GET /v1/license-check/licensee/{unique_id}/enforcement` | Enforcement cases for a person or company |
| `GET /v1/license-check/licensee/{unique_id}/enforcement/{case_id}` | Enforcement case details |
| `GET /v1/license-check/licensee/investoralert/alertdetail` | Investor alert details |
| `GET /v1/license-check/licensee/investoralert/{case_id}/alertaction` | Investor alert actions |
| `GET /v1/license-check/licensee/auditors` | Auditors |
| `GET /v1/license-check/licensee/auditingFirm` | Auditing firms |
| `GET /v1/license-check/licensee/auditors/search/name/{person_name}` | Search auditors by name |

## One Report v1

| Path | Use |
| --- | --- |
| `GET /v1/one-report/sbo/{report_year}/info/{language}` | Listed company general info |
| `GET /v1/one-report/sbo/{report_year}/rd/{unique_id}` | R&D data |
| `GET /v1/one-report/sbo/{report_year}/product_income/{unique_id}` | Product income data |
| `GET /v1/one-report/sbo/{report_year}/export_income/{unique_id}` | Export income data |
| `GET /v1/one-report/sbo/{report_year}/risk/{unique_id}` | Risk data |
| `GET /v1/one-report/sustainability/{report_year}/detail/{unique_id}` | Sustainability details |
| `GET /v1/one-report/sustainability/{report_year}/environment_issue/{unique_id}` | Environmental issue data |
| `GET /v1/one-report/sustainability/{report_year}/humanrights_issue/{unique_id}` | Human rights issue data |
| `GET /v1/one-report/scp/{report_year}/employee_info/{unique_id}` | Employee information |
| `GET /v1/one-report/scp/{report_year}/employee_development/{unique_id}` | Employee development |
| `GET /v1/one-report/scp/{report_year}/labor_dispute/{unique_id}` | Labor disputes |
| `GET /v1/one-report/scp/{report_year}/csr_activity/{unique_id}` | CSR activities |
| `GET /v1/one-report/cgp/{report_year}/governance/{unique_id}` | Governance data |
| `GET /v1/one-report/cgp/{report_year}/director/{unique_id}` | Director data |
| `GET /v1/one-report/cgp/{report_year}/code_of_conduct/{unique_id}` | Code of conduct data |
| `GET /v1/one-report/fs/{report_year}/financial_statement/{unique_id}` | Financial statements and ratios |
| `GET /v1/one-report/cgs/{report_year}/board/{unique_id}` | Board data |
| `GET /v1/one-report/cgs/{report_year}/employee/{unique_id}` | Employee governance data |
| `GET /v1/one-report/cgs/{report_year}/auditor_company/{unique_id}` | Auditor company data |
| `GET /v1/one-report/cgs/{report_year}/director_performance/{unique_id}` | Director performance |
| `GET /v1/one-report/cgs/{report_year}/bods/{unique_id}` | Board of directors data |
| `GET /v1/one-report/cgs/{report_year}/executives/{unique_id}` | Executive data |
| `GET /v1/one-report/cgs/{report_year}/committees/{unique_id}/others` | Other committee data |

## PVD v1

| Path | Use |
| --- | --- |
| `GET /v1/pvd/factsheet/amc` | PVD asset management companies |
| `GET /v1/pvd/factsheet/{unique_id}/fund` | PVD funds under AMC |
| `GET /v1/pvd/factsheet/{proj_id}/policy` | PVD policy data |
| `GET /v1/pvd/factsheet/{proj_id}/return` | PVD return data |
| `GET /v1/pvd/factsheet/{proj_id}/trailreturn` | PVD trailing return data |
| `GET /v1/pvd/factsheet/{proj_id}/fee` | PVD fees |
| `GET /v1/pvd/factsheet/{proj_id}/PVDFullPort/{period}` | PVD full portfolio |
| `GET /v1/pvd/factsheet/{proj_id}/statistics` | PVD statistics |
| `GET /v1/pvd/factsheet/{proj_id}/top5/assettype` | Top five asset types |
| `GET /v1/pvd/factsheet/{proj_id}/top5/securities` | Top five securities |
| `GET /v1/pvd/factsheet/{proj_id}/top5/foreign` | Top five foreign investments |
| `GET /v1/pvd/factsheet/{proj_id}/top5/industry` | Top five industries |
| `GET /v1/pvd/factsheet/{proj_id}/top5/issuer` | Top five issuers |
| `GET /v1/pvd/factsheet/{proj_id}/nav/{nav_date}` | PVD NAV by date |

## Pagination Fields

Paged v2 products use cursor-style pagination for large result sets:

| Field | Meaning |
| --- | --- |
| `message` | Status message |
| `page_size` | Number of returned items; maximum `100` |
| `next_cursor` | Cursor for the next page; empty string means final page |
| `items` | Data array for the current page |
