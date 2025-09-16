---
title: Creating product and category targeting expressions
description: Tutorial for creating Sponsored Products product and category targeting expressions using the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - Overview
    - Product targeting
---

# Sponsored Products product targeting overview

Manual Sponsored Products campaigns require you to add at least one targeting clause. You can either use [keyword targeting](guides/sponsored-products/keywords/overview) or product targeting (discussed in this document). Targeting is assigned at the ad group level. All products in an ad group will share targeting clauses. 

Product targeting can be applied at either the product, category, or brand level. For example, you can target a specific running shoe product by ASIN, or running shoes as a category using the category ID. 

You can learn more about campaign structure in [Campaign hierarchy](guides/sponsored-products/get-started/campaign-structure).

## Before you begin

Before creating product targeting expressions, you need to

* [Create a manual campaign](guides/sponsored-products/campaigns)
* [Create an ad group](guides/sponsored-products/ad-groups)
* [Create at least one product ad](guides/sponsored-products/product-ads) in each ad group
* [Get recommendations for what ASINs to target](guides/sponsored-products/product-targeting/suggestions) (optional)
* [Get recommendations for what categories to target](guides/sponsored-products/product-targeting/category-suggestions-refinements) (optional)

## Request

### Endpoint

[POST sp/targets](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/CreateSponsoredProductsTargetingClauses)

### Targeting individual products

