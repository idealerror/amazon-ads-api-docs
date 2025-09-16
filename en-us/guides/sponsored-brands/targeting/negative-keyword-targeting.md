---
title: Creating negative keyword targeting
description: A tutorial for negative keyword targets for Sponsored Brands, with example requests and responses.
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - negative keyword targeting
---

# Sponsored Brands negative keyword targeting

Negative keywords prevent your keyword targeted ads from displaying when a shopper’s shopping queries match your negative keywords. You can use negative keywords to exclude poorly performing keywords. This can help reduce your advertising cost and help increase your return on ad spend (ROAS). Negative keyword targeting can be used at the ad group level which will affect any ads under that specific ad group. 

Visit the support console for more information on [negative product targeting](https://advertising.amazon.com/help/G8W49VU65XQ4T2NS).

If you don't know what keywords to target, try using [negative keyword recommendations](sponsored-brands/3-0/openapi#tag/Negative-keywords/operation/listNegativeKeywords) resource to get a list of negative keywords.

## Before you begin

You must have the following before creating a keyword:

* [A campaign ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-a-campaign)
* [An ad group ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-ad-group)


## Create negative keyword targeting

>[NOTE] For Sponsored Brands, there is no campaign-level negative targeting.

**Endpoint:**
[POST /sb/negativeKeywords](sponsored-brands/3-0/openapi#tag/Negative-keywords/operation/createNegativeKeywords)

### Parameters


|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|adGroupId	|No	|string	|The ad group to associated the campaign with	|
|campaignId	|No	|string	|The campaign to associated the target	|
|keywordText	|No	|string	|The keyword text	|
|matchType	|No	|string	|The negative match type. For more information, see [negative keyword match types](https://advertising.amazon.com/help#GHTRFDZRJPW6764R) in the Amazon Ads support center.	|
|	|	|	|	|

### Example

```json
curl --location 'https://advertising-api.amazon.com/sb/negativeKeywords' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.`xxxxxxxx`' \
--header 'Authorization: Bearer `xxxxxxxx`' \
--header 'Amazon-Advertising-API-Scope: `xxxxxxxx`' \
--header 'Content-Type: application/json' \
--data '[
  {
    "adGroupId" : {{adGroupId}},
    "campaignId" : {{campaignId}},
    "keywordText": "shoes",
    "matchType": "negativeExact"
  }
]'
```

### Response

A successful request results in a `207` response that contains a `keywordId` as shown below:

```json
[
    {
        "code": "SUCCESS",
        "keywordId": 366468964014852
    }
]
```
