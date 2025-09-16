---
title: Category targeting suggestions and refinements
description: Tutorial for getting category suggestions and refinements using the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - product targeting
---

# Sponsored Products product category targeting suggestions and refinements

Category targeting allows you to target entire categories of products. There are various refinements available to filter the categories. 

## Get all categories

If you haven't done category targeting before, you first need to understand what categories are available.  

### Endpoint

[GET sp/targets/categories](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getTargetableCategories)

### Example

```
curl --location --request GET 'https://advertising-api.amazon.com/sp/targets/categories' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Accept: application/vnd.spproducttargeting.v3+json' \
--header 'Content-Type: application/vnd.spproducttargeting.v3+json' \
```

The response returns a tree of all categories currently available on Amazon.com.

## Get suggested categories

You can also request suggested categories based on a list of ASINs. This endpoint takes a list of ASINs and returns related categories that you could target. Note that all inputted ASINs should be similar, as the recommendations are for **all** ASINs in the request.

### Endpoint 

[POST /sp/targets/categories/recommendations](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getCategoryRecommendationsForASINs)

### Example

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets/categories/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.spproducttargetingresponse.v5+json' \
--header 'Content-Type: application/vnd.spproducttargeting.v3+json' \
--data-raw '{
  "includeAncestor": false,
  "asins": [
    "B08LM7291W"
  ]
}'
```

The response includes any associated categories, as well as the minimum and maximum number of products in that category.


```json
{
  "categories": [
    {
      "id": "11056341",
      "name": "Bath Soaps",
      "parentCategoryId": "11056281",
      "categoryPath": "/Beauty & Personal Care/Skin Care Products/Body Skin Care Products/Body Cleansers/Bath Soaps",
      "asinCounts": {
        "min": 8988,
        "max": 14980
      }
    }
  ]
}
```

## Category refinements

Categories may have refinements available that you can use to further target ASINs within a category. Possible refinements include brands, age ranges, and genres.

### Endpoint

[GET sp/targets/category/{categoryId}/refinements](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getRefinementsForCategory)

### Example

```
curl --location --request GET 'https://advertising-api.amazon.com/sp/targets/category/2522102011/refinements' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Accept: application/vnd.spproducttargeting.v3+json' \
--header 'Content-Type: application/vnd.spproducttargeting.v3+json' \

```

The response includes any brands, age ranges, and genres associated with the category. Not all categories will have associated brands, age ranges, and genres. You can use the IDs in the response as part of your targeting expression.


```json
{
  "brands": [
    {
      "name": "Back to Basics",
      "id": "7048034011"
    },
    {
      "name": "DJECO",
      "id": "18681839011"
    },
    {
      "name": "Disney",
      "id": "18726768011"
    },
    {
      "name": "Pipity",
      "id": "20774946011"
    },
    {
      "name": "ALEX Toys",
      "id": "2581933011"
    },
    {
      "name": "Fashion Angels",
      "id": "7481406011"
    }
  ],
  "ageRanges": [
    {
      "name": "Birth to 24 Months",
      "id": "165813011"
    },
    {
      "name": "2 to 4 Years",
      "id": "165890011"
    },
    {
      "name": "8 to 13 Years",
      "id": "5442387011"
    },
    {
      "name": "5 to 7 Years",
      "id": "165936011"
    }
  ],
  "genres": null
}
```

>[TIP] Use the [POST /sp/targets/products/count](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getTargetableASINCounts) endpoint to calculate how many products are available to target for a category and your desired refinements.

Now that you have category suggestions and refinements, you are ready to [create a category targeting expression](guides/sponsored-products/product-targeting/overview#targeting-product-categories)

