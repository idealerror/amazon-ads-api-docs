---
title: Creating negative product targeting
description: A tutorial for negative product targets for Sponsored Brands, with example requests and responses.
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - negative product targeting
---

# Sponsored Brands negative product targeting


Negative product targeting allows you to exclude certain products and brands from your targeting. You can create a negative targeting expression at the ad group level. All ads in an ad group will share targeting clauses.

Visit the console for more information on [negative product targeting](https://advertising.amazon.com/help/G8W49VU65XQ4T2NS).

## Before you begin

You must have the following before creating a keyword:

* [A campaign Id](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-a-campaign)
* [An ad group Id](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-ad-group)

## Creating negative product targeting

**Endpoint**
[POST /sb/negativeTargets](sponsored-brands/3-0/openapi#tag/Negative-product-targeting/operation/createNegativeTargets)


|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|adGroupId	|No	|string	|The ad group to associated the campaign with	|
|campaignId	|No	|string	|The campaign to associated the target	|
|expressions.type	|No	|string	|Either `asinBrandSameAs or `asinSameAs``	|
|expressions.value	|No	|string	|You can get suggested brand IDs by keyword using the [POST /sb/recommendations/targets/brand](sponsored-brands/3-0/openapi#tag/Targeting-recommendations/operation/getBrandRecommendations) endpoint.	|
|	|	|	|	|

```
curl --location 'https://advertising-api.amazon.com/sb/negativeTargets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
  "negativeTargets": [
    {
      "adGroupId": {{adGroupId}},
      "campaignId": {{campaignId}},
      "expressions": [
        {
          "type": "asinBrandSameAs",
          "value": "B070000000"
        }
      ]
    }
  ]
}'
```

A successful request results in a `200` response that contains a `targetId` as shown below:


```
{
    "createTargetSuccessResults": [
        {
            "targetRequestIndex": 0,
            "targetId": 468301703731455
        }
    ],
    "createTargetErrorResults": []
}
```



