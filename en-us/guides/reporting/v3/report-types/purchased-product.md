---
title: Sponsored ads report types (version 3)
description: Learn what report types and metrics are available in version 3 reporting for sponsored ads using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Purchased product reports

## Configurations

| Configuration | Sponsored Products | Sponsored Brands | Sponsored Display |
|----------|---------|------|------|
| `reportTypeId` | `spPurchasedProduct` | `sbPurchasedProduct`| `sdPurchasedProduct`|
| Maximum date range | 31 days | 731 days | 31 days |
| Data retention | 95 days | 731 days | 65 days |
| `timeUnit` | `SUMMARY` or `DAILY` |`SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` |
| `groupBy` | `asin` | `purchasedAsin` | `asin`|
| `format` | `GZIP_JSON` | `GZIP_JSON` |`GZIP_JSON` |

## Sponsored Products

Sponsored Products purchased product reports contain performance data for products that were purchased, but were not advertised as part of a campaign. The purchased product report contains both targeting expressions and keyword IDs. After you have received your report, you can filter on [keywordType](guides/reporting/v3/columns#keywordType) to distinguish between targeting expressions and keywords. 

### Base metrics

[date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startDate), [endDate](guides/reporting/v3/columns#endDate), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [portfolioId](guides/reporting/v3/columns#portfolioId), [campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [adGroupName](guides/reporting/v3/columns#adGroupName), [adGroupId](guides/reporting/v3/columns#adGroupId), [keywordId](guides/reporting/v3/columns#keywordId), [keyword](guides/reporting/v3/columns#keyword), [keywordType](guides/reporting/v3/columns#keywordType), [advertisedAsin](guides/reporting/v3/columns#advertisedAsin), [purchasedAsin](guides/reporting/v3/columns#purchasedAsin), [advertisedSku](guides/reporting/v3/columns#advertisedSku), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [matchType](guides/reporting/v3/columns#matchType), [unitsSoldClicks1d](guides/reporting/v3/columns#unitsSoldClicks1d), [unitsSoldClicks7d](guides/reporting/v3/columns#unitsSoldClicks7d), [unitsSoldClicks14d](guides/reporting/v3/columns#unitsSoldClicks14d), [unitsSoldClicks30d](guides/reporting/v3/columns#unitsSoldClicks30d), [sales1d](guides/reporting/v3/columns#sales1d), [sales7d](guides/reporting/v3/columns#sales7d), [sales14d](guides/reporting/v3/columns#sales14d), [sales30d](guides/reporting/v3/columns#sales30d), [purchases1d](guides/reporting/v3/columns#purchases1d), [purchases7d](guides/reporting/v3/columns#purchases7d), [purchases14d](guides/reporting/v3/columns#purchases14d), [purchases30d](guides/reporting/v3/columns#purchases30d), [unitsSoldOtherSku1d](guides/reporting/v3/columns#unitsSoldOtherSku1d), [unitsSoldOtherSku7d](guides/reporting/v3/columns#unitsSoldOtherSku7d), [unitsSoldOtherSku14d](guides/reporting/v3/columns#unitsSoldOtherSku14d), [unitsSoldOtherSku30d](guides/reporting/v3/columns#unitsSoldOtherSku30d), [salesOtherSku1d](guides/reporting/v3/columns#salesOtherSku1d), [salesOtherSku7d](guides/reporting/v3/columns#salesOtherSku7d), [salesOtherSku14d](guides/reporting/v3/columns#salesOtherSku14d), [salesOtherSku30d](guides/reporting/v3/columns#salesOtherSku30d), [purchasesOtherSku1d](guides/reporting/v3/columns#purchasesOtherSku1d), [purchasesOtherSku7d](guides/reporting/v3/columns#purchasesOtherSku7d), [purchasesOtherSku14d](guides/reporting/v3/columns#purchasesOtherSku14d), [purchasesOtherSku30d](guides/reporting/v3/columns#purchasesOtherSku30d), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d)
 

### Group by `asin`

Additional metrics: N/A

Filters: N/A

## Sponsored Brands

Sponsored Brands purchased product reports contain performance data for products that were purchased as a result of your campaign.

### Base metrics

[campaignId](guides/reporting/v3/columns#campaignId), [adGroupId](guides/reporting/v3/columns#adGroupId), [date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startDate), [endDate](guides/reporting/v3/columns#endDate), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignName](guides/reporting/v3/columns#campaignName), [campaignPriceTypeCode](guides/reporting/v3/columns#campaignPriceTypeCode), [adGroupName](guides/reporting/v3/columns#adGroupName), [attributionType](guides/reporting/v3/columns#attributionType), [purchasedAsin](guides/reporting/v3/columns#purchasedAsin), [ordersClicks14d](guides/reporting/v3/columns#ordersClicks14d), [productName](guides/reporting/v3/columns#productName), [productCategory](guides/reporting/v3/columns#productCategory), [sales14d](guides/reporting/v3/columns#sales14d), [salesClicks14d](guides/reporting/v3/columns#salesClicks14d), [orders14d](guides/reporting/v3/columns#orders14d), [unitsSold14d](guides/reporting/v3/columns#unitsSold14d), [newToBrandSales14d](guides/reporting/v3/columns#newToBrandSales14d), [newToBrandPurchases14d](guides/reporting/v3/columns#newToBrandPurchases14d), [newToBrandUnitsSold14d](guides/reporting/v3/columns#newToBrandUnitsSold14d), [newToBrandSalesPercentage14d](guides/reporting/v3/columns#newToBrandSalesPercentage14d), [newToBrandPurchasesPercentage14d](guides/reporting/v3/columns#newToBrandPurchasesPercentage14d), [newToBrandUnitsSoldPercentage14d](guides/reporting/v3/columns#newToBrandUnitsSoldPercentage14d), [unitsSoldClicks14d](guides/reporting/v3/columns#unitsSoldClicks14d), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d)
 

### Group by `purchasedAsin`

Additional metrics: N/A

Filters: N/A

## Sponsored Display

### Base metrics

[adGroupId](guides/reporting/v3/columns#adGroupId), [adGroupName](guides/reporting/v3/columns#adGroupName), [asinBrandHalo](guides/reporting/v3/columns#asinBrandHalo), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks),[addToListFromViews](guides/reporting/v3/columns#addToListFromViews), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromViews](guides/reporting/v3/columns#qualifiedBorrowsFromViews), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromViews](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromViews), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [conversionsBrandHalo](guides/reporting/v3/columns#conversionsBrandHalo), [conversionsBrandHaloClicks](guides/reporting/v3/columns#conversionsBrandHaloClicks), [date](guides/reporting/v3/columns#date), [endDate](guides/reporting/v3/columns#endDate), [promotedAsin](guides/reporting/v3/columns#promotedAsin), [promotedSku](guides/reporting/v3/columns#promotedSku), [salesBrandHalo](guides/reporting/v3/columns#salesBrandHalo), [salesBrandHaloClicks](guides/reporting/v3/columns#salesBrandHaloClicks), [startDate](guides/reporting/v3/columns#startDate), [unitsSoldBrandHalo](guides/reporting/v3/columns#unitsSoldBrandHalo), [unitsSoldBrandHaloClicks](guides/reporting/v3/columns#unitsSoldBrandHaloClicks)

### Group by `asin`

Additional metrics: N/A

Filters: N/A


## Sample calls

### Sponsored Products

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
    "name":"SP purchased product report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["asin"],
        "columns":["purchasedAsin","advertisedAsin","adGroupName","campaignName","sales14d","campaignId","adGroupId","keywordId","keywordType","keyword"],
        "reportTypeId":"spPurchasedProduct",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}'
```

### Sponsored Brands

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
    "name":"SB purchased product report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_BRANDS",
        "groupBy":["purchasedAsin"],
        "columns":["purchasedAsin","attributionType","adGroupName","campaignName","sales14d","startDate","endDate"],
        "reportTypeId":"sbPurchasedProduct",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}'
```

