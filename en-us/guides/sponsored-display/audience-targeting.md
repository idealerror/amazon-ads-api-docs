---
title: How to build Sponsored Display Audiences campaigns 
description: Learn how to build Audiences campaigns using the Amazon Ads API for Sponsored Display.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - audiences
    - audiences campaignsaudi
    - postman
---

# Amazon Ads Sponsored Display audiences API

Audiences provides flexible controls through on-site and off-site remarketing to help convert previous detail page visitors generated from organic or paid efforts; as well as audience discovery across exact products, similar products, or entire categories to help expand reach.

To complement our custom-built audiences, which leverage Amazon shopping signals to expand reach with intuitive, flexible targeting controls (i.e., Views) you can also use Amazon audiences, which gives advertisers access to a large catalog of audience segments that are easy-to-use and ready-to-go. Amazon audiences is a way to engage new customers as well as to help advertisers better learn about their brand on Amazon. Sponsored ads users are now able to select audiences in a similar way to how they would describe their brand’s core customer—for example, “Outdoor Enthusiasts” or “Environmentally Conscious Shoppers”, and these self-service advertisers can now directly connect the language used in their overall brand marketing strategy to Sponsored Display.

- [Learn more about Amazon audiences for Sponsored Display](https://advertising.amazon.com/library/guides/sponsored-display-audiences/)
- [View technical specifications for the audiences API](audiences)

Audiences campaigns follow the standard Amazon Ads API structure to create and manage campaigns. These campaigns are very similar to Sponsored Display contextual targeting campaigns or Sponsored Products campaigns. If you have previously created these types of campaigns, you will find the concepts very similar.

## Building a Sponsored Display audiences campaign

Follow these steps to create a Sponsored Display audiences campaign.

>[NOTE] Advertisers can create targeting clauses for both Audience and Contextual campaigns with tactic `T00030`.

### Step 1: Creating campaigns

In this example, the tactic field value of `T00030` specifies an **audiences campaign**.

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "name": "My Audiences campaign",
        "tactic": "T00030",
        "budgetType": "daily",
        "costType": "cpc",
        "portfolioId": 1234567,
        "budget": 200.00,
        "startDate": "20200810",
        "endDate": "20301201",
        "state": "enabled"
    }
]'
```

```json
[
    {
        "campaignId": 295680062861811,
        "code": "SUCCESS"
    }
]
```

A successful response returns a campaign identifier. This identifier is used to create an ad group in the next step. 

We now support a new `costType` of "vcpm" so you can optimize bids for viewable impressions for awareness campaigns, when the goal is to reach a larger audience. Keep in mind that "vcpm" bidding is fundamentally different than "cpc" bidding. With "vcpm" you're bidding for 1,000 viewable impressions, whereas with "cpc" you're bidding for 1 click. Here is an example:

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "name": "My Audiences campaign",
        "tactic": "T00030",
        "budgetType": "daily",
        "costType": "vcpm",
        "portfolioId": 1234567,
        "budget": 200.00,
        "startDate": "20200810",
        "endDate": "20301201",
        "state": "enabled"
    }
]'
```

```json
[
    {
        "campaignId": 295680062861815,
        "code": "SUCCESS"
    }
]
```

### Step 2: Create an ad group

In this example, we will be creating an ad group. We now include additional controls to optimize bids based on campaign objectives with `bidOptimization`. When building campaigns, advertisers can choose between higher consideration or conversion goals. When selecting “clicks”, ads will serve to audiences more likely to click on your ad. When selecting “conversions”, the bids will be optimized for higher conversion rates, showing your ads to shoppers more likely to purchase the product. When selecting “reach”, the bids will be optimized for viewable impressions, allowing your ads to show to more shoppers. Please note, when selecting `costType` of "cpc" the `bidOptimization` will be either "clicks" or "conversions".

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/adGroups' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "campaignId": 295680062861811,
        "name": "My AdGroup for Campaign 1234567890",
        "state": "enabled",
        "bidOptimization": "conversions",
        "defaultBid": 1.20
    }
]'
```

```json
[
    {
        "code": "SUCCESS",
        "adGroupId": 510480088424693
    }
]
```

Here is an example of creating an adGroup when the campaign's `costType` is set to "vcpm". Please note, when selecting `costType` of "vcpm" the `bidOptimization` must be set to "reach".

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/adGroups' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "campaignId": 295680062861816,
        "name": "My AdGroup for Campaign 1234567890",
        "state": "enabled",
        "bidOptimization": "reach",
        "defaultBid": "1.20"
    }
]'
```

