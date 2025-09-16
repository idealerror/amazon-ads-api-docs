---
title: Automatic targeting
description: Amazon Ads API version 3 Sponsored Products auto targeting reference.
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - Auto targeting
    - Expression types
---

# Sponsored Products auto targeting

>[NOTE] Auto targeting expressions are automatically created for every auto targeting campaign; you don't need to manually create auto targeting expressions. 

Auto targeting expressions let you can choose different bids for groups of search terms and products within an [auto targeting campaign](guides/sponsored-products/get-started/auto-campaigns).

If you don’t set up any auto targeting clauses, your ad group uses the default bid you assigned during ad group creation for all targeting. However, you can use auto targeting expressions to specify different bids for different types of targeting of targeting expressions.

Auto targeting supports four types of targeting based on the ASINs that are assigned to an ad group as a product ad. 

### Supported expression types

|Expression	|Console equivalent	|Description	|
|---	|---	|---	|
|`QUERY_BROAD_REL_MATCHES`	|loose match	|Search terms loosely correlated to your product.	|
|`QUERY_HIGH_REL_MATCHES`	|close match	|Search terms closely related to your product.	|
|`ASIN_ACCESSORY_RELATED`	|complements	|Detail pages for products that complement your product.	|
|`ASIN_SUBSTITUTE_RELATED`	|substitutes	|Detail pages for products that are substitutes for your product.	|


## Before you begin

1. [Create an auto targeting campaign](guides/sponsored-products/campaigns).
2. [Create an ad group](guides/sponsored-products/ad-groups). After this step, four auto targeting expressions are automatically created.
3. [Create at least one product ad in the ad group](guides/sponsored-products/product-ads). 

## Get target IDs

First you need to retrieve the target IDs for the auto targeting expressions created as part of your campaign. 

### Endpoint

[POST /sp/targets/list](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/ListSponsoredProductsTargetingClauses)

### Example

>[TIP] Use a `campaignId` or `adGroupId` filter in the request body to return only the targets for your auto campaign. 

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spTargetingClause.v3+json' \
--header 'Content-Type: application/vnd.spTargetingClause.v3+json' \
--data-raw '{
  "campaignIdFilter": {
    "include": [
      "auto_campaign_id"
    ]
  }
}'

```

The response body should include four auto targeting expressions (one for each supported expression type) that have been created for your ad group. The targeting clauses do not contain a bid at this stage. 

```
{
  "targetingClauses": [
    {
      "adGroupId": "184667233956202",
      "campaignId": "42586816713082",
      "expression": [
        {
          "type": "QUERY_HIGH_REL_MATCHES"
        }
      ],
      "expressionType": "AUTO",
      "resolvedExpression": [
        {
          "type": "QUERY_HIGH_REL_MATCHES"
        }
      ],
      "state": "ENABLED",
      "targetId": "31667521455534"
    },
    {
      "adGroupId": "184667233956202",
      "campaignId": "42586816713082",
      "expression": [
        {
          "type": "QUERY_BROAD_REL_MATCHES"
        }
      ],
      "expressionType": "AUTO",
      "resolvedExpression": [
        {
          "type": "QUERY_BROAD_REL_MATCHES"
        }
      ],
      "state": "ENABLED",
      "targetId": "275709419154538"
    },
    {
      "adGroupId": "184667233956202",
      "campaignId": "42586816713082",
      "expression": [
        {
          "type": "ASIN_ACCESSORY_RELATED"
        }
      ],
      "expressionType": "AUTO",
      "resolvedExpression": [
        {
          "type": "ASIN_ACCESSORY_RELATED"
        }
      ],
      "state": "ENABLED",
      "targetId": "54866135241200"
    },
    {
      "adGroupId": "184667233956202",
      "campaignId": "42586816713082",
      "expression": [
        {
          "type": "ASIN_SUBSTITUTE_RELATED"
        }
      ],
      "expressionType": "AUTO",
      "resolvedExpression": [
        {
          "type": "ASIN_SUBSTITUTE_RELATED"
        }
      ],
      "state": "ENABLED",
      "targetId": "14640417682469"
    }
  ],
  "totalResults": 4
}
```

Make a note of the `targetId` for each expression to use in future steps.

## Get bid recommendations

You can use [theme-based bid recommendations](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide) to get suggested bids for each expression type. You’ll need to enter the `campaignId` and `adGroupId`. Your `adGroupId` should have a valid product ad child entity. 

### Endpoint

[POST sp/targets/bid/recommendations](sponsored-products/3-0/openapi/prod#tag/ThemeBasedBidRecommendation/operation/GetThemeBasedBidRecommendationForAdGroup_v1)

### Request

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets/bid/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spthemebasedbidrecommendation.v3+json' \
--header 'Content-Type: application/vnd.spthemebasedbidrecommendation.v3+json' \
--data-raw '{
  "recommendationType": "BIDS_FOR_EXISTING_AD_GROUP",
  "targetingExpressions": [
    {
      "type": "CLOSE_MATCH"
    },
    {
      "type": "COMPLEMENTS"
    },
    {
      "type": "LOOSE_MATCH"
    },
    {
      "type": "SUBSTITUTES"
    }
  ],
  "campaignId": "auto_campaignId",
  "adGroupId": "auto_campaign_adGroupId"
}‘
```

