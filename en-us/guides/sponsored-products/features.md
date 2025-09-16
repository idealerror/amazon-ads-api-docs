---
title: Sponsored Product Features Availability by Marketplace
description: Lists descriptions and marketplace availability for all Sponsored Products features.
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - Feature by marketplace
    - Marketplace
    - Features
---

# Sponsored Products feature availability by marketplace

This document lists descriptions and marketplace availability for all Sponsored Products features.


## Campaigns

Campaigns allow you to advertise your products in high-visibility placements on Amazon.com. For more information, see [Sponsored Products overview](guides/sponsored-products/overview).


**Relevant resource:**

* [/sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredProductsCampaigns)


**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Turkey (TR), Poland (PL), Belgium (BE)
* Middle East: United Arab Emirates (UAE), Egypt (EG), Saudi Arabia (SA)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)
* Africa: South Africa (ZA)

## Ad groups

This [feature](guides/sponsored-products/ad-groups) provides the ability to organize, manage, and track performance of the products within your campaign.


**Relevant resource:**

* [/sp/adGroups](sponsored-products/3-0/openapi/prod#tag/AdGroups/operation/CreateSponsoredProductsAdGroups)


**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Turkey (TR), Poland (PL), Belgium (BE)
* Middle East: United Arab Emirates (UAE), Egypt (EG), Saudi Arabia (SA)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)
* Africa: South Africa (ZA)

## Product ads


[Product ads](guides/sponsored-products/product-ads) represent the individual products that you want to advertise within your campaign.

**Relevant resource:**

* [/sp/productAds](sponsored-products/3-0/openapi/prod#tag/ProductAds/operation/CreateSponsoredProductsProductAds)


**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Turkey (TR), Poland (PL), Belgium (BE)
* Middle East: United Arab Emirates (UAE), Egypt (EG), Saudi Arabia (SA)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)
* Africa: South Africa (ZA)

## Keyword targeting


This [feature](guides/sponsored-products/keywords/overview) lets you have more control over your targeting and spend, identifying keywords for which you want your ads to appear and optimizing bids for each.

**Relevant resource:**

* [/sp/keywords](sponsored-products/3-0/openapi/prod#tag/Keywords/operation/CreateSponsoredProductsKeywords)


**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Turkey (TR), Poland (PL), Belgium (BE)
* Middle East: United Arab Emirates (UAE), Egypt (EG), Saudi Arabia (SA)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)
* Africa: South Africa (ZA)

## Negative keyword targeting


This [feature](guides/sponsored-products/negative-targeting/keywords) lets you prevent ads from appearing on shopping results pages that don’t meet your campaign or ad group objectives.

**Relevant resource:**

* [/sp/negativeKeywords](sponsored-products/3-0/openapi/prod#tag/NegativeKeywords/operation/CreateSponsoredProductsNegativeKeywords)
* [/sp/campaignNegativeKeywords](sponsored-products/3-0/openapi/prod#tag/CampaignNegativeKeywords/operation/CreateSponsoredProductsCampaignNegativeKeywords)


**Marketplace availability:** 

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Turkey (TR), Poland (PL), Belgium (BE)
* Middle East: United Arab Emirates (UAE), Egypt (EG), Saudi Arabia (SA)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)
* Africa: South Africa (ZA)

## Product targeting


This [feature](guides/sponsored-products/product-targeting/overview) lets you have more control over your targeting and spend, identifying products for which you want your ads to appear and optimizing bids for each.

**Relevant resource:**

* [/sp/targets](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/CreateSponsoredProductsTargetingClauses)


**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Turkey (TR), Poland (PL), Belgium (BE)
* Middle East: United Arab Emirates (UAE), Egypt (EG), Saudi Arabia (SA)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)
* Africa: South Africa (ZA)



## Rule-based bidding


For any existing campaign running for at least 30 days, you can apply a [bidding](guides/rules/bidding-rules/overview) rule with a guardrail of ROAS and Amazon Ads may then adjust your base bids up and down to increase conversions while maintaining your guardrails.

**Relevant resource:**

* [/sp/rules/campaignOptimization](sponsored-products/3-0/openapi/prod#tag/Campaign-Optimization-Rules/operation/CreateOptimizationRule)

**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* Europe: United Kingdom (UK), Germany (DE), France (FR)
* Asia Pacific: Japan (JP)


## Consolidated recommendations


This feature gets the top consolidated recommendations across bid, budget, and targeting for a campaign. All recommendations are refreshed every day. 

**Relevant resource:**

* [/sp/campaign/recommendations](sponsored-products/3-0/openapi/prod#tag/Consolidated-Recommendations/operation/getCampaignRecommendations)

**Marketplace availability:**

* North America: United States (US)


## Product recommendations


This [feature](guides/sponsored-products/product-targeting/suggestions) returns suggested ASINs to target in a product targeting campaign.

**Relevant resources:**

* [/sp/targets/products/recommendations](sponsored-products/3-0/openapi/prod#tag/Product-Recommendation-Service/operation/getProductRecommendations)

**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Turkey (TR), Poland (PL), Belgium (BE)
* Middle East: United Arab Emirates (UAE), Egypt (EG), Saudi Arabia (SA)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)
* Africa: South Africa (ZA)


## Theme-based bid suggestions (Beta)


This [feature](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide) delivers suggestions and impact metrics for multiple themes, allowing you to choose a bid from a theme that is best aligned with your campaign.

**Relevant resources:** 

* [sp/targets/bid/recommendations](sponsored-products/3-0/openapi/prod#tag/ThemeBasedBidRecommendation/operation/GetThemeBasedBidRecommendationForAdGroup_v1)

**Marketplace availability:**

* North America: United States (US), Canada (CA)
* Europe: Germany (DE), Spain (ES), France (FR), United Kingdom (UK)
* Asia Pacific: Japan (JP), India (IN)

## Budget recommendation


This [feature](guides/sponsored-products/budget-recommendations-and-missed-opportunities) provides an estimated budget needed to keep the campaign in budget for the full 24-hour period, the share of time the campaign was in budget during the past seven days, and estimated impressions the campaign might have generated had it adopted the recommended budget.

**Relevant resources:** 

* [sp/campaigns/budgetRecommendations](sponsored-products/3-0/openapi/prod#tag/Budget-recommendations-and-missed-opportunities/operation/getBudgetRecommendations)

**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* Europe: Germany (DE), Spain (ES), France (FR), United Kingdom (UK), Italy (IT)
* Asia Pacific: Japan (JP), India (IN), Australia (AU)
* Middle East: United Arab Emirates (UAE)


