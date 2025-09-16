---
title: Campaign reports
description: Learn about requesting campaign reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Campaign reports

Campaign reports contain performance data broken down at the campaign level. Campaign reports include all campaigns of the requested sponsored ad type that have performance activity for the requested days. For example, a Sponsored Products campaign report returns performance data for all Sponsored Products campaigns that received impressions on the chosen dates. Campaign reports can also be grouped by ad group and placement for more granular data. 

>[NOTE] You can only use a filter that is supported by **all** `groupBy` values included in a report configuration. For campaign reports, this means that filters are only supported when you include a single `groupBy` value. 

## Configurations

| Configuration | Sponsored Products | Sponsored Brands | Sponsored Display | Sponsored Television | Amazon DSP | 
|----------|---------|---|------|-----|-----|
| `reportTypeId` | `spCampaigns` | `sbCampaigns` | `sdCampaigns`| `stCampaigns` | `dspCampaign` | 
| Maximum date range | 31 days | 31 days | 31 days | 31 days | 31 days | 
| Data retention | 95 days | 60 days | 65 days | 95 days | 95 days | 
| `timeUnit` | `SUMMARY`, or `DAILY` |`SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` | `SUMMARY` or `DAILY` | 
| `groupBy` | `campaign`, `adGroup`, or `campaignPlacement` |`campaign`| `campaign` or `matchedTarget` | `campaign` or `adGroup` | `campaign`, `ad`, or `creative` | 
| `format` | `GZIP_JSON` |`GZIP_JSON`| `GZIP_JSON`| `GZIP_JSON` | `GZIP_JSON` or `CSV` | 

## Sponsored Products

### Base metrics

