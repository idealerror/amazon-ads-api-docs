---
title: Sponsored TV ad groups
description: Details on how to create a Sponsored TV ad group.
type: guide
interface: api
keywords:
    - Sponsored TV
    - Ad group
---

# Sponsored TV ad groups

Ad groups are a way to organize, manage, and gain insights on the performance of the products within your campaign. Products placed together in an ad group share the same bids and targeting.


## Before you begin

In order to create an ad group, you must first create a campaign to get the `campaignId`. You can learn more about campaign creation in [Sponsored TV campaigns](guides/sponsored-tv/campaigns/get-started).


## Best practices

* **Ad group name:** Assign an ad group name that is descriptive and meaningful to you. Ad group names must be unique within a campaign, but you can use the same ad group name in different campaigns. Your ad group name will only be used for display purposes in the campaign manager and won't be visible to customers.
* **Default bid:** The default bid amount applies to all targeting expressions of the ad group if you don't define bids for the targeting expressions explicitly. Choosing a competitive bid can help you win more auctions and increase awareness of your brand and products.

## Request

Endpoint
POST [/st/adGroups](sponsored-tv-open-beta#tag/AdGroups/operation/CreateSponsoredTvAdGroups)


```
curl --location 'https: //[advertising-api.amazon.com/st/adGroups](http://advertising-api.amazon.com/st/adGroups)' \ 
—header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \ 
—header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \ 
—header 'Authorization: Bearer Atza|xxxxxxxxxx' \ 
—data '{
    "adGroups": [
        {
            "campaignId": "{{campaignId}}",
            "name": "ad group name",
            "state": "ENABLED",
            "defaultBid": {
                "bid": 25.0
            }
        }
    ]
}'
```

A successful response returns `adGroupId`. This identifier is used to specify target expressions, associate creative, and associate ads in the next steps.


```
{
    "adGroups": {
        "error":[],
        "success": [
            {
                "adGroup": {
                    "adGroupId":"0987654321",
                    "campaignId":"1234567890",
                    "defaultBid": {
                        "bid":25.0
                    },
                    "extendedData": {
                        "creationDate": "1687385416692",
                        "lastUpdateDate":"1687385416692",
                        "servingStatus":"AD_GROUP_INCOMPLETE"
                    },
                    "name": "ad group name",
                    "state":"ENABLED"
                },
                "adGroupId":"542165796724493",
                "index":0
            }
        ]
    }
}
```
