---
title: Managing Sponsored Brands optimization rules
description: A tutorial for managing optimization rules for Sponsored Brands, with example requests and responses.
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - optimization rules
    - cost controls
---

# Overview

Sponsored Brands [optimization rules (beta) API](sponsored-brands/3-0/openapi/prod#tag/Optimization-rules) simplifies the campaign management process. With only a few inputs, you can set-up performance-based parameters to remove the guess work from selecting an exact bid for each target.

For campaigns with a goal of PAGE_VISIT, you can apply a cost-based rule for clicks (CPC). We will then utilize machine learning models to define the bids while adjusting base bids up and down with the goal of increasing outcomes, all while adhering to the cost-based rules. Additionally, we may add targets while your campaign is running to try to stay at or below a cost per order value you have specified. 


## How it works

Advertisers specify an advertising objective in the form of their campaign goal, the products they’d like to advertise, a daily budget, and an average cost they are willing to pay per click or 1k view-able impressions. There are two important levers that can help influence marketing goals and costs today.

* Targets selected: Different targeting allows advertisers to reach different audiences, and ads may be more relevant to some than others. When selecting targets, they should be 1) relevant to the advertised products and 2) broad enough to help advertisers reach as many shoppers as possible. The machine learning models that serve ads will be adjusting bids in real time for every opportunity to ensure the cost stays at the desired value, wherever the ad is rendered, while only paying when it works.
* Bid values: Bids signal the most an advertiser is willing to pay for an action (view or click) on each of the targets. With how the Amazon Ads auction’s work, advertisers only pay the least bid amount needed in order to win, so they don’t need to worry about strategizing about how to set bids.

When advertisers use campaign goal, our machine learning models help choose the right targets and bids that can drive a desired action at the specified (maximum) cost and budget. We want each campaign to achieve as many possible actions as long as the costs are under the specified target cost per action (CPA), without spending more than the daily budget. Bids and targets are optimized by models that look at the past campaign performance and similar campaigns: if costs are too high, bids are reduced, and if they’re below the target, they are increased to get more total scale. Bids and targets can be quickly adjusted based on changes in competition (for example, another brand is targeting similar audiences with higher willingness to pay, or traffic changes due to seasonal factors) to ensure advertisers stay within their desired range, all while getting as much scale as possible. While Amazon Ads cannot guarantee performance, if the campaign is not meeting the cost target over a 21-day period (excluding special days), we recommend adjusting cost target or budget to allow our systems to achieve that goal.

### Step 1: Create a campaign

The first step is to create a campaign using the [POST /sb/v4/campaigns](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredBrandsCampaigns) endpoint. A successful call returns a `207` response code and indicates the campaignId of the campaign created. 

If campaign's goal is selected, `smartDefault` will be automatically added with "TARGETING" to help apply dynamic [theme based keywords](guides/sponsored-brands/targeting/theme-based-targeting), and in the future, "OPTIMIZATION" can be automatically added with an optimization rule with a recommended cost threshold based on the goal and promoted page. Otherwise, you can set the campaign's `smartDefault`  to "MANUAL" for complete targeting and bid controls. For additional ad group and ad creation instructions, see [Getting started with campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-2-create-ad-group).

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbcampaignresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
    "campaigns": [
        {
            "budgetType": "DAILY",
            "name": "Page Visit -goal based campaign",
            "state": "ENABLED",
            "startDate": "2024-06-01",
            "budget": 10,
            "goal": "PAGE_VISIT",
            "costType": "CPC",
            "smartDefault": ["TARGETING"]
        }
    ]
}'
```

### Step 2: Create an optimization rule

#### (Optional) 2.a: Retrieve recommended metric using the Optimization Recommendations API

Retrieve a recommended optimization rule metric using [POST /sb/recommendations/optimization](sponsored-brands/3-0/openapi/prod#tag/Recommendations/operation/SBOptimizationRecommendation). This API uses your landing page metadata and associated brand data to calculate an effective target metric, balancing auction performance with cost-effectiveness.

You can pass the same landing page information used in [ad endpoints](guides/sponsored-brands/ads/overview), either prior to creating them or after.

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/recommendations/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sboptimizationrecommendationresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
  "costControlMetric": "COST_PER_CLICK",
  "landingPages": [
    {
      "pageType": "PRODUCT_LIST",
      "asins": ["
        AA01234567",
        "AB01234567",
        "AC01234567"
      ]
    },
    {
      "pageType": "CUSTOM_URL",
      "url": "https://amazon.com/custom_url"
    },
    {
      "asins": [
        "AA01234567"
      ],
      "pageType": "DETAIL_PAGE",
      "url": "https://amazon.com/dp/AA01234567"
    }
  ]
}'
```

