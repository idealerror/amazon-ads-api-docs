---
title: Sponsored TV getting started
description: Sponsored TV getting started with details on how to create a campaign.
type: guide
interface: api
keywords:
    - Sponsored TV
---

# Getting started with Sponsored TV campaigns



## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in this tutorial.

## Step 1: Create a campaign

When creating a Sponsored TV campaign, you can specify the budget settings you want the campaign to have. Currently, only US marketplace is supported for the `budgetCurrencyCode` parameter.

Endpoint: POST [/st/campaigns](sponsored-tv-open-beta/#tag/Campaigns/operation/CreateSponsoredTvCampaigns)

```
curl --location --request POST 'https://advertising-api.amazon.com/st/campaigns' \
--header '`Amazon-Advertising-API-Scope: xxxxxxxxxx`' \
--header '`Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx`' \
--header '`Authorization: Bearer Atza|xxxxxxxxxx`' \
--header 'Prefer: return=representation, include=extendedData'
--data-raw '{
    "campaigns": [{
     "budgetSettings": {
         "budget": {
             "recurrenceType": "DAILY",
             "budgetValue": {
                 "amount": 10.00
             }
         }
     },
     "startDate": "2023-09-25T00:00:00Z",
     "endDate": "2023-09-25T00:00:00Z",
     "name": "campaign name",
     "state": "ENABLED",
     "tags": {
         "tag1": "value1",
         "tag2": "value2",
         "tag3": "value3"
     }
 }]
}'
```

A successful response returns `campaignId`. This identifier is used to create an ad group in the next step.

```
{
    "campaigns": {
        "error": [],
        "success": [
            {
                "campaign": {
                    "budgetSettings": {
                        "budget": {
                            "budgetValue": {
                                "amount": 10.00
                            },
                            "recurrenceType": "DAILY"
                        }
                    },
                    "campaignId": "123456789",
                    "name": "campaign name",
                    "startDate": "2023-06-21",
                    "endDate": "2024-06-21",
                    "state": "ENABLED"
                },
                "campaignId": "123456789",
                "index": 0
            }
        ]
    }
}
```

## Step 2: Create an ad group

Create at least one ad group using the POST [/st/adGroups](sponsored-tv-open-beta/#tag/AdGroups/operation/CreateSponsoredTvAdGroups) endpoint. Use the `campaignId` returned in step 1 to create your ad group. For more information, see [ad groups](guides/sponsored-tv/ad-groups).

## Step 3: Create a target clause

Once you’ve created an ad group, you can add a targeting expression focusing on content, in-market ASIN category or Amazon audience targeting. For more information, see [targeting](guides/sponsored-tv/targeting). Sponsored TV supports targeting recommendations based on ASIN, which can be accessed through the steps mentioned [here](sponsored-display/3-0/openapi#tag/Targeting-Recommendations). Targeting Recommendations can be searched for by keyword [here](https://advertising.amazon.com/API/docs/en-us/targetable-entities#operation/TextInputSearch).

Endpoint: POST [/st/targets](sponsored-tv-open-beta/#tag/TargetingClauses/operation/CreateSponsoredTvTargetingClauses)

Sponsored TV also supports targeting recommendations based on ASIN for 'In-market categories'. For more information, see [targeting recommendations based on ASIN](guides/sponsored-tv/targeting#targeting-recommendations-based-on-asin).

## Step 4: Upload and register assets

You can upload and register a video asset prior to creating an ad by using the [creative asset library API](guides/creative-asset/asset-library-overview).

## Step 5: Create a Creative

Once you have the `assetID` from the previous step, you can associate the asset to an ad group by creating a creative using the POST [/st/creatives](sponsored-tv-open-beta/#tag/Creatives/operation/CreateSponsoredTvCreatives) endpoint. For more information, see [creatives overview](guides/sponsored-tv/creatives/overview).

## Step 6: Create an ad

Using the `adGroupId`, you can create an ad by using the POST [/st/ads](sponsored-tv-open-beta/#tag/Ads/operation/CreateSponsoredTvAds) endpoint. The request to create an ad differs by whether you have a seller or vendor account. For more information, see [ads](guides/sponsored-tv/ads).


## Step 7: Check moderation status

Once you have created an ad, associated targeting clauses, and associated creative, your campaign will go through moderation (up to 72 hours to review) to then be active and serve ads. For more information, see [creative moderation](guides/sponsored-tv/creatives/creative-moderation).

