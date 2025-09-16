---
title: Targeting reports
description: Learn about requesting targeting reports using the Amazon Ads API.
interface: api
type: guide
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Targeting reports

Targeting reports contain performance metrics broken down by both targeting expressions and keywords. 

## Configurations

| Configuration | Sponsored Products | Sponsored Brands | Sponsored Display | Sponsored Television|
|----------|---------|------|------|------|
| `reportTypeId` | `spTargeting` | `sbTargeting` | `sdTargeting`| `stTargeting`|
| Maximum date range | 31 days | 31 days | 31 days |31 days |
| Data retention | 95 days | 60 days | 65 days |95 days | 
| `timeUnit` | `SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` |`SUMMARY` or `DAILY` |`SUMMARY` or `DAILY` |
| `groupBy` | `targeting` |`targeting` | `targeting`, `matchedTarget` |`campaign`, `adGroup`, or `targeting` |
| `format` | `GZIP_JSON` |`GZIP_JSON` |`GZIP_JSON` |`GZIP_JSON` |

>[NOTE]Targeting reports are not supported for Sponsored TV non-endemic advertisers.

## Requesting keywords vs. targets

To see only targeting expressions, set the `keywordType` filter to `TARGETING_EXPRESSION` and `TARGETING_EXPRESSION_PREDEFINED`. To see only keywords, set the `keywordType` filter to `BROAD`, `PHRASE`, and `EXACT`.

## Sponsored Products

### Base metrics

