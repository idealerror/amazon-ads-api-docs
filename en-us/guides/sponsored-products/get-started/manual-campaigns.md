---
title: Creating manual campaigns
description: Tutorial for creating a Sponsored Products manual campaign using the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - manual campaigns
---

# Get started with manual campaigns 

Manual campaigns give advertisers full control over targeting, including manually creating product targeting expressions and adding keywords. If you are new to creating Sponsored Products campaigns, we recommend starting with an [auto campaign](guides/sponsored-products/get-started/auto-campaigns). 

>[TIP] If you want to test out the creation flow for Sponsored Products campaigns without worrying about ad spend, you can create a [Test account](guides/account-management/test-accounts/overview) and use it to complete this tutorial. 

## Before you begin

* Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in the tutorial. 
* Understand the [general structure for Sponsored Products campaigns](guides/sponsored-products/get-started/campaign-structure).
* (Optional) [Create an auto campaign](guides/sponsored-products/get-started/auto-campaigns) so you can identify keywords or targeting expressions to use in your manual campaign.

## Steps

>[TIP] You can make all the calls mentioned in this tutorial using the Amazon Ads API [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman).

1. Create a campaign using the [POST sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredProductsCampaigns) endpoint. To create a manual campaign, make sure `targetingType` is set to `manual`. 

    - [More information on creating campaigns](guides/sponsored-products/campaigns).

2. Create at least one ad group using the [POST sp/adGroups](sponsored-products/3-0/openapi/prod#tag/AdGroups/operation/CreateSponsoredProductsAdGroups) endpoint. Each ad group should contain one or more products that share similar characteristics (e.g., price, category, genre).

    - [More information on creating ad groups](guides/sponsored-products/ad-groups).

3. Create at least one product ad per ad group using the [POST sp/productAds](sponsored-products/3-0/openapi/prod#tag/ProductAds/operation/CreateSponsoredProductsProductAds) endpoint.

    - [More information on creating product ads](guides/sponsored-products/product-ads).

4. Add targeting to the ad group. Ad groups for manual campaigns must have at least one associated targeting expression (product or category) or keyword. 
    - **Product targeting**
        1. Get recommended ASINs to target using the [POST /sp/targets/products/recommendations](sponsored-products/3-0/openapi/prod#tag/Product-Recommendation-Service/operation/getProductRecommendations) endpoint. [Learn more.](guides/sponsored-products/product-targeting/suggestions)
        2. Get bid recommendations for the targeting expression using the [POST /sp/targets/bid/recommendations](sponsored-products/2-0/openapi#/prod#tag/ThemeBasedBidRecommendation/operation/GetThemeBasedBidRecommendationForAdGroup_v1) endpoint.
        3. Create a product targeting expression using the [POST /sp/targets](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/CreateSponsoredProductsTargetingClauses) endpoint. [Learn more.](guides/sponsored-products/product-targeting/overview#targeting-individual-products)
    - **Category targeting**
        1. Explore available categories using the [GET /sp/targets/categories](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getTargetableCategories) endpoint, or get suggested categories to target using the [POST /sp/targets/categories/recommendations](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getCategoryRecommendationsForASINs) endpoint.
        2. Explore what refinements are available for your desired categories (brands, age ranges, genres) using the [GET sp/targets/category/{categoryId}/refinements](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getRefinementsForCategory) endpoint.
        3. Check the size of category with refinements using the [POST /sp/targets/products/count](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getTargetableASINCounts). 
        4. Get bid recommendations for the targeting expression using the [POST /sp/targets/bid/recommendations](sponsored-products/2-0/openapi#/prod#tag/ThemeBasedBidRecommendation/operation/GetThemeBasedBidRecommendationForAdGroup_v1) endpoint. 
        5. Create one or more category targeting expressions using the [POST /sp/targets](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/CreateSponsoredProductsTargetingClauses) endpoint. [Learn more.](guides/sponsored-products/product-targeting/overview#targeting-product-categories)
    - **Keyword targeting**
        1. Get recommended keywords and bids using the [POST /sp/targets/keywords/recommendations](sponsored-products/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getRankedKeywordRecommendation) endpoint.
        2. Create one or more keywords using the [POST sp/keywords](sponsored-products/3-0/openapi/prod#tag/Keywords/operation/CreateSponsoredProductsKeywords) endpoint. [Learn more.](guides/sponsored-products/keywords/overview)
5. (Optional) Set up negative targeting. You can use negative keyword targeting or negative product targeting to stop ads from showing on certain search results, brands, or product detail pages. Both types of negative targeting are available at the ad group ([POST sp/negativeKeywords](sponsored-products/3-0/openapi/prod#tag/NegativeKeywords/operation/CreateSponsoredProductsNegativeKeywords) and [POST sp/negativeTargets](sponsored-products/3-0/openapi/prod#tag/NegativeTargetingClauses/operation/CreateSponsoredProductsNegativeTargetingClauses)) and campaign ([POST sp/campaignNegativeKeywords](sponsored-products/3-0/openapi/prod#tag/CampaignNegativeKeywords/operation/CreateSponsoredProductsCampaignNegativeKeywords) and [POST sp/campaignNegativeTargets](sponsored-products/3-0/openapi/prod#tag/CampaignNegativeTargetingClauses/operation/CreateSponsoredProductsCampaignNegativeTargetingClauses)) levels. 

    - [More information on negative keywords](guides/sponsored-products/negative-targeting/keywords).
    - [More information on negative product targeting](guides/sponsored-products/negative-targeting/product-brand). 

Once you have created at least one active campaign, ad group, product ad, and targeting expression or keyword, your campaign is considered complete and ready to run. If the campaign start date is set to today and all entities have an `ACTIVE` status, your campaign starts serving immediately. 

## Next steps

* [Request a report](guides/reporting/v3/get-started) to review performance data. 

