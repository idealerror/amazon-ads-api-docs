---
title: Negative targeting
description: Amazon Ads API version 3 Sponsored Products Product Targeting reference.
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - negative product targeting
    - negative product
---

# Sponsored Products negative product and brand targeting overview

Both auto and manual Sponsored Products campaigns can use negative product targeting. Negative product targeting allows you to exclude certain products and brands from your targeting. You can lean more about negative product targeting in the [advertising console documentation](https://advertising.amazon.com/help?#G8W49VU65XQ4T2NS). 

You can create a negative targeting expression at either the ad group or campaign level. Any campaign-level negative product targeting expression will impact all ad groups using product targeting in the campaign. Negative product targeting is optional for all Sponsored Products campaigns.

You can learn more about campaign structure in [Campaign hierarchy](guides/sponsored-products/get-started/campaign-structure).


>[WARNING] Campaign-level negative product targeting for manual campaigns is only available for vendors. Sellers can add campaign-level negative targeting to auto campaigns, but not manual. 

## Endpoints

|Campaign-level targets	|Ad group-level targets	|
|---	|---	|
| [POST /sp/campaignNegativeTargets](sponsored-products/3-0/openapi/prod#tag/CampaignNegativeTargetingClauses/operation/CreateSponsoredProductsCampaignNegativeTargetingClauses)	| [POST /sp/negativeTargets](sponsored-products/3-0/openapi/prod#tag/NegativeTargetingClauses/operation/CreateSponsoredProductsNegativeTargetingClauses)	|

## Before you begin

Before creating product targeting expressions, you need to:

* [Create a manual or auto campaign](guides/sponsored-products/campaigns)
* [Create an ad group](guides/sponsored-products/ad-groups)
* [Create at least one product ad](guides/sponsored-products/product-ads)
* Add [product](guides/sponsored-products/product-targeting/overview) or [keyword](guides/sponsored-products/keywords/overview) targeting (manual campaigns only)

## Request

### Parameters

Requests to create a target use the following parameters:

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|campaignId	|No	|number	|The campaign to associate the targeting expression with.	|
|adGroupId	|No	|String	|The ad group to associate the campaign with. adGroupId is only required for ad group-level negative targeting.	|
|expression.type	|No	|string	|Either `ASIN_SAME_AS` or `ASIN_BRAND_SAME_AS`	|
|expression.value	|No	|string	|Either an ASIN or brand ID based on the expression type. You can get suggested brand IDs by keyword using the [POST sp/negativeTargets/brands/search](sponsored-products/3-0/openapi/prod#/Product%20Targeting/searchBrands) endpoint.	|
|state	|No	|string	|The state of the targeting expression, either ENABLED or PAUSED	|

### Examples

#### ASIN at the ad group level

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/negativeTargets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spNegativeTargetingClause.v3+json' \
--header 'Content-Type: application/vnd.spNegativeTargetingClause.v3+json' \
--data-raw '{
  "negativeTargetingClauses": [
    {
      "expression": [
        {
          "type": "ASIN_SAME_AS",
          "value": "B07YT8NVF9"
        }
      ],
      "campaignId": "26256301417055",
      "state": "ENABLED",
      "adGroupId": "169462822148652"
    }
  ]
}'
```

#### Brand at the campaign level

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/campaignNegativeTargets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spCampaignNegativeTargetingClause.v3+json' \
--header 'Content-Type: application/vnd.spCampaignNegativeTargetingClause.v3+json' \
--data-raw '{
  "negativeTargetingClauses": [
    {
      "expression": [
        {
          "type": "ASIN_BRAND_SAME_AS",
          "value": "18681839011"
        }
      ],
      "campaignId": "26256301417055",
      "state": "ENABLED"
    }
  ]
}'
```

## Response

A successful request results in a 207 response that contains individual status messages for each targeting clause you created. You should check that the success array contains all objects you intended to create. Any failures are indicated in the error array. 

>[TIP] If you want to return the whole targeting object in the request, make sure to include `Prefer: return=representation` as a header. 
