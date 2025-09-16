---
title: Creating product targeting
description: A tutorial for product targets and category refinement for Sponsored Brands, with example requests and responses.
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - product targeting
    - category refinement
---

# Sponsored Brands product targeting

Product targeting allows you to choose specific products, categories, brands, or other product features that are relevant to the product in your ad. Use this strategy to help customers discover your product when searching products on Amazon. You can either choose to target individual products, or categories of products. 

You can view the suggested products section to get a list of recommended products for targeting if you aren’t sure what product to target. If you need help choosing to a category to target, view the get all categories section to get a list of all available categories on Amazon.


## Before you begin

You must have the following before creating product targeting expressions:

* A [campaign ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-a-campaign)
* An [ad group ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-ad-group)
* [Create at least one product ad](guides/sponsored-brands/ads/overview) in each ad group.



## Creating product targeting

**Endpoint**
[POST /sb/targets](sponsored-brands/3-0/openapi#tag/Product-targeting/operation/createTargets)

Parameters

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|adGroupId	|No	|string	|The ad group to associated the campaign with	|
|campaignId	|No	|string	|The campaign to associated the target	|
|expression.type	|No	|string	|For product targeting, always set to `ASIN_SAME_AS` (target an exact ASIN) or `ASIN_EXPANDED_FROM` (target an exact ASIN and other similar ASINs).	|
|expression.value	|No	|string	|The ASIN of the product you want to target.	|
|bid	|yes	|float	|Bid for the expression. If not specified, will use the ad group bid. See [bid constraints by marketplace](https://advertising.amazon.com/API/docs/en-us/reference/concepts/limits#bid-constraints-by-marketplace) to see the maximum allowable bid (in local currency). For more informations on bidding, visit [cost-per-click bids](https://advertising.amazon.com/help/GTX8JYBTJX5EUCZW)	|

>[NOTE] In Sponsored Brands and Sponsored Display campaigns, if we find an opportunity where your ad is more likely to lead to your desired goal, we may increase your bid for that auction up to 300%. Similarly, if we find an opportunity that looks less likely to lead to your campaign goal, we might lower your bid for that auction. For example, Amazon can adjust a bid of $1 up to $4 when there are opportunities for higher expected result. For Sponsored Brands, these bid adjustments are available only on campaigns where you select "Drive page visits" as your campaign goal. For more information related to bidding, please see our updated [help page](https://advertising.amazon.com/help/GPDRGYSKVB7H4BUR).

>[NOTE] The bid is only mutable when the keyword's corresponding campaign does not have any enabled [optimization rule](sponsored-brands/3-0/openapi/prod#tag/Optimization-Rules-(beta)).

### Example

**One ASIN**
This example creates a product targeting expressions for ASIN B070000000. In this example, the bid will default to the ad group bid.

```
curl --location 'https://advertising-api.amazon.com/sb/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
  "targets": [
    {
      "adGroupId": {{adgroupId}},
      "campaignId": {{campaignId}},
      "expressions": [
        {
          "type": "asinSameAs",
          "value": "B070000000"
        }
      ]
    }
  ]
}'
```

### Response

A successful request results in a 200 response that contains individual status messages for each targeting clause you created. You should check that the success array contains all objects you intended to create. Any failures are indicated in the `createTargetErrorResults` error array.


```
{
    "createTargetSuccessResults": [
        {
            "targetRequestIndex": 0,
            "targetId":{{targetId}}
        }
    ],
    "createTargetErrorResults": []
}
```



**Multiple ASINs**
You can only add one ASIN per targeting clause. If you want to target multiple ASINs, you should create two targeting clauses.


```
{
   "targets":[
      {
         "expression":[
            {
               "type":"ASIN_SAME_AS",
               "value":"B07YT8NVF9"
            }
         ],
         "campaignId":"{{campaignId}}",
         "adGroupId":"{{adGroupId}}"
      },
      {
         "expression":[
            {
               "type":"ASIN_SAME_AS",
               "value":"B09LJWZT6G"
            }
         ],
         "campaignId":"{{campaignId}}",
         "adGroupId":"{{adGroupId}}"
      }
   ]
}
```

## Creating product category targeting

You can target entire product categories, or add refinements. 


### Supported expression types

All category targeting expressions must include `ASIN_CATEGORY_SAME_AS`. Different refinements are available based on the category.

|Predicate	|Description	|Example value	|
|---	|---	|---	|
|`ASIN_CATEGORY_SAME_AS`	|Target the same category as the category expressed. Value is a category ID.	|11056341	|
|`ASIN_BRAND_SAME_AS`	|Target the brand that is the same as the brand expressed. Value is a brand ID.	|7048034011	|
|`ASIN_PRICE_LESS_THAN`	|Target a price that is less than the price expressed. Value is a price in account currency.	|20.5	|
|`ASIN_PRICE_BETWEEN`	|Target a price that is between the prices expressed. Value is two prices in account currency separated by a hypen.	|10.5-12.5	|
|`ASIN_PRICE_GREATER_THAN`	|Target a price that is greater than the price expressed. Value is a price in account currency.	|10.75	|
|`ASIN_REVIEW_RATING_LESS_THAN`	|Target a review rating less than the review rating that is expressed. Value is a number less than 5.	|4	|
|`ASIN_REVIEW_RATING_BETWEEN`	|Target a review rating that is between the review ratings expressed.	| 3-5	|
|`ASIN_REVIEW_RATING_GREATER_THAN`	|Target a review rating that is greater than the review rating expressed. Value is a number greater than 1.	|3	|

**Category with multiple refinements**

Endpoint: POST [sb/targets](sponsored-brands/3-0/openapi#tag/Product-targeting/operation/createTargets)

The expression targets category `2522102011`  with products specific to `7048034011` brand with a review rating between 3 and 4.5 stars.



```
curl --location 'https://advertising-api.amazon.com/sb/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Prefer: return=representation' \
--data '{
  "targets": [
    {
      "adGroupId": {{adGroupId}},
      "campaignId": {{campaignId}},
      "expressions": [
        {
          "type": "asinCategorySameAs",
          "value": "2522102011"
        },
        {
            "type": "asinBrandSameAs",
            "value": "7048034011"
        },
        {
            "type": "asinReviewRatingBetween",
            "value": "3-4.5"
        }
      ],
      "bid": 10.0
    }
  ]
}'
```

### Response

A successful request results in a 200 response that contains individual status messages for each targeting clause you created. You should check that the success array contains all objects you intended to create. Any failures are indicated in the `createTargetErrorResults` array.


```
{
    "createTargetSuccessResults": [
        {
            "target": {
                "targetId": 520756961415270,
                "adGroupId": {{adGroupId}},
                "campaignId": {{campaignId}},
                "state": "enabled",
                "bid": 10,
                "expressions": [
                    {
                        "type": "asinCategorySameAs",
                        "value": "2522102011"
                    },
                    {
                        "type": "asinBrandSameAs",
                        "value": "7048034011"
                    },
                    {
                        "type": "asinReviewRatingBetween",
                        "value": "3-4.5"
                    }
                ],
                "resolvedExpressions": [
                    {
                        "type": "asinCategorySameAs",
                        "value": "Paper Dolls"
                    },
                    {
                        "type": "asinBrandSameAs",
                        "value": "Back to Basics"
                    },
                    {
                        "type": "asinReviewRatingBetween",
                        "value": "3-4.5"
                    }
                ]
            },
            "targetRequestIndex": 0,
            "targetId": 520756961415270
        }
    ],
    "createTargetErrorResults": []
}
```

## Product category targeting suggestions and refinements

### Suggested products

If you don’t know what products to target, Amazon can recommend products based on the ASINs you are advertising.

Endpoint: [POST sb/recommendations/targets/product/list](sponsored-brands/3-0/openapi#tag/Targeting-recommendations/operation/getProductRecommendations)

>[TIP] If you are including multiple ASINs in the request, make sure they are similar products. The response includes suggested products for **all** ASINs included in the request.

**Get a list of recommended products for targeting**


```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/targets/product/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data '{
  "maxResults": 5,
  "filters": [
    {
      "filterType": "ASINS",
      "values": [
        "B07YT8NVF9"
      ]
    }
  ]
}'
```

**Response**

If the `nextToken` is populated, this means that there are paginated results. To view the rest of the paginated results, you can specify this token for a future request to obtain the next page of results.

```
{
    "nextToken": "/u0NEl5BpwIAAAAAAAAAAXmoRxxl/XqL3LOy+KBHD9EohL8AI1vWqYty8J/ojtCV+BY7IbhguHT15K4NMQRy77nR35CqAlf5HKhjBkbMhhVoQqxXLJUciz9DQfMIFZ6DJ5BjiqEWw1bEdm4sKApkHuM/w2PVRbewCfibM4Yzp7uiVpM4WpjVN1fpe5qqjf2dMNRJd/PMPitIiE81aPJ8SudZegVH5lvNvVhuC3oF/08Md0MqSq2ifq5WcIuT4il/UweRuZv9X6ng6tT8JTfLOEUHjx0EmFHJbkC8ifgvcnuHJ57j4QlYicIlqUrtiiBLxot+ewqdtodrDVfQnKdK5oZ66M/cGj+TOMi2IqgnrPuzvHFkVyW0eX2cB6oMNrl8Au4N1CvY2E776EaaRYPCvtl/rC/Xes5jjra7Wye/1iQ=",
    "recommendedProducts": [
        {
            "recommendedTargetAsin": "B00IO9H8GI"
        },
        {
            "recommendedTargetAsin": "B07R213ZR3"
        },
        {
            "recommendedTargetAsin": "B0925X68D6"
        },
        {
            "recommendedTargetAsin": "B0735DQFPK"
        },
        {
            "recommendedTargetAsin": "B07HRD9BQB"
        }
    ]
}
```

### Get suggested categories

Endpoint: [POST sb/recommendations/targets/category](sponsored-brands/3-0/openapi#tag/Targeting-recommendations/operation/getTargetingCategories)

You can also request suggested categories based on a list of ASINs. This endpoint takes a list of ASINs and returns related categories that you could target. Note that all inputted ASINs should be similar, as the recommendations are for **all** ASINs in the request.


```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/targets/category' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data '{
  "asins": [
    "B00G96S4Y8"
  ]
}'
```

The `id` parameter shown here is the `categoryRefinementId`. The `categoryRefinementId`  is required in the path if using the GET /sb/targets/categories/{categoryRefinementId}/refinements endpoint.

```
{
    "categoryRecommendationResults": [
        {
            "id": 19240644011,
            "name": "Indoor Electric Space Heaters",
            "path": "/Home & Kitchen/Heating, Cooling & Air Quality/Indoor Space Heaters/Indoor Electric Space Heaters",
            "isTargetable": true
        },
        {
            "id": 1055398,
            "name": "Home & Kitchen",
            "path": "/Home & Kitchen",
            "isTargetable": true
        },
        {
            "id": 3206324011,
            "name": "Heating, Cooling & Air Quality",
            "path": "/Home & Kitchen/Heating, Cooling & Air Quality",
            "isTargetable": true
        },
        {
            "id": 510182,
            "name": "Indoor Space Heaters",
            "path": "/Home & Kitchen/Heating, Cooling & Air Quality/Indoor Space Heaters",
            "isTargetable": true
        }
    ]
}
```



### Get all categories

Endpoint: [GET /sb/targets/categories](sponsored-brands/3-0/openapi/prod#tag/Product-targeting-categories/operation/SBTargetingGetTargetableCategories)

This endpoint returns a tree of all categories currently available on Amazon.com. You must add the query parameter `supplySource` .

**Example**


```
curl --location 'https://advertising-api.amazon.com/sb/targets/categories?supplySource=AMAZON' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
```



### Category refinements

Endpoint: GET [/sb/targets/categories/{categoryRefinementId}/refinements](sponsored-brands/3-0/openapi/prod#tag/Product-targeting-categories/operation/SBTargetingGetRefinementsForCategory)

Categories may have refinements available that you can use to further target ASINs within a category. Possible refinements include brands, age ranges, and genres. 

**Example**


```
curl --location 'https://advertising-api.amazon.com/sb/targets/categories/{{categoryRefinementId/refinements' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
```

**Response**


```
{
   "brands":[
      {
         "name":"Back to Basics",
         "id":"7048034011"
      },
      {
         "name":"DJECO",
         "id":"18681839011"
      },
      {
         "name":"Disney",
         "id":"18726768011"
      },
      {
         "name":"Pipity",
         "id":"20774946011"
      },
      {
         "name":"ALEX Toys",
         "id":"2581933011"
      },
      {
         "name":"Fashion Angels",
         "id":"7481406011"
      }
   ],
   "ageRanges":[
      {
         "name":"Birth to 24 Months",
         "id":"165813011"
      },
      {
         "name":"2 to 4 Years",
         "id":"165890011"
      },
      {
         "name":"8 to 13 Years",
         "id":"5442387011"
      },
      {
         "name":"5 to 7 Years",
         "id":"165936011"
      }
   ],
   "genres":null
}
```

## Bid recommendations 

Amazon can recommend bids based on keywords or targeting expressions. 


### Examples

Endpoint: POST [/sb/recommendations/bids](sponsored-brands/3-0/openapi#tag/Bid-recommendations/operation/getBidsRecommendations)


**Bid recommendations by category**

```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/bids' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \

  "campaignId": {{campaignId}},
  "targets": [
    [
      {
        "type": "asinCategorySameAs",
        "value": "2522102011"
      }
    ]
  ],
  "adFormat": "productCollection"
 } '
```


**Response**


```
{
    "keywordsBidsRecommendationSuccessResults": [],
    "keywordsBidsRecommendationErrorResults": [],
    "targetsBidsRecommendationSuccessResults": [
        {
            "targets": [
                {
                    "type": "asinCategorySameAs",
                    "value": "2522102011"
                }
            ],
            "targetsIndex": 0,
            "recommendationId": "f9cafef70ec838243976893a28be669f",
            "recommendedBid": {
                "rangeStart": 0.42,
                "rangeEnd": 2.37,
                "recommended": 1.25
            }
        }
    ],
    "targetsBidsRecommendationErrorResults": [],
    "themesRecommendationSuccessResults": [],
    "themesRecommendationErrorResults": []
}
```


**Bid recommendations by ASIN**


```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/bids' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.f6cc0e51a33149309f44b91b1a67a24e' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
 {
  "campaignId": {{campaignId}},
  "targets": [
    [
      {
        "type": "asinSameAs",
        "value": "B084311KKN"
      }
    ]
  ],
  "adFormat": "productCollection"
 } '
```

**Response**


```
{
    "keywordsBidsRecommendationSuccessResults": [],
    "keywordsBidsRecommendationErrorResults": [],
    "targetsBidsRecommendationSuccessResults": [
        {
            "targets": [
                {
                    "type": "asinSameAs",
                    "value": "B084311KKN"
                }
            ],
            "targetsIndex": 0,
            "recommendationId": "05729efc9d3f81a039dededf07684456",
            "recommendedBid": {
                "rangeStart": 1.45,
                "rangeEnd": 6.46,
                "recommended": 2.68
            }
        }
    ],
    "targetsBidsRecommendationErrorResults": [],
    "themesRecommendationSuccessResults": [],
    "themesRecommendationErrorResults": []
}
```

