---
title: Negative keyword targeting
description: Amazon Ads API version 3 Sponsored Products negative keyword targeting reference.
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - negative keyword targeting
    - negative keywords
---

# Negative keyword targeting

Negative keywords help prevent ads from appearing on shopping results pages that don’t meet your campaign objectives. Negative keyword targeting can be used at either the campaign level or ad group level. 

## Campaign versus ad group-level negative keyword targeting

When using negative keyword targeting at the campaign level, the targeting will apply to all ad groups created under that  specific campaign. When the negative keyword targeting is done at the ad group level, it will only apply to that specific ad group. 

### Endpoints

|Campaign-level keywords	|Ad group-level keywords	|
|---	|---	|
| [POST /sp/campaignNegativeKeywords](sponsored-products/3-0/openapi/prod#tag/CampaignNegativeKeywords/operation/CreateSponsoredProductsCampaignNegativeKeywords)	| [POST /sp/negativeKeywords](sponsored-products/3-0/openapi/prod#tag/NegativeKeywords/operation/CreateSponsoredProductsNegativeKeywords)	|

## Campaign level negative keyword creation

### Parameters

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|campaignId	|No	|String	|The ID of the campaign	|
|matchType	|No	|Enum	|The type of match. Can be `NEGATIVE_EXACT` or `NEGATIVE_PHRASE`. For more information, see match types in the [advertising console help center](https://advertising.amazon.com/help?#GHTRFDZRJPW6764R).	|
|state	|No	|Enum	|The current resource state which can be `ENABLED` or `PAUSED`	|
|adGroupId	|No	|String	|The ID of the ad group. Only relevant for ad group level negative keywords.	|
|keywordText	|No	|String	|The keyword text.	|

### Example

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/campaignNegativeKeywords' \
--header 'Amazon-Advertising-API-ClientId: xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: xxxxxxxx \
--header 'Accept: application/vnd.spNegativeKeyword.v3+json' \
--header 'Content-Type: application/vnd.spNegativeKeyword.v3+json' \

--data-raw '[
  {
    "campaignId": 104465559608285,
    "state": "enabled",
    "keywordText": "toy",
    "matchType": "NEGATIVE_EXACT"
  }
]'
```

### Response


A successful response will return a `207 `that contains an error array for any errors present and a success array with the  `campaignNegativeKeywordId`


```
{
  "campaignNegativeKeywords": {
    "error": [],
    "success": [
      {
        "campaignNegativeKeywordId": "219480858473246",
        "index": 0
      }
    ]
  }
}
```

## Ad group level negative keyword creation

### Example


```
curl --location --request POST 'https://advertising-api.amazon.com/sp/negativeKeywords' \
--header 'Amazon-Advertising-API-ClientId: xxxxxxxx \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx \
--header 'Accept: application/vnd.spCampaignNegativeKeyword.v3+json' \
--header 'Content-Type: application/vnd.spCampaignNegativeKeyword.v3+json' \
--data-raw '[
  {
    "campaignId": 153839444046652,
    "adGroupId": 104465559608285,
    "state": "ENABLED",
    "keywordText": "expensive",
    "matchType": "NEGATIVE_EXACT"
  }
]'
```

### Response

A successful response will return a `207 `that contains an error array for any errors present and a success array with the  `NegativeKeywordId`.

```
{
  "negativeKeywords": {
    "error": [],
    "success": [
      {
        "index": 0,
        "negativeKeywordId": "227104043184993"
      }
    ]
  }
}
```

