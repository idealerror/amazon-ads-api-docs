---
title: Creating auto campaigns
description: Tutorial for creating a Sponsored Products auto campaign using the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - auto campaigns
---

# Get started with auto campaigns

Auto campaigns use Amazon’s automated targeting capabilities to assign relevant keywords and targeting expressions to Sponsored Products campaigns. If you are new to creating Sponsored Products campaigns, we recommend starting with an auto campaign to understand what targeting works best for your products. 

>[TIP] If you want to test out the creation flow for Sponsored Products campaigns without worrying about ad spend, you can create a [Test account](guides/account-management/test-accounts/overview) and use it to complete this tutorial. 

## Before you begin

* Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in the tutorial. 
* Understand the [structure of Sponsored products campaigns](guides/sponsored-products/get-started/campaign-structure).

## Steps

>[TIP] You can make all the calls mentioned in this tutorial using the Amazon Ads API [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman).

1. Create a campaign using the [POST sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredProductsCampaigns) endpoint. To create an auto campaign, make sure `targetingType` is set to `auto`. 

    - [More information on creating campaigns](guides/sponsored-products/campaigns).

2. Create at least one ad group using the [POST sp/adGroups](sponsored-products/3-0/openapi/prod#tag/AdGroups/operation/CreateSponsoredProductsAdGroups) endpoint. Each ad group should contain one or more products that share similar characteristics (e.g., price, category, genre).

    - [More information on creating ad groups](guides/sponsored-products/ad-groups).

3. Create at least one product ad per ad group using the [POST sp/productAds](sponsored-products/3-0/openapi/prod#tag/AdGroups/operation/CreateSponsoredProductsAdGroups) endpoint.

    - [More information on creating product ads](guides/sponsored-products/product-ads).

4. (Optional) Set up auto targeting using the [PUT sp/targets](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/UpdateSponsoredProductsTargetingClauses) endpoint. Auto targeting allows you to set bids for targeting groups based on the relationship to the products you are advertising. You can set different bids for search terms that are close matches or loose matches, or product detail pages that are substitutes or complements to your products. If you choose to use auto targeting expressions, the bids you set for each expression replace the default bid for the ad group. 

    >[NOTE] You get the auto targeting expression IDs to use in the PUT operation by calling [POST sp/targets/list](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/ListSponsoredProductsTargetingClauses).

    - [More information on auto targeting](guides/sponsored-products/auto-targeting).
    
5. (Optional) Set up negative targeting. You can use negative keyword targeting or negative product targeting to stop ads from showing on certain search results, brands, or product detail pages. Both types of negative targeting are available at the ad group ([POST sp/negativeKeywords](sponsored-products/3-0/openapi/prod#tag/NegativeKeywords/operation/CreateSponsoredProductsNegativeKeywords) and [POST sp/negativeTargets](sponsored-products/3-0/openapi/prod#tag/NegativeTargetingClauses/operation/CreateSponsoredProductsNegativeTargetingClauses)) and campaign ([POST sp/campaignNegativeKeywords](sponsored-products/3-0/openapi/prod#tag/CampaignNegativeKeywords/operation/CreateSponsoredProductsCampaignNegativeKeywords) and [POST sp/campaignNegativeTargets](sponsored-products/3-0/openapi/prod#tag/CampaignNegativeTargetingClauses/operation/CreateSponsoredProductsCampaignNegativeTargetingClauses)) levels. 

    - [More information on negative keywords](guides/sponsored-products/negative-targeting/keywords).
    - [More information on negative product targeting](guides/sponsored-products/negative-targeting/product-brand).

Once you have created at least one active campaign, ad group, and product ad, your campaign is considered complete and ready to run. If the campaign start date is set to today, your campaign starts serving immediately. 

## Next steps

* [Request a report](guides/reporting/v3/get-started) to review performance data. For auto campaigns, we suggest looking at the [Search term report](guides/reporting/v3/report-types#search-term-reports) to identify high performing keywords and ASINs. 
* After you review your performance data, consider [creating a manual campaign](guides/sponsored-products/get-started/manual-campaigns) using your top performing targeting expressions or keywords.