```json
[
    {
        "code": "SUCCESS",
        "adGroupId": 510480088424692
    }
]
```

A successful response returns an ad group identifer. This identifier is used to create product ads and target expressions in the next step.

### Step 3: Create product ads

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/productAds' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
  {
    "adGroupId": 510480088424692,
    "campaignId": 4295680062861816,
    "landingPageURL": "https://www.amazon.com/stores/xxxx",
    "landingPageType": "STORE",
    "state": "enabled",
    "adName": "My ad"
  }
]'
```

```json
[
    {
        "code": "SUCCESS",
        "adId": 510480088424692
    }
]
```

A successful response returns an ad identifer.

> [NOTE] In the request, vendors use the `asin` field where sellers use the `sku` field. Both fields can accept an ASIN.

### Step 4: Create target clauses

#### Example using views

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
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
    }],
    "bid": 1.25,
    "adGroupId": 209426065354910,
    "expressionType": "manual",
    "state": "enabled"
}
]'
```

```json
[
    {
        "code": "SUCCESS",
        "targetId": 314360426277856
    }
]
```

#### Example using audiences

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
     "expression": [
         {
             "type": "audience",
             "value": [
                 {
                     "type": "audienceSameAs",
                     "value": 365445316056904705
                 }
             ]
         }
     ],
     "bid": 1.25,
     "adGroupId": 209426065354910,
     "expressionType": "manual",
     "state": "enabled"
    }
]'
```

```json
[
    {
        "code": "SUCCESS",
        "targetId": 314360426277877
    }
]
```

Once you have created target clauses, your campaign is active and serving ads.

For each step described above, additional operations are available to read, update and delete the respective components (Campaign, Ad Group, Product Ads, Target Expressions). The [OpenAPI specification](sponsored-display/3-0/openapi) describes the format for each request. In the next section, we will explore our audience discovery tools.

## Discovering all the Amazon audiences available for targeting

### Taxonomy API

The taxonomy API allows advertisers to “browse” the Amazon audiences catalog hierarchy, level-by-level to discover audiences that they want to target. This API allows you to find audiences via a tree-like structure, where each level contains one or more categories. When you reach the bottom of the tree, the API will not return any categories (ie "categories": []). This API does not return the audience segments; instead, you use the category path from this API to call the audience discovery API to discover audiences.

- [View specifications for the taxonomy API](audiences#tag/Discovery/operation/fetchTaxonomy)

When you build a category path, you start with the top-level node using the following request and build the category path from each response.

```json
curl --location --request POST 'https://advertising-api.amazon.com/audiences/taxonomy/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adType": "SD",
    "categoryPath": []
}'
```

```json
{
    "categoryPath": [],
    "categories": [
        {
            "category": "In-market",
            "audienceCount": 6236
        },
        {
            "category": "Interest",
            "audienceCount": 968
        },
        {
            "category": "Lifestyle",
            "audienceCount": 672
        },
        {
            "category": "Life event",
            "audienceCount": 8
        }
    ]
}
```

The subsequent request can be the following:

```json
curl --location --request POST 'https://advertising-api.amazon.com/audiences/taxonomy/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adType": "SD",
    "categoryPath": ["In-market"]
}'
```

Finally, when you reach the bottom of the tree, it will return:

```json
{
    "categoryPath": [
        "In-market",
        "Sporting Goods",
        "Sports",
        "Basketball",
        "Backboards"
    ],
    "categories": [],
    "nextToken": null
}'
```

The next step will use the results of the `categoryPath` to discover audiences using the discovery API.

### Discovery API 

The discovery API includes many filters such as category, categoryPath , audienceName and audienceId. You can use multiple filters together such as categoryPath and audienceName to return audiences that satisfy both clauses.

- [View specifications for the audiences discovery API](audiences#tag/Discovery/operation/listAudiences)

```json
curl --location --request POST 'https://advertising-api.amazon.com/audiences/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adType": "SD",
    "filters": [
        {
            "field": "categoryPath",
            "values": [
                "In-market",
                "Sporting Goods",
                "Sports",
                "Basketball",
                "Backboards"
            ]
        },
        {
            "field": "audienceName",
            "values": [
                "*"
            ]
        }
    ]
}
```

```json
{
    "audiences": [
        {
            "audienceId": "365445316056904705",
            "audienceName": "IM - Basketball Hoops & Goals",
            "description": "People whose shopping activities indicate they are likely to purchase Basketball Hoops & Goals",
            "category": "In-market",
            "createDate": "2017-11-29T17:16:20.108Z",
            "updateDate": "2021-01-21T11:31:52Z",
            "status": "Active",
            "forecasts": {
                "inventoryForecasts": {
                    "all": {
                        "dailyReach": {
                            "lowerBoundInclusive": 500000,
                            "upperBoundExclusive": 600000
                        }
                    }
                }
            }
        }
    ],
    "matchCount": 1
}
```

> [NOTE] When using Audience Discovery for 'categoryPath', you are restricting the path to audience segments directly under the provided path. This search limits the number of audience returned based on the hierarchical structure of the Amazon audiences catalog.

You can also specify multiple search criteria within each filter `values` field - each of them is treated as an “OR” condition so if you want to search for audience names containing both **basketball** and **soccer**, you would specify these audience names within the `values` field. 

> [NOTE] This filter uses 'category' instead of 'categoryPat' so that the audiences are not limited to audiences directly within the category and audiences will be returned across one or more sub-categories (ie sub-tree). 

The following request returns categories from `in-market` that include both soccer and basketball audiences available.

```json
curl --location --request POST 'https://advertising-api.amazon.com/audiences/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adType": "SD",
    "filters": [
        {
            "field": "category",
            "values": [
                "In-market"
            ]
        },
        {
            "field": "audienceName",
            "values": [
                "basketball", "soccer"
            ]
        }
    ]
}'
```

## Overlapping audiences 

The [audience insights API](insights) helps surface other audiences that are similar to your selected audience and that you may want to target in your campaigns. You can use any audience returned from the discovery API as an input to generate overlapping audiences. 

- [View specifications for the audience insights API](insights)

For example, to retrieve overlapping audiences for an audience with ID equal to 1234567890:

```json
curl --location --request GET 'https://advertising-api.amazon.com/insights/audiences/1234567890/overlappingAudiences?adType=SD' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
```

The API will return the top 30 audiences that overlap with the selected audience and meet the specified criteria. Below you’ll see a sample with the selected audience and just its top 3 overlapping audiences shown. 

```json
{
  "requestedAudienceMetadata": {
     "audienceId": "0000000000",
     "name": "Example 1",
     "size": 0,
     "category": "Example Category 1",
     "impressionForecastRange": {
       "lowerBound": 000000000
    }
   },
   "overlappingAudiences": [
     {
       "audienceMetadata": {
         "audienceId": "0000000000",
         "name": "Example 2",
         "size": 0,
         "category": "Example Category 2",
         "impressionForecastRange": {
          "lowerBound": 00000000,
          "upperBound": 00000000        }
       },
       "affinity": 00.00
    },
     {
       "audienceMetadata": {
         "audienceId": "0000000000",
         "name": "Example 3",
         "size": 0,
         "category": "Example Category 3",
         "impressionForecastRange": { 
          "lowerBound": 000000000,
          "upperBound": 000000000
        }
       },
       "affinity": 00.00
    },
     {
       "audienceMetadata": {
         "audienceId": "0000000000",
         "name": "Example 4",
         "size": 0,
         "category": "Example Category 4",
         "impressionForecastRange": {
          "lowerBound": 000000000
        }
       },
       "affinity": 00.00
    }
}
```

You can use one or more filters to narrow the results. The API offers filters for audienceCategory, minimumAudienceSize, maximumAudienceSize, minimumOverlapAffinity, and maximumOverlapAffinity. 

For example, the following request returns the top 30 overlapping audiences that are in the in-market audienceCategory and have a minimumOverlapAffinity of 2.

```json
curl --location --request GET 'https://advertising-api.amazon.com/insights/audiences/1234567890/overlappingAudiences?adType=SD&audienceCategory=In-market&minimumOverlapAffinity=2' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
```

## Additional Campaign management examples

The retrieve operation uses the HTTP GET method with no HTTP request body to retrieve a single campaign or all campaigns.

For example, to retrieve a single campaign with an identifier equal to `1234567890`:

```json
curl --location --request GET 'https://advertising-api.amazon.com/sd/campaigns/4295680062861816 \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
```

Retrieve all campaigns for the advertiser:

```json
curl --location --request GET 'https://advertising-api.amazon.com/sd/campaigns \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
```

### Update Campaign Settings

The update operation uses the HTTP PUT method to modify campaign values. It uses the same structure as the create request except that it requires the `campaignId` field. For example:

```json
curl --location --request PUT 'https://advertising-api.amazon.com/sd/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "campaignId": 4295680062861818,
        "name": "My Audiences campaign",
        "tactic": "T00030",
        "budgetType": "daily",
        "budget": 200.00,
        "startDate": "20191010",
        "endDate": "20301201",
        "state": "enabled"
    }
]'
```

### Delete Campaign

The delete operation uses the HTTP DELETE method (no HTTP body) to delete a single campaign. For example, to delete a campaign with identifer equal to `1234567890`:

```json
curl --location --request DELETE 'https://advertising-api.amazon.com/sd/campaigns/4295680062861816' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
```

## Reference

The complete list of audience targeting clauses are defined below and in our [documentation](sponsored-display/3-0/openapi#tag/Targeting/operation/createTargetingClauses) :

| Targeting clause example | Description |
|---------------------------|-------------|
| views(exactProduct lookback=30)| Target an audience that has viewed the advertised asins in the past 7, 14, 30, 60, or 90 days, depending on the lookback. |
| views(similarProduct lookback=14) | Target an audience that has viewed similar products to the advertised asins in the past 7, 14, 30, 60, or 90 days, depending on the lookback. |
| purchases(exactProduct lookback=7)| Target an audience that has purchased the advertised asins in the past 7, 14, 30, 60, 90, 180 or 365 days, depending on the lookback. |
| purchases(relatedProduct lookback=14) | Target an audience that has purchased related products to the advertised asins in the past 7, 14, 30, 60, 90, 180 or 365 days, depending on the lookback. |
| asinCategorySameAs=12345 | Target an audience that has viewed or purchased products in the given category in the past, depending on the lookback. |
| asinBrandSameAs=12345 | Target an audience that has viewed or purchased products in the given brand in the past, depending on the lookback.
| asinPriceBetween=1-50, asinPriceGreaterThan=1, asinPriceLessThan, asinReviewRatingLessThan, asinReviewRatingGreaterThan, asinReviewRatingBetween | Target an audience that has viewed or purchased products based on ASIN specific criteria 
| asinIsPrimeShippingEligible | Target an audience that has viewed or purchased products that is Prime shipping eligible
| asinAgeRangeSameAs | Target an audience that has viewed or purchased products within a specific age range (only applies to specific categories such as toys & games)
| asinGenreSameAs | Target an audience that has viewed or purchased products within a specific genre (only applies to Books category
| lookback | The lookback window for the audience. You can now choose between 7, 14, 30, 60, or 90 days for "views" and 7, 14, 30, 60, 90, 180 or 365 days for "purchases".|