[impressions](guides/reporting/v3/columns#impressions), [addToList](guides/reporting/v3/columns#addToList), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [purchases1d](guides/reporting/v3/columns#purchases1d), [purchases7d](guides/reporting/v3/columns#purchases7d), [purchases14d](guides/reporting/v3/columns#purchases14d), [purchases30d](guides/reporting/v3/columns#purchases30d), [purchasesSameSku1d](guides/reporting/v3/columns#purchasesSameSku1d), [purchasesSameSku7d](guides/reporting/v3/columns#purchasesSameSku7d), [purchasesSameSku14d](guides/reporting/v3/columns#purchasesSameSku14d), [purchasesSameSku30d](guides/reporting/v3/columns#purchasesSameSku30d), [unitsSoldClicks1d](guides/reporting/v3/columns#unitsSoldClicks1d), [unitsSoldClicks7d](guides/reporting/v3/columns#unitsSoldClicks7d), [unitsSoldClicks14d](guides/reporting/v3/columns#unitsSoldClicks14d), [unitsSoldClicks30d](guides/reporting/v3/columns#unitsSoldClicks30d), [sales1d](guides/reporting/v3/columns#sales1d), [sales7d](guides/reporting/v3/columns#sales7d), [sales14d](guides/reporting/v3/columns#sales14d), [sales30d](guides/reporting/v3/columns#sales30d), [attributedSalesSameSku1d](guides/reporting/v3/columns#attributedSalesSameSku1d), [attributedSalesSameSku7d](guides/reporting/v3/columns#attributedSalesSameSku7d), [attributedSalesSameSku14d](guides/reporting/v3/columns#attributedSalesSameSku14d), [attributedSalesSameSku30d](guides/reporting/v3/columns#attributedSalesSameSku30d), [unitsSoldSameSku1d](guides/reporting/v3/columns#unitsSoldSameSku1d), [unitsSoldSameSku7d](guides/reporting/v3/columns#unitsSoldSameSku7d), [unitsSoldSameSku14d](guides/reporting/v3/columns#unitsSoldSameSku14d), [unitsSoldSameSku30d](guides/reporting/v3/columns#unitsSoldSameSku30d), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startDate), [endDate](guides/reporting/v3/columns#endDate), [campaignBiddingStrategy](guides/reporting/v3/columns#campaignBiddingStrategy), [costPerClick](guides/reporting/v3/columns#costPerClick), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [spend](guides/reporting/v3/columns#spend)

### Group by `campaign`

Additional metrics: [campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignRuleBasedBudgetAmount](guides/reporting/v3/columns#campaignRuleBasedBudgetAmount), [campaignApplicableBudgetRuleId](guides/reporting/v3/columns#campaignApplicableBudgetRuleId), [campaignApplicableBudgetRuleName](guides/reporting/v3/columns#campaignApplicableBudgetRuleName), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [topOfSearchImpressionShare](guides/reporting/v3/columns#topOfSearchImpressionShare)

Filters: 

- `campaignStatus` (values: ENABLED, PAUSED, ARCHIVED)

### Group by `adGroup`

Additional metrics: [adGroupName](guides/reporting/v3/columns#adGroupName), [adGroupId](guides/reporting/v3/columns#adGroupId), [adStatus](guides/reporting/v3/columns#adStatus)

Filters: 

- `adStatus` (values: ENABLED, PAUSED, ARCHIVED)

### Group by `campaignPlacement`

Additional metrics: [placementClassification](guides/reporting/v3/columns#placementClassification),[campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignRuleBasedBudgetAmount](guides/reporting/v3/columns#campaignRuleBasedBudgetAmount), [campaignApplicableBudgetRuleId](guides/reporting/v3/columns#campaignApplicableBudgetRuleId), [campaignApplicableBudgetRuleName](guides/reporting/v3/columns#campaignApplicableBudgetRuleName), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [topOfSearchImpressionShare](guides/reporting/v3/columns#topOfSearchImpressionShare)

Filters:

- `campaignSite` (values: AmazonBusiness)

>[NOTE] Amazon Business performance data is available starting 9/5/2024 onwards only.<br><br>The Amazon Business Bid Adjustment and Reporting for Sponsored Products will be coming soon to Bulksheets.<br><br>Other groupBy parameters apart from campaignPlacement are not supported. [LINK](guides/reporting/v3/get-started#groupby)

## Sponsored Brands

>[NOTE] This report currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [costType](guides/reporting/v3/columns#costType), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [kindleEditionNormalizedPagesRead14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRead14d), [kindleEditionNormalizedPagesRoyalties14d](guides/reporting/v3/columns#kindleEditionNormalizedPagesRoyalties14d), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewsClicks](guides/reporting/v3/columns#newToBrandDetailPageViewsClicks), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchasesPercentage](guides/reporting/v3/columns#newToBrandPurchasesPercentage), [newToBrandPurchasesRate](guides/reporting/v3/columns#newToBrandPurchasesRate), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandSalesPercentage](guides/reporting/v3/columns#newToBrandSalesPercentage), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [newToBrandUnitsSoldPercentage](guides/reporting/v3/columns#newToBrandUnitsSoldPercentage), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromoted](guides/reporting/v3/columns#purchasesPromoted), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromoted](guides/reporting/v3/columns#salesPromoted), [startDate](guides/reporting/v3/columns#startDate), [topOfSearchImpressionShare](guides/reporting/v3/columns#topOfSearchImpressionShare), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [video5SecondViewRate](guides/reporting/v3/columns#video5SecondViewRate), [video5SecondViews](guides/reporting/v3/columns#video5SecondViews), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v3/columns#videoUnmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate), [viewableImpressions](guides/reporting/v3/columns#viewableImpressions), [viewClickThroughRate](guides/reporting/v3/columns#viewClickThroughRate)

### Group by `campaign`

Additional metrics: [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [longTermSales](guides/reporting/v3/columns#longTermSales), [longTermROAS](guides/reporting/v3/columns#longTermROAS),  [topOfSearchImpressionShare](guides/reporting/v3/columns#topOfSearchImpressionShare)

Filters: 

- `campaignStatus` (values: ENABLED, PAUSED, ARCHIVED)

## Sponsored Display

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [addToList](guides/reporting/v3/columns#addToList), [addToListFromClicks](guides/reporting/v3/columns#addToListFromClicks), [addToListFromViews](guides/reporting/v3/columns#addToListFromViews), [qualifiedBorrows](guides/reporting/v3/columns#qualifiedBorrows), [qualifiedBorrowsFromClicks](guides/reporting/v3/columns#qualifiedBorrowsFromClicks), [qualifiedBorrowsFromViews](guides/reporting/v3/columns#qualifiedBorrowsFromViews), [royaltyQualifiedBorrows](guides/reporting/v3/columns#royaltyQualifiedBorrows), [royaltyQualifiedBorrowsFromClicks](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromClicks), [royaltyQualifiedBorrowsFromViews](guides/reporting/v3/columns#royaltyQualifiedBorrowsFromViews), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [brandedSearchRate](guides/reporting/v3/columns#brandedSearchRate), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [campaignId](guides/reporting/v3/columns#campaignId), [campaignName](guides/reporting/v3/columns#campaignName), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [endDate](guides/reporting/v3/columns#endDate), [impressions](guides/reporting/v3/columns#impressions), [impressionsViews](guides/reporting/v3/columns#impressionsViews), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandUnitsSoldClicks](guides/reporting/v3/columns#newToBrandUnitsSoldClicks), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesPromotedClicks](guides/reporting/v3/columns#purchasesPromotedClicks), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesPromotedClicks](guides/reporting/v3/columns#salesPromotedClicks), [startDate](guides/reporting/v3/columns#startDate), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v3/columns#videoUnmutes), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate), [viewClickThroughRate](guides/reporting/v3/columns#viewClickThroughRate)

### Group by `campaign`

Additional metrics: [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [costType](guides/reporting/v3/columns#costType), [cumulativeReach](guides/reporting/v3/columns#cumulativeReach), [impressionsFrequencyAverage](guides/reporting/v3/columns#impressionsFrequencyAverage), [longTermSales](guides/reporting/v3/columns#longTermSales), [longTermROAS](guides/reporting/v3/columns#longTermROAS), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales)

Filters: N/A

### Group by `matchedTarget`

Additional metrics: [matchedTargetAsin](guides/reporting/v3/columns#matchedTargetAsin)

Filters: N/A

## Sponsored Television

### Base metrics

[addToCart](guides/reporting/v3/columns#addToCart), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [brandedSearches](guides/reporting/v3/columns#brandedSearches), [brandedSearchesClicks](guides/reporting/v3/columns#brandedSearchesClicks), [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [clicks](guides/reporting/v3/columns#clicks), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [cost](guides/reporting/v3/columns#cost), [costPerThousandImpressions](guides/reporting/v3/columns#costPerThousandImpressions), [date](guides/reporting/v3/columns#date), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewsClicks](guides/reporting/v3/columns#detailPageViewsClicks), [detailPageViewsViews](guides/reporting/v3/columns#detailPageViewsViews), [endDate](guides/reporting/v3/columns#endDate), [frequencyAverage](guides/reporting/v3/columns#frequencyAverage), [householdFrequencyAverage](guides/reporting/v3/columns#householdFrequencyAverage), [householdReach](guides/reporting/v3/columns#householdReach), [impressions](guides/reporting/v3/columns#impressions), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchasesViews](guides/reporting/v3/columns#newToBrandPurchasesViews), [newToBrandSales](guides/reporting/v3/columns#newToBrandSales), [newToBrandSalesClicks](guides/reporting/v3/columns#newToBrandSalesClicks), [newToBrandSalesViews](guides/reporting/v3/columns#newToBrandSalesViews), [portfolioId](guides/reporting/v3/columns#portfolioId), [purchases](guides/reporting/v3/columns#purchases), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchasesViews](guides/reporting/v3/columns#purchasesViews), [reach](guides/reporting/v3/columns#reach), [roas](guides/reporting/v3/columns#roas), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesClicks), [salesViews](guides/reporting/v3/columns#salesViews), [startDate](guides/reporting/v3/columns#startDate), [unitsSold](guides/reporting/v3/columns#unitsSold), [unitsSoldClicks](guides/reporting/v3/columns#unitsSoldClicks), [unitsSoldViews](guides/reporting/v3/columns#unitsSoldViews), [videoCompleteViews](guides/reporting/v3/columns#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v3/columns#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v3/columns#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v3/columns#videoThirdQuartileViews)

### Group by `campaign`

Additional metrics: [campaignName](guides/reporting/v3/columns#campaignName), [campaignId](guides/reporting/v3/columns#campaignId), [campaignStatus](guides/reporting/v3/columns#campaignStatus), [campaignBudgetAmount](guides/reporting/v3/columns#campaignBudgetAmount), [campaignBudgetType](guides/reporting/v3/columns#campaignBudgetType), [campaignBudgetCurrencyCode](guides/reporting/v3/columns#campaignBudgetCurrencyCode), [longTermSales](guides/reporting/v3/columns#longTermSales), [longTermROAS](guides/reporting/v3/columns#longTermROAS)

Filters: 

- `campaignStatus` (values: ENABLED, PAUSED, ARCHIVED)

### Group by `adGroup`

Additional metrics: [adGroupName](guides/reporting/v3/columns#adGroupName), [adGroupId](guides/reporting/v3/columns#adGroupId), [adStatus](guides/reporting/v3/columns#adStatus)

Filters: 

- `adStatus` (values: ENABLED, PAUSED, ARCHIVED)

## Amazon DSP

### Base metrics

[date](guides/reporting/v3/columns#date), [intervalStart](guides/reporting/v3/columns#intervalStart), [intervalEnd](guides/reporting/v3/columns#intervalEnd), [totalCost](guides/reporting/v3/columns#totalCost), [supplyCost](guides/reporting/v3/columns#supplyCost), [amazonDSPAudienceFee](guides/reporting/v3/columns#amazonDSPAudienceFee), [advertiserTimezone](guides/reporting/v3/columns#advertiserTimezone), [advertiserCountry](guides/reporting/v3/columns#advertiserCountry), [impressions](guides/reporting/v3/columns#impressions), [clicks](guides/reporting/v3/columns#clicks), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [eCPM](guides/reporting/v3/columns#eCPM), [eCPC](guides/reporting/v3/columns#eCPC), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewViews](guides/reporting/v3/columns#detailPageViewViews), [detailPageViewClicks](guides/reporting/v3/columns#detailPageViewClicks), [detailPageViewRate](guides/reporting/v3/columns#detailPageViewRate), [eCPDetailPageView](guides/reporting/v3/columns#eCPDetailPageView), [productReviewPageVisits](guides/reporting/v3/columns#productReviewPageVisits), [productReviewPageVisitsViews](guides/reporting/v3/columns#productReviewPageVisitsViews), [productReviewPageVisitsClicks](guides/reporting/v3/columns#productReviewPageVisitsClicks), [productReviewPageVisitRate](guides/reporting/v3/columns#productReviewPageVisitRate), [eCPProductReviewPageVisit](guides/reporting/v3/columns#eCPProductReviewPageVisit), [addToList](guides/reporting/v3/columns#addToList), [addToListViews](guides/reporting/v3/columns#addToListViews), [addToListClicks](guides/reporting/v3/columns#addToListClicks), [addToListRate](guides/reporting/v3/columns#addToListRate), [eCPAddToList](guides/reporting/v3/columns#eCPAddToList), [addToCart](guides/reporting/v3/columns#addToCart), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [purchases](guides/reporting/v3/columns#purchases), [purchasesViews](guides/reporting/v3/columns#purchasesViews), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchaseRate](guides/reporting/v3/columns#purchaseRate), [eCPP](guides/reporting/v3/columns#eCPP), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesViews](guides/reporting/v3/columns#newToBrandPurchasesViews), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchaseRate](guides/reporting/v3/columns#newToBrandPurchaseRate), [NewToBrandECPP](guides/reporting/v3/columns#NewToBrandECPP), [addToWatchlist](guides/reporting/v3/columns#addToWatchlist), [addToWatchlistViews](guides/reporting/v3/columns#addToWatchlistViews), [addToWatchlistClicks](guides/reporting/v3/columns#addToWatchlistClicks), [addToWatchlistRate](guides/reporting/v3/columns#addToWatchlistRate), [eCPAddToWatchlist](guides/reporting/v3/columns#eCPAddToWatchlist), [downloadedVideoPlays](guides/reporting/v3/columns#downloadedVideoPlays), [downloadedVideoPlaysViews](guides/reporting/v3/columns#downloadedVideoPlaysViews), [downloadedVideoPlaysClicks](guides/reporting/v3/columns#downloadedVideoPlaysClicks), [downloadedVideoPlayRate](guides/reporting/v3/columns#downloadedVideoPlayRate), [eCPDownloadedVideoPlays](guides/reporting/v3/columns#eCPDownloadedVideoPlays), [videoStreams](guides/reporting/v3/columns#videoStreams), [videoStreamsViews](guides/reporting/v3/columns#videoStreamsViews), [videoStreamsClicks](guides/reporting/v3/columns#videoStreamsClicks), [videoStreamsRate](guides/reporting/v3/columns#videoStreamsRate), [eCPVideoStream](guides/reporting/v3/columns#eCPVideoStream), [playTrailers](guides/reporting/v3/columns#playTrailers), [playTrailersViews](guides/reporting/v3/columns#playTrailersViews), [playTrailersClicks](guides/reporting/v3/columns#playTrailersClicks), [playTrailerRate](guides/reporting/v3/columns#playTrailerRate), [eCPPlayTrailer](guides/reporting/v3/columns#eCPPlayTrailer), [rentals](guides/reporting/v3/columns#rentals), [rentalsViews](guides/reporting/v3/columns#rentalsViews), [rentalsClicks](guides/reporting/v3/columns#rentalsClicks), [rentalRate](guides/reporting/v3/columns#rentalRate), [eCPRental](guides/reporting/v3/columns#eCPRental), [videoDownloads](guides/reporting/v3/columns#videoDownloads), [videoDownloadsViews](guides/reporting/v3/columns#videoDownloadsViews), [videoDownloadsClicks](guides/reporting/v3/columns#videoDownloadsClicks), [videoDownloadRate](guides/reporting/v3/columns#videoDownloadRate), [eCPVideoDownload](guides/reporting/v3/columns#eCPVideoDownload), [newSubscribeAndSave](guides/reporting/v3/columns#newSubscribeAndSave), [newSubscribeAndSaveViews](guides/reporting/v3/columns#newSubscribeAndSaveViews), [newSubscribeAndSaveClicks](guides/reporting/v3/columns#newSubscribeAndSaveClicks), [newSubscribeAndSaveRate](guides/reporting/v3/columns#newSubscribeAndSaveRate), [eCPnewSubscribeAndSave](guides/reporting/v3/columns#eCPnewSubscribeAndSave), [subscribe](guides/reporting/v3/columns#subscribe), [subscribeViews](guides/reporting/v3/columns#subscribeViews), [subscribeClicks](guides/reporting/v3/columns#subscribeClicks), [subscribeCVR](guides/reporting/v3/columns#subscribeCVR), [subscribeCPA](guides/reporting/v3/columns#subscribeCPA), [application](guides/reporting/v3/columns#application), [applicationViews](guides/reporting/v3/columns#applicationViews), [applicationClicks](guides/reporting/v3/columns#applicationClicks), [applicationCVR](guides/reporting/v3/columns#applicationCVR), [applicationCPA](guides/reporting/v3/columns#applicationCPA), [mobileAppFirstStarts](guides/reporting/v3/columns#mobileAppFirstStarts), [mobileAppFirstStartViews](guides/reporting/v3/columns#mobileAppFirstStartViews), [mobileAppFirstStartClicks](guides/reporting/v3/columns#mobileAppFirstStartClicks), [mobileAppFirstStartCVR](guides/reporting/v3/columns#mobileAppFirstStartCVR), [mobileAppFirstStartCPA](guides/reporting/v3/columns#mobileAppFirstStartCPA), [addToShoppingCart](guides/reporting/v3/columns#addToShoppingCart), [addToShoppingCartViews](guides/reporting/v3/columns#addToShoppingCartViews), [addToShoppingCartClicks](guides/reporting/v3/columns#addToShoppingCartClicks), [addToShoppingCartCVR](guides/reporting/v3/columns#addToShoppingCartCVR), [addToShoppingCartCPA](guides/reporting/v3/columns#addToShoppingCartCPA), [videoAdStart](guides/reporting/v3/columns#videoAdStart), [videoAdFirstQuartile](guides/reporting/v3/columns#videoAdFirstQuartile), [videoAdMidpoint](guides/reporting/v3/columns#videoAdMidpoint), [videoAdThirdQuartile](guides/reporting/v3/columns#videoAdThirdQuartile), [videoAdComplete](guides/reporting/v3/columns#videoAdComplete), [videoAdCompletionRate](guides/reporting/v3/columns#videoAdCompletionRate), [eCPVideoAdCompletion](guides/reporting/v3/columns#eCPVideoAdCompletion), [videoAdPause](guides/reporting/v3/columns#videoAdPause), [videoAdResume](guides/reporting/v3/columns#videoAdResume), [videoAdMute](guides/reporting/v3/columns#videoAdMute), [videoAdUnmute](guides/reporting/v3/columns#videoAdUnmute), [otherClicks](guides/reporting/v3/columns#otherClicks), [other](guides/reporting/v3/columns#other), [otherCPA](guides/reporting/v3/columns#otherCPA), [otherCVR](guides/reporting/v3/columns#otherCVR), [otherViews](guides/reporting/v3/columns#otherViews), [pageViewClicks](guides/reporting/v3/columns#pageViewClicks), [pageViews](guides/reporting/v3/columns#pageViews), [pageViewCPA](guides/reporting/v3/columns#pageViewCPA), [pageViewCVR](guides/reporting/v3/columns#pageViewCVR), [pageViewViews](guides/reporting/v3/columns#pageViewViews), [searchClicks](guides/reporting/v3/columns#searchClicks), [search](guides/reporting/v3/columns#search), [searchCPA](guides/reporting/v3/columns#searchCPA), [searchCVR](guides/reporting/v3/columns#searchCVR), [searchViews](guides/reporting/v3/columns#searchViews), [contactClicks](guides/reporting/v3/columns#contactClicks), [contact](guides/reporting/v3/columns#contact), [contactCPA](guides/reporting/v3/columns#contactCPA), [contactCVR](guides/reporting/v3/columns#contactCVR), [contactViews](guides/reporting/v3/columns#contactViews), [checkoutClicks](guides/reporting/v3/columns#checkoutClicks), [checkout](guides/reporting/v3/columns#checkout), [checkoutCPA](guides/reporting/v3/columns#checkoutCPA), [checkoutCVR](guides/reporting/v3/columns#checkoutCVR), [checkoutViews](guides/reporting/v3/columns#checkoutViews), [leadClicks](guides/reporting/v3/columns#leadClicks), [lead](guides/reporting/v3/columns#lead), [leadCPA](guides/reporting/v3/columns#leadCPA), [leadCVR](guides/reporting/v3/columns#leadCVR), [leadViews](guides/reporting/v3/columns#leadViews), [advertiserId](guides/reporting/v3/columns#advertiserId), [advertiserName](guides/reporting/v3/columns#advertiserName), [entityId](guides/reporting/v3/columns#entityId), [agencyFee](guides/reporting/v3/columns#agencyFee), [totalFee](guides/reporting/v3/columns#totalFee), [3pFeeAutomotive](guides/reporting/v3/columns#3pFeeAutomotive), [3pFeeAutomotiveAbsorbed](guides/reporting/v3/columns#3pFeeAutomotiveAbsorbed), [3pFeeComScore](guides/reporting/v3/columns#3pFeeComScore), [3pFeeComScoreAbsorbed](guides/reporting/v3/columns#3pFeeComScoreAbsorbed), [3pFeeCPM1](guides/reporting/v3/columns#3pFeeCPM1), [3pFeeCPM1Absorbed](guides/reporting/v3/columns#3pFeeCPM1Absorbed), [3pFeeCPM2](guides/reporting/v3/columns#3pFeeCPM2), [3pFeeCPM2Absorbed](guides/reporting/v3/columns#3pFeeCPM2Absorbed), [3pFeeCPM3](guides/reporting/v3/columns#3pFeeCPM3), [3pFeeCPM3Absorbed](guides/reporting/v3/columns#3pFeeCPM3Absorbed), [3pFeeDoubleclickCampaignManager](guides/reporting/v3/columns#3pFeeDoubleclickCampaignManager), [3pFeeDoubleclickCampaignManagerAbsorbed](guides/reporting/v3/columns#3pFeeDoubleclickCampaignManagerAbsorbed), [3pFeeDoubleVerify](guides/reporting/v3/columns#3pFeeDoubleVerify), [3pFeeIntegralAdScience](guides/reporting/v3/columns#3pFeeIntegralAdScience), [3pFeeIntegralAdScienceAbsorbed](guides/reporting/v3/columns#3pFeeIntegralAdScienceAbsorbed), [3PFees](guides/reporting/v3/columns#3PFees), [unitsSold](guides/reporting/v3/columns#unitsSold), [sales](guides/reporting/v3/columns#sales), [ROAS](guides/reporting/v3/columns#ROAS), [eRPM](guides/reporting/v3/columns#eRPM), [newToBrandUnitsSold](guides/reporting/v3/columns#newToBrandUnitsSold), [newToBrandProductSales](guides/reporting/v3/columns#newToBrandProductSales), [newToBrandROAS](guides/reporting/v3/columns#newToBrandROAS), [newToBrandERPM](guides/reporting/v3/columns#newToBrandERPM), [totalProductReviewPageVisits](guides/reporting/v3/columns#totalProductReviewPageVisits), [totalProductReviewPageVisitsViews](guides/reporting/v3/columns#totalProductReviewPageVisitsViews), [totalProductReviewPageVisitsClicks](guides/reporting/v3/columns#totalProductReviewPageVisitsClicks), [totalProductReviewPageVisitRate](guides/reporting/v3/columns#totalProductReviewPageVisitRate), [totalECPProductReviewPageVisit](guides/reporting/v3/columns#totalECPProductReviewPageVisit), [totalPurchases](guides/reporting/v3/columns#totalPurchases), [totalPurchasesViews](guides/reporting/v3/columns#totalPurchasesViews), [totalPurchasesClicks](guides/reporting/v3/columns#totalPurchasesClicks), [totalPurchaseRate](guides/reporting/v3/columns#totalPurchaseRate), [totalECPP](guides/reporting/v3/columns#totalECPP), [totalNewToBrandPurchases](guides/reporting/v3/columns#totalNewToBrandPurchases), [totalNewToBrandPurchasesViews](guides/reporting/v3/columns#totalNewToBrandPurchasesViews), [totalNewToBrandPurchasesClicks](guides/reporting/v3/columns#totalNewToBrandPurchasesClicks), [totalNewToBrandPurchaseRate](guides/reporting/v3/columns#totalNewToBrandPurchaseRate), [totalNewToBrandECPP](guides/reporting/v3/columns#totalNewToBrandECPP), [totalROAS](guides/reporting/v3/columns#totalROAS), [totalERPM](guides/reporting/v3/columns#totalERPM), [totalNewToBrandUnitsSold](guides/reporting/v3/columns#totalNewToBrandUnitsSold), [totalNewToBrandSales](guides/reporting/v3/columns#totalNewToBrandSales), [totalNewToBrandROAS](guides/reporting/v3/columns#totalNewToBrandROAS), [totalNewToBrandERPM](guides/reporting/v3/columns#totalNewToBrandERPM), [viewableImpressions](guides/reporting/v3/columns#viewableImpressions), [measurableImpressions](guides/reporting/v3/columns#measurableImpressions), [measurableRate](guides/reporting/v3/columns#measurableRate), [viewabilityRate](guides/reporting/v3/columns#viewabilityRate), [totalDetailPageView](guides/reporting/v3/columns#totalDetailPageView), [totalDetailPageViewViews](guides/reporting/v3/columns#totalDetailPageViewViews), [totalDetailPageViewClicks](guides/reporting/v3/columns#totalDetailPageViewClicks), [totalDetailPageViewRate](guides/reporting/v3/columns#totalDetailPageViewRate), [totalECPDetailPageView](guides/reporting/v3/columns#totalECPDetailPageView), [totalAddToList](guides/reporting/v3/columns#totalAddToList), [totalAddToListViews](guides/reporting/v3/columns#totalAddToListViews), [totalAddToListClicks](guides/reporting/v3/columns#totalAddToListClicks), [totalAddToListRate](guides/reporting/v3/columns#totalAddToListRate), [totalECPAddToList](guides/reporting/v3/columns#totalECPAddToList), [totalAddToCart](guides/reporting/v3/columns#totalAddToCart), [totalAddToCartViews](guides/reporting/v3/columns#totalAddToCartViews), [totalAddToCartClicks](guides/reporting/v3/columns#totalAddToCartClicks), [totalAddToCartRate](guides/reporting/v3/columns#totalAddToCartRate), [totalECPAddToCart](guides/reporting/v3/columns#totalECPAddToCart), [totalSubscribeAndSave](guides/reporting/v3/columns#totalSubscribeAndSave), [totalSubscribeAndSaveViews](guides/reporting/v3/columns#totalSubscribeAndSaveViews), [totalSubscribeAndSaveClicks](guides/reporting/v3/columns#totalSubscribeAndSaveClicks), [totalSubscribeAndSaveRate](guides/reporting/v3/columns#totalSubscribeAndSaveRate), [totalECPSubscribeAndSave](guides/reporting/v3/columns#totalECPSubscribeAndSave), [offAmazonPurchases](guides/reporting/v3/columns#offAmazonPurchases), [offAmazonPurchasesViews](guides/reporting/v3/columns#offAmazonPurchasesViews), [offAmazonPurchasesClicks](guides/reporting/v3/columns#offAmazonPurchasesClicks), [offAmazonConversions](guides/reporting/v3/columns#offAmazonConversions), [offAmazonViews](guides/reporting/v3/columns#offAmazonViews), [offAmazonClicks](guides/reporting/v3/columns#offAmazonClicks), [offAmazonPurchaseRate](guides/reporting/v3/columns#offAmazonPurchaseRate), [offAmazonECPP](guides/reporting/v3/columns#offAmazonECPP), [offAmazonCVR](guides/reporting/v3/columns#offAmazonCVR), [offAmazonCPA](guides/reporting/v3/columns#offAmazonCPA), [offAmazonProductSales](guides/reporting/v3/columns#offAmazonProductSales), [offAmazonUnitsSold](guides/reporting/v3/columns#offAmazonUnitsSold), [offAmazonROAS](guides/reporting/v3/columns#offAmazonROAS), [offAmazonERPM](guides/reporting/v3/columns#offAmazonERPM), [signUp](guides/reporting/v3/columns#signUp), [signUpViews](guides/reporting/v3/columns#signUpViews), [signUpClicks](guides/reporting/v3/columns#signUpClicks), [signUpCVR](guides/reporting/v3/columns#signUpCVR), [signUpCPA](guides/reporting/v3/columns#signUpCPA), [combinedPurchases](guides/reporting/v3/columns#combinedPurchases), [combinedPurchasesViews](guides/reporting/v3/columns#combinedPurchasesViews), [combinedPurchasesClicks](guides/reporting/v3/columns#combinedPurchasesClicks), [combinedPurchaseRate](guides/reporting/v3/columns#combinedPurchaseRate), [combinedECPP](guides/reporting/v3/columns#combinedECPP), [combinedProductSales](guides/reporting/v3/columns#combinedProductSales), [combinedUnitsSold](guides/reporting/v3/columns#combinedUnitsSold), [combinedERPM](guides/reporting/v3/columns#combinedERPM), [combinedROAS](guides/reporting/v3/columns#combinedROAS), [brandSearches](guides/reporting/v3/columns#brandSearches), [brandSearchesViews](guides/reporting/v3/columns#brandSearchesViews), [brandSearchesClicks](guides/reporting/v3/columns#brandSearchesClicks), [brandSearchRate](guides/reporting/v3/columns#brandSearchRate), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [grossImpressions](guides/reporting/v3/columns#grossImpressions), [grossClickThroughs](guides/reporting/v3/columns#grossClickThroughs), [invalidImpressions](guides/reporting/v3/columns#invalidImpressions), [invalidClickThroughs](guides/reporting/v3/columns#invalidClickThroughs), [invalidImpressionRate](guides/reporting/v3/columns#invalidImpressionRate), [invalidClickThroughRate](guides/reporting/v3/columns#invalidClickThroughRate), [3pFeeDoubleVerifyAbsorbed](guides/reporting/v3/columns#3pFeeDoubleVerifyAbsorbed), [impressionFrequencyAverage](guides/reporting/v3/columns#impressionFrequencyAverage), [alexaSkillEnable](guides/reporting/v3/columns#alexaSkillEnable), [eCPAlexaSkillEnable](guides/reporting/v3/columns#eCPAlexaSkillEnable), [alexaSkillEnableClicks](guides/reporting/v3/columns#alexaSkillEnableClicks), [alexaSkillEnableRate](guides/reporting/v3/columns#alexaSkillEnableRate), [alexaSkillEnableViews](guides/reporting/v3/columns#alexaSkillEnableViews), [appStoreFirstOpens](guides/reporting/v3/columns#appStoreFirstOpens), [eCPAppStoreFirstOpens](guides/reporting/v3/columns#eCPAppStoreFirstOpens), [appStoreFirstOpensClicks](guides/reporting/v3/columns#appStoreFirstOpensClicks), [appStoreFirstOpensRate](guides/reporting/v3/columns#appStoreFirstOpensRate), [appStoreFirstOpensViews](guides/reporting/v3/columns#appStoreFirstOpensViews), [appStoreFirstSessionHours](guides/reporting/v3/columns#appStoreFirstSessionHours), [appStoreFirstSessionHoursClicks](guides/reporting/v3/columns#appStoreFirstSessionHoursClicks), [appStoreFirstSessionHoursViews](guides/reporting/v3/columns#appStoreFirstSessionHoursViews), [appStoreOpens](guides/reporting/v3/columns#appStoreOpens), [eCPAppStoreOpens](guides/reporting/v3/columns#eCPAppStoreOpens), [appStoreOpensClicks](guides/reporting/v3/columns#appStoreOpensClicks), [appStoreOpensRate](guides/reporting/v3/columns#appStoreOpensRate), [appStoreOpensViews](guides/reporting/v3/columns#appStoreOpensViews), [appStoreUsageHours](guides/reporting/v3/columns#appStoreUsageHours), [appStoreUsageHoursClicks](guides/reporting/v3/columns#appStoreUsageHoursClicks), [appStoreUsageHoursViews](guides/reporting/v3/columns#appStoreUsageHoursViews), [audioAdCompletions](guides/reporting/v3/columns#audioAdCompletions), [audioAdCompletionRate](guides/reporting/v3/columns#audioAdCompletionRate), [audioAdViews](guides/reporting/v3/columns#audioAdViews), [audioAdFirstQuartile](guides/reporting/v3/columns#audioAdFirstQuartile), [audioAdMidpoint](guides/reporting/v3/columns#audioAdMidpoint), [audioAdMute](guides/reporting/v3/columns#audioAdMute), [audioAdPause](guides/reporting/v3/columns#audioAdPause), [audioAdProgression](guides/reporting/v3/columns#audioAdProgression), [audioAdResume](guides/reporting/v3/columns#audioAdResume), [audioAdRewind](guides/reporting/v3/columns#audioAdRewind), [audioAdSkip](guides/reporting/v3/columns#audioAdSkip), [audioAdStart](guides/reporting/v3/columns#audioAdStart), [audioAdThirdQuartile](guides/reporting/v3/columns#audioAdThirdQuartile), [audioAdUnmute](guides/reporting/v3/columns#audioAdUnmute), [eCPFreeTrialSubscriptionSignup](guides/reporting/v3/columns#eCPFreeTrialSubscriptionSignup), [eCPPaidSubscriptionSignup](guides/reporting/v3/columns#eCPPaidSubscriptionSignup), [eCPSubscriptionSignup](guides/reporting/v3/columns#eCPSubscriptionSignup), [eCPAudioAdCompletion](guides/reporting/v3/columns#eCPAudioAdCompletion), [freeTrialSubscriptionSignups](guides/reporting/v3/columns#freeTrialSubscriptionSignups), [freeTrialSubscriptionSignupClicks](guides/reporting/v3/columns#freeTrialSubscriptionSignupClicks), [freeTrialSubscriptionSignupViews](guides/reporting/v3/columns#freeTrialSubscriptionSignupViews), [freeTrialSubscriptionSignupRate](guides/reporting/v3/columns#freeTrialSubscriptionSignupRate), [paidSubscriptionSignups](guides/reporting/v3/columns#paidSubscriptionSignups), [paidSubscriptionSignupClicks](guides/reporting/v3/columns#paidSubscriptionSignupClicks), [paidSubscriptionSignupRate](guides/reporting/v3/columns#paidSubscriptionSignupRate), [paidSubscriptionSignupViews](guides/reporting/v3/columns#paidSubscriptionSignupViews), [proposalId](guides/reporting/v3/columns#proposalId), [subscriptionSignupClicks](guides/reporting/v3/columns#subscriptionSignupClicks), [subscriptionSignupViews](guides/reporting/v3/columns#subscriptionSignupViews), [subscriptionSignups](guides/reporting/v3/columns#subscriptionSignups), [subscriptionSignupRate](guides/reporting/v3/columns#subscriptionSignupRate), [marketplace](guides/reporting/v3/columns#marketplace), [addToShoppingCartValueSum](guides/reporting/v3/columns#addToShoppingCartValueSum), [addToShoppingCartValueAverage](guides/reporting/v3/columns#addToShoppingCartValueAverage), [applicationValueSum](guides/reporting/v3/columns#applicationValueSum), [applicationValueAverage](guides/reporting/v3/columns#applicationValueAverage), [checkoutValueSum](guides/reporting/v3/columns#checkoutValueSum), [checkoutValueAverage](guides/reporting/v3/columns#checkoutValueAverage), [contactValueSum](guides/reporting/v3/columns#contactValueSum), [contactValueAverage](guides/reporting/v3/columns#contactValueAverage), [leadValueSum](guides/reporting/v3/columns#leadValueSum), [leadValueAverage](guides/reporting/v3/columns#leadValueAverage), [otherValueSum](guides/reporting/v3/columns#otherValueSum), [otherValueAverage](guides/reporting/v3/columns#otherValueAverage), [pageViewValueSum](guides/reporting/v3/columns#pageViewValueSum), [pageViewValueAverage](guides/reporting/v3/columns#pageViewValueAverage), [searchValueSum](guides/reporting/v3/columns#searchValueSum), [searchValueAverage](guides/reporting/v3/columns#searchValueAverage), [signUpValueSum](guides/reporting/v3/columns#signUpValueSum), [signUpValueAverage](guides/reporting/v3/columns#signUpValueAverage), [subscribeValueSum](guides/reporting/v3/columns#subscribeValueSum), [subscribeValueAverage](guides/reporting/v3/columns#subscribeValueAverage), [orderExternalId](guides/reporting/v3/columns#orderExternalId), [orderEndDate](guides/reporting/v3/columns#orderEndDate), [orderId](guides/reporting/v3/columns#orderId), [orderStartDate](guides/reporting/v3/columns#orderStartDate), [orderCurrency](guides/reporting/v3/columns#orderCurrency), [orderBudget](guides/reporting/v3/columns#orderBudget), [orderName](guides/reporting/v3/columns#orderName), [amazonDSPConsoleFee](guides/reporting/v3/columns#amazonDSPConsoleFee), [omnichannelMetricsFee](guides/reporting/v3/columns#omnichannelMetricsFee), [3PPreBidFee](guides/reporting/v3/columns#3PPreBidFee), [3PPreBidFeeDoubleVerify](guides/reporting/v3/columns#3PPreBidFeeDoubleVerify), [3PPreBidFeeIntegralAdScience](guides/reporting/v3/columns#3PPreBidFeeIntegralAdScience), [3PPreBidFeeOracleDataCloud](guides/reporting/v3/columns#3PPreBidFeeOracleDataCloud), [3PPreBidFeePixalate](guides/reporting/v3/columns#3PPreBidFeePixalate), [newToBrandPurchasesPercentage](guides/reporting/v3/columns#newToBrandPurchasesPercentage), [videoAdImpressions](guides/reporting/v3/columns#videoAdImpressions), [videoAdClicks](guides/reporting/v3/columns#videoAdClicks), [videoAdEndStateClicks](guides/reporting/v3/columns#videoAdEndStateClicks), [videoAdCreativeViews](guides/reporting/v3/columns#videoAdCreativeViews), [videoAdSkipForwards](guides/reporting/v3/columns#videoAdSkipForwards), [videoAdSkipBacks](guides/reporting/v3/columns#videoAdSkipBacks), [videoAdReplays](guides/reporting/v3/columns#videoAdReplays), [totalNewToBrandPurchasesPercentage](guides/reporting/v3/columns#totalNewToBrandPurchasesPercentage), [totalUnitsSold](guides/reporting/v3/columns#totalUnitsSold), [totalSales](guides/reporting/v3/columns#totalSales), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [totalNewToBrandDPVs](guides/reporting/v3/columns#totalNewToBrandDPVs), [totalNewToBrandDPVViews](guides/reporting/v3/columns#totalNewToBrandDPVViews), [totalNewToBrandDPVClicks](guides/reporting/v3/columns#totalNewToBrandDPVClicks), [totalNewToBrandDPVRate](guides/reporting/v3/columns#totalNewToBrandDPVRate), [totalNewToBrandECPDetailPageView](guides/reporting/v3/columns#totalNewToBrandECPDetailPageView), [audioAdCompanionBannerViews](guides/reporting/v3/columns#audioAdCompanionBannerViews), [audioAdCompanionBannerClicks](guides/reporting/v3/columns#audioAdCompanionBannerClicks), [interactiveImpressions](guides/reporting/v3/columns#interactiveImpressions), [notificationOpens](guides/reporting/v3/columns#notificationOpens), [notificationClicks](guides/reporting/v3/columns#notificationClicks), [skillInvocation](guides/reporting/v3/columns#skillInvocation), [skillInvocationViews](guides/reporting/v3/columns#skillInvocationViews), [skillInvocationClicks](guides/reporting/v3/columns#skillInvocationClicks), [skillInvocationRate](guides/reporting/v3/columns#skillInvocationRate), [eCPSkillInvocation](guides/reporting/v3/columns#eCPSkillInvocation)

### Group by `campaign`

Additional metrics: [exclusiveReachRate](guides/reporting/v3/columns#exclusiveReachRate), [incrementalReachRate](guides/reporting/v3/columns#incrementalReachRate), [reach](guides/reporting/v3/columns#reach), [householdReach](guides/reporting/v3/columns#householdReach), [frequencyAverage](guides/reporting/v3/columns#frequencyAverage), [householdFrequencyAverage](guides/reporting/v3/columns#householdFrequencyAverage), [longTermSales](guides/reporting/v3/columns#longTermSales), [longTermROAS](guides/reporting/v3/columns#longTermROAS)


Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `ad`

Additional metrics: [lineItemBudget](guides/reporting/v3/columns#lineItemBudget), [lineItemId](guides/reporting/v3/columns#lineItemId), [lineItemName](guides/reporting/v3/columns#lineItemName), [lineItemExternalId](guides/reporting/v3/columns#lineItemExternalId), [lineItemStartDate](guides/reporting/v3/columns#lineItemStartDate), [lineItemEndDate](guides/reporting/v3/columns#lineItemEndDate), [onTargetCPM](guides/reporting/v3/columns#onTargetCPM), [onTargetImpressionCost](guides/reporting/v3/columns#onTargetImpressionCost), [onTargetImpressions](guides/reporting/v3/columns#onTargetImpressions), [targetDemographic](guides/reporting/v3/columns#targetDemographic), [lineItemBudgetCap](guides/reporting/v3/columns#lineItemBudgetCap), [lineItemComment](guides/reporting/v3/columns#lineItemComment), [lineItemDeliveryRate](guides/reporting/v3/columns#lineItemDeliveryRate), [lineItemStatus](guides/reporting/v3/columns#lineItemStatus), [lineItemType](guides/reporting/v3/columns#lineItemType), [lineItemLanguageTargeting](guides/reporting/v3/columns#lineItemLanguageTargeting), [reach](guides/reporting/v3/columns#reach), [householdReach](guides/reporting/v3/columns#householdReach), [frequencyAverage](guides/reporting/v3/columns#frequencyAverage), [householdFrequencyAverage](guides/reporting/v3/columns#householdFrequencyAverage)


Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `creative`

Additional metrics: [creativeName](guides/reporting/v3/columns#creativeName), [creativeType](guides/reporting/v3/columns#creativeType), [creativeSize](guides/reporting/v3/columns#creativeSize), [creativeId](guides/reporting/v3/columns#creativeId), [creativeAdId](guides/reporting/v3/columns#creativeAdId), [creativeLanguage](guides/reporting/v3/columns#creativeLanguage), [creativeWeight](guides/reporting/v3/columns#creativeWeight), [lineItemApprovalStatus](guides/reporting/v3/columns#lineItemApprovalStatus)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

## Sample calls

#### Sponsored Products: Campaign daily report grouped by campaign and ad group

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data-raw '{
    "name":"SP campaigns report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["campaign","adGroup"],
        "columns":["impressions","clicks","cost","campaignId","adGroupId","date"],
        "reportTypeId":"spCampaigns",
        "timeUnit":"DAILY",
        "format":"GZIP_JSON"
    }
}'
```

#### Sponsored Products: Campaign summary report grouped by campaign and placement

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--data-raw '{
    "name":"SP campaigns report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["campaign","campaignPlacement"],
        "columns":["impressions","clicks","cost","campaignId","placementClassification","startDate","endDate"],
        "reportTypeId":"spCampaigns",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}'
```

#### Sponsored Products: Campaign summary report grouped by placement for Amazon Business
```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--data-raw '{
     "name":"SP campaigns report 9/07-9/10",
     "startDate":"2024-09-07",
     "endDate":"2024-09-10",
     "configuration":{
         "adProduct":"SPONSORED_PRODUCTS",
         "groupBy":["campaignPlacement"],
         "columns":["impressions","clicks","cost","campaignId","placementClassification","startDate","endDate"],
         "filters": [{"field":"campaignSite","values":["AmazonBusiness"]}],
         "reportTypeId":"spCampaigns",
         "timeUnit":"SUMMARY",
         "format":"GZIP_JSON"
    }
}'
```
  
>[NOTE] Grouping by campaignPlacement will generate the same report as grouping by campaign and campaignPlacement.

#### Sponsored Brands: Campaign summary report grouped by campaign 

```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "name": "SB campaigns report 9/5-9/10",
    "startDate": "2023-09-05",
    "endDate": "2023-09-10",
    "configuration": {
        "adProduct": "SPONSORED_BRANDS",
        "groupBy": [
            "campaign"
        ],
        "columns": [
            "impressions",
            "clicks",
            "cost",
            "campaignId",
            "startDate",
            "endDate"
        ],
        "reportTypeId": "sbCampaigns",
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```

#### Amazon DSP: Campaign summary report grouped by campaign and creative

```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "name": "DSP campaign report",
    "startDate": "2025-01-01",
    "endDate": "2025-01-07",
    "configuration": {
        "adProduct": "DEMAND_SIDE_PLATFORM",
        "groupBy": ["campaign", "creative"],
        "columns": ["orderId", "creativeName", "reach", "impressions", "clicks"],
        "reportTypeId": "dspCampaign",
        "filters": [{
            "field": "advertiserId", 
            "values": ["xxxxxxxxxxxx"]     # your advertiser ID here
        }],
        "timeUnit": "SUMMARY",
        "format": "GZIP_JSON"
    }
}'
```
