---
title: Getting started with the Amazon DSP deals API
description: A getting started guide for using the Amazon DSP deals API
type: guide
interface: api
tags: 
    - Amazon DSP
    - Campaign management
keywords:
    - guide
    - deals
    - internalDealId
    - externalDealId
    - line item
    - receommended advertisers
    - inferred attributes
---

# Deals API 

The Amazon Ads deals API allows you to manage third party (3P) advertising deals that you have negotiated and executed in Amazon DSP (ADSP) so you can use those deals to place ads through Amazon DSP platform. The ADSP deals API currently does not provide functionality to negotiate or execute a deal. The ADSP platform uses the `exchangeID` and the `externalDealID` to identify the deal during ad placement. The deals API also generates an internal `dealId` that must be used to manage the deals within ADSP.

## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your client ID and access token. You will need these to make all the calls referenced in this tutorial.

>[TIP:Header values]Some standard headers are included in all example requests below. To test your own requests, be sure to replace `xxxxxxxxxxxxxxxx` with your own values for the following:<ul><li>**Amazon-Advertising-API-ClientId**: The client ID you retrieved in the onboarding process.</li><li>**Authorization**: The string "Bearer" prepended to an access token retrieved as described in the getting started process.</li><li>**Amazon-Advertising-API-Scope**: A profile ID retrieved as described below.</li></ul>Be sure to also note the values for **Content-Type** in the example requests.

## Creating and using 3P deal objects

There are three high-level steps in creating and using a deals object in ADSP using the Amazon Ads API:

1. Retrieve the profile ID of the profile you want to create the deal object for. A profile ID in the Amazon Ads API corresponds to accounts in the Amazon Ads console.
2. Create and manage a deal object.
3. Associate the deal object with an ad group.

## Retrieve a profile ID

Profiles in the Amazon Ads API represent an advertiser's account in a specific marketplace. 
 
A profile ID is required for requests to the deals API. The list of profile IDs can be retrieved using [GET /v2/profiles](reference/2/profiles). Once you identify the profile you want to use, you will provide the `profileId` value as the `Amazon-Advertising-API-Scope` header in subsequent calls to the deals API.

The following call will retrieve a list of all accounts in the region associated with the user account that provided the access token:

```
curl --location --request GET 'https://advertising-api.amazon.com/v2/profiles'\
--header 'Amazon-Advertising-API-ClientId:xxxxxxxxxx'\ 
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' 
```

>[TIP:Learn more about profiles in the Amazon Ads API]<ul><li>[Profiles overview](guides/account-management/authorization/profiles)</li><li>[Authorization overview](guides/account-management/authorization/overview)</li></ul>

## Create and manage 3P deals in ADSP

### Discover the exchanges that Amazon DSP supports for deals

