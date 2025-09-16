---
title: Creating Sponsored Products product ads
description: Creating Sponsored Products product ads with the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - Product ads
---

# Sponsored Products product ads

Product ads are the smallest unit of your campaign and represent the individual products that you want to advertise. You can learn more about campaign structure in [Campaign hierarchy](guides/sponsored-products/get-started/campaign-structure).

Each product ad is associated to a single product, either by SKU (sellers) or ASIN (vendors and authors). 

Each product ad is associated to an [ad group](guides/sponsored-products/ad-groups). We recommend grouping similar products, or products that share similar targeting criteria, in the same ad group. 

## Before you begin

* [Create a campaign](guides/sponsored-products/campaigns)
* [Create an ad group](guides/sponsored-products/ad-groups)

## Checking product eligibility

To check if a product is eligible for advertising, you can use the [POST /eligibility/product/list](eligibility-prod-3p/#/Product%20Eligibility/productEligibility) endpoint. Products must in in stock in order for the ad to serve. 

>[NOTE] If there is an ASIN that is not valid within the list of valid ASINs requested, an error code of `"BAD_REQUEST", "details": "One or more requested ASINs are not valid."` is returned. If there is an ASIN that is not in the merchant's catalog, that ASIN will be excluded from the response body.

### Example

This example checks the eligibility of ASIN `B0B9VJLZ69`.

```
curl --location --request POST 'https://advertising-api.amazon.com/eligibility/product/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "adType": "sp",
  "productDetailsList": [
    {
      "asin": "B0B9VJLZ69"
    }
  ]
}'
```

## Request

To create a product ad, you must include `campaignId`, `adGroupId`, `asin` (vendors and KDP authors) or `sku` (sellers), and `state`.

### Endpoint

[POST sp/productAds](sponsored-products/3-0/openapi/prod#tag/ProductAds/operation/CreateSponsoredProductsProductAds)

### Examples

#### Vendor

Vendors should include an ASIN in the request body. 

```
[
  {
    "campaignId": {{campaignIdManual}},
    "adGroupId": {{adGroupIdManual}},
    "asin": "B09XBVBHR2",
    "state": "enabled"
  }
]
```

#### Seller

If you are a seller who needs to find a SKU associated with a product, you can look in Seller Central. If you know the ASIN associated with your product, you can also use the [Product metadata endpoint](product-metadata/#/Product%20selector) to get the SKU if you already know the ASIN. 

```
[
  {
    "campaignId": {{campaignIdManual}},
    "adGroupId": {{adGroupIdManual}},
    "sku": "a140aafa-1a23",
    "state": "enabled"
  }
]
```

#### KDP

KDP authors can choose whether to add custom ad copy to their ads using the `customText` field. Note that if you add `customText` to a product ad, you can only have one product ad in that ad group. If you don’t use `customText`, you can have multiple product ads in the ad group.

```
[
  {
    "campaignId": {{campaignIdManual}},
    "adGroupId": {{adGroupIdManual}},
    "asin": "B09XBVBHR2",
    "state": "enabled",
    "customText": "An exciting thriller for all!"
  }
]
```

#### Complete cURL

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/productAds' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spProductAd.v3+json' \
--header 'Content-Type: application/vnd.spProductAd.v3+json' \
--data-raw '{
  "productAds": [
    {
      "campaignId": "52978233888897",
      "state": "ENABLED",
      "asin": "B087TKYT4X",
      "adGroupId": "36453868083101"
    }
  ]
}'
```

## Response

A successful call returns a 207 response code. The response body contains a JSON array of objects that have the status for each `productAd` object in the request.

>[TIP] If you want to receive the full campaign objects in the response, use the `Prefer` header set to `return=representation`.

## Next steps

For [manual campaigns](guides/sponsored-products/get-started/manual-campaigns), once you have created a product ad, you need to add either a keyword or targeting clause. 

* [Add keywords](guides/sponsored-products/keywords/overview)
* [Add targeting clauses](guides/sponsored-products/product-targeting/overview)

For [auto campaigns](guides/sponsored-products/get-started/auto-campaigns), after you create a product ad, you should be all set!