When you target individual products, you can choose to a specific ASIN exactly, or target a specific ASIN and other similar ASINs, including substitutes, complements, and other related products. You can learn more about expanded ASIN targeting in the [advertising console help center](https://advertising.amazon.com/help?#GB2JECV9CJK6R6AL).

#### Parameters

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|campaignId	|No	|number	|The campaign to associate the targeting expression with.	|
|adGroupId	|No	|String	|The ad group to associate the campaign with.	|
|expression.type	|No	|string	|For product targeting, always set to `ASIN_SAME_AS` (target an exact ASIN) or `ASIN_EXPANDED_FROM` (target an exact ASIN and other similar ASINs).	|
|expression.value	|No	|string	|The ASIN of the product you want to target. If you're not sure what ASINs you should target, use the [product recommendations endpoint](guides/sponsored-products/product-targeting/suggestions). 	|
|expressionType	|No	|string	|Always set to `MANUAL`.	|
|state	|No	|string	|The state of the targeting expression, either `ENABLED` or `PAUSED`.	|
|bid	|Yes	|float	|Bid for the expression. If not specified, will use the ad group bid. If you're not sure what to use for a bid, try the [bid recommendations endpoint](sponsored-products/2-0/openapi#/Bid%20recommendations/getBidRecommendations).	|

#### Examples

**One ASIN**

This example creates a product targeting expressions for ASIN B07FKDZPZW.

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Accept: application/vnd.spTargetingClause.v3+json' \
--header 'Content-Type: application/vnd.spTargetingClause.v3+json' \
--header 'Prefer: return=representation' \
--data-raw '{
  "targetingClauses": [
    {
      "expression": [
        {
          "type": "ASIN_SAME_AS",
          "value": "B07FKDZPZW"
        }
      ],
      "campaignId": "123456789",
      "expressionType": "MANUAL",
      "state": "ENABLED",
      "bid": 1.50,
      "adGroupId": "987654321"
    }
  ]
}'
```

**Multiple ASINs**

You can only add one ASIN per targeting clause. If you want to target multiple ASINs, you should create two targeting clauses. 


```json
{
  "targetingClauses": [
    {
      "expression": [
        {
          "type": "ASIN_SAME_AS",
          "value": "B07YT8NVF9"
        }
      ],
      "campaignId": "26256301417055",
      "expressionType": "MANUAL",
      "state": "ENABLED",
      "bid": 1.50,
      "adGroupId": "169462822148652"
    },
    {
      "expression": [
        {
          "type": "ASIN_SAME_AS",
          "value": "B09LJWZT6G"
        }
      ],
      "campaignId": "26256301417055",
      "expressionType": "MANUAL",
      "state": "ENABLED",
      "bid": 2.50,
      "adGroupId": "169462822148652"
    }
  ]
}
```

**ASIN and related products**

This example creates a targeting expression for ASIN B07YT8NVF9, as well as other related ASINs.

```json
{
  "targetingClauses": [
    {
      "expression": [
        {
          "type": "ASIN_EXPANDED_FROM",
          "value": "B07YT8NVF9"
        }
      ],
      "campaignId": "26256301417055",
      "expressionType": "MANUAL",
      "state": "ENABLED",
      "bid": 1.50,
      "adGroupId": "169462822148652"
    }
}
```


### Targeting product categories

Creating targeting expressions for product categories supports more refinements than expressions for a single ASIN.

#### Parameters

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|campaignId	|No	|number	|The campaign to associate the targeting expression with.	|
|adGroupId	|No	|String	|The ad group to associate the campaign with.	|
|expression.type	|No	|string	|See the supported expression types table below. At minimum, you must include `ASIN_CATEGORY_SAME_AS`. See the **Supported expression types** table below for more details. 	|
|expression.value	|No	|string	|The value differs based on the expression type. See the table below for more information.	|
|expressionType	|No	|string	|Always set to `MANUAL`.	|
|state	|No	|string	|The state of the targeting expression, either `ENABLED` or `PAUSED`.	|
|bid	|Yes	|float	|Bid for the expression. If not specified, the expression will use the ad group bid. If you're not sure what to use for a bid, try the [bid recommendations endpoint](sponsored-products/2-0/openapi#/Bid%20recommendations/getBidRecommendations).	|

#### Supported expression types

All category targeting expressions must include `ASIN_CATEGORY_SAME_AS`. Different refinements are available based on the category. To understand what refinements are available, see [Product category targeting suggestions and refinements](guides/sponsored-products/product-targeting/category-suggestions-refinements).

|Predicate	|Description	|Example value	|
|---	|---	|---	|
|`ASIN_CATEGORY_SAME_AS`	|Negatively Target the same category as the category expressed. Value is a category ID.	|11056341	|
|`ASIN_BRAND_SAME_AS`	|Target the brand that is the same as the brand expressed. Value is a brand ID.	|7048034011	|
|`ASIN_PRICE_LESS_THAN`	|Target a price that is less than the price expressed. Value is a price in account currency.	|20.5	|
|`ASIN_PRICE_BETWEEN`	|Target a price that is between the prices expressed. Value is two prices in account currency separated by a hypen.	|10.5-12.5	|
|`ASIN_PRICE_GREATER_THAN`	|Target a price that is greater than the price expressed. Value is a price in account currency.	|10.75	|
|`ASIN_REVIEW_RATING_LESS_THAN`	|Target a review rating less than the review rating that is expressed. Value is a number less than 5.	|4	|
|`ASIN_REVIEW_RATING_BETWEEN`	|Target a review rating that is between the review ratings expressed.	|4-5	|
|`ASIN_REVIEW_RATING_GREATER_THAN`	|Target a review rating that is greater than the review rating expressed. Value is a number greater than 1.	|3	|
|`ASIN_IS_PRIME_SHIPPING_ELIGIBLE`	|Target products that are Prime Shipping Eligible. This refinement can be applied at a category or brand level only. Value is a boolean.	|TRUE	|
|`ASIN_AGE_RANGE_SAME_AS`	|Target an age range that is in the expressed range. This refinement can be applied for toys and games categories only. Value is an age range ID.	|165890011	|
|`ASIN_GENRE_SAME_AS`	|Value is a genre ID.	|1235687	|

#### Examples

**Category without refinements**

This example shows the expression to target an entire category with no refinements. 

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spTargetingClause.v3+json' \
--header 'Content-Type: application/vnd.spTargetingClause.v3+json' \
--data-raw '{
  "targetingClauses": [
    {
      "expression": [
        {
            "type":"ASIN_CATEGORY_SAME_AS",
            "value":"2522102011"
        }
      ],
      "campaignId": "26256301417055",
      "expressionType": "MANUAL",
      "state": "ENABLED",
      "bid": 1.50,
      "adGroupId": "169462822148652"
    }
  ]
}'
```

**Category with refinements**

This example shows how to create a targeting expression with multiple refinements (brand and rating between). When using refinements, you must first define the category.

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spTargetingClause.v3+json' \
--header 'Content-Type: application/vnd.spTargetingClause.v3+json' \
--data-raw '{
  "targetingClauses": [
    {
      "expression": [
        {
            "type":"ASIN_CATEGORY_SAME_AS",
            "value":"2522102011"
        },
        {
          "type": "ASIN_BRAND_SAME_AS",
          "value": "7048034011"
        },
        {
          "type": "ASIN_REVIEW_RATING_BETWEEN",
          "value": "3-4.5"
        }
      ],
      "campaignId": "26256301417055",
      "expressionType": "MANUAL",
      "state": "ENABLED",
      "bid": 1.50,
      "adGroupId": "169462822148652"
    }
  ]
}'
```

## Response

A successful request results in a 207 response that contains individual status messages for each targeting clause you created. You should check that the success array contains all objects you intended to create. Any failures are indicated in the error array. 

For category targeting expressions that contain an ID (category ID, brand ID, age range ID, or genre ID), the resolved expression object contains a readable description related to the ID.

>[TIP] If you want to return the whole targeting object in the request, make sure to include `Prefer: return=representation` as a header. 

### Example

```
{
  "targetingClauses": {
    "error": [],
    "success": [
      {
        "index": 0,
        "targetId": "269280454302686",
        "targetingClause": {
          "adGroupId": "169462822148652",
          "bid": 1.5,
          "campaignId": "26256301417055",
          "expression": [
            {
              "type": "ASIN_SAME_AS",
              "value": "B07FKDZPZW"
            }
          ],
          "expressionType": "MANUAL",
          "resolvedExpression": [
            {
              "type": "ASIN_SAME_AS",
              "value": "B07FKDZPZW"
            }
          ],
          "state": "ENABLED",
          "targetId": "269280454302686"
        }
      }
    ]
  }
}
```