ADSP supports a list of exchanges for deals. Before creating an object, you can use [POST /dsp/inventory/deals/exchanges/list](dsp-deals-3p#tag/Deal-Exchanges/operation/listDealsExchangesDspDeals) to get the list of all supported exchanges. Exchanges in the response include an exchange ID, which will be required to create a deal object.

>[NOTE]You can create a deal object for most of the supported exchanges. Amazon media, APS, AdX, Magnite DV+ and Xandr deals are automatically ingested, and therefore customers cannot create deals on these exchanges.

The following call will return all supported exchanges:

```
curl --location --request POST 'https://advertising-api.amazon.com/dsp/inventory/deals/exchanges/list' \
--header 'Amazon-Advertising-API-ClientId:xxxxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxx' \
--data '{"pageSize": 50}'
```

#### Sample response

```json
{
  "exchanges": [
      {
          "name": "Exchange 1",
          "id": "1000"
      },
      {
          "name": "Exchange 2",
          "id": "1001"
      },
      {
          "name": "Exchange 3",
          "id": "1002"
      },
      {
          "name": "Exchange 4",
          "id": "1003"
      }
  ]
}
```

### Create a new 3P deal

The `exchangeId`, `externalDealId`, and terms of the deal must be provided to create a deal object. The terms of the deal are used during the ad bidding process. A 3P deal can be created in ADSP using [POST /dsp/inventory/deals](dsp-create-update-deals#tag/Deal/operation/CreateDeal). Once created, the deal can used to to target negotiated supply on a line item. 

The following API call will create a deal with the attributes specified in the data section:

```
curl --location --request POST 'https://advertising-api.amazon.com/dsp/inventory/deals' \
--header 'Amazon-Advertising-API-ClientId:xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
--data '{ 
    "dealName": "TEST_DEAL_3", 
    "exchangeId": "1994", 
    "priceMillis": 12400, 
    "startDateTime": "2025-08-24T14:15:22Z", 
    "externalDealId": "Deal_ID_3", 
    "priceType": "FIXED_CPM", 
    "publisher": "Test_Pub_3", 
    "currency": "USD", 
    "dealType": "PREFERRED", 
    "endDateTime": "2025-08-25T14:15:22Z" 
}'
```

#### Key attributes

- **priceMillis**: Floor price for private auction deal and Fixed CPM for preferred deal
- **priceType**: `FLOOR_RATE` for private auction deals and "FIXED_CPM" for preferred deals
- **dealType**: `PRIVATE_AUCTION` or  `PREFERRED` or `PROGRAMMATIC_GUARANTEED`

#### Notes

* Users cannot create 3P deals on Supply Side Platforms (SSPs) integrated for automated ingestion of deals: Amazon media, Amazon Publisher Services, Google Ads Manager (AdX), Magnite DV+ (Rubicon Project), Xandr (Appnexus). Deals will be automatically ingested into the customer’s ADSP account.
* `externalDealId` is the deal ID provided by the publisher or 3P SSP. Amazon DSP generates an internal `dealId` returned in the success message that must be used to refer to the deal object moving forward (e.g., for updating the created 3P deal).
* Amazon Audience Guaranteed deal types cannot be created via API and are on a path to deprecation stretching to 2025. These deals can however be discovered using the list deals operation described below.

#### Sample response

```json
{
    "dealId": "583268183267378467"
}
```

### List existing deals

You can retrieve a list of deals for the specified `profileId` in an ADSP account using [POST /dsp/inventory/deals/list](dsp-deals-3p#tag/Deal/operation/listDealsDspDeals). You can use the filters described in the specification to narrow down the list. 

The following API call will retrieve all the deals in scope:

```
curl --location --request POST 'https://advertising-api.amazon.com/dsp/inventory/deals/list' \
--header 'Amazon-Advertising-API-ClientId:xxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--data '{"pageSize": 50}'
```

#### Sample response

```json
{
  "totalResults": 4046,
  "nextToken": "d7b050ee-ddda-466c-bab0-024fb5540726g2g1",
  "deals": [
      {
          "dealName": "APS_Web_Catalog_PA_Floor_CNET-Group-GAMING_Desktop-MWeb_320x50-300x250-300x600-728x90_9.00",
          "internalDealId": "578263296727835419",
          "externalDealId": "aps048aadbf",
          "dealType": "PRIVATE_AUCTION",
          "status": "EXPIRED",
          "createdDateTime": "2024-03-14T04:51:54.361Z",
          "startDateTime": "2020-11-19T08:00:00Z",
          "endDateTime": "2023-01-01T07:59:59Z",
          "currency": "USD",
          "priceMillis": 900000,
          "priceType": "FLOOR_RATE",
          "budgetMillis": null,
          "marketplaceDeal": false,
          "exchangeId": "100",
          "publisher": "CBSI MATCH BUY",
          "productId": null,
          "entityId": "ENTITYA6I16E0BHHHY"
      },
      {
          "dealName": "APD Select_Streaming TV_SI_Men's Grooming & Personal Care",
          "internalDealId": "579223588920946208",
          "externalDealId": "aps3e239e0e",
          "dealType": "PRIVATE_AUCTION",
          "status": "ACTIVE",
          "createdDateTime": "2024-03-14T04:52:40.625Z",
          "startDateTime": "2023-07-19T00:00:00Z",
          "endDateTime": "2024-12-31T00:00:00Z",
          "currency": "USD",
          "priceMillis": 2000000,
          "priceType": "FLOOR_RATE",
          "budgetMillis": null,
          "marketplaceDeal": false,
          "exchangeId": "100",
          "publisher": "APD Select - Multi-Pub",
          "productId": null,
          "entityId": "ENTITYA6I16E0BHHHY"
      }
  ],
  "listData": [
      {
          "dealName": "APS_Web_Catalog_PA_Floor_CNET-Group-GAMING_Desktop-MWeb_320x50-300x250-300x600-728x90_9.00",
          "internalDealId": "578263296727835419",
          "externalDealId": "aps048aadbf",
          "dealType": "PRIVATE_AUCTION",
          "status": "EXPIRED",
          "createdDateTime": "2024-03-14T04:51:54.361Z",
          "startDateTime": "2020-11-19T08:00:00Z",
          "endDateTime": "2023-01-01T07:59:59Z",
          "currency": "USD",
          "priceMillis": 900000,
          "priceType": "FLOOR_RATE",
          "budgetMillis": null,
          "marketplaceDeal": false,
          "exchangeId": "100",
          "publisher": "CBSI MATCH BUY",
          "productId": null,
          "entityId": "ENTITYA6I16E0BHHHY"
      },
      {
          "dealName": "APD Select_Streaming TV_SI_Men's Grooming & Personal Care",
          "internalDealId": "579223588920946208",
          "externalDealId": "aps3e239e0e",
          "dealType": "PRIVATE_AUCTION",
          "status": "ACTIVE",
          "createdDateTime": "2024-03-14T04:52:40.625Z",
          "startDateTime": "2023-07-19T00:00:00Z",
          "endDateTime": "2024-12-31T00:00:00Z",
          "currency": "USD",
          "priceMillis": 2000000,
          "priceType": "FLOOR_RATE",
          "budgetMillis": null,
          "marketplaceDeal": false,
          "exchangeId": "100",
          "publisher": "APD Select - Multi-Pub",
          "productId": null,
          "entityId": "ENTITYA6I16E0BHHHY"
      }
  ]
} 
```

### Update deals

After a deal is created, it can be updated using [PUT /dsp/inventory/deals/{dealId}](dsp-create-update-deals#tag/Deal/operation/UpdateDeal). 

The following API call will update the `priceMillis` attribute of the deal created in the create new 3P deal section with 22400 

```
curl --location --request PUT 'https://advertising-api.amazon.com/dsp/inventory/deals/583268183267378467' \
--header 'Amazon-Advertising-API-ClientId:xxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:1213913282869999' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{ 
    "dealName": "TEST_DEAL_3", 
    "exchangeId": "1994", 
    "priceMillis": 22400, 
    "startDateTime": "2025-08-24T14:15:22Z", 
    "externalDealId": "Deal_ID_3", 
    "priceType": "FIXED_CPM", 
    "publisher": "Test_Pub_4", 
    "currency": "USD", 
    "dealType": "PREFERRED", 
    "endDateTime": "2025-08-25T14:15:22Z" 
    }'
```

#### Notes

* `dealType` cannot change from `PROGRAMMATIC_GUARANTEED` to anything else. But `dealType` can change between `PRIVATE_AUCTION` and `PREFERRED`.
* `externalDealId` and `exchangeId` cannot be updated after a deal has been created.
* `startDateTime` cannot be updated once it has become current or past.

No body is returned for the response.

## Assign recommended advertisers

You can use [PUT /dsp/inventory/deals/{dealId}/advertiserPermissions/recommendedAdvertisers](dsp-deals-permissions#tag/Deal-Advertiser-Permissions/operation/UpdateRecommendedAdvertiserListDspDeals) to update recommended advertiser permissions by adding advertisers to or removing advertisers from the advertiser permission list for a given deal.

```
curl --location --request PUT 'https://advertising-api.amazon.com/dsp/inventory/deals/xxxxxxxxxxx/advertiserPermissions/recommendedAdvertisers' \
--header 'Amazon-Advertising-API-ClientId:amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json'\ 
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--data '{ "advertiserIds": [ "xxxxxxxxxxxxx" ], "operationType": "ADD" }'
```

#### Sample response

```json
{
    "success": [
        {
            "advertiserId": "587090316937703893",
            "index": 0
        }
    ]
}
```

#### Other operations for recommended advertisers

* [POST /dsp/inventory/deals/{dealId}/advertiserPermissions/recommendedAdvertiser/list](dsp-deals-permissions#tag/Deal-Advertiser-Permissions/operation/ListRecommendedAdvertiserDspDeals) provides the list of `advertiserIds` currently set as recommended for a given deal.
* [DELETE /dsp/inventory/deals/{dealId}/advertiserPermissions/recommendedAdvertisers](dsp-deals-permissions#tag/Deal-Advertiser-Permissions/operation/DeleteRecommendedAdvertiserListDspDeals) deletes the entire list of `advertiserIds` currently set as recommended on a given deal.

## Associate the deal object with an ad line item

When a deal object is created in ADSP, it is not used until it is associated with the an advertisement line item. Please see [ADSP developer guide](guides/dsp/developer-guide) for additional details on ADSP advertisement object hierarchy. Associating a deal with a line item involves two steps:

1. Identify a deal to associate.
2. Associate the deal with the line item.

### Identify a deal to associate

You can call [POST /dsp/inventorySourceDeals/list](dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals) to retrieve a list of deals matching the submitted filters and sorted per the chosen criteria. The API response also includes metadata associated to each deal targeted by an ad group. 

```
curl --location --request POST 'https://advertising-api.amazon.com/dsp/inventorySourceDeals/list' \
--header 'Amazon-Advertising-API-ClientId:xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId:xxxxxxxxxxxxxx' \
--header 'Content-Type:application/vnd.dspinventorysourcedeals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{ 
    "filters": 
    [ { "include": true, "field": "DEAL_TYPE", "values": [ "PREFERRED" ] } ], 
    "sortOrder": "ASCENDING", 
    "sortField": "DEAL_ID", 
    "maxResults": 1000 
    }'
```

#### Sample response

```json
{
  "deals": [
      {
          "id": "576473550482359889",
          "name": "BR_BR_Amazon_Reality_None_Indirect Sales_Scatter_PD",
          "inventoryType": "VIDEO",
          "dealPrice": {
              "amount": 600000,
              "currency": "USD",
              "priceType": "FIXED_CPM"
          },
          "dealBudget": null,
          "dealStatus": "ACTIVE",
          "marketplaceDeal": true,
          "recommendedDeal": false,
          "externalDealId": "VIA-BRB-00021",
          "dealType": "PREFERRED",
          "publisherName": "PlutoTV",
          "startDateTime": "2023-12-14T00:00:00.000+00:00",
          "endDateTime": "2025-12-14T00:00:00.000+00:00",
          "exchangeId": "1973",
          "targetedPgDeal": null
      },
      {
          "id": "576496760933194752",
          "name": "CURIOSITY_MEDIA_MATCH_BUY_1614374061665",
          "inventoryType": "UNKNOWN",
          "dealPrice": {
              "amount": 1000,
              "currency": "USD",
              "priceType": "FIXED_CPM"
          },
          "dealBudget": null,
          "dealStatus": "EXPIRED",
          "marketplaceDeal": false,
          "recommendedDeal": false,
          "externalDealId": "aps98551281",
          "dealType": "PREFERRED",
          "publisherName": "CURIOSITY MEDIA MATCH BUY",
          "startDateTime": "2021-02-26T08:00:00.000+00:00",
          "endDateTime": "2022-01-03T07:59:59.000+00:00",
          "exchangeId": "100",
          "targetedPgDeal": null
      }
  ],
  "totalResults": 2,
  "nextToken": null
}
```

### Associate the deal with the line item

You can use [PATCH /dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspListAdGroupV1) to update a line item with the programmatically guaranteed deal you identified in the previous step. You can also create a new line item with [POST /dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspListAdGroupV1).

```
curl --location --request PATCH 'https://advertising-api.amazon.com/dsp/v1/adGroups' \
--header 'Amazon-Advertising-API-ClientId:xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
--data '{
  "adGroups": [
   {
      "adGroupId": "xxxxxxxxxx",
      "targetingSettings": {
        "targetedPGDealId": "xxxxxxx"
      }
    }
  ]
}'
```

#### Sample response

```json
{
    "success": [
        {
            "index": 0,
            "adGroupId": "578234571236057644"
        }
    ],
    "requestId": "a0034dd4-dca7-4ada-b359-a9dccd14609c"
}
```

For preferred and private auction deals, use [/dsp/v1/targets](dsp-universal-targeting#tag/Targets/operation/CreateDspTargetsV1) to associate with campaigns.

### Get list of line items targeting a given deal

You can use [GET /dsp/inventory/deals/{dealId}/lineItems](dsp-deals-3p#tag/Deal/operation/getDealLineItemsDspDeals) to retrieve all line items targeting a given deal. In order to receive bid responses, deals must be actively targeted by a line item as a supply source.

```
curl --location --request GET 'https://advertising-api.amazon.com/dsp/inventory/deals/{dealId}/lineItems' \
--header 'Amazon-Advertising-API-ClientId:amzn1.application-oa2-client.xxxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxxxx'
```

#### Sample response

```json
{
    "lineItemIds": [
        "578234571236057644",
        "593886113627774584",
        "577668312061559702",
        "592249976318508132"
    ]
}
```

### Retrieve performance data on a deal

The [POST /dsp/inventory/deals/performance/list](dsp-deals-3p#tag/Deal-Performance/operation/listDealsPerformanceDspDeals) endpoint enables you to retrieve campaign performance and supply-side metrics for selected deals, either submitted as a query input, or matching the submitted filters.

```
curl --location --request POST 'https://advertising-api.amazon.com/dsp/inventory/deals/performance/list' \
--header 'Amazon-Advertising-API-ClientId:amzn1.application-oa2-client.xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxx' \
--data '{"performanceStartDate": "2022-02-22T22:22:22Z", "dealIdFilter": [ "xxxxxxx" ], "performanceEndDate": "2022-02-22T22:22:22Z", "pageSize": 50}'
```

#### Sample response

```json
{
    "dealPerformances": [],
    "metricsLastUpdated": null,
    "totalResults": 0,
    "nextToken": null,
    "listData": []
}
```

### Retrieve inferred attributes on a deal

You can use [GET /dsp/inventory/deals/{dealId}/inferredAttributes](dsp-deals-3p#tag/Deal-Inferred-Attributes/operation/dealInferredAttributes) to retrieve inferred directional attributes for a specific deal. Attributes are deemed inferred when they are the result of observing the bid requests for each deal. Inferred attributes differ from declarative attributes, which are set once by publishers and customers when registering a deal. Because inferred attributes are the result of observation, they may change over time.

```
curl --location --request GET 'https://advertising-api.amazon.com/dsp/inventory/deals/xxxxxxxx/inferredAttributes' \
--header 'Amazon-Advertising-API-ClientId:amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope:xxxxxxxxx' \
--header 'Content-Type:application/vnd.deals.v1+json' \
--header 'Authorization: Bearer Atza|xxxxxxxx'
```

#### Sample response

```json
{
    "mediaTypes": [],
    "devices": [],
    "locations": [],
    "environments": []
}
```
