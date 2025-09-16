---
title: Amazon Ads Sponsored Display contextual targeting API 
description: Learn how to use the Amazon Ads API for Sponsored Display Brand Safety features to target your product to customers actively browsing your product, similar products, or in similar categories to your product's category.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - contextual targeting
    - targeting
    - similar products
    - similar categories
---

# Amazon Ads Sponsored Display contextual targeting API

Contextual targeting helps promote your product among audiences who are actively browsing your product or similar products and categories with ads that appear on relevant product detail pages. Additionally, category and similar product targeting will allow advertisers to reach more of their audiences at more points in their shopping and entertainment journey. The contextual targeting strategy will now allow advertisers to reach audiences both in the Amazon store and outside the Amazon store when customers view content related to targeting strategies. 
 
Contextual targeting campaigns follow the standard Amazon Ads API structure to create and manage campaigns, using the same grammar as Sponsored Products campaigns. If you have previously created Sponsored Products campaigns, you will find the concepts very similar.

>[TIP:Dynamic targeting]We recommend using "similarProduct" targeting for all of your campaigns in order to target additional products that are similar to those being advertised. The products selected by "similarProduct" are based on the advertised product and are dynamically generated using machine learning models that are built on retail data. [Learn more about dynamic targeting.](guides/sponsored-display/dynamic-segments)

## Building Sponsored Display contextual targeting campaigns

Follow these steps to create a Sponsored Display contextual targeting campaign.

>[NOTE] Advertisers can create targeting clauses for both Audience and Contextual campaigns with tactic `T00030`.

### Step 1: Creating campaigns

Either `tactic` field value of `T00020` or `T00030` can specify a contextual targeting campaign:

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
    "name": "My contextual targeting campaign",
    "tactic": "T00030",
    "budgetType": "daily",
    "costType": "cpc",
    "portfolioId": 1234567,
    "budget": 200.00,
    "startDate": "20200810",
    "endDate": "20301201",
    "state": "enabled"
}
]
```

```json
[
  {
    "campaignId": "1234567890",
    "code" : "SUCCESS"
  }
]
```

A successful response returns a campaign identifier. This identifier is used to create an ad group in the next step.  

We support a `costType` of "vcpm" so you can optimize bids for viewable impressions for awareness campaigns, when the goal is to reach a larger audience. Keep in mind that "vcpm" bidding is fundamentally different than "cpc" bidding. With "vcpm" you're bidding for 1,000 viewable impressions, whereas with "cpc" you're bidding for 1 click. Here is an example:

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
  {
    "name": "My contextual targeting campaign",
    "tactic": "T00030",
    "budgetType": "daily",
    "costType": "vcpm",
    "portfolioId": 1234567,
    "budget": 200.00,
    "startDate": "20200810",
    "endDate": "20301201",
    "state": "enabled"
  }
]
```

```json
[
  {
    "campaignId": 489272151685325,
    "code" : "SUCCESS"
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
    "campaignId": 489272151685325,
    "name": "My AdGroup for Campaign 1234567890",
    "state": "enabled",
    "bidOptimization": "clicks",
    "defaultBid": 1.20
  }
]
```

```json
[
  {
    "adGroupId": 295680062861814,
    "code" : "SUCCESS"
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
    "campaignId": 295680062861815,
    "name": "My AdGroup for Campaign 1234567890",
    "state": "enabled",
    "bidOptimization": "reach",
    "defaultBid": 5.0
  }
]
```

```json
[
  {
    "adGroupId": 295680062861815,
    "code" : "SUCCESS"
  }
]
```

A successful response returns an ad group identifer. This identifier is used to create product ads and target expressions in the next steps.

### Step 3: Create product ads

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/productAds' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
  {
    "adGroupId": 295680062861814,
    "campaignId": 295680062861815,
    "landingPageURL": "https://www.amazon.com/stores/xxxx",
    "landingPageType": "STORE",
    "state": "enabled",
    "adName": "My ad"
  }
]
```

```json
[
  {
    "adId": 364555441609193,
    "code" : "SUCCESS"
  }
]
```

A successful response returns an ad identifer.

### Step 4: Create target clauses examples

#### `similarProduct` targeting

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
  {
    "adGroupId": 295680062861814,
    "state": "enabled",
    "expressionType": "manual",
    "bid": 2.00,
    "expression": [
   {
       "type": "similarProduct",
   }
  ]
  }
]
```

