---
title: Conversion path reports
description: Learn about requesting Conversion path reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
    - Amazon DSP
---

# Conversion path reports

Conversion path reporting enables you to visualize combinations of ad products and formats that drive desired business outcomes. It displays your customers’ 30-day path to conversion, and top-converting paths across Amazon DSP, Sponsored TV, Sponsored Display, Sponsored Brands, and Sponsored Products.

Additional benefits of using conversion paths:

- Reveals the combination of ad products and formats that are most effective at moving customers through your funnel.
- Reveals cross-channel paths to conversion.
- Helps balance brand building and performance marketing.
- Demonstrates full impact of media mixes.
- Provides fast, continuous insights.
- Enables quick identification of optimization opportunities.
- Allows near real-time adjustments to Amazon Ads strategies.
- Reporting is provided at the brand level, and can encompass multiple Amazon DSP and sponsored ads accounts, whether owned by agencies or the advertiser.

>[NOTE]You must be a brand owner or representative on Amazon Stores to be eligible for conversion path reporting.

## Configurations

| Configuration | All ad products | 
|----------|-----|
| `reportTypeId` | `conversionPath` |
| Maximum date range | 90 days |
| Data retention | 95 days |
| `timeUnit` | `SUMMARY` |
| `groupBy` | `brandConversionPath` |
| `format` | `CSV` or `GZIP_JSON` |

## All ad products

### Base metrics

[startDate](guides/reporting/v3/columns#startDate),
[endDate](guides/reporting/v3/columns#endDate),
[brandName](guides/reporting/v3/columns#brandName)

### Group by `brandConversionPath`

Additional metrics: 
[simplifiedPath](guides/reporting/v3/columns#simplifiedPath),
[sales](guides/reporting/v3/columns#sales),
[purchases](guides/reporting/v3/columns#purchases),
[newToBrandSales](guides/reporting/v3/columns#newToBrandSales),
[newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases),
[sponsoredBrandsDisplayFrequency](guides/reporting/v3/columns#sponsoredBrandsDisplayFrequency),
[sponsoredDisplayDisplayFrequency](guides/reporting/v3/columns#sponsoredDisplayDisplayFrequency),
[amazonDspDisplayFrequency](guides/reporting/v3/columns#amazonDspDisplayFrequency),
[sponsoredBrandsVideoFrequency](guides/reporting/v3/columns#sponsoredBrandsVideoFrequency),
[sponsoredDisplayOnlineVideoFrequency](guides/reporting/v3/columns#sponsoredDisplayOnlineVideoFrequency),
[amazonDspOnlineVideoFrequency](guides/reporting/v3/columns#amazonDspOnlineVideoFrequency),
[sponsoredTvStreamingTvFrequency](guides/reporting/v3/columns#sponsoredTvStreamingTvFrequency),
[amazonDspStreamingTvFrequency](guides/reporting/v3/columns#amazonDspStreamingTvFrequency),
[amazonDspAudioFrequency](guides/reporting/v3/columns#amazonDspAudioFrequency),
[sponsoredProductsFrequency](guides/reporting/v3/columns#sponsoredProductsFrequency),
[sponsoredDisplayFrequency](guides/reporting/v3/columns#sponsoredDisplayFrequency)

Filters: 

- [brandName](guides/reporting/v3/columns#brandName)


## Sample calls

#### Sponsored Ads: Conversion Path report for SSPA advertiser

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data-raw '{
    "name": "Conversion Path Report",
    "startDate": "2025-06-01",
    "endDate": "2025-06-08",
    "configuration": {
      "adProduct": "ALL",
      "groupBy": ["brandConversionPath"],
      "columns": [
        "startDate",
        "endDate",
        "brandName",
        "simplifiedPath",
        "sales",
        "purchases",
        "newToBrandSales",
        "newToBrandPurchases",
        "sponsoredBrandsDisplayFrequency",
        "sponsoredDisplayDisplayFrequency",
        "amazonDspDisplayFrequency",
        "sponsoredBrandsVideoFrequency",
        "sponsoredDisplayOnlineVideoFrequency",
        "amazonDspOnlineVideoFrequency",
        "sponsoredTvStreamingTvFrequency",
        "amazonDspStreamingTvFrequency",
        "amazonDspAudioFrequency",
        "sponsoredProductsFrequency",
        "sponsoredDisplayFrequency"
      ],
      "reportTypeId": "conversionPath",
      "timeUnit": "SUMMARY",
      "format": "CSV"
    }
}'
```

#### Amazon DSP: Conversion Path report for Amazon DSP advertiser


```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Amazon-Ads-AccountId: xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data-raw '{
    "name": "Conversion Path Report",
    "startDate": "2025-06-01",
    "endDate": "2025-06-08",
    "configuration": {
      "adProduct": "ALL",
      "groupBy": ["brandConversionPath"],
      "columns": [
        "startDate",
        "endDate",
        "brandName",
        "simplifiedPath",
        "sales",
        "purchases",
        "newToBrandSales",
        "newToBrandPurchases",
        "sponsoredBrandsDisplayFrequency",
        "sponsoredDisplayDisplayFrequency",
        "amazonDspDisplayFrequency",
        "sponsoredBrandsVideoFrequency",
        "sponsoredDisplayOnlineVideoFrequency",
        "amazonDspOnlineVideoFrequency",
        "sponsoredTvStreamingTvFrequency",
        "amazonDspStreamingTvFrequency",
        "amazonDspAudioFrequency",
        "sponsoredProductsFrequency",
        "sponsoredDisplayFrequency"
      ],
      "reportTypeId": "conversionPath",
      "timeUnit": "SUMMARY",
      "format": "CSV"
    }
}'
```
