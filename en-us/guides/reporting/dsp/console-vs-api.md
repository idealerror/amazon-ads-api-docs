---
title: Reporting in the DSP console vs. API
description: Understand how reporting functionality differs in the DSP console and Amazon Ads API.  
type: guide
interface: api
tags:
    - Reporting
    - DSP
---

# DSP console vs. API reports

The [DSP console](https://advertising.amazon.com/dsp/entities) provides advertisers and agencies with a UI to manually download reporting data. There are a few key differences between the DSP console report center and reports retrieved using the API.

## Metrics

The DSP console supports more metrics compared to the API. The metrics available differ based on whether you have a self-service or managed account. 

[Some metrics](#missing-metrics) are available in the DSP console but not in the API. 

#### API vs. console naming comparison

|API  field name	|Console field name	|
|---	|---	|
|date	|Date	|
|intervalStart	|Interval start	|
|intervalEnd	|Interval end	|
|entityId	|Entity ID	|
|advertiserName	|Advertiser name	|
|advertiserId	|Advertiser Id	|
|orderName	|Order name	|
|orderId	|Order ID	|
|orderStartDate	|Order start date	|
|orderEndDate	|Order end date	|
|orderBudget	|Order budget	|
|orderlExternalId	|PO Number	|
|orderCurrency	|Order currency	|
|lineItemName	|Line item name	|
|lineItemId	|Line item ID	|
|lineItemStartDate	|Line item start date	|
|lineItemEndDate	|Line item end date	|
|lineItemBudget	|Line item budget	|
|lineItemExternalId	|Line item external ID	|
|lineitemtype	|Line item type	|
|targetingMethod	|Targeting method	|
|segment	|Segment	|
|agencyFee	|Agency fee	|
|totalFee	|Total fee	|
|3pFeeAutomotive	|3P fee - automotive	|
|3pFeeAutomotiveAbsorbed	|3P fee - automotive (absorbed)	|
|3pFeeComScore	|3P fee - ComScore	|
|3pFeeComScoreAbsorbed	|3P fee - ComScore (absorbed)	|
|3pFeeCPM1	|3P fee - CPM 1	|
|3pFeeCPM1Absorbed	|3P fee - CPM 1 (absorbed)	|
|3pFeeCPM2	|3P fee - CPM 2	|
|3pFeeCPM2Absorbed	|3P fee - CPM 2 (absorbed)	|
|3pFeeCPM3	|3P fee - CPM 3	|
|3pFeeCPM3Absorbed	|3P fee - CPM 3 (absorbed)	|
|3pFeeDoubleclickCampaignManager	|3P fee - DoubleClick Campaign Manager	|
|3pFeeDoubleclickCampaignManagerAbsorbed	|3P fee - DoubleClick Campaign Manager (absorbed)	|
|3pFeeDoubleVerify	|3P fee - DoubleVerify	|
|3pFeeDoubleVerifyAbsorbed	|3P fee - DoubleVerify (absorbed)	|
|3pFeeIntegralAdScience	|3P fee - Integral Ad Science	|
|3pFeeIntegralAdScienceAbsorbed	|3P fee - Integral Ad Science (absorbed)	|
|3PFees	|Third party (3P) fees	|
|unitsSold14d	|Product units sold	|
|sales14d	|Product sales	|
|ROAS14d	|Return on advertising spend	|
|eRPM14d	|Effective return per thousand impressions (eRPM)	|
|newToBrandUnitsSold14d	|New-to-brand product units sold	|
|newToBrandProductSales14d	|New-to-brand product sales	|
|newToBrandROAS14d	|New-to-brand return on advertising spend	|
|newToBrandERPM14d	|New-to-brand eRPM	|
|totalPRPV14d	|Total product review page views (Total PRPV)	|
|totalPRPVViews14d	|Total product review page views view-through conversions (Total PRPV  views)	|
|totalPRPVClicks14d	|Total product review page views click-through conversions (Total PRPV  clicks)	|
|totalPRPVr14d	|Total Product review page view rate (Total PRPVR)	|
|totalECPPRPV14d	|Total Effective cost per product review page view (Total eCPPRPV)	|
|totalPurchases14d	|Total purchases	|
|totalPurchasesViews14d	|Total purchases view-through conversions (Total purchases views)	|
|totalPurchasesClicks14d	|Total purchases click-through conversions (Total purchases clicks)	|
|totalPurchaseRate14d	|Total purchases rate	|
|totalECPP14d	|Total Effective cost per purchase (Total eCPP)	|
|totalNewToBrandPurchases14d	|Total new-to-brand purchases	|
|totalNewToBrandPurchasesViews14d	|Total new-to-brand purchases view-through conversions	|
|totalNewToBrandPurchasesClicks14d	|Total new-to-brand purchases clicks	|
|totalNewToBrandPurchaseRate14d	|Total new-to-brand purchase rate	|
|totalNewToBrandECPP14d	|Total new-to-brand effective cost per purchase	|
|totalPercentOfPurchasesNewToBrand14d	|Total percent of purchases new-to-brand	|
|totalUnitsSold14d	|Total units sold	|
|totalSales14d	|Total product sales	|
|totalROAS14d	|Total return on advertising spend (Total ROAS)	|
|totalERPM14d	|Total effective return per thousand impressions (Total eRPM)	|
|totalNewToBrandUnitsSold14d	|Total new-to-brand units sold	|
|totalNewToBrandProductSales14d	|Total new-to-brand product sales	|
|totalNewToBrandROAS14d	|Total new-to-brand return on advertising spend (Total new-to-brand ROAS)	|
|totalNewToBrandERPM14d	|Total new-to-brand effective return per thousand impressions  (New-to-brand total eRPM)	|
|viewableImpressions	|Viewable impressions	|
|measurableImpressions	|Measurable impressions	|
|measurableRate	|Measurable rate	|
|viewabilityRate	|Viewability rate	|
|totalDetailPageViews14d	|Total detail page views (Total DPV)	|
|totalDetailPageViewViews14d	|Total detail page views view-through conversions (Total DPV views)	|
|totalDetailPageClicks14d	|Total detail page views click-through conversions (Total DPV clicks)	|
|totalDetailPageViewsCVR14d	|Total detail page view rate (Total DPVR)	|
|totalDetaiPageViewCPA14d	|Total Effective cost per detail page view (Total eCPDPV)	|
|totalAddToList14d	|Total Add to Lists (Total ATL)	|
|totalAddToListViews14d	|Total Add to List view-through conversions (Total ATL views)	|
|totalAddToListClicks14d	|Total Add to List click-through conversions (Total ATL clicks)	|
|totalAddToListCVR14d	|Total Add to List rate (Total ATLR)	|
|totalAddToListCPA14d	|Total Effective cost per Total Add to List (Total eCPATL)	|
|totalAddToCart14d	|Total Add to Carts (Total ATC)	|
|totalAddToCartViews14d	|Total Add to Cart view-through conversions (Total ATC views)	|
|totalAddToCartClicks14d	|Total Add to Cart click-through conversions (Total ATC clicks)	|
|totalAddToCartCVR14d	|Total Add to Cart rate (Total ATCR)	|
|totalAddToCartCPA14d	|Total Effective cost per Total Add to Cart (Total eCPATC)	|
|totalSubscribeAndSaveSubscriptions14d	|Total Subscribe & Save subscriptions (Total SnSS)	|
|totalSubscribeAndSaveSubscriptionViews14d	|Total Subscribe & Save subscriptions view-through conversions (Total  SnSS views)	|
|totalSubscribeAndSaveSubscriptionClicks14d	|Total Subscribe & Save subscriptions click-through conversions (Total  SnSS clicks)	|
|totalSubscribeAndSaveSubscriptionCVR14d	|Total Subscribe & Save subscriptions rate (Total SnSSR)	|
|totalSubscribeAndSaveSubscriptionCPA14d	|Total Effective cost per Subscribe & Save subscription (Total  eCPSnSS)	|
|creativeName	|Creative name	|
|creativeID	|Creative  Id	|
|creativeAdId	|Creative  id	|
|creativeType	|Creative type	|
|creativeSize	|Creative size	|
|totalCost	|Total cost	|
|supplyCost	|Supply cost	|
|amazonAudienceFee	|Amazon DSP audience fee	|
|advertiserTimezone	|Advertiser timezone	|
|advertiserCountry	|Advertiser country	|
|amazonPlatformFee	|Amazon DSP console fee	|
|impressions	|Impressions	|
|clickThroughs	|Click-throughs	|
|CTR	|Click-through rate (CTR)	|
|eCPM	|Effective cost per thousand (eCPM)	|
|eCPC	|Effective cost per click-through (eCPC)	|
|dpv14d	|Detail page views (DPV)	|
|dpvViews14d	|Detail page views view-through conversions (DPV views)	|
|dpvClicks14d	|Detail page views click-through conversions (DPV clicks)	|
|dpvr14d	|Detail page view rate (DPVR)	|
|eCPDPV14d	|Effective cost per detail page view (eCPDPV)	|
|pRPV14d	|Product review page views (PRPV)	|
|pRPVViews14d	|Product review page views view-through conversions (PRPV views)	|
|pRPVClicks14d	|Product review page views click-through conversions (PRPV clicks)	|
|pRPVr14d	|Product review page view rate (PRPVR)	|
|eCPPRPV14d	|Effective cost per product review page view (eCPPRPV)	|
|atl14d	|Add to List (ATL)	|
|atlViews14d	|Add to List view-through conversions (ATL views)	|
|atlClicks14d	|Add to List click-through conversions (ATL clicks)	|
|atlr14d	|Add to List rate (ATLR)	|
|eCPAtl14d	|Effective cost per Add to List (eCPATL)	|
|atc14d	|Add to Cart (ATC)	|
|atcViews14d	|Add to Cart view-through conversions (ATC views)	|
|atcClicks14d	|Add to Cart click-through conversions (ATC clicks)	|
|atcr14d	|Add to Cart rate (ATCR)	|
|eCPAtc14d	|Effective cost per Add to Cart (eCPATC)	|
|purchases14d	|Purchases	|
|purchasesViews14d	|Purchases view-through conversions	|
|purchasesClicks14d	|Purchases click-through conversions	|
|purchaseRate14d	|Purchase rate	|
|eCPP14d	|Effective cost per purchase (eCPP)	|
|newToBrandPurchases14d	|New-to-brand purchases	|
|newToBrandPurchasesViews14d	|New-to-brand purchases view-through conversions	|
|newToBrandPurchasesClicks14d	|New-to-brand purchases clicks	|
|newToBrandPurchaseRate14d	|New-to-brand purchase rate	|
|newToBrandECPP14d	|New-to-brand effective cost per purchase	|
|percentOfPurchasesNewToBrand14d	|Percent of purchases new-to-brand	|
|addToWatchlist14d	|Add to Watchlist (ATW)	|
|addToWatchlistViews14d	|Add to Watchlist view-through conversions (ATW views)	|
|addToWatchlistClicks14d	|Add to Watchlist click-through conversions (ATW clicks)	|
|addToWatchlistCVR14d	|Add to Watchlist rate (ATWR)	|
|addToWatchlistCPA14d	|Effective cost per Add to Watchlist (eCPATW)	|
|downloadedVideoPlays14d	|Downloaded video plays (DVP)	|
|downloadedVideoPlaysViews14d	|Downloaded video play view-through conversions (DVP views)	|
|downloadedVideoPlaysClicks14d	|Downloaded video play click-through conversions (DVP clicks)	|
|downloadedVideoPlayRate14d	|Downloaded video play rate (DVPR)	|
|eCPDVP14d	|Effective cost per downloaded video play (eCPDVP)	|
|videoStreams14d	|Video streams	|
|videoStreamsViews14d	|Video streams view-through conversions	|
|videoStreamsClicks14d	|Video streams click-through conversions	|
|videoStreamsRate14d	|Video streams rate	|
|eCPVS14d	|Effective cost per video stream (eCPVS)	|
|playTrailers14d	|Play trailers	|
|playTrailersViews14d	|Play trailers view-through conversions	|
|playerTrailersClicks14d	|Play trailers click-through conversions	|
|playTrailerRate14d	|Play trailer rate	|
|eCPPT14d	|Effective cost per Play trailer (eCPPT)	|
|rentals14d	|Rentals	|
|rentalsViews14d	|Rentals view-through conversions	|
|rentalsClicks14d	|Rentals click-through conversions	|
|rentalRate14d	|Rental rate	|
|ecpr14d	|Effective cost per rental (eCPR)	|
|videoDownloads14d	|Video downloads	|
|videoDownloadsViews14d	|Video downloads view-through conversions	|
|videoDownloadsClicks14d	|Video downloads click-through conversions	|
|videoDownloadRate14d	|Video download rate	|
|ecpvd14d	|Effective cost per video download (eCPVD)	|
|newSubscribeAndSave14d	|Subscribe & Save subscriptions (SnSS)	|
|newSubscribeAndSaveViews14d	|Subscribe & Save subscriptions view-through conversions (SnSS views)	|
|newSubscribeAndSaveClicks14d	|Subscribe & Save subscriptions click-through conversions (SnSS  clicks)	|
|newSubscribeAndSaveRate14d	|Subscribe & Save subscription rate (SnSSR)	|
|eCPnewSubscribeAndSave14d	|Effective cost per Subscribe & Save subscription (eCPSnSS)	|
|offAmazonConversions14d	|Total pixel conversions	|
|offAmazonViews14d	|Total pixel view-through conversions	|
|offAmazonClicks14d	|Total pixel click-through conversions	|
|offAmazonCVR14d	|Total pixel conversion rate	|
|offAmazonCPA14d	|Total pixel cost per acquisition	|
|marketingLandingPage14d	|Marketing landing page	|
|marketingLandingPageViews14d	|Marketing landing page view-through conversions	|
|marketingLandingPageClicks14d	|Marketing landing page click-through conversions	|
|marketingLandingPageCVR14d	|Marketing landing page conversion rate	|
|marketingLandingPageCPA14d	|Marketing landing page cost per acquisition	|
|subscribe14d	|Subscription page	|
|subscribeViews14d	|Subscription page view-through conversions	|
|subscribeClicks14d	|Subscription page click-through conversions	|
|subscribeCVR14d	|Subscription page conversion rate	|
|subscribeCPA14d	|Subscription page cost per acquisition	|
|signUpPage14d	|Sign up page	|
|signUpPageViews14d	|Sign up page view-through conversions	|
|signUpPageClicks14d	|Sign up page click-through conversions	|
|signUpPageCVR14d	|Sign up page conversion rate	|
|signUpPageCPA14d	|Sign up page cost per acquisition	|
|application14d	|Application	|
|applicationViews14d	|Application view-through conversions	|
|applicationClicks14d	|Application click-through conversions	|
|applicationCVR14d	|Application conversion rate	|
|applicationCPA14d	|Application cost per acquisition	|
|gameLoad14d	|Game load	|
|gameLoadViews14d	|Game load view-through conversions	|
|gameLoadClicks14d	|Game load click-through conversions	|
|gameLoadCVR14d	|Game load conversion rate	|
|gameLoadCPA14d	|Game load cost per acquisition	|
|widgetLoad14d	|Widget load	|
|widgetLoadViews14d	|Widget load view-through conversions	|
|widgetLoadClicks14d	|Widget load click-through conversions	|
|widgetLoadCVR14d	|Widget load conversion rate	|
|widgetLoadCPA14d	|Widget load cost per acquisition	|
|surveyStart14d	|Survey start	|
|surveyStartViews14d	|Survey start view-through conversions	|
|surveyStartClicks14d	|Survey start click-through conversions	|
|surveyStartCVR14d	|Survey start conversion rate	|
|surveyStartCPA14d	|Survey start cost per acquisition	|
|surveyFinish14d	|Survey finish	|
|surveyFinishViews14d	|Survey finish view-through conversions	|
|surveyFinishClicks14d	|Survey finish click-through conversions	|
|surveyFinishCVR14d	|Survey finish conversion rate	|
|surveyFinishCPA14d	|Survey finish cost per acquisition	|
|bannerInteraction14d	|Banner interaction	|
|bannerInteractionViews14d	|Banner interaction view-through conversions	|
|bannerInteractionClicks14d	|Banner interaction click-through conversions	|
|bannerInteractionCVR14d	|Banner interaction conversion rate	|
|bannerInteractionCPA14d	|Banner interaction cost per acquisition	|
|widgetInteraction14d	|Widget interaction	|
|widgetInteractionViews14d	|Widget interaction view-through conversions	|
|widgetInteractionClicks14d	|Widget interaction click-through conversions	|
|widgetInteractionCVR14d	|Widget interaction conversion rate	|
|widgetInteractionCPA14d	|Widget interaction cost per acquisition	|
|gameInteraction14d	|Game interaction	|
|gameInteractionViews14d	|Game interaction view-through conversions	|
|gameInteractionClicks14d	|Game interaction click-through conversions	|
|gameInteractionCVR14d	|Game interaction conversion rate	|
|gameInteractionCPA14d	|Game interaction cost per acquisition	|
|emailLoad14d	|Email load	|
|emailLoadViews14d	|Email load view-through conversions	|
|emailLoadClicks14d	|Email load click-through conversions	|
|emailLoadCVR14d	|Email load conversion rate	|
|emailLoadCPA14d	|Email load cost per acquisition	|
|emailInteraction14d	|Email interaction	|
|emailInteractionViews14d	|Email interaction view-through conversions	|
|emailInteractionClicks14d	|Email interaction click-through conversions	|
|emailInteractionCVR14d	|Email interaction conversion rate	|
|emailInteractionCPA14d	|Email interaction cost per acquisition	|
|submitButton14d	|Submit button	|
|submitButtonViews14d	|Submit button view-through conversions	|
|submitButtonClicks14d	|Submit button click-through conversions	|
|submitButtonCVR14d	|Submit button pixel conversion rate	|
|submitButtonCPA14d	|Submit button cost per acquisition	|
|purchaseButton14d	|Purchase button	|
|purchaseButtonViews14d	|Purchase button view-through conversions	|
|purchaseButtonClicks14d	|Purchase button click-through conversions	|
|purchaseButtonCVR14d	|Purchase button conversion rate	|
|purchaseButtonCPA14d	|Purchase button cost per acquisition	|
|clickOnRedirect14d	|Click on redirect	|
|clickOnRedirectViews14d	|Click on redirect view-through conversions	|
|clickOnRedirectClicks14d	|Click on redirect click-through conversions	|
|clickOnRedirectCVR14d	|Click on redirect conversion rate	|
|clickOnRedirectCPA14d	|Click on redirect cost per acquisition	|
|signUp14d	|Sign up button	|
|signUpViews14d	|Sign up button view-through conversions	|
|signUpClicks14d	|Sign up button click-through conversions	|
|signUpCVR14d	|Sign up button conversion rate	|
|signUpCPA14d	|Sign up button cost per acquisition	|
|subscriptionButton14d	|Subscription button	|
|subscriptionButtonViews14d	|Subscription button view-through conversions	|
|subscriptionButtonClicks14d	|Subscription button click-through conversions	|
|subscriptionButtonCVR14d	|Subscription button conversion rate	|
|subscriptionButtonCPA14d	|Subscription button cost per acquisition	|
|successPage14d	|Success page	|
|successPageViews14d	|Success page view-through conversions	|
|successPageClicks14d	|Success page click-through conversions	|
|successPageCVR14d	|Success page conversion rate	|
|successPageCPA14d	|Success page cost per acquisition	|
|thankYouPage14d	|Thank you page	|
|thankYouPageViews14d	|Thank you page view-through conversions	|
|thankYouPageClicks14d	|Thank you page click-through conversions	|
|thankYouPageCVR14d	|Thank you page conversion rate	|
|thankYouPageCPA14d	|Thank you page cost per acquisition	|
|registrationForm14d	|Registration form	|
|registrationFormViews14d	|Registration form view-through conversions	|
|registrationFormClicks14d	|Registration form click-through conversions	|
|registrationFormCVR14d	|Registration form conversion rate	|
|registrationFormCPA14d	|Registration form cost per acquisition	|
|registrationConfirmPage14d	|Registration confirm page	|
|registrationConfirmPageViews14d	|Registration confirm page view-through conversions	|
|registrationConfirmPageClicks14d	|Registration confirm page click-through conversions	|
|registrationConfirmPageCVR14d	|Registration confirm page conversion rate	|
|registrationConfirmPageCPA14d	|Registration confirm page cost per acquisition	|
|storeLocatorPage14d	|Store locator page	|
|storeLocatorPageViews14d	|Store locator page view-through conversions	|
|storeLocatorPageClicks14d	|Store locator page click-through conversions	|
|storeLocatorPageCVR14d	|Store locator page conversion rate	|
|storeLocatorPageCPA14d	|Store locator page cost per acquisition	|
|mobileAppFirstStarts14d	|Mobile app first starts	|
|mobileAppFirstStartViews14d	|Mobile app first start view-through conversions	|
|mobileAppFirstStartClicks14d	|Mobile app first start click-through conversions	|
|mobileAppFirstStartCVR14d	|Mobile app first start conversion rate	|
|mobileAppFirstStartsCPA14d	|Mobile app first start cost per acquisition	|
|brandStoreEngagement1	|Brand store engagement 1	|
|brandStoreEngagement1Views	|Brand store engagement 1 view-through conversions	|
|brandStoreEngagement1Clicks	|Brand store engagement 1 click-through conversions	|
|brandStoreEngagement1CVR	|Brand store engagement 1 conversion rate	|
|brandStoreEngagement1CPA	|Brand store engagement 1 cost per acquisition	|
|brandStoreEngagement2	|Brand store engagement 2	|
|brandStoreEngagement2Views	|Brand store engagement 2 view-through conversions	|
|brandStoreEngagement2Clicks	|Brand store engagement 2 click-through conversions	|
|brandStoreEngagement2CVR	|Brand store engagement 2 conversion rate	|
|brandStoreEngagement2CPA	|Brand store engagement 2 cost per acquisition	|
|brandStoreEngagement3	|Brand store engagement 3	|
|brandStoreEngagement3Views	|Brand store engagement 3 view-through conversions	|
|brandStoreEngagement3Clicks	|Brand store engagement 3 click-through conversions	|
|brandStoreEngagement3CVR	|Brand store engagement 3 conversion rate	|
|brandStoreEngagement3CPA	|Brand store engagement 3 cost per acquisition	|
|brandStoreEngagement4	|Brand store engagement 4	|
|brandStoreEngagement4Views	|Brand store engagement 4 view-through conversions	|
|brandStoreEngagement4Clicks	|Brand store engagement 4 click-through conversions	|
|brandStoreEngagement4CVR	|Brand store engagement 4 conversion rate	|
|brandStoreEngagement4CPA	|Brand store engagement 4 cost per acquisition	|
|brandStoreEngagement5	|Brand store engagement 5	|
|brandStoreEngagement5Views	|Brand store engagement 5 view-through conversions	|
|brandStoreEngagement5Clicks	|Brand store engagement 5 click-through conversions	|
|brandStoreEngagement5CVR	|Brand store engagement 5 conversion rate	|
|brandStoreEngagement5CPA	|Brand store engagement 5 cost per acquisition	|
|brandStoreEngagement6	|Brand store engagement 6	|
|brandStoreEngagement6Views	|Brand store engagement 6 view-through conversions	|
|brandStoreEngagement6Clicks	|Brand store engagement 6 click-through conversions	|
|brandStoreEngagement6CVR	|Brand store engagement 6 conversion rate	|
|brandStoreEngagement6CPA	|Brand store engagement 6 cost per acquisition	|
|brandStoreEngagement7	|Brand store engagement 7	|
|brandStoreEngagement7Views	|Brand store engagement 7 view-through conversions	|
|brandStoreEngagement7Clicks	|Brand store engagement 7 click-through conversions	|
|brandStoreEngagement7CVR	|Brand store engagement 7 conversion rate	|
|brandStoreEngagement7CPA	|Brand store engagement 7 cost per acquisition	|
|addedToShoppingCart14d	|Add to shopping cart (ATSC)	|
|addedToShoppingCartViews14d	|Add to shopping cart view-through conversions	|
|addedToShoppingCartClicks14d	|Add to shopping cart click-through conversions (ATC clicks)	|
|addedToShoppingCartCVR14d	|Add to shopping cart conversion rate	|
|addedToShoppingCartCPA14d	|Add to shopping cart cost per acquisition	|
|offAmazonPurchases14d	|Product purchased	|
|offAmazonPurchasesViews14d	|Product purchased view-through conversions	|
|offAmazonPurchasesClicks14d	|Product purchased click-through conversions	|
|offAmazonPurchaseRate14d	|Product purchased conversion rate	|
|offAmazonECPP14d	|Product purchased cost per acquisition	|
|homepageVisit14d	|Homepage visit	|
|homepageVisitViews14d	|Homepage visit view-through conversions	|
|homepageVisitClicks14d	|Homepage visit click-through conversions	|
|homepageVisitCVR14d	|Homepage visit conversion rate	|
|homepageVisitCPA14d	|Homepage visit cost per acquisition	|
|videoStarted	|Video started	|
|videoStartedViews	|Video started view-through conversions	|
|videoStartedClicks	|Video started click-through conversions	|
|videoStartedCVR	|Video started conversion rate	|
|videoStartedCPA	|Video started cost per acquisition	|
|videoCompleted	|Video completed	|
|videoCompletedViews	|Video completed view-through conversions	|
|videoEndClicks	|Video end click-through conversions	|
|videoCompletedCVR	|Video completed conversion rate	|
|videoCompletedCPA	|Video completed cost per acquisition	|
|messageSent14d	|Message sent	|
|messageSentViews14d	|Message sent view-through conversions	|
|messageSentClicks14d	|Message sent click-through conversions	|
|messageSentCVR14d	|Message sent conversion rate	|
|messageSentCPA14d	|Message sent cost per acquisition	|
|mashupClickToPage	|Mashup click to page	|
|mashupClickToPageViews	|Mashup click to page view-through conversions	|
|mashupClickToPageClicks	|Mashup click to page click-through conversions	|
|mashupClickToPageCVR	|Mashup click to page conversion rate	|
|mashupClickToPageCPA	|Mashup click to page cost per acquisition	|
|mashupBackupImage	|Mashup backup image	|
|mashupBackupImageViews	|Mashup backup image click view-through conversions	|
|mashupBackupImageClicks	|Mashup backup image click-through conversions	|
|mashupBackupImageCVR	|Mashup backup image conversion rate	|
|mashupBackupImageCPA	|Mashup backup image cost per acquisition	|
|mashupAddToCart14d	|Mashup Add to Cart click	|
|mashupAddToCartViews14d	|Mashup Add to Cart click view-through conversions	|
|mashupAddToCartClicks14d	|Mashup Add to Cart click-through conversions	|
|mashupAddToCartClickCVR14d	|Mashup Add to Cart click pixel conversion rate	|
|mashupAddToCartCPA14d	|Mashup Add to Cart click cost per acquisition	|
|mashupAddToWishlist14d	|Mashup Add to Wishlist click	|
|mashupAddToWishlistViews14d	|Mashup Add to Wishlist click view-through conversions	|
|mashupAddToWishlistClicks14d	|Mashup Add to Wishlist click-through conversions	|
|mashupAddToWishlistCVR14d	|Mashup Add to Wishlist click conversion rate	|
|mashupAddToWishlistCPA14d	|Mashup Add to Wishlist click cost per acquisition	|
|mashupSubscribeAndSave14d	|Mashup Subscribe and Save click	|
|mashupSubscribeAndSaveClickViews14d	|Mashup Subscribe and Save view-through conversions	|
|mashupSubscribeAndSaveClick14d	|Mashup Subscribe and Save click click-through conversions	|
|mashupSubscribeAndSaveCVR14d	|Mashup Subscribe and Save conversion rate	|
|mashupSubscribeAndSaveCPA14d	|Mashup Subscribe and Save click cost per acquisition	|
|mashupClipCouponClick14d	|Mashup Clip Coupon click	|
|mashupClipCouponClickViews14d	|Mashup Clip Coupon view-through conversions	|
|mashupClipCouponClickClicks14d	|Mashup Clip Coupon click click-through conversions	|
|mashupClipCouponClickCVR14d	|Mashup Clip Coupon conversion rate	|
|mashupClipCouponClickCPA14d	|Mashup Clip Coupon cost per acquisition	|
|mashupShopNowClick14d	|Mashup Shop Now click	|
|mashupShopNowClickViews14d	|Mashup Shop Now click view-through conversions	|
|mashupShopNowClickClicks14d	|Mashup Shop Now click click-through conversions	|
|mashupShopNowClickCVR14d	|Mashup Shop Now click conversion rate	|
|mashupShopNowClickCPA14d	|Mashup Shop Now click cost per acquisition	|
|referral14d	|Referral	|
|referralViews14d	|Referral view-through conversions	|
|referralClicks14d	|Referral click-through conversions	|
|referralCVR14d	|Referral conversion rate	|
|referralCPA14d	|Referral cost per acquisition	|
|accept14d	|Accept	|
|acceptViews14d	|Accept view-through conversions	|
|acceptClicks14d	|Accept click-through conversions	|
|acceptCVR14d	|Accept conversion rate	|
|acceptCPA14d	|Accept cost per acquisition	|
|decline14d	|Decline	|
|declineViews14d	|Decline view-through conversions	|
|declineClicks14d	|Decline click-through conversions	|
|declineCVR14d	|Decline conversion rate	|
|declineCPA14d	|Decline cost per acquisition	|
|videoStart	|Video start	|
|videoFirstQuartile	|Video first quartile	|
|videoMidpoint	|Video midpoint	|
|videoThirdQuartile	|Video third quartile	|
|videoComplete	|Video complete	|
|videoCompletionRate	|Video completion rate	|
|ecpvc	|Effective cost per Video complete (eCPVC)	|
|videoPause	|Video pause	|
|videoResume	|Video resume	|
|videoMute	|Video mute	|
|videoUnmute	|Video unmute	|
|dropDownSelection14d	|Drop down selection	|
|dropDownSelectionViews14d	|Drop down selection view-through conversions	|
|dropDownSelectionClicks14d	|Drop down selection click-through conversions	|
|dropDownSelectionCVR14d	|Drop down selection conversion rate	|
|dropDownSelectionCPA14d	|Drop down selection cost per acquisition	|
|brandSearch14d	|Branded searches	|
|brandSearchViews14d	|Branded search views	|
|brandSearchClicks14d	|Branded search clicks	|
|brandSearchRate14d	|Branded search rate	|
|brandSearchCPA14d	|Cost per branded search	|
|grossImpressions	|Gross impressions	|
|grossClickThroughs	|Gross click-throughs	|
|invalidImpressions	|Invalid impressions	|
|invalidClickThroughs	|Invalid click-throughs	|
|invalidImpressionRate	|Invalid impression rate	|
|invalidClickThroughsRate	|Invalid click-throughs rate	|
|country	|Country	|
|region	|State or region	|
|city	|City	|
|designatedMarketAreaCode	|Designated Market Area® code	|
|designatedMarketAreaName	|Designated Market Area® name	|
|postalCode	|Postal Code	|
|placementName	|Placement	|
|placementSize	|Placement size	|
|dealType	|Deal type	|
|siteName	|Site name	|
|supplySourceName	|Supply source name	|
|deal	|Deal	|
|dealID	|Deal ID	|
|reportGranularity	|Report granularity	|
|amazonStandardId	|Amazon Standard Identification Number	|
|parentASIN	|Parent ASIN	|
|marketplace	|Marketplace	|
|brandName	|Brand name	|
|asinConversionType	|ASIN conversion type	|
|featuredASIN	|Featured ASIN	|
|productName	|Product name	|
|productGroup	|Product group	|
|productCategory	|Product category	|
|productSubcategory	|Product subcategory	|
|brandHaloDetailPage14d	|Detail page views - brand halo (DPV BH)	|
|brandHaloDetailPageViews14d	|Detail page views view-through conversions - brand halo (DPV views BH)	|
|brandHaloDetailPageClicks14d	|Detail page views click-through conversions - brand halo (DPV clicks BH)	|
|brandHaloProductReviewPage14d	|Product review page views - brand halo (PRPV BH)	|
|brandHaloProductReviewPageViews14d	|Product review page views view-through conversions - brand halo (PRPV  views BH)	|
|brandHaloProductReviewPageClicks14d	|Product review page views click-through conversions - brand halo (PRPV  clicks BH)	|
|brandHaloAddToList14d	|Add to List - brand halo (ATL BH)	|
|brandHaloAddToListViews14d	|Add to List view-through conversions - brand halo (ATL views BH)	|
|brandHaloAddToListClicks14d	|Add to List click-through conversions - brand halo (ATL clicks BH)	|
|brandHaloAddToCart14d	|Add to Cart - brand halo (ATC BH)	|
|brandHaloAddToCartViews14d	|Add to Cart view-through conversions - brand halo (ATC views BH)	|
|brandHaloAddToCartClicks14d	|Add to Cart click-through conversions - brand halo (ATC clicks BH)	|
|brandHaloPurchases14d	|Purchases - brand halo	|
|brandHaloPurchasesViews14d	|Purchases view-through conversions - brand halo	|
|brandHaloPurchasesClicks14d	|Purchases click-through conversions - brand halo	|
|brandHaloNewToBrandPurchases14d	|New-to-brand purchases - brand halo	|
|brandHaloNewToBrandPurchasesViews14d	|New-to-brand purchases view-through conversions - brand halo	|
|brandHaloNewToBrandPurchasesClicks14d	|New-to-brand purchases click-through conversions - brand halo	|
|brandHaloPercentOfPurchasesNewToBrand14d	|Percent of purchases new-to-brand - brand halo	|
|brandHaloNewSubscribeAndSave14d	|Subscribe & Save subscriptions - brand halo (SnSS BH)	|
|brandHaloNewSubscribeAndSaveViews14d	|Subscribe & Save subscriptions view-through conversions - brand halo  (SnSS views BH)	|
|brandHaloNewSubscribeAndSaveClicks14d	|Subscribe & Save subscriptions click-through conversions - brand halo  (SnSS clicks BH)	|
|brandHaloTotalUnitsSold14d	|Product units sold - brand halo	|
|brandHaloTotalSales14d	|Product sales - brand halo	|
|brandHaloTotalNewToBrandSales14d	|New-to-brand product sales - brand halo	|
|brandHaloTotalNewToBrandUnitsSold14d	|New-to-brand product units sold - brand halo	|
|operatingSystem	|Operating system	|
|browser	|Browser	|
|browserVersion	|Browser version	|
|device	|Device	|
|environmentType	|Environment type	|

### Missing metrics

Certain metrics that are available in the DSP console report center are not available in the API. 

|Metric	|Report types	|
|---	|---	|
|Line  item comments	|Campaign,  geography	|
|Cumulative reach	|Campaign	|
|Average impression frequency	|Campaign	|
|Off-Amazon conversions	|Campaign, inventory,  geography, audience, technology	|
|Off-Amazon view-through  conversions	|Campaign, inventory,  geography, audience, technology	|
|Off-Amazon click-through  conversions	|Campaign, inventory,  geography, audience, technology	|
|Off-Amazon conversion rate	|Campaign, inventory,  audience	|
|Off-Amazon cost per  acquisition	|Campaign, inventory,  audience	|
|Subscribe	|Campaign, inventory,  geography, audience, technology	|
|Subscribe view-through  conversions	|Campaign, inventory,  geography, audience, technology	|
|Subscribe click-through  conversions	|Campaign, inventory,  geography, audience, technology	|
|Subscribe conversion rate	|Campaign, inventory,  audience	|
|Subscribe cost per acquisition	|Campaign, inventory,  audience	|
|Sign-up	|Campaign, inventory,  geography, audience, technology	|
|Sign-up view-through  conversions	|Campaign, inventory,  geography, audience, technology	|
|Sign-up click-through  conversions	|Campaign, inventory,  geography, audience, technology	|
|Sign-up conversion rate	|Campaign, inventory,  audience	|
|Sign-up cost per acquisition	|Campaign, inventory,  audience	|
|Add to shopping cart  view-through conversions (ATSC views)	|Campaign, inventory,  geography, audience, technology	|
|Add to shopping cart  click-through conversions (ATSC clicks)	|Campaign, inventory,  geography, audience, technology	|
|Add to shopping cart  conversion rate (ATSC CVR)	|Campaign, inventory,  audience	|
|Add to shopping cart cost per  acquisition (ATSC CPA)	|Campaign, inventory,  audience	|
|Off-Amazon purchases	|Campaign, inventory,  geography, audience, technology	|
|Off-Amazon purchases  view-through conversions	|Campaign, inventory,  geography, audience, technology	|
|Off-Amazon purchases  click-through conversions	|Campaign, inventory,  geography, audience, technology	|
|Off-Amazon purchase rate	|Campaign, inventory,  audience	|
|Off-Amazon effective cost per  purchase (eCPP)	|Campaign, inventory,  audience	|
|Subscription sign-up purchases	|Campaign, inventory,  geography, audience, technology, products	|
|Subscription sign-up purchase  views	|Campaign, inventory,  geography, audience, technology, products	|
|Subscription sign-up purchase  clicks	|Campaign, inventory,  geography, audience, technology, products	|
|Subscription sign-up rate	|Campaign, inventory,  audience	|
|Cost per subscription	|Campaign, inventory,  audience	|
|Free trial subscription  sign-up purchases	|Campaign, inventory,  geography, audience, technology, products	|
|Free trial subscription  sign-up purchase views	|Campaign, inventory,  geography, audience, technology, products	|
|Free trial subscription  sign-up purchase clicks	|Campaign, inventory,  geography, audience, technology, products	|
|Free trial subscription  sign-up rate	|Campaign, inventory,  audience	|
|Cost per free trial  subscription	|Campaign, inventory,  audience	|
|Paid subscription sign-up  purchases	|Campaign, inventory,  geography, audience, technology, products	|
|Paid subscription sign-up  purchase views	|Campaign, inventory,  geography, audience, technology, products	|
|Paid subscription sign-up  purchase clicks	|Campaign, inventory,  geography, audience, technology, products	|
|Paid subscription sign-up rate	|Campaign, inventory,  audience	|
|Cost per paid subscription	|Campaign, inventory,  audience	|
|Appstore opens	|Campaign, products	|
|Appstore opens views	|Campaign, products	|
|Appstore opens clicks	|Campaign, products	|
|Appstore open rate	|Campaign	|
|Cost per appstore opens	|Campaign	|
|Appstore first opens	|Campaign, products	|
|Appstore first-open views	|Campaign, products	|
|Appstore first-open clicks	|Campaign, products	|
|Appstore first-open rate	|Campaign	|
|Cost per appstore first open	|Campaign	|
|Appstore first-session hours	|Campaign, products	|
|Appstore first-session hours  clicks	|Campaign, products	|
|Appstore first-session hours  views	|Campaign, products	|
|Appstore usage hours	|Campaign, products	|
|Appstore usage hours clicks	|Campaign, products	|
|Appstore usage hours views	|Campaign, products	|
|Video impression	|Campaign, inventory	|
|Video click	|Campaign, inventory	|
|Video creative view	|Campaign, inventory	|
|Video end state click	|Campaign, inventory	|
|Video replay	|Campaign, inventory	|
|Video skip back	|Campaign, inventory	|
|Video skip forward	|Campaign, inventory	|
|Audio view	|Campaign, inventory,  geography, audience, technology	|
|Audio start	|Campaign, inventory,  geography, audience, technology	|
|Audio first quartile	|Campaign, inventory,  geography, audience, technology	|
|Audio midpoint	|Campaign, inventory,  geography, audience, technology	|
|Audio third quartile	|Campaign, inventory,  geography, audience, technology	|
|Audio complete	|Campaign, inventory,  geography, audience, technology	|
|Audio pause	|Campaign, inventory,  geography, audience, technology	|
|Audio resume	|Campaign, inventory,  geography, audience, technology	|
|Audio rewind	|Campaign, inventory,  geography, audience, technology	|
|Audio skip	|Campaign, inventory,  geography, audience, technology	|
|Audio mute	|Campaign, inventory,  geography, audience, technology	|
|Audio unmute	|Campaign, inventory,  geography, audience, technology	|
|Audio progress	|Campaign, inventory,  geography, audience, technology	|
|Audio companion banner view	|Campaign, inventory,  geography, audience, technology	|
|Audio companion banner click	|Campaign, inventory,  geography, audience, technology	|
|Audio completion rate	|Campaign, inventory,  audience	|
|Effective cost per audio  complete (eCPAC)	|Campaign, inventory,  audience	|
|App subscription sign-ups	|Campaign, inventory,  geography, technology, products	|
|App subscription sign-up views	|Campaign, inventory,  geography, technology, products	|
|App subscription sign-up  clicks	|Campaign, inventory,  geography, technology, products	|
|App subscription sign-up rate	|Campaign, inventory,  geography, technology	|
|Cost per app subscription	|Campaign, inventory,  geography, technology	|
|Free trial app subscription  sign-ups	|Campaign, inventory,  geography, technology, products	|
|Free trial app subscription  sign-up views	|Campaign, inventory,  geography, technology, products	|
|Free trial app subscription  sign-up clicks	|Campaign, inventory,  geography, technology, products	|
|Free trial app subscription  sign-up rate	|Campaign, inventory,  geography, technology	|
|Cost per free trial app  subscription	|Campaign, inventory,  geography, technology	|
|Paid app subscription sign-ups	|Campaign, inventory,  geography, technology, products	|
|Paid app subscription sign-ups  views	|Campaign, inventory,  geography, technology, products	|
|Paid app subscription sign-ups  clicks	|Campaign, inventory,  geography, technology, products	|
|Paid app subscription sign-up  rate	|Campaign, inventory,  geography, technology	|
|Cost per paid app subscription	|Campaign, inventory,  geography, technology	|
|Interactive STV impressions	|Campaign	|
|Interactive STV Buy and Shop  Now invoked	|Campaign	|
|Interactive STV call to action  engagement	|Campaign	|
|Interactive STV Add to List  invoked	|Campaign	|
|Interactive STV Add to Cart  invoked	|Campaign	|
|Interactive STV Send email  invoked	|Campaign	|
|Interactive STV Learn More  invoked	|Campaign	|
|Interactive STV Send Me More  invoked	|Campaign	|
|Interactive STV Shopping Shop  Now invoked	|Campaign	|
|Interactive STV Quick Look  invoked	|Campaign	|
|Interactive STV Shopping Add  To Cart Quick Look invoked	|Campaign	|
|Interactive STV call to action  engagement rate	|Campaign	|
|Creative	|Inventory	|
|Creative ID	|Inventory	|
|Targeting status	|Audience	|
|PO number	|Industry Standard	|
|Order budget	|Industry Standard	|
|Order start date	|Industry Standard	|
|Order end date	|Industry Standard	|
|Line item external ID	|Industry Standard	|
|Line item budget	|Industry Standard	|
|Line item start date	|Industry Standard	|
|Line item end date	|Industry Standard	|
|Impressions	|Industry Standard	|
|Gross impressions	|Industry Standard	|
|Click-throughs	|Industry Standard	|
|Gross click-throughs	|Industry Standard	|
