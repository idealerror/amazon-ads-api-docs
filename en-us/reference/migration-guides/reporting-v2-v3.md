---
title: Sponsored ads reporting migration guide (version 3)
description: Understand the major differences between version 2 and 3 for sponsored ads reporting using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Migrating from version 2 to version 3 of the reporting API

>[NOTE] Sponsored Brands reports are currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

New features include:

- A single unified endpoint for all ad types
- Multiple days of data in a single report
- Summary and daily grain reporting 
- Additional and standardized metrics
- Sponsored Brands purchased product report 
- Consistent dimensional data reporting
- Bug fixes

>[TIP] For more details on reporting version 3, view our [Frequently asked questions](guides/reporting/v3/faq).

## Report type comparison

Naming conventions used for report types have changed in version 3. The table below explains the relationships between version 2 report types and version 3 report types.

| Version 2 report name | Sponsored Products V3 report | Sponsored Brands V3 report | Sponsored Display V3 report |
|-----|-----|------|----|
| [Campaign](guides/reporting/v2/report-types#campaign-reports) | [Campaign](guides/reporting/v3/report-types/campaign) grouped by campaign | [Campaign](guides/reporting/v3/report-types/campaign) | [Campaign](guides/reporting/v3/report-types/campaign) |
| [Placement](guides/reporting/v2/report-types#placement-reports)  | [Campaign](guides/reporting/v3/report-types/campaign) grouped by placement and campaign | [Placement](guides/reporting/v3/report-types/placement) | N/A |
| [Ad group](guides/reporting/v2/report-types#ad-group-reports) | [Campaign](guides/reporting/v3/report-types/campaign) grouped by ad group and campaign | [Ad group](guides/reporting/v3/report-types/ad-group) | [Ad group](guides/reporting/v3/report-types/ad-group) |
| [Product ad](guides/reporting/v2/report-types#product-ad-reports) | [Advertised product](guides/reporting/v3/report-types/advertised-product) | N/A | [Advertised product](guides/reporting/v3/report-types/advertised-product) | 
| [Target](guides/reporting/v2/report-types#target-reports) | [Targeting](guides/reporting/v3/report-types/targeting) filtered by [keywordType](guides/reporting/v3/columns#keywordType) | [Targeting](guides/reporting/v3/report-types/targeting) filtered by [keywordType](guides/reporting/v3/columns#keywordType) | [Targeting](guides/reporting/v3/report-types/targeting)|
| [Keyword](guides/reporting/v2/report-types#keyword-reports) | [Targeting](guides/reporting/v3/report-types/targeting) filtered by [keywordType](guides/reporting/v3/columns#keywordType) | [Targeting](guides/reporting/v3/report-types/targeting) filtered by [keywordType](guides/reporting/v3/columns#keywordType) | N/A |
| [Search term](guides/reporting/v2/report-types#search-term-reports) | [Search term](guides/reporting/v3/report-types/search-term) | [Search term](guides/reporting/v3/report-types/search-term) | N/A |
| [ASIN](guides/reporting/v2/report-types#asin-reports) | [Purchased product](guides/reporting/v3/report-types/purchased-product) | [Purchased product](guides/reporting/v3/report-types/purchased-product)| [Purchased product](guides/reporting/v3/report-types/purchased-product)|
| [Ad](guides/reporting/v2/report-types#ad-reports) | N/A | [Ad](guides/reporting/v3/report-types/ad)
| [Matched target](guides/reporting/v2/report-types#matched-target-reports) | N/A | N/A | [Campaign](guides/reporting/v3/report-types/campaign) grouped by matchedTarget <br /> [Ad group](guides/reporting/v3/report-types/ad-group) grouped by matchedTarget <br /> [Targeting](guides/reporting/v3/report-types/targeting) grouped by matchedTarget |

>[NOTE] Some advertisers that participated in the version 3 closed beta may also have access to a separate `spKeywords` report. We recommend using [spTargeting](guides/reporting/v3/report-types/targeting) with a filter to get keyword performance data going forward. 

## Endpoint equivalencies

| Version 2 reporting | Version 3 reporting|
|-------|------|
| POST /v2/sp/{recordType}/report <br /> [POST /v2/hsa/{recordType}/report](sponsored-brands/3-0/openapi#tag/Reports/paths/~1v2~1hsa~1{recordType}~1report/post) <br> [POST /sd/{recordType}/reports](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport) |  [POST /reporting/reports](offline-report-prod-3p#tag/Asynchronous-Reports/operation/createAsyncReport) |
| [GET /v2/reports/{reportId}](sponsored-brands/3-0/openapi#tag/Reports/paths/~1v2~1reports~1{reportId}/get) | [GET /reporting/reports/{reportId}](offline-report-prod-3p#tag/Asynchronous-Reports/operation/getAsyncReport) |
| [GET /v2/reports/{reportId}/download](sponsored-brands/3-0/openapi#tag/Reports/operation/downloadReport) | [GET /reporting/reports/{reportId}](offline-report-prod-3p#tag/Asynchronous-Reports/operation/getAsyncReport) |

### New report: Sponsored Brands purchased product report

Advertisers can now gain product-level insights on ad-attributed sales data for Sponsored Brands campaigns.

The new report includes all ad-attributed sales, including promoted products (purchases for your brand's products made by shoppers that clicked on your ad) and brand-halo (sales of any product in your advertised brands made by Amazon or other sellers).

[Learn more](guides/reporting/v3/report-types#purchased-product-reports).

## Metrics

Version 3 of the reporting API introduces some new metric or column names. Use this table to understand which metrics have changed.

>[NOTE] Sponsored Products and the Sponsored Brands purchased product report contain some metrics with a different naming convention than the rest of the Sponsored Brands reports. New reports added going forward will use the same naming convention as Sponsored Brands. 

### Metric/column comparison

|Version 2 metric name	|Version 3 column name (SP & SB purchased product)	|Version 3 column name (All other reports)	|
|----|------|-----|
|[applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId)	|[campaignApplicableBudgetRuleId](guides/reporting/v3/columns#campaignApplicableBudgetRuleId)	|	|
|[applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName)	|[campaignApplicableBudgetRuleName](guides/reporting/v3/columns#campaignApplicableBudgetRuleName)	|	|
|[asin](guides/reporting//v2/metrics#asin)	|[advertisedAsin](guides/reporting/v3/columns#advertisedAsin)	| [promotedAsin](guides/reporting/v3/columns#promotedAsin)	|
|[attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d)	|	|[brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks)	|
|[attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d)	|[purchases14d](guides/reporting/v3/columns#purchases14d)	|[purchasesClicks](guides/reporting/v3/columns#purchasesClicks)	|
|[attributedConversions14dOtherSKU](guides/reporting/v2/metrics#attributedConversions14dOtherSKU)	|[purchasesOtherSku14d](guides/reporting/v3/columns#purchasesOtherSku14d)	| [conversionsBrandHaloClicks](guides/reporting/v3/columns#conversionsBrandHaloClicks)	|
|[attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU)	|[purchasesSameSku14d](guides/reporting/v3/columns#purchasesSameSku14d)	|[purchasesPromotedClicks](guides/reporting/v3/columns#purchasesPromotedClicks)	|
|[attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d)	|[purchases1d](guides/reporting/v3/columns#purchases1d)	|	|
|[attributedConversions1dOtherSKU](guides/reporting/v2/metrics#attributedConversions1dOtherSKU)	|[purchasesOtherSku1d](guides/reporting/v3/columns#purchasesOtherSku1d)	|	|
|[attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU)	|[purchasesSameSku1d](guides/reporting/v3/columns#purchasesSameSku1d)	|	|
|[attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d)	|[purchases30d](guides/reporting/v3/columns#purchases30d)	|	|
|[attributedConversions30dOtherSKU](guides/reporting/v2/metrics#attributedConversions30dOtherSKU)	|[purchasesOtherSku30d](guides/reporting/v3/columns#purchasesOtherSku30d)	|	|
|[attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU)	|[purchasesSameSku30d](guides/reporting/v3/columns#purchasesSameSku30d)	|	|
|[attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d)	|[purchases7d](guides/reporting/v3/columns#purchases7d)	|	|
|[attributedConversions7dOtherSKU](guides/reporting/v2/metrics#attributedConversions7dOtherSKU)	|[purchasesOtherSku7d](guides/reporting/v3/columns#purchasesOtherSku7d)	|	|
|[attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU)	|[purchasesSameSku7d](guides/reporting/v3/columns#purchasesSameSku7d)	|	|
|[attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d)	|	|[detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks)	|
|[attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d)	|[kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d)	|	|
|[attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d)	|[kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d)	|	|
|[attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d)	|	|[newToBrandPurchasesRate](guides/reporting/v3/columns#newToBrandPurchasesRate)	|
|[attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d)	|[newToBrandPurchases14d](guides/reporting/v3/columns#newToBrandPurchases14d)	|[newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks)	|
|[attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d )	|[newToBrandPurchasesPercentage14d](guides/reporting/v3/columns#newToBrandPurchasesPercentage14d)	|[newToBrandPurchasesPercentage](guides/reporting/v3/columns#newToBrandPurchasesPercentage)	|
|[attributedSales14d](guides/reporting/v2/metrics#attributedSales14d)	|[sales14d](guides/reporting/v3/columns#sales14d)	|[salesClicks](guides/reporting/v3/columns#salesClicks)	|
|[attributedSales14dOtherSKU](guides/reporting/v2/metrics#attributedSales14dOtherSKU)	|[salesOtherSku14d](guides/reporting/v3/columns#salesOtherSku14d)	|	[salesBrandHaloClicks](guides/reporting/v3/columns#salesBrandHaloClicks)|
|[attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU)	|[attributedSalesSameSku14d](guides/reporting/v3/columns#attributedSalesSameSku14d)	|[salesPromotedClicks](guides/reporting/v3/columns#salesPromotedClicks)	|
|[attributedSales1d](guides/reporting/v2/metrics#attributedSales1d)	|[sales1d](guides/reporting/v3/columns#sales1d)	|	|
|[attributedSales1dOtherSKU](guides/reporting/v2/metrics#attributedSales1dOtherSKU)	|[salesOtherSku1d](guides/reporting/v3/columns#salesOtherSku1d)	|	|
|[attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU)	|[attributedSalesSameSku1d](guides/reporting/v3/columns#attributedSalesSameSku1d)	|	|
|[attributedSales30d](guides/reporting/v2/metrics#attributedSales30d)	|[sales30d](guides/reporting/v3/columns#sales30d)	|	|
|[attributedSales30dOtherSKU](guides/reporting/v2/metrics#attributedSales30dOtherSKU)	|[salesOtherSku30d](guides/reporting/v3/columns#salesOtherSku30d)	|	|
|[attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU)	|[attributedSalesSameSku30d](guides/reporting/v3/columns#attributedSalesSameSku30d)	|	|
|[attributedSales7d](guides/reporting/v2/metrics#attributedSales7d)	|[sales7d](guides/reporting/v3/columns#sales7d)	|	|
|[attributedSales7dOtherSKU](guides/reporting/v2/metrics#attributedSales7dOtherSKU)	|[salesOtherSku7d](guides/reporting/v3/columns#salesOtherSku7d)	|	|
|[attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU)	|[attributedSalesSameSku7d](guides/reporting/v3/columns#attributedSalesSameSku7d)	|	|
|[attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d)	|	|[newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks)	|
|[attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d)	|	|[newToBrandSalesPercentage](guides/reporting/v3/columns#newToBrandSalesPercentage)	|
|[attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d)	|[unitsSoldClicks14d](guides/reporting/v3/columns#unitsSoldClicks14d)	|	|
|[attributedUnitsOrdered14dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dOtherSKU)	|[unitsSoldOtherSku14d](guides/reporting/v3/columns#unitsSoldOtherSku14d)	| [unitsSoldBrandHaloClicks](guides/reporting/v3/columns#unitsSoldBrandHaloClicks)	|
|[attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU)	|[unitsSoldSameSku14d](guides/reporting/v3/columns#unitsSoldSameSku14d)	|	|
|[attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d)	|[unitsSoldClicks1d](guides/reporting/v3/columns#unitsSoldClicks1d)	|	|
|[attributedUnitsOrdered1dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dOtherSKU)	|[unitsSoldOtherSku1d](guides/reporting/v3/columns#unitsSoldOtherSku1d)	|	|
|[attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU)	|[unitsSoldSameSku1d](guides/reporting/v3/columns#unitsSoldSameSku1d)	|	|
|[attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d)	|[unitsSoldClicks30d](guides/reporting/v3/columns#unitsSoldClicks30d)	|	|
|[attributedUnitsOrdered30dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dOtherSKU)	|[unitsSoldOtherSku30d](guides/reporting/v3/columns#unitsSoldOtherSku30d)	|	|
|[attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU)	|[unitsSoldSameSku30d](guides/reporting/v3/columns#unitsSoldSameSku30d)	|	|
|[attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d)	|[unitsSoldClicks7d](guides/reporting/v3/columns#unitsSoldClicks7d)	|	|
|[attributedUnitsOrdered7dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dOtherSKU)	|[unitsSoldOtherSku7d](guides/reporting/v3/columns#unitsSoldOtherSku7d)	|	|
|[attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU)	|[unitsSoldSameSku7d](guides/reporting/v3/columns#unitsSoldSameSku7d)	|	|
|[attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d)	|	|[newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks)	|
|[attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d)	|	|[newToBrandUnitsSoldPercentage](guides/reporting/v3/columns#newToBrandUnitsSoldPercentage)	|
|[biddingStrategy](guides/reporting/v2/metrics#biddingStrategy)	|[campaignBiddingStrategy](guides/reporting/v3/columns#campaignBiddingStrategy)	|	|
|[campaignBudget](guides/reporting/v2/metrics#campaignBudget)	|[campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount)	|	|
|[campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget)	|[campaignRuleBasedBudgetAmount](guides/reporting/v3/columns#campaignRuleBasedBudgetAmount)	|	|
|[cost](guides/reporting/v2/metrics#cost)	|[cost](guides/reporting/v3/columns#cost), [spend](guides/reporting/v3/columns#spend)	|[cost](guides/reporting/v3/columns#cost)	|
|[cumulativeReach](guides/reporting/v2/metrics#cumulativeReach) |	|[cumulativeReach](guides/reporting/v3/columns#cumulativeReach)	|
|[currency](guides/reporting/v2/metrics#currency)	|[campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode)	|	|
|[keywordStatus](guides/reporting/v2/metrics#keywordStatus)	|[adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus)	|[adKeywordStatus](guides/reporting/v3/columns#adKeywordStatus)	|
|[keywordText](guides/reporting/v2/metrics#keywordText)	|[keyword](guides/reporting/v3/columns#keyword)	|[keywordText](guides/reporting/v3/columns#keywordText)	|
|[matchType](guides/reporting/v2/metrics#matchType)	|[keywordType](guides/reporting/v3/columns#keywordType), [matchType](guides/reporting/v3/columns#matchType)	|[matchType](guides/reporting/v3/columns#matchType)	|
|[otherAsin](guides/reporting/v2/metrics#otherAsin)	|[purchasedAsin](guides/reporting/v3/columns#purchasedAsin)	|[asinBrandHalo](guides/reporting/v3/columns#asinBrandHalo)	|
|[placement](guides/reporting/v2/metrics#placement)	|[placementClassification](guides/reporting/v3/columns#placementClassification)	|[placementClassification](guides/reporting/v3/columns#placementClassification)	|
|[query](guides/reporting/v2/metrics#query)	|[searchTerm](guides/reporting/v3/columns#searchTerm)	|[searchTerm](guides/reporting/v3/columns#searchTerm)	|
|[sku](guides/reporting/v2/metrics#sku)	|[advertisedSku](guides/reporting/v3/columns#advertisedSku)	| [promotedSku](guides/reporting/v3/columns#promotedSku)	|
|[targetId](guides/reporting/v2/metrics#targetId)	|[keywordId](guides/reporting/v3/columns#keywordId)	|[targetingId](guides/reporting/v3/columns#targetingId)	|
|[targetingExpression](guides/reporting/v2/metrics#targetingExpression)	|[keyword](guides/reporting/v3/columns#keyword)	|[targetingExpression](guides/reporting/v3/columns#targetingExpression)	|
|[targetingText](guides/reporting/v2/metrics#targetingText)	|[targeting](guides/reporting/v3/columns#targeting)	|[targetingText](guides/reporting/v3/columns#targetingText)	|
|[targetingType](guides/reporting/v2/metrics#targetingType)	| [keywordType](guides/reporting/v3/columns#keywordType), [matchType](guides/reporting/v3/columns#matchType) |[targetingType](guides/reporting/v3/columns#targetingType)	|
|[unitsSold14d](guides/reporting/v2/metrics#unitsSold14d)	|[unitsSold14d](guides/reporting/v3/columns#unitsSold14d)	|[unitsSold](guides/reporting/v3/columns#unitsSold)	|
|[vctr](guides/reporting/v2/metrics#vctr)	|	|[viewClickThroughRate](guides/reporting/v3/columns#viewClickThroughRate)	|
|[vtr](guides/reporting/v2/metrics#vtr)	|	|[viewabilityRate](guides/reporting/v3/columns#viewabilityRate)	|
| [viewAttributedUnitsOrdered14dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14dOtherSKU) | | [unitsSoldBrandHalo](guides/reporting/v3/columns#unitsSoldBrandHalo)|
| [viewAttributedSales14dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales14dOtherSKU) | | [salesBrandHalo](guides/reporting/v3/columns#salesBrandHalo) |
| [viewAttributedConversions14dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions14dOtherSKU) | | [conversionsBrandHalo](guides/reporting/v3/columns#conversionsBrandHalo) | 
| [viewImpressions](guides/reporting/v2/metrics#viewImpressions) | | [impressionsViews](guides/reporting/v3/columns#impressionsViews)| 
| [impressionsFrequencyAverage](guides/reporting/v2/metrics#impressionsFrequencyAverage) | | [avgImpressionsFrequency](guides/reporting/v3/columns#avgImpressionsFrequency) | 
| [matchedTarget](guides/reporting/v2/metrics#) | | [matchedTargetAsin](guides/reporting/v3/columns#matchedTargetAsin)|
| [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d) | | [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks) |
| [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d)| | [unitsSold](guides/reporting/v3/columns#unitsSold) | 
| [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d) | | [detailPageViews](guides/reporting/v3/columns#detailPageViews)| 
| [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d) | |[purchases](guides/reporting/v3/columns#purchases) |
| [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d) | | [sales](guides/reporting/v3/columns#sales) |
| [viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d) | |[newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases)|
| [ viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewAttributedSalesNewToBrand14d) | | [newToBrandSales](guides/reporting/v3/columns#newToBrandSales)|
| [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewAttributedUnitsOrderedNewToBrand14d) | | [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold)| 

### New columns

Version 3 also introduces new columns not available in version 2, including: [purchaseClickRate1d](guides/reporting/v3/columns#purchaseClickRate1d), [purchaseClickRate7d](guides/reporting/v3/columns#purchaseClickRate7d), [purchaseClickRate14d](guides/reporting/v3/columns#purchaseClickRate14d), [purchaseClickRate30d](guides/reporting/v3/columns#purchaseClickRate30d), [costPerClick](guides/reporting/v3/columns#costPerClick), [acos7d](guides/reporting/v3/columns#acos7d), [roas7d](guides/reporting/v3/columns#roas7d), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [impressions1y](guides/reporting/v3/columns#impressions1y), [clicks1y](guides/reporting/v3/columns#clicks1y), [spend1y](guides/reporting/v3/columns#spend1y), [costPerClick1y](guides/reporting/v3/columns#costPerClick1y), [attributionType](guides/reporting/v3/columns#attributionType), [productName](guides/reporting/v3/columns#productName), [productCategory](guides/reporting/v3/columns#productCategory), [date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startDate), [endDate](guides/reporting/v3/columns#endDate), [acosClicks7d](guides/reporting/v3/columns#acosClicks7d), [acosClicks14d](guides/reporting/v3/columns#acosClicks14d), [roasClicks7d](guides/reporting/v3/columns#roasClicks7d), [roasClicks14d](guides/reporting/v3/columns#roasClicks14d), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandUnitsSold14d](guides/reporting/v3/columns#newToBrandUnitsSold14d), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [salesClicks](guides/reporting/v3/columns#salesClicks), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewsClicks](guides/reporting/v3/columns#newToBrandDetailPageViewsClicks), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#), [newToBrandECPDetailPageView](guides/reporting/v3/columns#), [addToCart](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [eCPAddToCart](guides/reporting/v3/columns#ECPAddToCart), [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [brandedSearchRate](guides/reporting/v3/columns#brandedSearchRate), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [addToCartView](guides/reporting/v3/columns#addToCartView), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks).


>[NOTE] Version 3 does not support the [bidPlus](guides/reporting/v2/metrics#bidPlus) metric, as this functionality was deprecated in 2019.

### Expanded reporting period

- Many report types now support a 95 day reporting period (up from 60 days in version 2).
- You can choose from daily or summary reporting.
- Reports can contain multiple days worth of data. The number of days is determined by the report type. See [Report types](reporting/v3/report-types) for details. 

### Dimensional reporting data

[Some report types in version 2](guides/reporting/v2/faq#can-i-filter-a-sponsored-brands-report) contain all historical campaign entities, regardless of whether performance data is available for a particular entity on the requested day. In version 3 reporting, reports only contain records with performance activity (clicks, impressions, attributed purchases, etc.) during the specified time period. 

### Standard 14-day attribution window for Sponsored Brands and Sponsored Display

In version 3 reporting, Sponsored Display and Sponsored Brands adopt a standard 14-day attribution window for all applicable metrics. This release deprecated metrics with 1, 7, and 28 day attribution windows and drops suffixes in metric names, making it easier to read and analyze reporting information. For more details, see the [Metric comparison table](reference/migration-guides/reporting-v2-v3#metriccolumn-comparison).

### View-based metrics for Sponsored Brands

Version 3 reporting supports view-based metrics for Sponsored Brands campaigns. Columns that don't end with `Clicks` generally include data attributed to both clicks and views. Columns that end with `Clicks` are only attributed to ad clicks. For full descriptions for each column, see [Columns](guides/reporting/v3/columns). 

### Sponsored Display tactic

In version 2 reporting, you needed to specify the `tactic` as part of the report request, and the resulting report included only campaign data for that tactic. In version 3 reporting, you no longer need to specify tactic when requesting a report. Data from all Sponsored Display tactic types are returned in version 3 reports. If you want to check which tactic is associated to reporting data, you should call [GET /sd/campaigns](sponsored-display/3-0/openapi#tag/Campaigns/operation/listCampaigns) to retrieve the `tactic` for a given campaign, then join with the reporting data on `campaignId`.

### Resolved Sponsored Products data quality issues

Version 2 contained two known issues around Sponsored Products data quality.

1. A small number of Sponsored Products reports contained Sponsored Display performance data.
2. A small number of Sponsored Products reports did not include complete campaign data.

These issues have been fixed in the version 3 reporting API.

