---
title: Creating Sponsored Products campaigns
description: Creating Sponsored Products campaigns with the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - Campaigns
---

# Sponsored Products campaigns

Campaigns are the highest level of the Sponsored Products entity hierarchy. You can learn more about the other components of campaigns in [Campaign hierarchy](guides/sponsored-products/get-started/campaign-structure).

Budget, targeting type, start date, and end date are all defined at the campaign level.

Learn more:

* [Building an auto targeting campaign](guides/sponsored-products/get-started/auto-campaigns)
* [Building a manual campaign](guides/sponsored-products/get-started/manual-campaigns)

## Request

### Endpoint 

[POST /sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredProductsCampaigns)

### Parameters

|Name	|Description	|
|---	|---	|
|portfolioId	|Portfolios allow advertisers to organize campaigns into groups based on business needs. Learn more about [Portfolios](https://advertising.amazon.com/help?#GPEJN2E6R52G7C2T), or see how to [Create a Portfolio](reference/2/portfolios#/Portfolios/createPortfolios).	|
|name	|A name for the campaign. Note that idempotency for this field works differently for sellers and vendors. Sellers aren't allowed to have duplicate campaign names, but vendors can have duplicate campaign names.	|
|targetingType	|Options are either `MANUAL` or `AUTO`. Learn more about [auto campaigns](guides/sponsored-products/get-started/auto-campaigns) and [manual campaigns.](guides/sponsored-products/get-started/manual-campaigns)	|
|state	|Options are `ENABLED`, or `PAUSED`. If you don't want your campaign to serve right away, set state to `PAUSED`. 	|
|budget.budgetType	|The only supported value is `DAILY`.	|
|budget.budget	|Amount in the currency for your profile. The maximum and minimum budgets allowed depend on your geography. For details, see [Budget constraints by marketplace](reference/concepts/limits#sponsored-products). |
|startDate	|The start date for the campaign in the form YYYY-MM-DD. This date can't be in the past. Note: If set to today and state is set to `ENABLED`, the campaign begins serving as soon as the campaign has an active ad group, product ad, and targeting clause or keyword.	|
|endDate	|The end date for the campaign. If not specified, the campaign will not have an end date.	|
|dynamicBidding.strategy	|Bidding strategy for the campaign. Possible values are `LEGACY_FOR_SALES`, `AUTO_FOR_SALES`, `MANUAL`, and `RULE_BASED`. [Learn more about bidding strategies.](https://advertising.amazon.com/help?#GCU2BUWJH2W3A8Z7)	|
|dynamicBidding.placementBidding.percentage	|The percentage you want to adjust bids for the specified placement.	|
|dynamicBidding.placementBidding.placement	|The Amazon.com placement you want to adjust the bid for. [Learn more about placements.](https://advertising.amazon.com/help?#GYYZVM7LGSRYGWV5) 	|
|tags	|Optional tags your can add to your campaign for administrative or reporting purposes. 	|

### Examples

#### Manual campaign

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxx' \
--header 'Accept: application/vnd.spCampaign.v3+json' \
--header 'Content-Type: application/vnd.spCampaign.v3+json' \
--data-raw '{
  "campaigns": [
    {
      "endDate": "2022-11-08",
      "name": "Test manual targeting SP campaign",
      "targetingType": "MANUAL",
      "state": "ENABLED",
      "dynamicBidding": {
        "placementBidding": [
          {
            "percentage": 900,
            "placement": "PLACEMENT_TOP"
          }
        ],
        "strategy": "LEGACY_FOR_SALES"
      },
      "startDate": "2022-10-31",
      "budget": {
        "budgetType": "DAILY",
        "budget": 100
      }
    }
  ]
}'
```

#### Auto campaign

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Accept: application/vnd.spCampaign.v3+json' \
--header 'Content-Type: application/vnd.spCampaign.v3+json' \
--data-raw '{
  "campaigns": [
    {
      "endDate": "2022-11-08",
      "name": "Test SP auto campaign",
      "targetingType": "AUTO",
      "state": "ENABLED",
      "dynamicBidding": {
        "placementBidding": [
          {
            "percentage": 900,
            "placement": "PLACEMENT_TOP"
          }
        ],
        "strategy": "LEGACY_FOR_SALES"
      },
      "startDate": "2022-10-31",
      "budget": {
        "budgetType": "DAILY",
        "budget": 100
      }
    }
  ]
}'
```

## Response

A successful call returns a 207 response code and indicates the indexes and campaign IDs of the campaigns created. 


```
{
    "campaigns": {
        "error": [],
        "success": [
            {
                "campaignId": "218293333782035",
                "index": 0
            }
        ]
    }
}
```

>[TIP] If you want to receive the full campaign objects in the response, use the `Prefer` header set to `return=representation`.

## Next steps

Once you have created a campaign, you need to create at least one ad group.

* [Create an ad group](guides/sponsored-products/ad-groups)

