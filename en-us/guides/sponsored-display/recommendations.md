---
title: Sponsored Display Recommendations API
description: Learn how to use the Amazon Ads API for Sponsored Display to optimize Sponsored Display campaigns with recommendations.
type: guide
interface: api 
tags:
    - Sponsored Display
    - Budget Rules
keywords:
    - campaign optimization
    - recommendations
---

# Sponsored Display Recommendations API

Amazon provides insight into potential products, categories, and bids that can help increase the probability of ad clicks and drive product awareness and conversions. When creating or managing campaigns to target products or categories, you can create target clauses containing ASINs or categories to show your advertised product both on and off Amazon. This strategy allows you to defend your product pages, upsell products within your catalog, or grow your business in adjacent product or categories. 

The following recommendations are currently supported:

| Targeting Strategy | Identifier in the API | Recommendations Type 
|--------------------|-----------------|--------|
| Product targeting | `T00020` | product, similar product, category, & bids | 
| Audiences | `T00030`  | exact product, similar product, category, & bids | 

**Product & Category Targeting Recommendations**

The Sponsored Display product & category Recommendations enable you to programmatically query for recommended products or categories to target based on a given a set of advertised products (ASINs or SKU). These recommendations are based on view-to-purchase data (shoppers viewing these recommended products are likely to purchase your advertised product). We also recommend targets for new products with minimal low page views using catalog metadata, thereby ensuring better coverage for you. The key factors driving better scale and performance is to provide optimal targeting that is broad enough to get scale and relevant enough to get good performance. This data is refreshed as we learn new information about optimal products or categories to target so you should actively query the API to learn about new recommendations. The recommended products in the response are also ranked based on detail page views of target products and their similarity with advertised products, such that highest ranked recommendation is likely to generate more ad clicks for you. The recommendation service is built specifically for Sponsored Display and should be used instead of Sponsored Products or Sponsored Brand Product recommendation APIs. 

If you plan to include multiple similar ASINs within an Ad Group than you should call the API with those ASINs in a single request. Alternatively, you can also call the API with a single ASIN only so you get recommendations for an ASIN only. The service can be called without creating campaigns so you can experiment and gain insight into potential ASINs to target. You can then easily create or update campaigns using the Amazon Ads API and include the ASINs within target clauses.

**Using the Targeting Recommendations API**

The recommendations API is described in the Sponsored Display [Open API](sponsored-display/3-0/openapi) specification. The request body allows you to define multiple input parameters, for example:

```bash
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/vnd.sdtargetingrecommendations.v3.1+json' \
--data-raw '[
    {
     "expression": [
         {
             "type": "audiences",
             "value": [
                 {
                     "type": "audienceSameAs",
                     "value": "111122223333344441"
                 }
             ]
         }
     ],
     "bid": "1.25",
     "adGroupId": 209426065354910,
     "expressionType": "manual",
     "state": "enabled"
    }
]'
{
    "tactic": "T00020",
    "products": [
        {
            "asin": "B015FK1EH2"
        },
        {
            "asin": "B00NMOYPVG"
        }
    ],
    "typeFilter": [
        "PRODUCT", "CATEGORY"
    ]
}
```

- The `Content-Type` field specifies the version of the Recommendation API. The original version (3.0) only supported products. The latest release (3.1) supports both products and categories.
- The `tactic` field defines the campaign type. 
- The `products` array contains one or more goal ASINs. These ASINs represent the advertised product within a campaign. The top 100 ASINs within the request are used to generate recommendations. 
- The `typeFilter` is an array of values that define the type of recommendations. The supported values are `PRODUCT` & `CATEGORY`. Please note, tactic "T00030" only allow `CATEGORY`.

The response from this request returns the following:

```JSON
{
    "recommendations": {
        "products": [
            {
                "asin": "B07XJ8C8F5",
                "rank": 1
            },
            {
                "asin": "B07XQ5QV37",
                "rank": 2
            },
            {
                "asin": "B08D7JL5RM",
                "rank": 100
            }
        ]
        "categories": [
            {
                "category": 1234,
                "name": "test category 1",
                "path": [
                    "root",
                    "node1",
                    "node2"
                ],
                "targetableAsinCountRange": {
                    "rangeLower": 1,
                    "rangeUpper": 4
                }
            },
            {
                "category": 4567,
                "name": "test category 2",
                "path": [
                    "root",
                    "node1",
                    "node2"
                ],
                "targetableAsinCountRange": {
                    "rangeLower": 1,
                    "rangeUpper": 4
                }
            }
        ]
    }
}
```

The response from the recommendations service contains an array of product ASINS ranked by potential scale. Specifically, ASINs with a higher rank have the potential to display on a larger number of product detail pages. Even ASINs with lower rank are still relevant so you should review all the recommendations to make sure you don't miss out on the opportunities. The categories returned contain a field `targetableAsinCountRange` which contains a range of ASINs within each category to provide guidance on the potential scale.

**Using the Bid Recommendations API**

Bid recommendations provides recommended bids for target clauses with median, lower and upper boundaries based on historical bid values for the ASINs in a recent period. The specified bid optimization and cost type are used to optimize the bid recommendations for your campaign goals. These recommendations help advertisers select a bid which will more likely win impressions and generate clicks for the targeted ad to the shopper. The bid recommendation is provided at the target level using the exact structure of existing target clauses, which enables you to programmatically reuse campaign target clauses and obtain bid recommendations.