### Response

The response contains minimum, median, and maximum suggested bids for each type of targeting expression.

```
{
    "bidRecommendations": [
        {
            "theme": "CONVERSION_OPPORTUNITIES",
            "impactMetrics": {
                "clicks": {
                    "values": [
                        {
                            "lower": 24,
                            "upper": 32
                        },
                        {
                            "lower": 48,
                            "upper": 64
                        },
                        {
                            "lower": 71,
                            "upper": 96
                        }
                    ]
                },
                "orders": {
                    "values": [
                        {
                            "lower": 3,
                            "upper": 4
                        },
                        {
                            "lower": 5,
                            "upper": 7
                        },
                        {
                            "lower": 8,
                            "upper": 10
                        }
                    ]
                }
            },
            "bidRecommendationsForTargetingExpressions": [
                {
                    "targetingExpression": {
                        "type": "CLOSE_MATCH",
                        "value": null
                    },
                    "bidValues": [
                        {
                            "suggestedBid": 0.77
                        },
                        {
                            "suggestedBid": 1
                        },
                        {
                            "suggestedBid": 1.28
                        }
                    ]
                },
                {
                    "targetingExpression": {
                        "type": "COMPLEMENTS",
                        "value": null
                    },
                    "bidValues": [
                        {
                            "suggestedBid": 1.01
                        },
                        {
                            "suggestedBid": 1.11
                        },
                        {
                            "suggestedBid": 1.25
                        }
                    ]
                },
                {
                    "targetingExpression": {
                        "type": "LOOSE_MATCH",
                        "value": null
                    },
                    "bidValues": [
                        {
                            "suggestedBid": 0.12
                        },
                        {
                            "suggestedBid": 0.13
                        },
                        {
                            "suggestedBid": 0.16
                        }
                    ]
                },
                {
                    "targetingExpression": {
                        "type": "SUBSTITUTES",
                        "value": null
                    },
                    "bidValues": [
                        {
                            "suggestedBid": 0.64
                        },
                        {
                            "suggestedBid": 1.01
                        },
                        {
                            "suggestedBid": 1.36
                        }
                    ]
                }
            ]
        }
    ]
}

```

## Update bids for targeting expressions

Once you know what bid you’d like to use for each auto targeting expression, you can update the bids.

### Endpoint

[PUT sp/targets](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/UpdateSponsoredProductsTargetingClauses)

### Example

```
curl --location --request PUT 'https://advertising-api.amazon.com/sp/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spTargetingClause.v3+json' \
--header 'Content-Type: application/vnd.spTargetingClause.v3+json' \
--data-raw '{
  "targetingClauses": [
    {
      "targetId": "137528803021084",
      "bid": 3.25
    }
  ]
}'
```

## Next steps

* [Add negative product targeting](guides/sponsored-products/negative-targeting/product-brand)
* [Add negative keyword targeting](guides/sponsored-products/negative-targeting/keywords)

