---
title: Sponsored TV targeting
description: Details on how to  create a targeting clause.
type: guide
interface: api
keywords:
    - Sponsored TV
    - Targeting
---

# Sponsored TV targeting

Sponsored TV targeting consists of two targeting expressions types. The `CONTENT_CATEGORY_SIMILAR_TO` expression type targets audiences that are likely to be interested in specific types of content, including documentaries, music videos, and TV genres (such as drama) or products in categories sold on Amazon like "Built-in Dishwashers" or "Women's Sports Apparel." The `ASIN_CATEGORY_SIMILAR_TO` expression type targets the ASIN category that is similar to the specified category. Standard Amazon audiences can be used using the expression types `AUDIENCE_SAME_AS_INTEREST`, `AUDIENCE_SAME_AS_IN_MARKET`, `AUDIENCE_SAME_AS_LIFESTYLE`, and `AUDIENCE_SAME_AS_LIFE_EVENT`. 

## Targeting recommendations

Sponsored TV supports targeting recommendations based on either of the following:

* [ASIN](sponsored-display/3-0/openapi#tag/Targeting-Recommendations)
* [Search keyword](targetable-entities#operation/TextInputSearch)

## Before you begin

* You must have an [ad group ID](guides/sponsored-tv/ad-groups)

## Creating a targeting clause

Below is a table of all the likely content interests that are usable with `CONTENT_CATEGORY_SIMILAR_TO` expression type:

|Content Interest	|Value	|
|---	|---	|
|Action or Adventure	|amzn1.iab-content.325	|
|Animation or Anime	|amzn1.iab-content.641	|
|Biographies	|amzn1.iab-content.44	|
|Comedy	|amzn1.iab-content.646	|
|Documentary	|amzn1.iab-content.332	|
|Drama	|amzn1.iab-content.647	|
|Factual	|amzn1.iab-content.648	|
|Family or Children	|amzn1.iab-content.645	|
|Fantasy	|amzn1.iab-content.335	|
|History	|amzn1.iab-content.EZWB7V	|
|Holiday	|amzn1.iab-content.649	|
|Horror	|amzn1.iab-content.336	|
|Lifestyle	|amzn1.iab-content.TIFQA5	|
|Music Video	|amzn1.iab-content.650	|
|Musical	|amzn1.iab-content.156	|
|Mystery	|amzn1.iab-content.331	|
|News	|amzn1.iab-content.1020	|
|Reality TV	|amzn1.iab-content.651	|
|Romance	|amzn1.iab-content.326	|
|Science Fiction	|amzn1.iab-content.652	|
|Soap Opera	|amzn1.iab-content.642	|
|Special Interest (Indie or Art House)	|amzn1.iab-content.643	|
|Sports	|amzn1.iab-content.483	|
|Talk Show	|amzn1.iab-content.A0AH3G	|
|True Crime	|amzn1.iab-content.KHPC5A	|
|Video Game	|amzn1.iab-content.680	|
|Western	|amzn1.iab-content.KHPC6A	|
|Young Adult	|amzn1.iab-content.51	|



### Request

Endpoint: POST [/st/targets](sponsored-tv-open-beta#tag/TargetingClauses/operation/CreateSponsoredTvTargetingClauses)


```json
curl --location --request POST 'https://advertising-api.amazon.com/st/targets' \ 
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \ 
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \ 
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \ 
--header 'Prefer: return=representation --data-raw '{
    "targetingClauses": [
        {
            "adGroupId": "123456789123456",
            "state": "ENABLED",
            "expression": [{
                "value": "amzn1.iab-content.646",
                "type": "CONTENT_CATEGORY_SIMILAR_TO"
            }],
            "bid": 25.0
        }, {
            "adGroupId": "123456789123456",
            "state": "ENABLED",
            "expression": [{
                "value": "723463011",
                "type": "ASIN_CATEGORY_SIMILAR_TO"
            }],
            "bid": 25.0
        }, {
            "adGroupId": "123456789123456",
            "state": "ENABLED",
            "expression": [{
                "value": "2238192011",
                "type": "ASIN_CATEGORY_SIMILAR_TO"
            }],
            "bid": 25.0
        }, {
            "adGroupId": "123456789123456",
            "state": "ENABLED",
            "expression": [{
                "value": "398770975356829103",
                "type": "AUDIENCE_SAME_AS_IN_MARKET"
            }]
        }
    ]
}'
```

### Response


```json
{
    "targetingClauses": {
        "error": [],
        "success": [{
                "index": 0,
                "targetId": "123456789123456",
                "targetingClause": {
                    "adGroupId": "123456789123456",
                    "bid": 25.0,
                    "campaignId": "123456789123456",
                    "expression": [{
                        "type": "CONTENT_CATEGORY_SIMILAR_TO",
                        "value": "amzn1.iab-content.646"
                    }],
                    "state": "ENABLED",
                    "targetId": "123456789123456"
                }
            },
            {
                "index": 1,
                "targetId": "123456789123456",
                "targetingClause": {
                    "adGroupId": "123456789123456",
                    "bid": 25.0,
                    "campaignId": "123456789123456",
                    "expression": [{
                        "type": "ASIN_CATEGORY_SIMILAR_TO",
                        "value": "723463011"
                    }],
                    "state": "ENABLED",
                    "targetId": "123456789123456"
                }
            },
            {
                "index": 2,
                "targetId": "123456789123456",
                "targetingClause": {
                    "adGroupId": "123456789123456",
                    "bid": 25.0,
                    "campaignId": "123456789123456",
                    "expression": [{
                        "type": "ASIN_CATEGORY_SIMILAR_TO",
                        "value": "2238192011"
                    }],
                    "state": "ENABLED",
                    "targetId": "123456789123456"
                }
            }, {
                "index": 3,
                "targetId": "123456789123456",
                "targetingClause": {
                    "adGroupId": "123456789123456",
                    "campaignId": "123456789123456",
                    "state": "ENABLED",
                    "expression": [{
                        "value": "398770975356829103",
                        "type": "AUDIENCE_SAME_AS_IN_MARKET"
                    }],
                    "state": "ENABLED",
                    "targetId": "123456789123456"
                }
            }
        ]
    }
}
   
```

## Targeting recommendations based on ASIN

Sponsored TV supports targeting recommendations based on ASIN for 'In-market categories', which can be accessed through the steps mentioned in targeting recommendations API reference with typeFilter `CATEGORY`.

Endpoint: POST [/sd/targets/recommendations](sponsored-display/3-0/openapi#tag/Targeting-Recommendations).


### Request

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets/recommendations?locale=en_US' \ 
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \ 
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \ 
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \ 
--header 'Prefer: return=representation --data-raw '{
    "tactic": "T00020",
    "products": [
        {
            "asin": "B00PN11UNW"
        }
    ],
    "typeFilter": [
        "CATEGORY"
    ]
}'
```

### Response

```json
{
    "recommendations": {
        "products": null,
        "categories": [
            {
                "category": 724737011,
                "name": "Packaged Broths",
                "translatedName": "Packaged Broths",
                "path": [
                    "Grocery & Gourmet Food",
                    "Pantry Staples",
                    "Soups, Stocks & Broths",
                    "Packaged Broths"
                ],
                "translatedPath": [
                    "Grocery & Gourmet Food",
                    "Pantry Staples",
                    "Soups, Stocks & Broths",
                    "Packaged Broths"
                ],
                "targetableAsinCountRange": {
                    "rangeLower": 12,
                    "rangeUpper": 20
                },
                "rank": 1
            },
            {
                "category": 345831011,
                "name": "Almond Butter",
                "translatedName": "Almond Butter",
                "path": [
                    "Grocery & Gourmet Food",
                    "Pantry Staples",
                    "Nut & Seed Butters",
                    "Almond Butter"
                ],
                "translatedPath": [
                    "Grocery & Gourmet Food",
                    "Pantry Staples",
                    "Nut & Seed Butters",
                    "Almond Butter"
                ],
                "targetableAsinCountRange": {
                    "rangeLower": 460,
                    "rangeUpper": 766
                },
                "rank": 2
            }
        ],
        "audiences": null,
        "themes": null
    }
}
```