For more information, visit the updated [Bid Recommendations API](sponsored-display/3-0/openapi#tag/Bid-Recommendations).

You'll need:

- The `targetingClauses`, which you can gather from:

```json
curl --location --request GET 'https://advertising-api.amazon.com/sd/targets/{targetId} ' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
```

- The `products` (ASIN) in an adGroup, which you can gather from:  

```json
curl --location --request GET 'https://advertising-api.amazon.com/sd/productAds?adGroupIdFilter={adGroupId}' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
```

Using this information, here is an example of a bid recommendations call:

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets/bid/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/vnd.sdtargetingrecommendations.v3.2+json ' \
--data-raw '[
    {
     "expression": [
         {
             "type": "audiences",
             "value": [
                 {
                     "type": "audienceSameAs",
                     "value": "111122223333344441"
                 }
             ]
         }
     ],
     "bid": "1.25",
     "adGroupId": 209426065354910,
     "expressionType": "manual",
     "state": "enabled"
    }
]'
{
   "bidOptimization": "clicks",
   "costType": "cpc",
   "products":[
      {
         "asin":"B123456789"
      }
   ],
   "targetingClauses":[
      {
         "targetingClause":{
            "expressionType":"manual",
            "expression":[
               {
                  "type":"asinSameAs",
                  "value":"B123456789"
               }
            ]
         }
      }
   ]
}
```

- The `Content-Type` header specifies the version of the Recommendation API. The latest release (3.2) supports bid recommendations and bidOptimization inputs.
- The `bidOptimization` field in the payload contains a bidOptimization of "clicks" or "conversions". 
- The `costType` field in the payload contains a costType of the campaign, this can be "cpc". 
- The `products` field in the payload contains one or more goal ASINs. These ASINs represent the advertised product within a campaign. The top 100 ASINs within the request are used to generate recommendations. 
- The `targetingClauses` field in the payload is used alongside the `products` to determine the bid recommendation. Currently the API will accept up to 100 targeting clauses, which will return you a set of bid values in an array with the same order as the request.

The response from the above request returns the following bid recommendations based on the product (product advertised) and targeting clause:

```json
{
   "bidOptimization": "clicks",
   "costType":"cpc",
   "bidRecommendations":[
      {
         "code":"200",
         "rangeLower":0.25,
         "rangeUpper":0.79,
         "recommended":0.39
      }
   ]
}
```

if you have multiple target clauses, you will get the same number of bid recommendations

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets/bid/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/vnd.sdtargetingrecommendations.v3.2+json ' \
{
    "bidOptimization": "conversions",
    "costType": "cpc",
    "products": [
        {
            "asin": "B123456789"
        }
    ],
    "targetingClauses": [
        {
            "targetingClause": {
                "expressionType": "manual",
                "expression": [
                    {
                        "type": "asinCategorySameAs",
                        "value": "12345"
                    }
                ]
            }
        },
        {
            "targetingClause": {
                "expressionType": "manual",
                "expression": [
                    {
                        "type": "views",
                        "value": [
                            {
                                "type": "asinCategorySameAs",
                                "value": "12345"
                            },
                            {
                                "type": "lookback",
                                "value": "30"
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```

which returns

```JSON
{
   "bidOptimization": "conversions",
   "costType":"cpc",
   "bidRecommendations":[
      {
         "code":"200",
         "rangeLower":0.25,
         "rangeUpper":0.79,
         "recommended":0.39
      },
      {
         "code":"200",
         "rangeLower":0.30,
         "rangeUpper":0.90,
         "recommended":0.45
      }
   ]
}
```

Additional examples for audience views with similarProduct & exactProduct targeting. 

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets/bid/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/vnd.sdtargetingrecommendations.v3.2+json' \
{
    "bidOptimization": "conversions",
    "costType": "cpc",
    "products": [
        {
            "asin": "B123456789"
        }
    ],
    "targetingClauses": [
        {
            "targetingClause": {
                "expressionType": "auto",
                "expression": [
                    {
                        "type": "views",
                        "value": [
                            {
                                "type": "similarProduct"
                            },
                            {
                                "type": "lookback",
                                "value": "30"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "targetingClause": {
                "expressionType": "auto",
                "expression": [
                    {
                        "type": "views",
                        "value": [
                            {
                                "type": "exactProduct"
                            },
                            {
                                "type": "lookback",
                                "value": "30"
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```

which returns

```json
{
   "bidOptimization": "conversions",
   "costType":"cpc",
   "bidRecommendations":[
      {
         "code":"200",
         "rangeLower":0.28,
         "rangeUpper":0.79,
         "recommended":0.39
      },
      {
         "code":"200",
         "rangeLower":0.34,
         "rangeUpper":0.95,
         "recommended":0.45
      }
   ]
}
```

**Summary**

When you have identified the ideal ASIN or categories for your campaign, you can then include them in your product targeting: 

1. Create new campaigns with targeting clauses using the recommended ASINs or categories.
2. Update existing campaigns with targeting clauses using the recommendations.
3. For 1) and 2), update target bid value based on bid recommendations. 

For more details on how to manage campaigns using these recommendations, see the tutorial [here](guides/sponsored-display/tutorials/optimize-campaigns-recommendations).