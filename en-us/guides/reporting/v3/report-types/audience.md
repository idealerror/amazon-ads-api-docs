---
title: Audience reports
description: Learn about requesting audience reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - DSP
    - Sponsored Products
    - Sponsored Display
---

# Audience reports

Audience reports contain data based on your audience. 

## Configurations

| Configuration | Amazon DSP | Sponsored Products	| Sponsored Brands	|
| -------- | --- | --- | --- |
| `reportTypeId` | `dspAudience` | `spAudiences`	|`sbAudiences`	|
| Maximum date range | 31 days | 31 days	|31 days	|
| Data retention | 95 days | 95 days	|95 days	|
| `timeUnit` | `SUMMARY` or `DAILY` | `SUMMARY` or `DAILY`	|`SUMMARY` or `DAILY`	|
| `groupBy` | `order`, or `lineItem` | `campaign_bid_boost_segment` | `campaign_bid_boost_segment`|
| `format` | `GZIP_JSON` or `CSV` | `GZIP_JSON` or `XLSX`	|`GZIP_JSON` or `XLSX`	|

## Amazon DSP

### Base metrics

[intervalStart](guides/reporting/v3/columns#intervalStart), [intervalEnd](guides/reporting/v3/columns#intervalEnd), [advertiserName](guides/reporting/v3/columns#advertiserName), [advertiserId](guides/reporting/v3/columns#advertiserId), [totalCost](guides/reporting/v3/columns#totalCost), [supplyCost](guides/reporting/v3/columns#supplyCost), [amazonDSPAudienceFee](guides/reporting/v3/columns#amazonDSPAudienceFee), [amazonDSPConsoleFee](guides/reporting/v3/columns#amazonDSPConsoleFee), [impressions](guides/reporting/v3/columns#impressions), [clicks](guides/reporting/v3/columns#clicks), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [eCPC](guides/reporting/v3/columns#eCPC), [eCPM](guides/reporting/v3/columns#eCPM), [brandSearches](guides/reporting/v3/columns#brandSearches), [brandSearchesViews](guides/reporting/v3/columns#brandSearchesViews), [brandSearchesClicks](guides/reporting/v3/columns#brandSearchesClicks), [brandSearchRate](guides/reporting/v3/columns#brandSearchRate), [detailPageViews](guides/reporting/v3/columns#detailPageViews), [detailPageViewViews](guides/reporting/v3/columns#detailPageViewViews), [detailPageViewClicks](guides/reporting/v3/columns#detailPageViewClicks), [detailPageViewRate](guides/reporting/v3/columns#detailPageViewRate), [eCPDetailPageView](guides/reporting/v3/columns#eCPDetailPageView), [productReviewPageVisits](guides/reporting/v3/columns#productReviewPageVisits), [productReviewPageVisitsViews](guides/reporting/v3/columns#productReviewPageVisitsViews), [productReviewPageVisitsClicks](guides/reporting/v3/columns#productReviewPageVisitsClicks), [productReviewPageVisitRate](guides/reporting/v3/columns#productReviewPageVisitRate), [eCPProductReviewPageVisit](guides/reporting/v3/columns#eCPProductReviewPageVisit), [addToList](guides/reporting/v3/columns#addToList), [addToListViews](guides/reporting/v3/columns#addToListViews), [addToListClicks](guides/reporting/v3/columns#addToListClicks), [addToListRate](guides/reporting/v3/columns#addToListRate), [eCPAddToList](guides/reporting/v3/columns#eCPAddToList), [addToCart](guides/reporting/v3/columns#addToCart), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [addToCartClicks](guides/reporting/v3/columns#addToCartClicks), [addToCartRate](guides/reporting/v3/columns#addToCartRate), [eCPAddToCart](guides/reporting/v3/columns#eCPAddToCart), [purchases](guides/reporting/v3/columns#purchases), [purchasesViews](guides/reporting/v3/columns#purchasesViews), [purchasesClicks](guides/reporting/v3/columns#purchasesClicks), [purchaseRate](guides/reporting/v3/columns#purchaseRate), [eCPP](guides/reporting/v3/columns#eCPP), [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), [newToBrandPurchasesViews](guides/reporting/v3/columns#newToBrandPurchasesViews), [newToBrandPurchasesClicks](guides/reporting/v3/columns#newToBrandPurchasesClicks), [newToBrandPurchaseRate](guides/reporting/v3/columns#newToBrandPurchaseRate), [NewToBrandECPP](guides/reporting/v3/columns#NewToBrandECPP), [subscriptionSignups](guides/reporting/v3/columns#subscriptionSignups), [subscriptionSignupViews](guides/reporting/v3/columns#subscriptionSignupViews), [subscriptionSignupClicks](guides/reporting/v3/columns#subscriptionSignupClicks), [subscriptionSignupRate](guides/reporting/v3/columns#subscriptionSignupRate), [eCPSubscriptionSignup](guides/reporting/v3/columns#eCPSubscriptionSignup), [freeTrialSubscriptionSignups](guides/reporting/v3/columns#freeTrialSubscriptionSignups), [freeTrialSubscriptionSignupViews](guides/reporting/v3/columns#freeTrialSubscriptionSignupViews), [freeTrialSubscriptionSignupClicks](guides/reporting/v3/columns#freeTrialSubscriptionSignupClicks), [freeTrialSubscriptionSignupRate](guides/reporting/v3/columns#freeTrialSubscriptionSignupRate), [eCPFreeTrialSubscriptionSignup](guides/reporting/v3/columns#eCPFreeTrialSubscriptionSignup), [paidSubscriptionSignups](guides/reporting/v3/columns#paidSubscriptionSignups), [paidSubscriptionSignupViews](guides/reporting/v3/columns#paidSubscriptionSignupViews), [paidSubscriptionSignupClicks](guides/reporting/v3/columns#paidSubscriptionSignupClicks), [paidSubscriptionSignupRate](guides/reporting/v3/columns#paidSubscriptionSignupRate), [eCPPaidSubscriptionSignup](guides/reporting/v3/columns#eCPPaidSubscriptionSignup), [newToBrandPurchasesPercentage](guides/reporting/v3/columns#newToBrandPurchasesPercentage), [addToWatchlist](guides/reporting/v3/columns#addToWatchlist), [addToWatchlistViews](guides/reporting/v3/columns#addToWatchlistViews), [addToWatchlistClicks](guides/reporting/v3/columns#addToWatchlistClicks), [addToWatchlistRate](guides/reporting/v3/columns#addToWatchlistRate), [eCPAddToWatchlist](guides/reporting/v3/columns#eCPAddToWatchlist), [downloadedVideoPlays](guides/reporting/v3/columns#downloadedVideoPlays), [downloadedVideoPlaysViews](guides/reporting/v3/columns#downloadedVideoPlaysViews), [downloadedVideoPlaysClicks](guides/reporting/v3/columns#downloadedVideoPlaysClicks), [downloadedVideoPlayRate](guides/reporting/v3/columns#downloadedVideoPlayRate), [eCPDownloadedVideoPlays](guides/reporting/v3/columns#eCPDownloadedVideoPlays), [videoStreams](guides/reporting/v3/columns#videoStreams), [videoStreamsViews](guides/reporting/v3/columns#videoStreamsViews), [videoStreamsClicks](guides/reporting/v3/columns#videoStreamsClicks), [videoStreamsRate](guides/reporting/v3/columns#videoStreamsRate), [videoAdClicks](guides/reporting/v3/columns#videoAdClicks), [videoAdCreativeViews](guides/reporting/v3/columns#videoAdCreativeViews), [videoAdEndStateClicks](guides/reporting/v3/columns#videoAdEndStateClicks), [videoAdImpressions](guides/reporting/v3/columns#videoAdImpressions), [videoAdReplays](guides/reporting/v3/columns#videoAdReplays), [videoAdSkipForwards](guides/reporting/v3/columns#videoAdSkipForwards), [videoAdSkipBacks](guides/reporting/v3/columns#videoAdSkipBacks), [eCPVideoStream](guides/reporting/v3/columns#eCPVideoStream), [playTrailers](guides/reporting/v3/columns#playTrailers), [playTrailersViews](guides/reporting/v3/columns#playTrailersViews), [playTrailersClicks](guides/reporting/v3/columns#playTrailersClicks), [playTrailerRate](guides/reporting/v3/columns#playTrailerRate), [eCPPlayTrailer](guides/reporting/v3/columns#eCPPlayTrailer), [rentals](guides/reporting/v3/columns#rentals), [rentalsViews](guides/reporting/v3/columns#rentalsViews), [rentalsClicks](guides/reporting/v3/columns#rentalsClicks), [rentalRate](guides/reporting/v3/columns#rentalRate), [eCPRental](guides/reporting/v3/columns#eCPRental), [videoDownloads](guides/reporting/v3/columns#videoDownloads), [videoDownloadsViews](guides/reporting/v3/columns#videoDownloadsViews), [videoDownloadsClicks](guides/reporting/v3/columns#videoDownloadsClicks), [videoDownloadRate](guides/reporting/v3/columns#videoDownloadRate), [eCPVideoDownload](guides/reporting/v3/columns#eCPVideoDownload), [newSubscribeAndSave](guides/reporting/v3/columns#newSubscribeAndSave), [newSubscribeAndSaveViews](guides/reporting/v3/columns#newSubscribeAndSaveViews), [newSubscribeAndSaveClicks](guides/reporting/v3/columns#newSubscribeAndSaveClicks), [newSubscribeAndSaveRate](guides/reporting/v3/columns#newSubscribeAndSaveRate), [eCPnewSubscribeAndSave](guides/reporting/v3/columns#eCPnewSubscribeAndSave), [offAmazonViews](guides/reporting/v3/columns#offAmazonViews), [offAmazonClicks](guides/reporting/v3/columns#offAmazonClicks), [subscribe](guides/reporting/v3/columns#subscribe), [subscribeViews](guides/reporting/v3/columns#subscribeViews), [subscribeClicks](guides/reporting/v3/columns#subscribeClicks), [subscribeCVR](guides/reporting/v3/columns#subscribeCVR), [subscribeCPA](guides/reporting/v3/columns#subscribeCPA), [subscribeValueSum](guides/reporting/v3/columns#subscribeValueSum), [subscribeValueAverage](guides/reporting/v3/columns#subscribeValueAverage), [application](guides/reporting/v3/columns#application), [applicationViews](guides/reporting/v3/columns#applicationViews), [applicationClicks](guides/reporting/v3/columns#applicationClicks), [applicationCVR](guides/reporting/v3/columns#applicationCVR), [applicationCPA](guides/reporting/v3/columns#applicationCPA), [applicationValueSum](guides/reporting/v3/columns#applicationValueSum), [applicationValueAverage](guides/reporting/v3/columns#applicationValueAverage), [signUpViews](guides/reporting/v3/columns#signUpViews), [signUpCVR](guides/reporting/v3/columns#signUpCVR), [signUpCPA](guides/reporting/v3/columns#signUpCPA), [signUpValueSum](guides/reporting/v3/columns#signUpValueSum), [signUpValueAverage](guides/reporting/v3/columns#signUpValueAverage), [mobileAppFirstStarts](guides/reporting/v3/columns#mobileAppFirstStarts), [mobileAppFirstStartViews](guides/reporting/v3/columns#mobileAppFirstStartViews), [mobileAppFirstStartClicks](guides/reporting/v3/columns#mobileAppFirstStartClicks), [mobileAppFirstStartCVR](guides/reporting/v3/columns#mobileAppFirstStartCVR), [mobileAppFirstStartCPA](guides/reporting/v3/columns#mobileAppFirstStartCPA), [addToShoppingCart](guides/reporting/v3/columns#addToShoppingCart), [addToShoppingCartViews](guides/reporting/v3/columns#addToShoppingCartViews), [addToShoppingCartClicks](guides/reporting/v3/columns#addToShoppingCartClicks), [addToShoppingCartCVR](guides/reporting/v3/columns#addToShoppingCartCVR), [addToShoppingCartCPA](guides/reporting/v3/columns#addToShoppingCartCPA), [addToShoppingCartValueSum](guides/reporting/v3/columns#addToShoppingCartValueSum), [addToShoppingCartValueAverage](guides/reporting/v3/columns#addToShoppingCartValueAverage), [offAmazonPurchasesViews](guides/reporting/v3/columns#offAmazonPurchasesViews), [offAmazonPurchasesClicks](guides/reporting/v3/columns#offAmazonPurchasesClicks), [offAmazonECPP](guides/reporting/v3/columns#offAmazonECPP), [other](guides/reporting/v3/columns#other), [otherViews](guides/reporting/v3/columns#otherViews), [otherClicks](guides/reporting/v3/columns#otherClicks), [otherCVR](guides/reporting/v3/columns#otherCVR), [otherCPA](guides/reporting/v3/columns#otherCPA), [otherValueSum](guides/reporting/v3/columns#otherValueSum), [otherValueAverage](guides/reporting/v3/columns#otherValueAverage), [pageViews](guides/reporting/v3/columns#pageViews), [pageViewViews](guides/reporting/v3/columns#pageViewViews), [pageViewClicks](guides/reporting/v3/columns#pageViewClicks), [pageViewCVR](guides/reporting/v3/columns#pageViewCVR), [pageViewCPA](guides/reporting/v3/columns#pageViewCPA), [pageViewValueSum](guides/reporting/v3/columns#pageViewValueSum), [pageViewValueAverage](guides/reporting/v3/columns#pageViewValueAverage), [search](guides/reporting/v3/columns#search), [searchViews](guides/reporting/v3/columns#searchViews), [searchClicks](guides/reporting/v3/columns#searchClicks), [searchCVR](guides/reporting/v3/columns#searchCVR), [searchCPA](guides/reporting/v3/columns#searchCPA), [searchValueSum](guides/reporting/v3/columns#searchValueSum), [searchValueAverage](guides/reporting/v3/columns#searchValueAverage), [contact](guides/reporting/v3/columns#contact), [contactViews](guides/reporting/v3/columns#contactViews), [contactClicks](guides/reporting/v3/columns#contactClicks), [contactCVR](guides/reporting/v3/columns#contactCVR), [contactCPA](guides/reporting/v3/columns#contactCPA), [contactValueSum](guides/reporting/v3/columns#contactValueSum), [contactValueAverage](guides/reporting/v3/columns#contactValueAverage), [checkout](guides/reporting/v3/columns#checkout), [checkoutViews](guides/reporting/v3/columns#checkoutViews), [checkoutClicks](guides/reporting/v3/columns#checkoutClicks), [checkoutCVR](guides/reporting/v3/columns#checkoutCVR), [checkoutCPA](guides/reporting/v3/columns#checkoutCPA), [checkoutValueSum](guides/reporting/v3/columns#checkoutValueSum), [checkoutValueAverage](guides/reporting/v3/columns#checkoutValueAverage), [lead](guides/reporting/v3/columns#lead), [leadViews](guides/reporting/v3/columns#leadViews), [leadClicks](guides/reporting/v3/columns#leadClicks), [leadCVR](guides/reporting/v3/columns#leadCVR), [leadCPA](guides/reporting/v3/columns#leadCPA), [leadValueSum](guides/reporting/v3/columns#leadValueSum), [leadValueAverage](guides/reporting/v3/columns#leadValueAverage), [videoAdStart](guides/reporting/v3/columns#videoAdStart), [videoAdFirstQuartile](guides/reporting/v3/columns#videoAdFirstQuartile), [videoAdMidpoint](guides/reporting/v3/columns#videoAdMidpoint), [videoAdThirdQuartile](guides/reporting/v3/columns#videoAdThirdQuartile), [videoAdComplete](guides/reporting/v3/columns#videoAdComplete), [videoAdCompletionRate](guides/reporting/v3/columns#videoAdCompletionRate), [eCPVideoAdCompletion](guides/reporting/v3/columns#eCPVideoAdCompletion), [videoAdPause](guides/reporting/v3/columns#videoAdPause), [videoAdResume](guides/reporting/v3/columns#videoAdResume), [videoAdMute](guides/reporting/v3/columns#videoAdMute), [videoAdUnmute](guides/reporting/v3/columns#videoAdUnmute), [audioAdViews](guides/reporting/v3/columns#audioAdViews), [audioAdStart](guides/reporting/v3/columns#audioAdStart), [audioAdFirstQuartile](guides/reporting/v3/columns#audioAdFirstQuartile), [audioAdMidpoint](guides/reporting/v3/columns#audioAdMidpoint), [audioAdThirdQuartile](guides/reporting/v3/columns#audioAdThirdQuartile), [audioAdCompletions](guides/reporting/v3/columns#audioAdCompletions), [audioAdPause](guides/reporting/v3/columns#audioAdPause), [audioAdResume](guides/reporting/v3/columns#audioAdResume), [audioAdRewind](guides/reporting/v3/columns#audioAdRewind), [audioAdSkip](guides/reporting/v3/columns#audioAdSkip), [audioAdMute](guides/reporting/v3/columns#audioAdMute), [audioAdUnmute](guides/reporting/v3/columns#audioAdUnmute), [audioAdProgression](guides/reporting/v3/columns#audioAdProgression), [audioAdCompanionBannerViews](guides/reporting/v3/columns#audioAdCompanionBannerViews), [audioAdCompanionBannerClicks](guides/reporting/v3/columns#audioAdCompanionBannerClicks), [audioAdCompletionRate](guides/reporting/v3/columns#audioAdCompletionRate), [eCPAudioAdCompletion](guides/reporting/v3/columns#eCPAudioAdCompletion), [newToBrandDetailPageViews](guides/reporting/v3/columns#newToBrandDetailPageViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#newToBrandDetailPageViewClicks), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newToBrandDetailPageViewRate), [newToBrandECPDetailPageView](guides/reporting/v3/columns#newToBrandECPDetailPageView), [totalNewToBrandDPVs](guides/reporting/v3/columns#totalNewToBrandDPVs), [totalNewToBrandDPVViews](guides/reporting/v3/columns#totalNewToBrandDPVViews), [totalNewToBrandDPVClicks](guides/reporting/v3/columns#totalNewToBrandDPVClicks), [totalNewToBrandDPVRate](guides/reporting/v3/columns#totalNewToBrandDPVRate), [totalNewToBrandECPDetailPageView](guides/reporting/v3/columns#totalNewToBrandECPDetailPageView), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [interactiveImpressions](guides/reporting/v3/columns#interactiveImpressions), [notificationOpens](guides/reporting/v3/columns#notificationOpens), [notificationClicks](guides/reporting/v3/columns#notificationClicks), [segmentName](guides/reporting/v3/columns#segmentName), [segmentId](guides/reporting/v3/columns#segmentId), [segmentClassCode](guides/reporting/v3/columns#segmentClassCode), [segmentSource](guides/reporting/v3/columns#segmentSource), [segmentType](guides/reporting/v3/columns#segmentType), [segmentMarketplaceId](guides/reporting/v3/columns#segmentMarketplaceId), [targetingMethod](guides/reporting/v3/columns#targetingMethod), [masterImpressions](guides/reporting/v3/columns#masterImpressions), [masterClicks](guides/reporting/v3/columns#masterClicks), [clickToApply](guides/reporting/v3/columns#clickToApply), [clickToApplyViews](guides/reporting/v3/columns#clickToApplyViews), [clickToApplyClicks](guides/reporting/v3/columns#clickToApplyClicks), [clickToApplyConversionRate](guides/reporting/v3/columns#clickToApplyConversionRate), [averageCostPerClickToApply](guides/reporting/v3/columns#averageCostPerClickToApply), [kindleDetailPageView](guides/reporting/v3/columns#kindleDetailPageView), [kindleDetailPageViewViews](guides/reporting/v3/columns#kindleDetailPageViewViews), [kindleDetailPageViewClicks](guides/reporting/v3/columns#kindleDetailPageViewClicks), [kindleDetailPageViewRate](guides/reporting/v3/columns#kindleDetailPageViewRate), [averageCostPerKindleDetailPageView](guides/reporting/v3/columns#averageCostPerKindleDetailPageView), [skillInvocation](guides/reporting/v3/columns#skillInvocation), [skillInvocationViews](guides/reporting/v3/columns#skillInvocationViews), [skillInvocationClicks](guides/reporting/v3/columns#skillInvocationClicks), [skillInvocationRate](guides/reporting/v3/columns#skillInvocationRate), [effectiveCostPerSkillInvocation](guides/reporting/v3/columns#effectiveCostPerSkillInvocation), [detailPageViewRatePromotedClicks](guides/reporting/v3/columns#detailPageViewRatePromotedClicks), [effectiveCostPerDetailPageViewPromotedClicks](guides/reporting/v3/columns#effectiveCostPerDetailPageViewPromotedClicks), [effectiveCostPerPurchasePromotedClicks](guides/reporting/v3/columns#effectiveCostPerPurchasePromotedClicks), [purchasesClicksRate](guides/reporting/v3/columns#purchasesClicksRate), [offAmazonConversions](guides/reporting/v3/columns#offAmazonConversions), [date](guides/reporting/v3/columns#date), [invalidClickThroughRate](guides/reporting/v3/columns#invalidClickThroughRate), [signUp](guides/reporting/v3/columns#signUp), [signUpClicks](guides/reporting/v3/columns#signUpClicks), [entityId](guides/reporting/v3/columns#entityId), [advertiserTimezone](guides/reporting/v3/columns#advertiserTimezone), [advertiserCountry](guides/reporting/v3/columns#advertiserCountry), [offAmazonCVR](guides/reporting/v3/columns#offAmazonCVR), [offAmazonCPA](guides/reporting/v3/columns#offAmazonCPA), [offAmazonPurchaseRate](guides/reporting/v3/columns#offAmazonPurchaseRate), [grossImpressions](guides/reporting/v3/columns#grossImpressions), [grossClickThroughs](guides/reporting/v3/columns#grossClickThroughs), [invalidImpressions](guides/reporting/v3/columns#invalidImpressions), [invalidClickThroughs](guides/reporting/v3/columns#invalidClickThroughs), [invalidImpressionRate](guides/reporting/v3/columns#invalidImpressionRate)

### Group by `order`

Additional metrics: [orderName](guides/reporting/v3/columns#orderName), [orderId](guides/reporting/v3/columns#orderId), [orderCurrency](guides/reporting/v3/columns#orderCurrency), [omsProposalId](guides/reporting/v3/columns#omsProposalId), [orderStartDate](guides/reporting/v3/columns#orderStartDate), [orderEndDate](guides/reporting/v3/columns#orderEndDate)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `lineItem`

Additional metrics: [lineItemName](guides/reporting/v3/columns#lineItemName), [lineItemId](guides/reporting/v3/columns#lineItemId), [lineItemComments](guides/reporting/v3/columns#lineItemComments), [lineItemType](guides/reporting/v3/columns#lineItemType), [omsLineItemId](guides/reporting/v3/columns#omsLineItemId), [lineItemStartDate](guides/reporting/v3/columns#lineItemStartDate), [lineItemEndDate](guides/reporting/v3/columns#lineItemEndDate)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

## Sponsored Products

### Base metrics

[campaignId](guides/reporting/v3/columns#campaignid), [segmentName](guides/reporting/v3/columns#segmentName), [segmentClassCode](guides/reporting/v3/columns#segmentClassCode), [impressions](guides/reporting/v3/columns#impressions), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [spend](guides/reporting/v3/columns#spend), [purchases1d](guides/reporting/v3/columns#purchases1d), [purchases7d](guides/reporting/v3/columns#purchases7d), [purchases14d](guides/reporting/v3/columns#purchases14d), [purchases30d](guides/reporting/v3/columns#purchases30d), [purchasesSameSku1d](guides/reporting/v3/columns#purchasessamesku1d), [purchasesSameSku7d](guides/reporting/v3/columns#purchasessamesku7d), [purchasesSameSku14d](guides/reporting/v3/columns#purchasessamesku14d), [purchasesSameSku30d](guides/reporting/v3/columns#purchasessamesku30d), [unitsSoldClicks1d](guides/reporting/v3/columns#unitssoldclicks1d), [unitsSoldClicks7d](guides/reporting/v3/columns#unitssoldclicks7d), [unitsSoldClicks14d](guides/reporting/v3/columns#unitssoldclicks14d), [unitsSoldClicks30d](guides/reporting/v3/columns#unitssoldclicks30d), [sales1d](guides/reporting/v3/columns#sales1d), [sales7d](guides/reporting/v3/columns#sales7d), [sales14d](guides/reporting/v3/columns#sales14d), [sales30d](guides/reporting/v3/columns#sales30d), [attributedSalesSameSku1d](guides/reporting/v3/columns#attributedsalessamesku1d), [attributedSalesSameSku7d](guides/reporting/v3/columns#attributedsalessamesku7d), [attributedSalesSameSku14d](guides/reporting/v3/columns#attributedsalessamesku14d), [attributedSalesSameSku30d](guides/reporting/v3/columns#attributedsalessamesku30d), [unitsSoldSameSku1d](guides/reporting/v3/columns#unitssoldsamesku1d), [unitsSoldSameSku7d](guides/reporting/v3/columns#unitssoldsamesku7d), [unitsSoldSameSku14d](guides/reporting/v3/columns#unitssoldsamesku14d), [unitsSoldSameSku30d](guides/reporting/v3/columns#unitssoldsamesku30d), [date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startdate), [endDate](guides/reporting/v3/columns#enddate), [campaignBiddingStrategy](guides/reporting/v3/columns#campaignbiddingstrategy), [costPerClick](guides/reporting/v3/columns#costperclick), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [campaignName](guides/reporting/v3/columns#campaignname), [campaignStatus](guides/reporting/v3/columns#campaignstatus), [campaignBudgetAmount](guides/reporting/v3/columns#campaignbudgetamount), [campaignBudgetType](guides/reporting/v3/columns#campaignbudgettype), [marketplaceId](guides/reporting/v3/columns#marketplace) 

### Group by `campaign_bid_boost_segment`

Additional metrics: segmentName
Filters: None

## Sponsored Brands

### Base metrics

[campaignId](guides/reporting/v3/columns#campaignid),  [segmentName](guides/reporting/v3/columns#segmentName), [impressions](guides/reporting/v3/columns#impressions), [clicks](guides/reporting/v3/columns#clicks), [cost](guides/reporting/v3/columns#cost), [spend](guides/reporting/v3/columns#spend), [purchases](guides/reporting/v3/columns#purchases), [sales](guides/reporting/v3/columns#sales), [salesClicks](guides/reporting/v3/columns#salesclicks), [unitsSold](guides/reporting/v3/columns#unitssold), [unitsSoldClicks](guides/reporting/v3/columns#unitssoldclicks), [date](guides/reporting/v3/columns#date), [startDate](guides/reporting/v3/columns#startdate), [endDate](guides/reporting/v3/columns#enddate), [costPerClick](guides/reporting/v3/columns#costperclick), [clickThroughRate](guides/reporting/v3/columns#clickThroughRate), [campaignName](guides/reporting/v3/columns#campaignname), [campaignStatus](guides/reporting/v3/columns#campaignstatus), [campaignBudgetAmount](guides/reporting/v3/columns#campaignbudgetamount), [campaignBudgetType](guides/reporting/v3/columns#campaignbudgettype), [marketplaceId](guides/reporting/v3/columns#marketplace), [segmentClassCode](guides/reporting/v3/columns#segmentclasscode)

### Group by `campaign_bid_boost_segment`

Additional metrics: segmentName
Filters: None

## Sample requests

### `dspAudience` report request body

```json
{
    "name":"TEST",
    "startDate":"2024-03-05",
    "endDate":"2024-03-10",
    "configuration":{
        "adProduct": "DEMAND_SIDE_PLATFORM",
        "groupBy":["lineItem", "order"],
        "columns":["lineItemName", "lineItemId", "lineItemComments", "lineItemType", "omsLineItemId", "orderName", "orderId", "orderCurrency", "omsProposalId"],
        "reportTypeId":"dspAudience",
        "timeUnit":"SUMMARY",
        "filters": [{"field":"advertiserId","values":["......."]}],
        "format":"GZIP_JSON"
    }
}
```

### Sponsored Products

```
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data '{
    "name": "test",
    "startDate": "2024-12-01",
    "endDate": "2024-12-03",
    "configuration": {
        "adProduct": "SPONSORED_PRODUCTS",
        "reportTypeId": "spAudiences",
        "groupBy": ["campaign_bid_boost_segment"],
        "columns": [
            "startDate", "endDate", "campaignId", "campaignName", "segmentName",
            "impressions", "cost", "campaignStatus", "campaignBudgetAmount", "spend", "sales7d", "costPerClick", "clickThroughRate",
            "clicks"
        ],
        "timeUnit": "SUMMARY",
        "format": "XLSX"
    }
}'
```

### Sponsored Brands

```
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--data '{
    "name": "test",
    "startDate": "2024-12-01",
    "endDate": "2024-12-29",
    "configuration": {
        "adProduct": "SPONSORED_BRANDS",
        "reportTypeId": "sbAudiences",
        "groupBy": [
            "campaign_bid_boost_segment"
        ],
        "columns": [
            "startDate",
            "endDate",
            "campaignId",
            "campaignName",
            "segmentName",
            "impressions",
            "cost",
            "campaignStatus",
            "campaignBudgetAmount",
            "spend",
            "clicks"
        ],
        "timeUnit": "SUMMARY",
        "format": "XLSX"
    }
}'
```
