---
title: Creating Sponsored Products target promotions
description: Creating Sponsored Products target promotions with the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
    - Target promotion
keywords:
    - Target promotion
---

# Target Promotion

Target promotion is a performance advertising strategy that identifies targets for further refinement to drive more efficient spend against high performing targets and reduce spend on low performing targets across all campaigns at scale. This combines the common practice of ‘target harvesting’, which identifies targets to move from broader targeting (e.g., automatic targeting) to more focused targeting (e.g., manual targeting with broad match), with optimized bids and budget changes.


## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in this tutorial.

## Examples

Endpoint: [POST /sp/targetPromotionGroups](sponsored-products/3-0/openapi/prod#tag/TargetPromotionGroups/operation/CreateTargetPromotionGroups)

This endpoint creates a target promotion group and an associated manual keyword targeting campaign, ad group, and ads, as well as an associated manual product targeting campaign, ad group, and ads, based on the given auto-targeting campaign's `adGroupId` and selection of `adIds`.

Additionally, you can use this endpoint to create a target promotion group by specifying existing manual targeting campaign's ad groups to associate with a matching auto-targeting ad group. The match is based on the products (ASIN/SKU) in the auto-targeting ad group's ads.

Request for creating new manual targeting campaigns: 

```json
curl --location --request POST 'https://advertising-api.amazon.com/sp/targetPromotionGroups' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adGroupId": "243564645645645",
    "adIds": ["22343432423423", "12342423423"],
    "newCampaignDetails": {
        "namePrefix": "CampaignABC",
        "startDate": "2023-01-01",
        "endDate": "2025-12-31",
        "dynamicBidding": {
            "strategy": "AUTO_FOR_SALES",
            "placementBidding": {
                "percentage": 50,
                "placement": "PLACEMENT_TOP"
            }
        },
        "budget": {
            "budgetType": "DAILY",
            "budget": 25.00
        },
        "defaultBid": 1.00
    }
}'
```

Request to associate an existing manual targeting campaign ad group with an auto-targeting campaign ad group:

```json
curl --location --request POST 'https://advertising-api.amazon.com/sp/targetPromotionGroups' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adGroupId": "243564645645645",
    "existingCampaignDetails": {
        "keywordCampaignAdGroupIds": [
            "2344325423234234"
        ],
        "productCampaignAdGroupIds": [
            "234254352342321312"
        ]
    }
}'
```


Response:


```json
{
  "targetPromotionGroup": {
    "productCampaignAdGroupIds": [
      "203435345431311"
    ],
    "targetPromotionGroupId": "string",
    "autoTargetingCampaignAdIds": [
      "string"
    ],
    "keywordCampaignAdGroupIds": [
      "234464767877"
    ],
    "state": "ENABLED",
    "targetPromotionGroupName": "TargetingGroup-1",
    "autoTargetingCampaignAdGroupId": "43565324231232"
  }
}
```

Endpoint: [POST /sp/targetPromotionGroups/recommendations](sponsored-products/3-0/openapi/prod#tag/TargetPromotionGroups/operation/GetTargetPromotionGroupsRecommendations)

This endpoint returns the target recommendations for a combination of advertiser, marketplace, and any of these filters: `adGroupId`, `adId`, and `campaignId` filter.

In this example, we are filtering results by `adId`:


```json
curl --location --request POST 'https://advertising-api.amazon.com/sp/targetPromotionGroups/recommendations' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
  "maxResults": 1,
  "nextToken": "string",
  "adIdFilter": {
    "include": [
      "{{adId}}",
      "{{adId}}"
    ]
  }
}'
```

Response:


```json
{
   "targets": [
        {
            "campaignId": "232423432423423",
            "adGroupId": "23232432432423",
            "adId": "20344534523113",
            "adAsin": "ASIN1",
            "recommendedTarget":"keyword1",
            "targetType": "KEYWORD",
            "recommendationReasons": [
                {
                    "reason": "HISTORICAL_PERF",
                    "data": {
                        "CLICKS": 10,
                        "CONVERSIONS": 2
                    }
                }
            ]
        },
        {
            "campaignId": "265665797831232",
            "adGroupId": "2454657687699",
            "adId": "2992435433231",
            "adAsin": "ASIN2",
            "recommendedTarget": "ASIN3",
            "targetType": "PRODUCT",
            "recommendationReasons": [
                {
                    "reason": "HISTORICAL_PERF",
                    "data": {
                        "CLICKS": 12,
                        "CONVERSIONS": 25
                    }
                }
            ]               
        }
   ],
   "totalResults": 100
}
```


Endpoint: [POST /sp/targetPromotionGroups/list](sponsored-products/3-0/openapi/prod#tag/TargetPromotionGroups/operation/ListTargetPromotionGroups)

This endpoint returns the target promotion groups for an advertiser, marketplace, and any of these filters: `targetPromotionGroupId`, `adGroupId`, and `campaignId` filter.

In this example, we are filtering results by `adGroupId`:

```json
curl --location --request POST '/sp/targetPromotionGroups/list' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "maxResults": 0,
    "nextToken": "string",
    "targetPromotionGroupIdFilter": {
        "include": [
            "{{targetPromotionGroupId}}"
        ]
    },
    "adGroupIdFilter": {
        "include": [
            "{{adGroupId}}"
        ]
    }
}'
```

Response:


```json
{
    "targetPromotionGroups": [
        {
            "targetPromotionGroup": {
                "productCampaignAdGroupIds": [
                "24235532221314"
                ],
                "targetPromotionGroupId": "string",
                "autoTargetingCampaignAdIds": [
                "43565324231234"
                ],
                "keywordCampaignAdGroupIds": [
                "24235532221313"
                ],
                "state": "ENABLED",
                "targetPromotionGroupName": "TargetingGroup-1",
                "autoTargetingCampaignAdGroupId": "442364767877"
            }
            }
    ],
    "previousToken": "efgergregergerfewqe12e312r243g3rebeger/q===",
    "nextToken": "efgergregergerfewqe12e312r243g3rebeger/q=",
    "totalResults": 50
}
```

```json
{
    "targetPromotionGroups": [
        {
            "targetPromotionGroupId": "203435345431311",
            "targetPromotionGroupName": "TargetingGroup-1",
            "autoTargetingCampaign": {
                "campaignId": "234464767876"
            },
            "keywordCampaigns": [
                "campaignId": "234464767877"
            ],
            "productCampaigns": [
                "campaignId": "234464767878"
            ]
        }
    ],
    "previousToken": "efgergregergerfewqe12e312r243g3rebeger/q===",
    "nextToken": "efgergregergerfewqe12e312r243g3rebeger/q=",
    "totalResults": 50
}
```


Endpoint: [POST /sp/targetPromotionGroups/targets](sponsored-products/3-0/openapi/prod#tag/TargetPromotionGroups/operation/CreateTargetPromotionGroupTargets)

This endpoint assigns keyword and/or product targets to the respective manual targeting ad groups within the specified target promotion group.

Request:

```json
curl --location --request POST '/sp/targetPromotionGroups/targets' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "targetPromotionGroupId": "243564645645645",
    "targets": [
        {
            "target": "keyword1",
            "expressionType": "QUERY_BROAD_MATCHES",
            "bid": 0.75
        },
        {
            "target": "B332432M0",
            "expressionType": "ASIN_EXPANDED_FROM",
            "bid": 0.99
        }
    ]
}'
```


Response:


```json
{
    "success": [{
        "targetDetails": {
            "targetPromotionGroupId": "203435345431311",
            "targetId": "2454365654567658",
            "manualTargetingAdGroupId": "34234324242411221"
        },
        "target": "keyword1"
    }],
    "errors": [{}]
}
```


Endpoint: [POST /sp/targetPromotionGroups/targets/list](sponsored-products/3-0/openapi/prod#tag/TargetPromotionGroups/operation/ListTargetPromotionGroupTargets)

This endpoint retrieves targets created through target promotion group APIs for a specific advertiser and marketplace, and allows filtering by `targetPromotionGroupID`, `adGroupID`, and `campaignID`.

In this example, we are filtering results by `targetPromotionGroupId`:

```json
curl --location --request POST '/sp/targetPromotionGroups/targets/list' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "targetPromotionGroupIdFilter": {
        "include": [
            "{{targetPromotionGroupId}}"
        ]
    },
    "maxResults": 10,
    "nextToken": "Afgdfbfdewfwe/w@44dsvdsFDSrewweewefsdvrt5y65y65htvdqsasv"
}'
```

Response:

```json
{
    "targets": [
        {
            "targetPromotionGroupId": "1243242232312",
            "targetId": "2454365654567658",
            "target": "keyword1",
            "expressionType": "KEYWORD_BROAD",
            "bid": 0.75
        },
        {
            "targetPromotionGroupId": "1243242232312",
            "targetId": "2454365654567659",
            "target": "B332432M0",
            "expressionType": "ASIN_EXPANDED_FROM",
            "bid": 0.99
        }
    ],
    "previousToken": "efgergregergerfewqe12e312r243g3rebeger/q===",
    "nextToken": "efgergregergerfewqe12e312r243g3rebeger/q=",
    "totalResults": 50
}
```


