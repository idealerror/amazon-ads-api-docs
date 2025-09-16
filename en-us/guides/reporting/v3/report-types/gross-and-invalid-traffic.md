---
title: Gross and invalid traffic reports
description: Learn about requesting gross and invalid traffic reports using the Amazon Ads API.
interface: api
type: guide
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Gross and invalid traffic reports

Gross and invalid traffic report provides Sponsored Products, Sponsored Brands and Sponsored Display advertisers transparency into the nature of traffic on their campaigns. This report include all campaigns of the requested ad type and provides transparency on gross and invalid traffic metrics at campaign level for the requested days. For example, a Sponsored Products gross and invalid traffic report returns gross and invalid traffic metrics for all Sponsored Products campaigns that received impressions on the chosen dates.

## Configurations

| Configuration | Sponsored Products | Sponsored Brands | Sponsored Display | 
|----------|---------|------|------|
| `reportTypeId` | `spGrossAndInvalids` | `sbGrossAndInvalids` | `sdGrossAndInvalids`| 
| Maximum date range | 365 days | 365 days | 365 days |
| Data retention | 365 days | 365 days | 365 days |
| `timeUnit` | `SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` |`SUMMARY` or `DAILY` |
| `groupBy` | `campaign` |`campaign` | `campaign` |
| `format` | `GZIP_JSON` or `CSV` |`GZIP_JSON` or `CSV` |`GZIP_JSON` or `CSV` |

Sponsored Products, Sponsored Brands, and Sponosred Display all support the same columns and configurations for the gross and invalid traffic report.

### Base metrics

[campaignName](guides/reporting/v3/columns#campaignName), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [clicks](guides/reporting/v3/columns#clicks), [date](guides/reporting/v3/columns#date), [endDate](guides/reporting/v3/columns#endDate), [grossClickThroughs](guides/reporting/v3/columns#grossClickThroughs), [grossImpressions](guides/reporting/v3/columns#grossImpressions), [impressions](guides/reporting/v3/columns#impressions), [invalidClickThroughRate](guides/reporting/v3/columns#invalidClickThroughRate), [invalidClickThroughs](guides/reporting/v3/columns#invalidClickThroughs), [invalidImpressionRate](guides/reporting/v3/columns#invalidImpressionRate), [invalidImpressions](guides/reporting/v3/columns#invalidImpressions), [startDate](guides/reporting/v3/columns#startDate)

### Group by `campaign`

Additional metrics: N/A

Filters: 

- `campaignStatus` (values: ENABLED, PAUSED, ARCHIVED)


## Sample call

```bash
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--data '{
    "name": "SP Gross and Invalid Traffic",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_PRODUCTS",
        "groupBy": [
            "campaign"
        ],
        "columns": [
            "campaignName",
            "grossImpressions",
            "grossClickThroughs",
            "invalidClickThroughs",
            "invalidClickThroughRate",
            "startDate",
            "endDate"
        ],
        "reportTypeId": "spGrossAndInvalids",
        "timeUnit": "SUMMARY",
        "format": "CSV"
    }
}'
```