The response includes a recommended value to set for your optimization rule campaign along with a minimum recommended value.

```shell
{
  "costControlMetric": "COST_PER_CLICK",
  "recommendedValue": 2.5,
  "minimumValue": 0.5
}
```

#### 2.b: Create optimization rule and associate a campaign

Create an optimization rule using the [POST /sb/rules/optimization](sponsored-brands/3-0/openapi/prod#tag/Optimization-rules/operation/CreateSponsoredBrandsOptimizationRules) endpoint. This will help each campaign achieve as many possible actions as long as the costs are under the specified target cost.  

Use the `campaignId`  from step 1 in the `entityId`. A successful call returns a `207` response code and indicates the `optimizationRuleId` of the campaign created.

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/rules/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbruleoptimization.v4+json' \
--header 'Content-Type: application/json' \
--data '{
  "optimizationRules": [
    {
      "entityType": "CAMPAIGN",
      "entityId": "12345",
      "conditions": [
        {
          "criteria": {
            "comparisonOperator": "LESS_THAN_OR_EQUAL_TO",
            "value": 2.5
          },
          "attributeName": "COST_PER_CLICK"
        }
      ]
    }
  ]
}'
```

### Step 3: Manage optimization rules for your campaigns 

If you need to adjust or update an optimization rule, use the [PUT /sb/rules/optimization](sponsored-brands/3-0/openapi/prod#tag/Optimization-rules/operation/UpdateSponsoredBrandsOptimizationRules) endpoint. Similarly, if you want to disassociate an optimization rule from a campaign, you can leverage the [POST /sb/rules/optimization/disassociate](sponsored-brands/3-0/openapi/prod#tag/Optimization-rules/operation/DisassociateSponsoredBrandsOptimizationRules) endpoint.  

#### 3.a Update a rule

```shell
curl --location --request PUT 'https://advertising-api.amazon.com/sb/rules/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbruleoptimization.v4+json' \
--header 'Content-Type: application/json' \
--data '{
  "optimizationRules": [
    {
      "optimizationRuleId": "111222",
      "conditions": [
        {
          "criteria": {
            "comparisonOperator": "LESS_THAN_OR_EQUAL_TO",
            "value": 2.0
          },
          "attributeName": "COST_PER_CLICK"
        }
      ]
    }
  ]
}'
```

#### 3.b Disassociate a rule

You can dissociate a rule by calling the following endpoint. Once the rule has been dissociated from the campaign, you can re-use the same rule by calling the associate endpoint.

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/rules/optimization/disassociate' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbruleoptimization.v4+json' \
--header 'Content-Type: application/json' \
--data '{
  "optimizationRuleDisassociations": [
    {
      "entityType": "CAMPAIGN",
      "entityId": "12345",
      "optimizationRuleId": "111222"
    }
  ]
}'
```

#### 3.c Associate optimization rule to existing campaign 

To associate a rule to an existing campaign, use the [POST /sb/rules/optimization/associate](sponsored-brands/3-0/openapi/prod#tag/Optimization-rules/operation/AssociateSponsoredBrandsOptimizationRules) endpoint. You can use the `optimizationRuleId` that doesn’t have an association with a campaign. A successful call returns a `207` response code and indicates that the campaign has been associated with the `optimizationRuleId` passed in the request.

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/rules/optimization/associate' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbruleoptimization.v4+json' \
--header 'Content-Type: application/json' \
--data '{
  "optimizationRuleAssociations": [
    {
      "entityType": "CAMPAIGN",
      "entityId": "[campaignId]",
      "optimizationRuleId": "[optimizationRuleId]"
    }
  ]
}'
```
