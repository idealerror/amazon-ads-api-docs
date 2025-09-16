---
title: Placement reports
description: Learn about requesting placement reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Brands
---

# Placement reports

Placement reports contain performance data broken down by ad placement.

>[NOTE] For Sponsored Products, there is not a separate placement report. You can get placement-level data using the 'campaignPlacement' `groupBy` in a [campaign report](guides/reporting/v3/report-types/campaign#group-by-campaignplacement).

## Configurations

| Configuration  | Sponsored Brands |
|----------|---------|
| `reportTypeId` | `sbCampaignPlacement` |
| Maximum date range | 31 days |
| Data retention |  60 days |
| `timeUnit` | `SUMMARY` or `DAILY` |
| `groupBy` | `campaign`|
| `format` | `GZIP_JSON` |

## Sponsored Brands

>[NOTE] This report is currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled` set to `FALSE` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [costType](guides/reporting/v3/columns#costType), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewsClicks](guides/reporting/v3/columns#newToBrandDetailPageViewsClicks), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchasesPercentage](guides/reporting/v3/columns#newToBrandPurchasesPercentage), [newToBrandPurchasesRate](guides/reporting/v3/columns#newToBrandPurchasesRate), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandSalesPercentage](guides/reporting/v3/columns#newToBrandSalesPercentage), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [newToBrandUnitsSoldPercentage](guides/reporting/v3/columns#newToBrandUnitsSoldPercentage), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromoted](guides/reporting/v3/columns#purchasesPromoted), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromoted](guides/reporting/v3/columns#salesPromoted), [startDate](guides/reporting/v3/columns#startDate), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [video5SecondViewRate](guides/reporting/v3/columns#video5SecondViewRate), [video5SecondViews](guides/reporting/v3/columns#video5SecondViews), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v3/columns#videoUnmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate), [viewableImpressions](guides/reporting/v3/columns#viewableImpressions), [viewClickThroughRate](guides/reporting/v3/columns#viewClickThroughRate)

### Group by `campaignPlacement`

Additional metrics: [placementClassification](guides/reporting/v3/columns#placementClassification)

### Group by `campaign`

Additional metrics: N/A

## Sample calls

#### Sponsored Brands: campaign placement summary report 

```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--data '{
    "name": "SB placement report 9/5-9/10",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_BRANDS",
        "groupBy": [
            "campaignPlacement"
        ],
        "columns": [
            "impressions",
            "clicks",
            "cost",
            "campaignId",
            "placementClassification",
            "startDate",
            "endDate"
        ],
        "reportTypeId": "sbCampaignPlacement",
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```
