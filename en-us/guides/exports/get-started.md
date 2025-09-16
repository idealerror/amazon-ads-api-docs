---
title: Exports tutorial
description: Understand the steps to request exports for Amazon Ads campaigns. 
type: guide
interface: api
tags:
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
    - Campaign management
---

# Get started with exports

This tutorial outlines how to asynchronously request and download exports for Amazon Ads campaigns. 

Exports contain metadata related to your campaigns. You can request exports at different levels (e.g., campaign, ad group, target). Exports contain information about campaigns or ad groups at the time of the request. It's not possible to retrieve historical metadata via export.

Since exports are asynchronous, there are three calls you need to make to generate an export:

1. [Request an export](guides/exports/get-started#requesting-exports)
2. [Wait for export to generate](guides/exports/get-started#checking-export-status)
3. [Download the export](guides/exports/get-started#downloading-exports)

## Headers

Each request described in this tutorial requires three header parameters: 

| Parameter | Description |
|--------------------------|---------------|
| `Amazon-Advertising-API-ClientId` | The client ID associated with your Login with Amazon application. Used for authentication. |
| `Authorization` | Your [access token](guides/account-management/authorization/access-tokens). Used for authentication. |
| `Amazon-Advertising-API-Scope` | The [profile ID](guides/account-management/authorization/profiles) associated with an advertising account in a specific marketplace. |

For an introduction to API authentication, see [Authorization overview](guides/account-management/authorization/overview).

## Requesting exports

>[TIP:Try it in Postman] Follow along with this tutorial using **Exports** folder of the Amazon Ads API Postman collection. For set up instructions, see our [Postman tutorial](guides/postman).

First, define what type of export you need. 

1. Choose a type for your export. Each type has its own path. The request body structure for each type is the same, but note that a different `Accept` / `Content-Type` header must be used depending on the type of export (campaign or ad group).

| Type | Endpoint | Accept / Content-Type header | 
|------------------------|----------------------------|------|------|
| Campaigns |[POST /campaigns/export](exports) | application/vnd.campaignsexport.v1+json |
| Ad groups | [POST /adGroups/export](exports) | application/vnd.adgroupsexport.v1+json |
| Targets | [POST /targets/export](exports) | application/vnd.targetsexport.v1+json |
| Ads | [POST /ads/export](exports) | application/vnd.adsexport.v1+json

2. Add an `adProductFilter`. In the request body, you must specify at least one ad product for the export, for example `"adProductFilter":"SPONSORED_PRODUCTS"`. 

Optionally, you can also add a filter to return only entities of a certain status by setting `stateFilter` to `ENABLED`, `PAUSED`, or `ARCHIVED`. You can request multiple statuses, for example `"stateFilter": "ENABLED,PAUSED"`.  

3. To request an export, call the desired POST endpoint (see step 1) with your assembled request body. 

     <details class="details-bar">
    <summary><strong>Example campaign export request</strong></summary>

    <p>The code snippet below requests a campaigns export for all sponsored ads.</p>

    <p>If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/campaigns/export' \
    --header 'Accept: application/vnd.campaignsexport.v1+json' \
    --header 'Content-Type: application/vnd.campaignsexport.v1+json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
        "adProductFilter": [
            "SPONSORED_BRANDS","SPONSORED_PRODUCTS", "SPONSORED_DISPLAY"
        ]
    }'
    ```
    </details>

    <details class="details-bar">
    <summary><strong>Example ad group export request</strong></summary>

    <p>The code snippet below requests an export of enabled ad groups for Sponsored Products.</p>

    <p>If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/adGroups/export' \
    --header 'Accept: application/vnd.adgroupsexport.v1+json' \
    --header 'Content-Type: application/vnd.adgroupsexport.v1+json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
        "adProductFilter": [
            "SPONSORED_PRODUCTS"
        ],
        "stateFilter": [
            "ENABLED"
        ]
    }'
    ```
    </details>
    <details class="details-bar">
    <summary><strong>Example target export request</strong></summary>

    <p>The code snippet below requests an export of paused targets for Sponsored Brands.</p>

    <p>If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/targets/export' \
    --header 'Accept: application/vnd.targetsexport.v1+json' \
    --header 'Content-Type: application/vnd.targetsexport.v1+json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
        "adProductFilter": [
            "SPONSORED_BRANDS"
        ],
        "stateFilter": [
            "PAUSED"
        ]
    }'
    ```
    </details>

    <details class="details-bar">
    <summary><strong>Example ad export request</strong></summary>

    <p>The code snippet below requests an export of paused ad for Sponsored Brands.</p>

    <p>If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/ads/export' \
    --header 'Accept: application/vnd.adsexport.v1+json' \
    --header 'Content-Type: application/vnd.adsexport.v1+json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
        "adProductFilter": [
            "SPONSORED_BRANDS"
        ],
        "stateFilter": [
            "PAUSED"
        ]
    }'
    ```
    </details>

4. Successful calls result in a `202` response that includes an `exportId` and `status` of `IN_PROGRESS`. Take note of the `exportId` for use in the next step. 

    A successful response body should resemble:

    ```json
    {
        "exportId":"xxxxxxxxxxxxxxxxxxx",
        "status":"IN_PROGRESS",
    }
    ```

## Checking export status

You can check the export generation status by using the `exportId` returned in the previous step to call the GET exports/{exportsId} endpoint.

>[NOTE] You need to use a different Accept / Content-Type header depending on the type of export you are requesting. For campaign exports, use `application/vnd.campaignsexport.v1+json`, and for ad group exports, use `application/vnd.adgroupsexport.v1+json`.

| Endpoint |
|-----|
| [GET /exports/{exportId}](exports)|

 <details class="details-bar">
    <summary><strong>Example request (campaign export)</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique export ID.</p>

    ```shell
    curl --location 'https://advertising-api.amazon.com/exports/xxxxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
    --header 'Content-Type: application/vnd.campaignsexport.v1+json' \
    --header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
    --header 'Accept: application/vnd.campaignsexport.v1+json'
    ```
</details>

 <details class="details-bar">
    <summary><strong>Example request (ad group export)</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique export ID.</p>

    ```shell
    curl --location 'https://advertising-api.amazon.com/exports/xxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
    --header 'Content-Type: application/vnd.adgroupsexport.v1+json' \
    --header 'Authorization: Bearer Atza| xxxxxxxxx' \
    --header 'Accept: application/vnd.adgroupsexport.v1+json'
    ```
</details>

<details class="details-bar">
    <summary><strong>Example request (target export)</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique export ID.</p>

    ```shell
    curl --location 'https://advertising-api.amazon.com/exports/xxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
    --header 'Content-Type: application/vnd.targetsexport.v1+json' \
    --header 'Authorization: Bearer Atza| xxxxxxxxx' \
    --header 'Accept: application/vnd.targetsexport.v1+json'
    ```
</details>

<details class="details-bar">
    <summary><strong>Example request (ad export)</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique export ID.</p>

    ```shell
    curl --location 'https://advertising-api.amazon.com/exports/xxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
    --header 'Content-Type: application/vnd.adsexport.v1+json' \
    --header 'Authorization: Bearer Atza| xxxxxxxxx' \
    --header 'Accept: application/vnd.adsexport.v1+json'
    ```
</details>

Successful calls to the GET status endpoint return a `200` status code.

To check if export generation has completed, check the `status` property in the response body. If the report is still generating, `status` is set to `IN_PROGRESS`.

When your export is ready to download, the `status` returns as `COMPLETED`:

```
{
    "exportId": "xxxxxxxxxxxxxxx",
    "status": "COMPLETED",
    "url": "https://snapshots-prod-us-east-1.s3.us-east-1.amazonaws.com/CAMPAIGN/amzn1.application-oa2-client.xxxxxxxxxxxxxx",
    "fileSize": 794,
    "urlExpiresAt": "2023-09-19T17:53:55.778Z",
    "generatedAt": "2023-09-19T16:53:53.867Z",
    "createdAt": "2023-09-19T16:53:53.405Z"
}
``` 

## Downloading exports

Once your export is ready, you can download the export file directly from the `url` returned in the GET /exports/{exportId} call. 

Each call to GET /exports/{exportId} will generate a new download url that is valid for only one hour.
The `urlExpiresAt` property indicates the expiration time of the download url. 

Calls to GET /exports/{exportId} will generate a new download url for an export up to 24 hours after the successful completion of the export.

The export downloads as JSON compressed in gzip format. 

To view sample JSON, see:

- [Campaigns](reference/common-models/campaigns#json-examples)
- [Ad groups](reference/common-models/ad-groups#json-examples)
- [Targets](reference/common-models/targets#json-examples)
- [Ads](reference/common-models/ads#json-examples)

## Using export metadata

Use exports in your application logic to improve the efficiency of other requests to the Amazon Ads API. For example, to generate reports most efficiently, we recommend excluding campaign metadata from report requests, then joining report results to export metadata using the unique IDs for entities depending on the entity.