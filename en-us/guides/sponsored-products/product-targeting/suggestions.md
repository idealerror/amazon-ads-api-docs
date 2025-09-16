---
title: Product targeting suggestions
description: Tutorial for getting product targeting suggestions for Sponsored Products using the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - ASIN
    - Product targeting
---

# Sponsored Products product targeting recommendations

If you don’t know what products to target, Amazon can recommend products based on the ASINs you are advertising. 

## Request

>[TIP] If you are including multiple ASINs in the request, make sure they are similar products. The response includes suggested products for **all** ASINs included in the request.

You can either request recommendations by theme or by ASIN. Recommendations by theme returns a list of themes (for example, similar items or complementary items). Each theme includes a list of ASINs that fall into that category. Recommendations by ASIN returns a list of ASINs. Each ASIN includes an explanation of what theme it matches. See the examples below for more information. The type of response is determined by the value of the Accept header. 

### Endpoint

[GET /sp/targets/products/recommendations endpoint](sponsored-products/3-0/openapi/prod#tag/Product-Recommendation-Service/operation/getProductRecommendations)

### By ASIN

#### Request

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets/products/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Accept: application/vnd.spproductrecommendationresponse.asins.v3+json' \
--header 'Content-Type: application/vnd.spproductrecommendationresponse.asins.v3+json' \
--data-raw '{
  "adAsins": [
    "B09LJWZT6G"
  ],
  "count": "5",
  "locale": "en_US"
}'
```

#### Response

```
{
  "recommendations": [
    {
      "recommendedAsin": "B08YJY5KW5",
      "themes": [
        "Similar items (frequently viewed together)"
      ]
    },
    {
      "recommendedAsin": "B09TGKBFCM",
      "themes": [
        "Similar items (frequently viewed together)"
      ]
    },
    {
      "recommendedAsin": "B01LL9ITX8",
      "themes": [
        "Similar items (frequently viewed together)"
      ]
    },
    {
      "recommendedAsin": "B07C8RCNXD",
      "themes": [
        "Similar items (frequently viewed together)"
      ]
    },
    {
      "recommendedAsin": "B06XWRGMVT",
      "themes": [
        "Similar items (frequently viewed together)"
      ]
    }
  ],
  "nextCursor": "h9oGnf2jn2qd3ymV4nWLsA=="
}
```

>[NOTE] Count in the request determines the number of ASINs that are returned in the response. You can use the `nextCursor` or `previousCursor` values in a new request to return the next set of ASINs.

### By theme

#### Request

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targets/products/recommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: 904444984714944' \
--header 'Accept: application/vnd.spproductrecommendationresponse.themes.v3+json' \
--header 'Content-Type: application/vnd.spproductrecommendationresponse.themes.v3+json' \
--data-raw '{
  "adAsins": [
    "B09LJWZT6G"
  ],
  "locale": "en_US",
  "count": 5
}'
```

#### Response

```
{
  "recommendations": [
    {
      "theme": "Similar items (frequently viewed together)",
      "description": "Items that shoppers frequently view and click along with your advertised item during a shopping session.",
      "recommendedAsins": [
        "B08YJY5KW5",
        "B09TGKBFCM",
        "B01LL9ITX8",
        "B07C8RCNXD",
        "B06XWRGMVT",
        "B09TK2394N"
      ]
    },
    {
      "theme": "Complements",
      "description": "Items frequently bought together as complements (e.g. tennis balls with tennis raquets)",
      "recommendedAsins": [
        "B091K1C3KD",
        "B09VPFJYVR",
        "B09TLM53P7",
        "B099QV15NM",
        "B0B4BFTB3D",
        "B09B4QD42W"
      ]
    }
  ],
  "nextCursor": null,
  "previousCursor": null
}
```

>[NOTE] The `count` in the request determines the number of themes that are returned in the response. You can use the `nextCursor` and `previousCursor` values in a new request to return the next set of themes.

You can then use the recommendations to [create a targeting expression](guides/sponsored-products/product-targeting/overview). 


