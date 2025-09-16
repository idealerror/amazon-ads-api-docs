---
title: Campaign reports
description: Learn about requesting ad group reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Brands
---

# Ad group reports

Ad group reports contain performance data broken down at the ad group level. Ad group reports include all campaigns of the requested sponsored ad type that have performance activity for the requested days. For example, a Sponsored Brands ad group report returns performance data for all Sponsored Brands ad groups that received impressions on the chosen dates. 

>[NOTE] For Sponsored Products, there is not a separate ad group report. You can get ad group-level data using the ad group `groupBy` in a [campaign report](guides/reporting/v3/report-types/campaign#group-by-adgroup).

## Configurations

| Configuration  | Sponsored Brands | Sponsored Display |
|----------|---------|---|
| `reportTypeId` | `sbAdGroup` | `sdAdGroup` |
| Maximum date range | 31 days | 31 days |
| Data retention |  60 days | 65 days |
| `timeUnit` | `SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` |
| `groupBy` | `adGroup`| `adGroup` or `matchedTarget`|
| `format` | `GZIP_JSON` | `GZIP_JSON` |

## Sponsored Brands

>[NOTE] This report currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [adGroupId](guides/reporting/v3/columns#adGroupId), [adGroupName](guides/reporting/v3/columns#adGroupName), [adStatus](guides/reporting/v3/columns#adStatus), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [costType](guides/reporting/v3/columns#costType), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewsClicks](guides/reporting/v3/columns#newToBrandDetailPageViewsClicks), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchasesPercentage](guides/reporting/v3/columns#newToBrandPurchasesPercentage), [newToBrandPurchasesRate](guides/reporting/v3/columns#newToBrandPurchasesRate), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandSalesPercentage](guides/reporting/v3/columns#newToBrandSalesPercentage), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [newToBrandUnitsSoldPercentage](guides/reporting/v3/columns#newToBrandUnitsSoldPercentage), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromoted](guides/reporting/v3/columns#purchasesPromoted), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromoted](guides/reporting/v3/columns#salesPromoted), [startDate](guides/reporting/v3/columns#startDate), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [video5SecondViewRate](guides/reporting/v3/columns#video5SecondViewRate), [video5SecondViews](guides/reporting/v3/columns#video5SecondViews), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v3/columns#videoUnmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate)

### Group by `adGroup`

Additional metrics: N/A

Filters: 

- `adStatus` (values: ENABLED, PAUSED, ARCHIVED)

### Group by `campaign`

Additional metrics: N/A

## Sponsored Display

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [adGroupId](guides/reporting/v3/columns#adGroupId), [adGroupName](guides/reporting/v3/columns#adGroupName), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [addToListFromViews](guides/reporting/v3/columns#addToListFromViews), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [qualifiedBorrowsFromViews](guides/reporting/v3/columns#qualifiedBorrowsFromViews), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [royaltyQualifiedBorrowsFromViews](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromViews), [bidOptimization](guides/reporting/v3/columns#bidOptimization), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [brandedSearchRate](guides/reporting/v3/columns#brandedSearchRate), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [cumulativeReach](guides/reporting/v3/columns#cumulativeReach), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [impressionsViews](guides/reporting/v3/columns#impressionsViews), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromotedClicks](guides/reporting/v3/columns#purchasesPromotedClicks), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromotedClicks](guides/reporting/v3/columns#salesPromotedClicks), [startDate](guides/reporting/v3/columns#startDate), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v3/columns#videoUnmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate), [viewClickThroughRate](guides/reporting/v3/columns#viewClickThroughRate)

### Group by `adGroup`

Additional metrics: [cumulativeReach](guides/reporting/v3/columns#cumulativeReach), [impressionsFrequencyAverage](guides/reporting/v3/columns#impressionsFrequencyAverage), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView)

Filters: N/A

### Group by `matchedTarget`

Additional metrics: [matchedTargetAsin](guides/reporting/v3/columns#matchedTargetAsin)

Filters: N/A

## Sample calls

#### Sponsored Brands: Ad group summary report grouped by ad group 

```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--data '{
    "name": "SB ad group report 9/5-9/10",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_BRANDS",
        "groupBy": [
            "adGroup"
        ],
        "columns": [
            "impressions",
            "clicks",
            "cost",
            "campaignId",
            "adGroupId",
            "startDate",
            "endDate"
        ],
        "reportTypeId": "sbAdGroup",
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```
