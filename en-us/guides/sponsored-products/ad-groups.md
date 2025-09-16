---
title: Creating Sponsored Products ad groups
description: Creating Sponsored Products ad groups with the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - Ad groups
---

# Sponsored Products ad groups

## Creating ad groups

Ad groups provide the ability to organize, manage, and track performance of the products within your campaign.
Products placed together in an ad group share the same bids and targeting (keyword or product). Each campaign consists of one or more ad groups.


## Before you begin

In order to create an ad group, you must first create a campaign to get the `campaignId`. You can learn more about campaign creation in [Sponsored Products campaigns](guides/sponsored-products/campaigns).


## Best practices


* **Ad group name:** Assign an ad group name that is descriptive and meaningful to you. Ad group names must be unique within a campaign, but you can use the same ad group name in different campaigns. Your ad group name is only used for display purposes in the campaign manager and is not visible to shoppers.
* **Keyword or product targeting:**
    * In auto-targeted campaigns, negative keyword and negative product targeting is available to apply to the ad group. Amazon assigns targeting expressions and keywords.
    * In manual-targeted campaigns, you can apply a common group of keyword or product targets to all the products within the ad group. You can use Amazon's [keyword recommendations](sponsored-products/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getRankedKeywordRecommendation), your own [keywords](sponsored-products/3-0/openapi/prod#tag/Keywords/operation/CreateSponsoredProductsKeywords), [negative keywords](sponsored-products/3-0/openapi/prod#tag/NegativeKeywords/operation/CreateSponsoredProductsNegativeKeywords), or target by [product](sponsored-products/3-0/openapi/prod#tag/TargetingClauses/operation/CreateSponsoredProductsTargetingClauses).
* **Default Bid:** The default bid amount applies to all child entities (keywords, targeting expressions, etc.) of the ad group if you don't define bids for the child entities explicitly. For instance, if a keyword child entity doesn't include a defined bid, the keyword assumes the default bid for the ad group. To learn more, see [Sponsored Products campaign structure](guides/sponsored-products/get-started/campaign-structure).


## Request

### Endpoint

[POST /sp/adGroups](sponsored-products/2-0/openapi#/prod#tag/AdGroups/operation/CreateSponsoredProductsAdGroups)

### Example

If you are copying the sample, be sure to enter your own client ID, access token, campaign ID, and profile ID. 

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/adGroups' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx' \
--header 'Accept: application/vnd.spAdGroup.v3+json' \
--header 'Content-Type: application/vnd.spAdGroup.v3+json' \
--data-raw '[
  {
    "name": name,
    "campaignId": 153839444046652,
    "defaultBid": "3.0",
    "state": "enabled"
  }
]'
```

### Response

A successful response returns a `207` response code. The `error` array contains any error messages that can occur when creating an ad group. The `success` array returns the ad group object which contains the `adGroupId.`

```
 {
    "adGroups": {
        "error": [],
        "success": [
            {
                "adGroup": {
                    "adGroupId": "38308592845076",
                    "name": "adGroupTest"
                },
                "adGroupId": "38308592845076",
                "index": 0
            }
        ]
    }
}
```

### Next steps

* [Create a product ad](guides/sponsored-products/product-ads)

