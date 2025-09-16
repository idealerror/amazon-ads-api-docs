---
title: Make your first call
description: Guide to making your first call to the Amazon Ads API
type: guide
interface: api
tags:
    - Onboarding
    - Authorization
keywords:
    - Postman
    - get started
    - headers
---

# Make your first call to the Amazon Ads API

You can use the Ads API to manage campaigns, pull reporting data, and more. This tutorial helps you understand how to list all of your active sponsored ads (Sponsored Products, Sponsored Brands, and Sponsored Display) campaigns using the relevant GET campaigns endpoint.

## Before you begin

>[NOTE] This tutorial assumes you have already completed the [onboarding](guides/onboarding/overview) and [getting started](guides/get-started/overview) processes for the Ads API. 

To complete this tutorial, make sure you have:

- [Access to the Ads API](guides/onboarding/apply-for-access)
- [A Login with Amazon application client ID](guides/onboarding/create-lwa-app)
- [A valid access token](guides/account-management/authorization/access-tokens)
- [A profile ID](guides/account-management/authorization/profiles) for an Amazon Ads account

## Request

### URL prefixes

The prefix you should use when calling the Ads API is based on the geography of the [profile](guides/account-management/authorization/profiles) used in the request.

| URL | Region and marketplaces |
| --- | --- |
| https://<span></span>advertising-api.amazon.com | North America (NA). Covers US, CA, MX, and BR marketplaces. |
| https://<span></span>advertising-api-eu.amazon.com | Europe (EU). Covers UK, FR, IT, ES, DE, NL, SE, PL, BE, ZA, EG, AE, SA, TR, and IN marketplaces. |
| https://<span></span>advertising-api-fe.amazon.com | Far East (FE). Covers JP, AU, and SG marketplaces. |

### Headers

Most calls to the Amazon Ads API require common headers:

| Header | Required? | Description | 
|--------|-------------|
| `Amazon-Advertising-API-ClientId` | Yes | The client ID related to a Login with Amazon application. |
| `Authorization` | Yes | A valid API access token in the format `Bearer access_token`. An access token is only valid for one hour. |
| `Amazon-Advertising-API-Scope` | Yes | Amazon Ads profile ID. |
| `Accept` | No |  The accept header is used to specify the version. If no `Accept` header is specified, it defaults to `application/json`. |

### Sample requests

If you are copying the samples, be sure to enter your own client ID, access token, and profile ID.

#### Sponsored Products

Full reference: [POST sp/campaigns/list](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredProductsCampaigns)

<details class="details-bar">
    <summary>cURL</summary>

    <p>This example shows a list Sponsored Products campaigns request using the North America URL prefix.</p>

    ```bash
        curl --location --request POST 'https://advertising-api.amazon.com/sp/campaigns/list' \
        --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
        --header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
        --header 'Authorization: Bearer xxxxxxxxxxxx' \
        --header 'Accept: application/vnd.spCampaign.v3+json' \
        --header 'Content-Type: application/vnd.spCampaign.v3+json' \
    ```

</details>

<details class="details-bar">
    <summary>Postman</summary>

  1. Make sure you have the Amazon Ads API Postman collection and environment files imported into Postman. If you haven't completed Postman setup, see our [Postman tutorial](guides/postman). 
  2. From Postman, navigate to the **Amazon Ads API** collection. 
  3. Go to the **First call** folder and open the **`POST` List SP campaigns** endpoint.
  3. In the **Headers** tab, ensure your environment variables are populating properly.
  4. Send your request.

</details>

#### Sponsored Brands

Full reference: [POST sb/v4/campaigns/list](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns)

<details class="details-bar">
    <summary>cURL</summary>

    <p>This example shows a list Sponsored Brands campaigns request using the North America URL prefix.</p>

    ```bash
        curl --location --request GET 'https://advertising-api.amazon.com/sb/v4/campaigns/list' \
        --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
        --header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
        --header 'Authorization: Bearer xxxxxxxxxxxx' \
        --header 'Accept: application/vnd.sbcampaignresource.v4+json'
        --header 'Content-Type: application/vnd.sbcampaignresource.v4+json'
    ```

</details>

<details class="details-bar">
    <summary>Postman</summary>

  1. Make sure you have the Amazon Ads API Postman collection and environment files imported into Postman. If you haven't completed Postman setup, see our [Postman tutorial](guides/postman). 
  2. From Postman, navigate to the **Amazon Ads API** collection. 
  3. Go to the **First call** folder and open the **`POST` List SB campaigns** endpoint.
  3. In the **Headers** tab, ensure your environment variables are populating properly.
  4. Send your request.

</details>

#### Sponsored Display

Full reference: [GET sd/campaigns](sponsored-display/3-0/openapi#tag/Campaigns/operation/listCampaigns)

<details class="details-bar">
    <summary>cURL</summary>

    <p>This example shows a GET Sponsored Display campaigns request using the North America URL prefix.</p>

    ```bash
        curl --location --request GET 'https://advertising-api.amazon.com/sd/campaigns' \
        --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
        --header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
        --header 'Authorization: Bearer xxxxxxxxxxxx' \
    ```

</details>

<details class="details-bar">
    <summary>Postman</summary>

  1. Make sure you have the Amazon Ads API Postman collection and environment files imported into Postman. If you haven't completed Postman setup, see our [Postman tutorial](guides/postman). 
  2. From Postman, navigate to the **Amazon Ads API** collection. 
  3. Go to the **First call** folder and open the **`GET` List SD campaigns** endpoint.
  3. In the **Headers** tab, ensure your environment variables are populating properly.
  4. Send your request.

</details>

## Response

A successful response to any of the GET campaigns endpoints returns a `200` response code. The response body contains a JSON array of campaign objects. 

### Sample

The following sample response contains one Sponsored Display campaign.

```json
[
    {
        "campaignId": 127519806194475,
        "name": "SdTestCampaign-26/01/2022 15:37:31",
        "tactic": "T00020",
        "startDate": "20220126",
        "state": "enabled",
        "costType": "cpc",
        "budget": 100,
        "budgetType": "daily",
        "deliveryProfile": "as_soon_as_possible"
    }
]      
```    

### Receiving an empty response

If you don't have any campaigns of a certain ad type created yet, you receive a `200` response that contains an empty JSON array (`[]`). To check if you have active campaigns, log in to the Amazon [advertising console](https://advertising.amazon.com/sign-in). 

>[TIP] If you are new to Amazon Ads and don't have any campaigns, try creating a [test account](guides/account-management/test-accounts/overview). Once you have a test account, you can practice building campaigns without impacting your spend.

## Next steps

Now that you understand the basic request and response structure for the Amazon Ads API, you can start working on the use case that suits your needs.

### For advertisers with active campaigns

- [Pull sponsored ads performance data](guides/reporting/v3/get-started)
- [Retrieve all sponsored ads campaigns, ad groups, ads, and targets](guides/exports/get-started)
- [Create a Sponsored Products manual campaigns](guides/sponsored-products/get-started/manual-campaigns)

### For advertisers new to Amazon Ads

- [Create a test account](guides/account-management/test-accounts/overview)
- [Get started with Sponsored Products auto campaigns](guides/sponsored-products/get-started/auto-campaigns)
