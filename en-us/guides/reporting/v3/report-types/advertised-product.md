---
title: Advertised product reports
description: Learn about requesting advertised product reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Advertised product reports

Advertised product reports contain performance data for products that are advertised as part of your campaigns. 

## Configurations

| Configuration | Sponsored Products | Sponsored Display |
|----------|---------|-------|------|
| `reportTypeId` | `spAdvertisedProduct` | `sdAdvertisedProduct`|
| Maximum date range | 31 days | 31 days | 
| Data retention | 95 days | 65 days |
| `timeUnit` | `SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` |
| `groupBy` | `advertiser` | `advertiser` | 
| `format` | `GZIP_JSON` | `GZIP_JSON` |

## Sponsored Products

### Base metrics

[date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startDate), [endDate](guides/reporting/v3/columns#endDate), [campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [adGroupName](guides/reporting/v3/columns#adGroupName), [adGroupId](guides/reporting/v3/columns#adGroupId), [adId](guides/reporting/v3/columns#adId), [addToList](guides/reporting/v3/columns#addToList), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [portfolioId](guides/reporting/v3/columns#portfolioId), [impressions](guides/reporting/v3/columns#impressions), [clicks](guides/reporting/v3/columns#clicks), [costPerClick](guides/reporting/v3/columns#costPerClick), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [cost](guides/reporting/v3/columns#cost), [spend](guides/reporting/v3/columns#spend), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [advertisedAsin](guides/reporting/v3/columns#advertisedAsin), [advertisedSku](guides/reporting/v3/columns#advertisedSku), [purchases1d](guides/reporting/v3/columns#purchases1d), [purchases7d](guides/reporting/v3/columns#purchases7d), [purchases14d](guides/reporting/v3/columns#purchases14d), [purchases30d](guides/reporting/v3/columns#purchases30d), [purchasesSameSku1d](guides/reporting/v3/columns#purchasesSameSku1d), [purchasesSameSku7d](guides/reporting/v3/columns#purchasesSameSku7d), [purchasesSameSku14d](guides/reporting/v3/columns#purchasesSameSku14d), [purchasesSameSku30d](guides/reporting/v3/columns#purchasesSameSku30d), [unitsSoldClicks1d](guides/reporting/v3/columns#unitsSoldClicks1d), [unitsSoldClicks7d](guides/reporting/v3/columns#unitsSoldClicks7d), [unitsSoldClicks14d](guides/reporting/v3/columns#unitsSoldClicks14d), [unitsSoldClicks30d](guides/reporting/v3/columns#unitsSoldClicks30d), [sales1d](guides/reporting/v3/columns#sales1d), [sales7d](guides/reporting/v3/columns#sales7d), [sales14d](guides/reporting/v3/columns#sales14d), [sales30d](guides/reporting/v3/columns#sales30d), [attributedSalesSameSku1d](guides/reporting/v3/columns#attributedSalesSameSku1d), [attributedSalesSameSku7d](guides/reporting/v3/columns#attributedSalesSameSku7d), [attributedSalesSameSku14d](guides/reporting/v3/columns#attributedSalesSameSku14d), [attributedSalesSameSku30d](guides/reporting/v3/columns#attributedSalesSameSku30d), [salesOtherSku7d](guides/reporting/v3/columns#salesOtherSku7d), [unitsSoldSameSku1d](guides/reporting/v3/columns#unitsSoldSameSku1d), [unitsSoldSameSku7d](guides/reporting/v3/columns#unitsSoldSameSku7d), [unitsSoldSameSku14d](guides/reporting/v3/columns#unitsSoldSameSku14d), [unitsSoldSameSku30d](guides/reporting/v3/columns#unitsSoldSameSku30d), [unitsSoldOtherSku7d](guides/reporting/v3/columns#unitsSoldOtherSku7d), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [acosClicks7d](guides/reporting/v3/columns#acosClicks7d), [acosClicks14d](guides/reporting/v3/columns#acosClicks14d), [roasClicks7d](guides/reporting/v3/columns#roasClicks7d), [roasClicks14d](guides/reporting/v3/columns#roasClicks14d)
 

#### Group by `advertiser`

Additional metrics: N/A

Filters: 

- `adCreativeStatus` (values: `ENABLED`,`PAUSED`,`ARCHIVED`)

## Sponsored Display

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [adGroupId](guides/reporting/v3/columns#adGroupId), [adGroupName](guides/reporting/v3/columns#adGroupName), [adId](guides/reporting/v3/columns#adId), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [addToListFromViews](guides/reporting/v3/columns#addToListFromViews), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [qualifiedBorrowsFromViews](guides/reporting/v3/columns#qualifiedBorrowsFromViews), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [royaltyQualifiedBorrowsFromViews](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromViews), [bidOptimization](guides/reporting/v3/columns#bidOptimization), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [brandedSearchRate](guides/reporting/v3/columns#brandedSearchRate), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [cumulativeReach](guides/reporting/v3/columns#cumulativeReach), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [impressionsFrequencyAverage](guides/reporting/v3/columns#impressionsFrequencyAverage), [impressionsViews](guides/reporting/v3/columns#impressionsViews), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [promotedAsin](guides/reporting/v3/columns#promotedAsin), [promotedSku](guides/reporting/v3/columns#promotedSku), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromotedClicks](guides/reporting/v3/columns#purchasesPromotedClicks), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromotedClicks](guides/reporting/v3/columns#salesPromotedClicks), [startDate](guides/reporting/v3/columns#startDate), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v3/columns#videoUnmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate), [viewClickThroughRate](guides/reporting/v3/columns#viewClickThroughRate)

#### Group by `advertiser`

Additional metrics: N/A

Filters: N/A

## Sample calls

### Sponsored Products

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
--data-raw '{
    "name":"SP advertised product report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["advertiser"],
        "columns":["impressions","clicks","cost","campaignId","advertisedAsin"],
        "reportTypeId":"spAdvertisedProduct",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}'
```

### Sponsored Display

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
--data-raw '{
    "name":"SD advertised product report 3/5-3/10",
    "startDate":"2025-03-05",
    "endDate":"2025-03-10",
    "configuration":{
        "adProduct":"SPONSORED_DISPLAY",
        "groupBy":["advertiser"],
        "columns":["impressions","clicks","cost","campaignId", "newToBrandSalesClicks", "detailPageViews"],
        "reportTypeId":"sdAdvertisedProduct",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}'
```
