# SEC API Catalog

Confirm exact schemas, parameter formats, and subscription product names in the SEC API Portal before production use.

## Base URL and Auth

- Base URL: `https://api.sec.or.th`
- Header: `Ocp-Apim-Subscription-Key: <product subscription key>`
- Common JSON headers: `Accept: application/json`, `Content-Type: application/json`

## Fund Factsheet

Base path: `/FundFactsheet`

| Endpoint pattern | Use |
| --- | --- |
| `GET /fund/amc` | List AMCs |
| `GET /fund/amc/{unique_id}` | Funds under an AMC |
| `POST /fund` | Search funds by abbreviation or partial name |
| `GET /fund/{proj_id}/URLs` | Factsheet and annual report URLs |
| `GET /fund/{proj_id}/IPO` | Offering data |
| `GET /fund/{proj_id}/investment` | Investment unit values and limits |
| `GET /fund/{proj_id}/project_type` | Project type and duration |
| `GET /fund/{proj_id}/policy` | Fund policy classification |
| `GET /fund/{proj_id}/specification` | Special fund characteristics |
| `GET /fund/{proj_id}/feeder_fund` | Feeder fund data |
| `GET /fund/{proj_id}/redemption` | Sale and redemption period |
| `GET /fund/{proj_id}/suitability` | Investor suitability and fund risk |
| `GET /fund/{proj_id}/risk` | Key risk factors |
| `GET /fund/{proj_id}/asset` | Asset allocation |
| `GET /fund/{proj_id}/turnover_ratio` | Portfolio turnover ratio |
| `GET /fund/{proj_id}/return` | Estimated return and investment period |
| `GET /fund/{proj_id}/buy_and_hold` | Buy-and-hold portfolio estimate |
| `GET /fund/{proj_id}/benchmark` | Benchmarks |
| `GET /fund/{proj_id}/fund_compare` | Peer comparison category |
| `GET /fund/{proj_id}/class_fund` | Fund unit classes |
| `POST /fund/class_fund` | Search class fund by abbreviation or name |
| `GET /fund/{proj_id}/performance` | Historical performance |
| `GET /fund/{proj_id}/5YearLost` | Five-year max loss and volatility |
| `GET /fund/{proj_id}/dividend` | Dividend policy by class |
| `GET /fund/{proj_id}/fee` | Fund fees |
| `GET /fund/{proj_id}/InvolveParty` | Related parties |
| `GET /fund/{proj_id}/FundPort/{period}` | Quarter-end fund portfolio |
| `GET /fund/{proj_id}/FundFullPort/{period}` | Portfolio allocation |
| `GET /fund/{proj_id}/FundTop5/{period}` | Top five holdings |
| `GET /fund/{proj_id}/FundHist` | History of name, policy, foreign investment, project type |
| `GET /fund/{proj_id}/FundTrackingError` | One-year tracking error |

## Fund Daily Info

Base path: `/FundDailyInfo`

| Endpoint pattern | Use |
| --- | --- |
| `GET /{proj_id}/dailynav/{nav_date}` | Daily NAV |
| `GET /{proj_id}/dividend` | Dividend history |
| `GET /amc` | AMCs that submit daily data |

## Common Reference Data

Base path: `/common/ref`

| Endpoint pattern | Use |
| --- | --- |
| `/license_type/company` | Company approval license types |
| `/business_act/company` | Approved company business activities |
| `/license_type/person` | Person approval license types |
| `/role/person` | Approved person roles |
| `/fund/portfolio/asset_type` | Fund portfolio asset types |
| `/product/secu_type` | Securities product types |
| `/product/offering_type` | Product offering types |
| `/product/currency_code` | Currencies |
| `/product/debenture/coupon_code` | Debenture coupon types |
| `/product/debenture/redemption_code` | Debenture redemption types |
| `/product/debenture/embedded_code` | Debenture embedded option types |
| `/product/debenture/secured_code` | Debenture secured types |
| `/investoralert/action_type` | Investor alert action types |
| `/bond/function_type` | Bond related-party function types |
| `/bond/corporation_type` | Bond corporation types |
| `/digitalasset/customer_type` | Digital asset customer types |
| `/digitalasset/asset_type` | Digital asset types |
| `/pvd/policy_code` | PVD policy codes |
| `/onereport/financial_statement` | Financial statements and ratios |
| `/onereport/social_performance_code` | Social performance codes |
| `/onereport/risk_code` | Risk codes |
| `/onereport/export_code` | Foreign revenue codes |
| `/onereport/environment_code` | Environment codes |

## License Check

Use for approved persons, licensed companies, enforcement records, and investor alerts. Common operations include person search, company search, person licenses, company personnel, person work info, company licenses, company business activities, enforcement, investor alert details, and investor alert actions.

## Bond

Use for outstanding issuer, issue, offer type, coupon, issue age, offering unit, issue rating, redemption, involved party, investor type, sector type, and outstanding value data. Most detail calls use `issued_ref_id`; outstanding value also uses `outstanding_date`.

## Digital Asset

Use for intermediary profiles, monthly customer trading, monthly asset trading, monthly active accounts, weekly asset trading, daily OHLCV trade summaries, daily investor-type summaries, and daily deposit/withdraw/transfer summaries. Date-driven endpoints typically use `trade_date`.

## PVD Factsheet

Use for PVD AMC list, funds under AMC, policy, returns, fees, and portfolio allocation. Inputs commonly include `unique_id`, `proj_id`, and `period`.

## One Report

One Report reference categories are available through Common API. Use the API Portal for exact One Report endpoint paths and payload schemas before implementation.
