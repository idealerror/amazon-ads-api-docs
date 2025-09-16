---
title: Amazon Ads Sponsored Display dynamic segments overview 
description: Learn how to use the Amazon Ads API for Sponsored Display to target additional products that are similar to those being advertised.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - dynamic segments
    - targeting
    - similar products
    - complementary products
---

# Amazon Ads Sponsored Display dynamic segments overview

“Similar to advertised products” product targeting for Sponsored Display is a dynamic segment option that allows advertisers to leverage machine learning to help reach relevant audiences on popular detail pages of other similar and complementary products.

Dynamic segments can be used through the Amazon Ads API by creating a [contextual targeting](guides/sponsored-display/contextual-targeting) clause of type `"similarProduct"`.
 
## Benefits

With dynamic segments, advertisers can target additional products that are similar to those being advertised. The products selected by similar product targeting are based on the advertised product and are dynamically generated based on the retail data. 

Dynamic segments help advertisers reach relevant audiences most likely to click and help drive traffic to advertised products. These segments allow advertisers to launch campaigns quickly and easily to help create product consideration. 

## Usage

### "Similar product" targeting in the Amazon Ads console

When advertisers create new campaigns in the Amazon Ads console, "Similar to advertised products" targeting is added automatically. This targeting can be optionally removed.

![Dynamic segment in the Amazon Ads console](/_images/sponsored-display/dynamic-targeting-console.png)

### Creating a `similarProduct` target in the Amazon Ads API

For advertisers creating campaigns through the Amazon Ads API, a targeting clause of type `"similarProduct"` must be explicitly created to add a dynamic segment.

```json
curl --location --request POST 'https://advertising-api.amazon.com/sd/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '
[{
  "adGroupId": 295680062861813,
  "state": "enabled",
  "expressionType": "manual",
  "bid": 2.00,
  "expression": [
    {
      "type": "similarProduct",
    }
  ]
}]
```

```json
[
  {
    "adId": 364555441609199,
    "code" : "SUCCESS"
  }
]
```

## Usage notes and recommendations

### Availability

Dynamic targets are available in the Amazon Ads console and API across US, CA, UK, DE, FR, IT, ES, AE, JP, AU, NL, IN and MX. 

### Tactics

While dynamic targets are best suited for the “Optimize for page visits” tactic as it helps to display ads on detail pages of similar products, it can be used for expanding reach while using “Optimize for viewable impressions” or it can be used for driving conversions from customers already evaluating similar products when using “Optimize for conversions”. 

### With other product targeting

Dynamic segments should be used to broaden the targeting for any contextual campaigns being created. When used with other product targeting clauses, it helps advertisers show ads on product pages which are not covered by the explicit list of products being targeted but are often viewed or purchased as a substitute or complement of advertised product. 

### With campaign targeting

Dynamic segments should also be used along with category targeting, as unlike retail categories, these are dynamic targets and can help advertisers engage users on similar products across multiple retail categories. This is also different from category targeting because you could also target products that are similar but not necessarily within the same category.

## Additional Information

For more examples of building campaigns, ad groups, product ads, and targets, please review our [contextual targeting developer guide](guides/sponsored-display/contextual-targeting). 

For full technical details, please review our updated [Sponsored Display targeting API documentation](sponsored-display/3-0/openapi#tag/Targeting).