```json
[
  {
    "targetId": 314360426277899,
    "code" : "SUCCESS"
  }
]
```

#### `asinSameAs` targeting

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '[
  {
    "adGroupId": 295680062861814,
    "state": "enabled",
    "expressionType": "manual",
    "bid": 0.10,
    "expression": [
    {
       "type": "asinSameAs",
       "value": "B00362RVG0"
    }
  ]
  }
]
```

```json
[
  {
    "targetId": 314360426277898,
    "code" : "SUCCESS"
  }
]
```

Once you have created targeting expressions, your campaign is active and serving ads.

For each step described above, additional operations are available to read, update, and delete the respective components (Campaign, Ad Group, Product Ads, Target Expressions). The [Open API specification](sponsored-display/3-0/openapi) describes the format for each request. Let's explore examples for campaign management in the next section.

## Campaign management

The retrieve operation uses the HTTP GET method with no HTTP request body to retrieve a single campaign or all campaigns.

For example, to retrieve a single campaign with an identifier equal to `1234567890`: 

```bash
GET /sd/campaigns/1234567890 HTTP/1.1
```

Similarly, you can retrieve all campaigns for the advertiser:

```bash
GET /sd/campaigns/ HTTP/1.1
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
        "campaignId": 295680062861813,
        "name": "My Audiences campaign",
        "tactic": "T00030",
        "budgetType": "daily",
        "budget": 200.00,
        "startDate": "20191010",
        "endDate": "20301201",
        "state": "enabled"
    }
]'
[
  {
    "campaignId": 295680062861811,
    "name": "My contextual targeting campaign",
    "tactic": "T00030",
    "budgetType": "daily",
    "budget": 200.00,
    "startDate": "20191010",
    "endDate": "20301201",
    "state": "enabled"
  }
]
```

### Delete Campaign

The delete operation uses the HTTP DELETE method (no HTTP body) to delete a single campaign. For example, to delete a campaign with identifer equal to `295680062861815`:

```json
curl --location --request DELETE 'https://advertising-api.amazon.com/sd/campaigns/295680062861815' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
```

## Negative targeting

Negative Targeting is available to contextual targeting campaigns using the `/sd/negativeTargets` resource. This resource follows the same format as regular targeting expressions defined in `/sd/targets` with the exception being that all targeting expressions are negated within the ad group.

For example, if you have already created targeting expressions to target a category with an identifer equal to `45678`. 

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '
[{
  "adGroupId": 295680062861817,
  "state": "enabled",
  "expressionType": "manual",
  "bid": 1.10,
  "expression": [
   {
       "type": "asinCategorySameAs",
       "value": 45678
   }
  ]
}]
```

You can exclude brands from being targeted within the category, such as a brand identifier equal to `98765` by using the following (same body but different resource):

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/negativeTargets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '
[{
  "adGroupId": 295680062861817,
  "state": "enabled",
  "expressionType": "manual",
  "expression": [
   {
       "type": "asinBrandSameAs",
       "value": 98765
   }
  ]
}]
```

## Reference

Each available contextual targeting clause is defined below.

| Contextual targeting clause | Description |
|---------------------------|-------------|
| asinSameAs | Target products with the same ASIN |
| asinCategorySameAs | Target products with the same category |
| asinBrandSameAs | Target products with the same brand |
| asinPriceBetween, asinPriceGreaterThan, asinPriceLessThan, asinReviewRatingLessThan, asinReviewRatingGreaterThan, asinReviewRatingBetween | Target an audience that has viewed products based on ASIN specific criteria |
| asinIsPrimeShippingEligible | Target products that are Prime shipping eligible |
| asinAgeRangeSameAs | Target products within a specific age range (only applies to specific categories such as toys & games) |
| asinGenreSameAs | Target products products within a specific genre (only applies to Books category |
| similarProduct | Target products that are similar to the advertised ASIN |


**Next steps**

If you've already [set up your account](guides/onboarding/overview), you're ready to [create Sponsored Display campaigns using Postman](guides/sponsored-display/tutorials/postman).