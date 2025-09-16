---
title: Search term reports
description: Learn what report types and metrics are available in version 3 reporting for sponsored ads using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Search term reports

Search term reports contain search term performance metrics broken down by targeting expressions and keywords. Note that search term reports only include impressions that resulted in at least one ad click. Use the `keywordType` filter to include either targeting expressions or keywords in your report. 

> [NOTE] If a placement does not have a search keyword associated with it on a product detail page, the search term in the report will be an asterisk `*`.

## Configurations

| Configuration | Sponsored Products | Sponsored Brands |
|----------|---------|------|
| `reportTypeId` | `spSearchTerm` | `sbSearchTerm` |
| Maximum date range | 31 days | 31 days |
| Data retention | 95 days | 60 days |
| `timeUnit` | `SUMMARY` or `DAILY` |`SUMMARY` or `DAILY` |
| `groupBy` | `searchTerm` |`searchTerm` |
| `format` | `GZIP_JSON` |`GZIP_JSON` |

## Sponsored Products

### Base metrics

[impressions](guides/reporting/v3/columns#impressions), [addToList](guides/reporting/v3/columns#addToList), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [clicks](guides/reporting/v3/columns#clicks), [costPerClick](guides/reporting/v3/columns#costPerClick), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [cost](guides/reporting/v3/columns#cost), [purchases1d](guides/reporting/v3/columns#purchases1d), [purchases7d](guides/reporting/v3/columns#purchases7d), [purchases14d](guides/reporting/v3/columns#purchases14d), [purchases30d](guides/reporting/v3/columns#purchases30d), [purchasesSameSku1d](guides/reporting/v3/columns#purchasesSameSku1d), [purchasesSameSku7d](guides/reporting/v3/columns#purchasesSameSku7d), [purchasesSameSku14d](guides/reporting/v3/columns#purchasesSameSku14d), [purchasesSameSku30d](guides/reporting/v3/columns#purchasesSameSku30d), [unitsSoldClicks1d](guides/reporting/v3/columns#unitsSoldClicks1d), [unitsSoldClicks7d](guides/reporting/v3/columns#unitsSoldClicks7d), [unitsSoldClicks14d](guides/reporting/v3/columns#unitsSoldClicks14d), [unitsSoldClicks30d](guides/reporting/v3/columns#unitsSoldClicks30d), [sales1d](guides/reporting/v3/columns#sales1d), [sales7d](guides/reporting/v3/columns#sales7d), [sales14d](guides/reporting/v3/columns#sales14d), [sales30d](guides/reporting/v3/columns#sales30d), [attributedSalesSameSku1d](guides/reporting/v3/columns#attributedSalesSameSku1d), [attributedSalesSameSku7d](guides/reporting/v3/columns#attributedSalesSameSku7d), [attributedSalesSameSku14d](guides/reporting/v3/columns#attributedSalesSameSku14d), [attributedSalesSameSku30d](guides/reporting/v3/columns#attributedSalesSameSku30d), [unitsSoldSameSku1d](guides/reporting/v3/columns#unitsSoldSameSku1d), [unitsSoldSameSku7d](guides/reporting/v3/columns#unitsSoldSameSku7d), [unitsSoldSameSku14d](guides/reporting/v3/columns#unitsSoldSameSku14d), [unitsSoldSameSku30d](guides/reporting/v3/columns#unitsSoldSameSku30d), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [salesOtherSku7d](guides/reporting/v3/columns#salesOtherSku7d), [unitsSoldOtherSku7d](guides/reporting/v3/columns#unitsSoldOtherSku7d), [acosClicks7d](guides/reporting/v3/columns#acosClicks7d), [acosClicks14d](guides/reporting/v3/columns#acosClicks14d), [roasClicks7d](guides/reporting/v3/columns#roasClicks7d), [roasClicks14d](guides/reporting/v3/columns#roasClicks14d), [keywordId](guides/reporting/v3/columns#keywordId), [keyword](guides/reporting/v3/columns#keyword), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startDate), [endDate](guides/reporting/v3/columns#endDate), [portfolioId](guides/reporting/v3/columns#portfolioId), [searchTerm](guides/reporting/v3/columns#searchTerm), [campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [keywordBid](guides/reporting/v3/columns#keywordBid), [adGroupName](guides/reporting/v3/columns#adGroupName), [adGroupId](guides/reporting/v3/columns#adGroupId), [keywordType](guides/reporting/v3/columns#keywordType), [matchType](guides/reporting/v3/columns#matchType), [targeting](guides/reporting/v3/columns#targeting), [adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus)
 

### Group by `searchTerm`

Additional metrics: [adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus)

Filters: 

- `keywordType` (values: `BROAD`, `PHRASE`, `EXACT`, `TARGETING_EXPRESSION`, `TARGETING_EXPRESSION_PREDEFINED`)

## Sponsored Brands

>[NOTE] This report is currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled` set to `FALSE` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

### Base metrics

[adGroupId](guides/reporting/v3/columns#adGroupId), [adGroupName](guides/reporting/v3/columns#adGroupName), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks),  [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [costType](guides/reporting/v3/columns#costType), [date](guides/reporting/v3/columns#date), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [keywordBid](guides/reporting/v3/columns#keywordBid), [keywordId](guides/reporting/v3/columns#keywordId), [keywordText](guides/reporting/v3/columns#keywordText), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [matchType](guides/reporting/v3/columns#matchType), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [searchTerm](guides/reporting/v3/columns#searchTerm), [startDate](guides/reporting/v3/columns#startDate), [unitsSold](guides/reporting/v3/columns#unitsSold), [video5SecondViewRate](guides/reporting/v3/columns#video5SecondViewRate), [video5SecondViews](guides/reporting/v3/columns#video5SecondViews), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v3/columns#videoUnmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate), [viewableImpressions](guides/reporting/v3/columns#viewableImpressions), [viewClickThroughRate](guides/reporting/v3/columns#viewClickThroughRate)

### Group by `searchTerm`

Additional metrics: [adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus)

Filters: 

- `keywordType` (values: `BROAD`, `PHRASE`, `EXACT`, `TARGETING_EXPRESSION`, `TARGETING_EXPRESSION_PREDEFINED`)


## Sample calls

### Sponsored Products: Targeting expressions only

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data-raw '{
    "name":"SP search term report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["searchTerm"],
        "columns":["impressions","clicks","cost","campaignId","adGroupId","date","targeting","searchTerm","keywordType","keywordId"],
        "filters": [
            {
                "field": "keywordType",
                "values": [
                    "TARGETING_EXPRESSION",
                    "TARGETING_EXPRESSION_PREDEFINED"
                ]
            }
        ],
        "reportTypeId":"spSearchTerm",
        "timeUnit":"DAILY",
        "format":"GZIP_JSON"
    }
}'
```
    
#### Sponsored Products: Keywords only

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data-raw '{
    "name":"SB search terms report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["searchTerm"],
        "columns":["impressions","clicks","cost","campaignId","adGroupId","startDate","endDate","keywordType","keyword","matchType","keywordId","searchTerm"],
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
        "reportTypeId":"spSearchTerm",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}'
```

#### Sponsored Brands

```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--data '{
    "name": "SP search terms report 9/5-9/10",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_BRANDS",
        "groupBy": [
            "searchTerm"
        ],
        "columns": [
            "impressions",
            "clicks",
            "cost",
            "campaignId",
            "adGroupId",
            "startDate",
            "endDate",
            "matchType",
            "keywordId",
            "searchTerm"
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
        "reportTypeId": "sbSearchTerm",
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```