[impressions](guides/reporting/v3/columns#impressions), [addToList](guides/reporting/v3/columns#addToList), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [clicks](guides/reporting/v3/columns#clicks), [costPerClick](guides/reporting/v3/columns#costPerClick), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [cost](guides/reporting/v3/columns#cost), [purchases1d](guides/reporting/v3/columns#purchases1d), [purchases7d](guides/reporting/v3/columns#purchases7d), [purchases14d](guides/reporting/v3/columns#purchases14d), [purchases30d](guides/reporting/v3/columns#purchases30d), [purchasesSameSku1d](guides/reporting/v3/columns#purchasesSameSku1d), [purchasesSameSku7d](guides/reporting/v3/columns#purchasesSameSku7d), [purchasesSameSku14d](guides/reporting/v3/columns#purchasesSameSku14d), [purchasesSameSku30d](guides/reporting/v3/columns#purchasesSameSku30d), [unitsSoldClicks1d](guides/reporting/v3/columns#unitsSoldClicks1d), [unitsSoldClicks7d](guides/reporting/v3/columns#unitsSoldClicks7d), [unitsSoldClicks14d](guides/reporting/v3/columns#unitsSoldClicks14d), [unitsSoldClicks30d](guides/reporting/v3/columns#unitsSoldClicks30d), [sales1d](guides/reporting/v3/columns#sales1d), [sales7d](guides/reporting/v3/columns#sales7d), [sales14d](guides/reporting/v3/columns#sales14d), [sales30d](guides/reporting/v3/columns#sales30d), [attributedSalesSameSku1d](guides/reporting/v3/columns#attributedSalesSameSku1d), [attributedSalesSameSku7d](guides/reporting/v3/columns#attributedSalesSameSku7d), [attributedSalesSameSku14d](guides/reporting/v3/columns#attributedSalesSameSku14d), [attributedSalesSameSku30d](guides/reporting/v3/columns#attributedSalesSameSku30d), [unitsSoldSameSku1d](guides/reporting/v3/columns#unitsSoldSameSku1d), [unitsSoldSameSku7d](guides/reporting/v3/columns#unitsSoldSameSku7d), [unitsSoldSameSku14d](guides/reporting/v3/columns#unitsSoldSameSku14d), [unitsSoldSameSku30d](guides/reporting/v3/columns#unitsSoldSameSku30d), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [salesOtherSku7d](guides/reporting/v3/columns#salesOtherSku7d), [unitsSoldOtherSku7d](guides/reporting/v3/columns#unitsSoldOtherSku7d), [acosClicks7d](guides/reporting/v3/columns#acosClicks7d), [acosClicks14d](guides/reporting/v3/columns#acosClicks14d), [roasClicks7d](guides/reporting/v3/columns#roasClicks7d), [roasClicks14d](guides/reporting/v3/columns#roasClicks14d), [keywordId](guides/reporting/v3/columns#keywordId), [keyword](guides/reporting/v3/columns#keyword), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startDate), [endDate](guides/reporting/v3/columns#endDate), [portfolioId](guides/reporting/v3/columns#portfolioId), [campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [keywordBid](guides/reporting/v3/columns#keywordBid), [adGroupName](guides/reporting/v3/columns#adGroupName), [adGroupId](guides/reporting/v3/columns#adGroupId), [keywordType](guides/reporting/v3/columns#keywordType), [matchType](guides/reporting/v3/columns#matchType), [targeting](guides/reporting/v3/columns#targeting), [topOfSearchImpressionShare](guides/reporting/v3/columns#topOfSearchImpressionShare)
 
### Group by `targeting`

Additional metrics: [adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus)

Filters: 

- `adKeywordStatus` (values: `ENABLED`,`PAUSED`,`ARCHIVED`)
- `keywordType` (values: `BROAD`, `PHRASE`, `EXACT`, `TARGETING_EXPRESSION`, `TARGETING_EXPRESSION_PREDEFINED`)

## Sponsored Brands

>[NOTE] This report currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [adGroupId](guides/reporting/v3/columns#adGroupId), [adGroupName](guides/reporting/v3/columns#adGroupName), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [costType](guides/reporting/v3/columns#costType), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [keywordBid](guides/reporting/v3/columns#keywordBid), [keywordId](guides/reporting/v3/columns#keywordId), [adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus), [keywordText](guides/reporting/v3/columns#keywordText), [keywordType](guides/reporting/v3/columns#keywordType), [matchType](guides/reporting/v3/columns#matchType), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewsClicks](guides/reporting/v3/columns#newToBrandDetailPageViewsClicks), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchasesPercentage](guides/reporting/v3/columns#newToBrandPurchasesPercentage), [newToBrandPurchasesRate](guides/reporting/v3/columns#newToBrandPurchasesRate), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandSalesPercentage](guides/reporting/v3/columns#newToBrandSalesPercentage), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [newToBrandUnitsSoldPercentage](guides/reporting/v3/columns#newToBrandUnitsSoldPercentage), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromoted](guides/reporting/v3/columns#purchasesPromoted), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromoted](guides/reporting/v3/columns#salesPromoted), [startDate](guides/reporting/v3/columns#startDate), [targetingExpression](guides/reporting/v3/columns#targetingExpression), [targetingId](guides/reporting/v3/columns#targetingId), [targetingText](guides/reporting/v3/columns#targetingText), [targetingType](guides/reporting/v3/columns#targetingType), [topOfSearchImpressionShare](guides/reporting/v3/columns#topOfSearchImpressionShare), [unitsSold](guides/reporting/v3/columns#unitsSold)

### Group by `targeting`

Additional metrics: N/A

Filters: 

- `adKeywordStatus` (values: `ENABLED`,`PAUSED`,`ARCHIVED`)
- `keywordType` (values: `BROAD`, `PHRASE`, `EXACT`, `TARGETING_EXPRESSION`, `TARGETING_EXPRESSION_PREDEFINED`, `THEME`)

## Sponsored Display

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [adGroupId](guides/reporting/v3/columns#adGroupId), [adGroupName](guides/reporting/v3/columns#adGroupName), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [addToListFromViews](guides/reporting/v3/columns#addToListFromViews), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [qualifiedBorrowsFromViews](guides/reporting/v3/columns#qualifiedBorrowsFromViews), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [royaltyQualifiedBorrowsFromViews](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromViews), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [brandedSearchRate](guides/reporting/v3/columns#brandedSearchRate), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [impressionsViews](guides/reporting/v3/columns#impressionsViews), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromotedClicks](guides/reporting/v3/columns#purchasesPromotedClicks), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromotedClicks](guides/reporting/v3/columns#salesPromotedClicks), [startDate](guides/reporting/v3/columns#startDate), [targetingExpression](guides/reporting/v3/columns#targetingExpression), [targetingId](guides/reporting/v3/columns#targetingId), [targetingText](guides/reporting/v3/columns#targetingText), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videofirstquartileviews), [videoMidpointViews](guides/reporting/v3/columns#videomidpointviews), [videoThirdQuartileViews](videothirdquartileviews), [videoUnmutes](guides/reporting/v3/columns#videounmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityrate), [viewClickThroughRate](viewclickthroughrate)

### Group by `targeting`

Additional metrics: [adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView)

Filters: N/A

### Group by `matchedTarget`

Additional metrics: [matchedTargetAsin](guides/reporting/v3/columns#matchedTargetAsin)

Filters: N/A

## Sponsored Television

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [clicks](guides/reporting/v3/columns#clicks), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [cost](guides/reporting/v3/columns#cost), [costPerThousandImpressions](guides/reporting/v3/columns#costPerThousandImpressions), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [detailPageViewsViews](guides/reporting/v3/columns#detailPageViewsViews), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [keyword](guides/reporting/v3/columns#keyword), [keywordBid](guides/reporting/v3/columns#keywordBid), [keywordId](guides/reporting/v3/columns#keywordId), [keywordType](guides/reporting/v3/columns#keywordType), [matchType](guides/reporting/v3/columns#matchType), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchasesViews](guides/reporting/v3/columns#newToBrandPurchasesViews), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandSalesViews](guides/reporting/v3/columns#newToBrandSalesViews), [portfolioId](guides/reporting/v3/columns#portfolioId), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesViews](guides/reporting/v3/columns#purchasesViews), [roas](guides/reporting/v3/columns#roas), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesViews](guides/reporting/v3/columns#salesViews), [startDate](guides/reporting/v3/columns#startDate), [targetingText](guides/reporting/v3/columns#targetingText), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [unitsSoldViews](guides/reporting/v3/columns#unitsSoldViews), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews)

### Group by `campaign`
  
Additional metrics: [campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode)
  
Filters: 
  
- `campaignStatus` (values: ENABLED, PAUSED, ARCHIVED)

### Group by `adGroup`
  
Additional metrics: [adGroupName](guides/reporting/v3/columns#adGroupName), [adGroupId](guides/reporting/v3/columns#adGroupId), [adStatus](guides/reporting/v3/columns#adStatus)
  
Filters: 
  
- `adStatus` (values: ENABLED, PAUSED, ARCHIVED)

### Group by `targeting`

Additional metrics: [adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus)

Filters: 

- `adKeywordStatus` (values: `ENABLED`,`PAUSED`,`ARCHIVED`)

## Sample calls

### Sponsored Products: Targeting expressions only

 ```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--data '{
    "name": "SP targeting report 9/5-9/10",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_PRODUCTS",
        "groupBy": [
            "targeting"
        ],
        "columns": [
            "adGroupId",
            "campaignId",
            "targeting",
            "keywordId",
            "matchType",
            "impressions",
            "clicks",
            "cost",
            "purchases1d",
            "purchases7d",
            "purchases14d",
            "purchases30d",
            "startDate",
            "endDate"
        ],
        "filters": [
            {
                "field": "keywordType",
                "values": [
                    "TARGETING_EXPRESSION",
                    "TARGETING_EXPRESSION_PREDEFINED"
                ]
            }
        ],
        "reportTypeId": "spTargeting",
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```

#### Sponsored Products: Keywords only

 ```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data '{
    "name": "SP keywords report 9/5-9/10",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_PRODUCTS",
        "groupBy": [
            "targeting"
        ],
        "columns": [
            "adGroupId",
            "campaignId",
            "keywordId",
            "matchType",
            "keyword",
            "impressions",
            "clicks",
            "cost",
            "purchases1d",
            "purchases7d",
            "purchases14d",
            "purchases30d",
            "startDate",
            "endDate"
        ],
        "filters": [
            {
                "field": "keywordType",
                "values": [
                    "BROAD",
                    "PHRASE",
                    "EXACT"
                ]
            }
        ],
        "reportTypeId": "spTargeting",
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```

#### Sponsored Brands: Keywords only

 ```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--data '{
    "name": "SB keywords report 9/5-9/10",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_BRANDS",
        "groupBy": [
            "targeting"
        ],
        "columns": [
            "adGroupId",
            "campaignId",
            "keywordId",
            "matchType",
            "keywordText",
            "impressions",
            "clicks",
            "cost",
            "startDate",
            "endDate"
        ],
        "filters": [
            {
                "field": "keywordType",
                "values": [
                    "BROAD",
                    "PHRASE",
                    "EXACT"
                ]
            }
        ],
        "reportTypeId": "sbTargeting",
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```