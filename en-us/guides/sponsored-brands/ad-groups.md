---
title: Creating Sponsored Brands ad groups
description: Creating Sponsored Brands ad groups with the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Brands
keywords:
    - Ad groups
---

# Sponsored Brands ad groups

## Creating ad groups

Ad groups provide the ability to organize, manage, and track performance of the products within your campaign.
Ads placed together in an ad group share the targeting (keyword or product). Each campaign consists of one or more ad groups.


## Before you begin

In order to create an ad group, you must first create a campaign to get the `campaignId`. You can learn more about campaign creation in [Sponsored Brands campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns).


## Best practices


* **Ad group name:** Assign an ad group name that is descriptive and meaningful to you. Ad group names must be unique within a campaign, but you can use the same ad group name in different campaigns. Your ad group name is only used for display purposes in the campaign manager and is not visible to shoppers.
* **Keyword or product targeting:**
    * You can apply a common group of keyword or product targets to all the products within the ad group. You can use Amazon's [keyword recommendations](sponsored-brands/3-0/openapi#tag/Keyword-Recommendations/operation/getKeywordRecommendations), your own [keywords](guides/sponsored-brands/targeting/keyword-targeting), [negative keywords](guides/sponsored-brands/targeting/negative-keyword-targeting), or target by [product](guides/sponsored-brands/targeting/product-targeting).


## Request

### Endpoint

[POST /sb/v4/adGroups](sponsored-brands/3-0/openapi/prod#tag/Ad-groups/operation/CreateSponsoredBrandsAdGroups)

### Example

If you are copying the sample, be sure to enter your own client ID, access token, campaign ID, and profile ID. 

```
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/adGroups' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbadgroupresource.v4+json' \
--header 'Content-Type: application/json' \
--data-raw '{
  "adGroups": [
    {
      "campaignId": "314097457998349",
      "name": "Name for ad group",
      "state": "ENABLED"
    }
  ]
}'
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
                    "adGroupId": "557450367668505",
                    "campaignId": "314097457998349",
                    "extendedData": {
                        "creationDate": 1707333398496,
                        "lastUpdateDate": 1707333398496,
                        "servingStatus": "AD_GROUP_INCOMPLETE",
                        "servingStatusDetails": [
                            "AD_GROUP_INCOMPLETE_DETAIL"
                        ]
                    },
                    "name": "Name for ads",
                    "state": "ENABLED"
                },
                "adGroupId": "557450367668505",
                "index": 0
            }
        ]
    }
}
```

### Next steps

* [Create an ad](guides/sponsored-brands/ads/overview)