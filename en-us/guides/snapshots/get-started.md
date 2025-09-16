---
title: Sponsored ads snapshots tutorial
description: Understand the steps to request snapshots for Sponsored Products, Sponsored Display, and Sponsored Brands campaigns. 
type: guide
interface: api
tags:
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
    - Campaign management
---

# Get started with sponsored ads snapshots

>[WARNING] Snapshots APIs are deprecated and will be shut off on October 15, 2024. For replacement functionality, see the [exports](guides/exports/overview) API. To learn more, view the [migration guide](reference/migration-guides/snapshots-exports). 

This tutorial outlines how to asynchronously request and download snapshots for Sponsored Products, Sponsored Brands, and Sponsored Display campaigns. 

Snapshots contain metadata related to your campaigns. You can request snapshots at different levels (e.g., campaign, ad group, keyword). Snapshots contain information about the requested entity at the time of the request. It's not possible to retrieve historical metadata via snapshot.

Since snapshots are asynchronous, there are three calls you need to make to generate a snapshot:

1. [Request a snapshot](#requesting-snapshots)
2. [Wait for snapshot to generate](#checking-snapshot-status)
3. [Download the snapshot](#downloading-snapshots)

>[TIP:Try it in Postman] Follow along with this tutorial using **Snapshots** folder of the Amazon Ads API Postman collection. For set up instructions, see our [Postman tutorial](guides/postman).

## Headers

Each request described in this tutorial requires four header parameters: 

| Parameter | Description |
|--------------------------|---------------|
| `Amazon-Advertising-API-ClientId` | The client ID associated with your Login with Amazon application. Used for authentication. |
| `Authorization` | Your [access token](guides/account-management/authorization/access-tokens). Used for authentication. |
| `Amazon-Advertising-API-Scope` | The [profile ID](guides/account-management/authorization/profiles) associated with an advertising account in a specific marketplace. |
| `Content-Type` | Set to `application/json`. |

For an introduction to API authentication, see [Authorization overview](guides/account-management/authorization/overview).

## Requesting snapshots

First, define what type of snapshot you need. Each snapshot contains metadata for a specified campaign entity within a chosen ad type. 

1. Choose an ad type for your snapshot. Each type of sponsored ad has a designated endpoint with different specifications. Choose the correct endpoint from the table below:

    | Ad type | Endpoint | Documentation |
    |------------------------|----------------------------|------|
    | Sponsored Products | /v2/sp/{recordType}/snapshot | [Link](sponsored-products/2-0/openapi#tag/Snapshots/operation/requestSnapshot) |
    | Sponsored Brands | 	/v2/hsa/{recordType}/snapshot | [Link](reference/sponsored-brands/2/snapshots) |
    | Sponsored Display | /sd/{recordType}/snapshot | [Link](sponsored-display/3-0/openapi#tag/Snapshots/operation/createSnapshot) |
2. Choose a snapshot type. The snapshot type determines the level of detail returned (for example, at the ad group or campaign level). Each sponsored ad type supports different snapshot types. 
    
    For more information on the types of snapshots available for each sponsored ad type, see [Snapshot types](guides/snapshots/overview#snapshot-types-by-ad-type). 

    Enter the desired snapshot type into the path in place of `recordType`. 

3. (Optional) Add a filter. In the request body, you can add a filter to return only entities of a certain status by setting `stateFilter` to `enabled`, `paused`, or `archived`. You can request multiple statuses, for example `"stateFilter": "enabled,paused"`. If you don't include a filter, the snapshot includes all entities regardless of status. 

     >[NOTE] For **Sponsored Display** snapshots, you can also filter based on `tactic` which segments your snapshot based on the type of targeting (product or audience) used in the campaign. If you don't specify a tactic, the snapshot includes all campaigns.<br/><br/> For more information, see the [Sponsored Display snapshots reference](sponsored-display/3-0/openapi#tag/Snapshots/operation/createSnapshot). 

4. To request a snapshot, call the desired POST endpoint (see step 1) with your assembled request body. Note that the request must include a request body, even if it is empty. 

     <details class="details-bar">
    <summary><strong>Example request</strong></summary>

    <p>The code snippet below requests a Sponsored Products snapshot at the ad group level.</p>

    <p>If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/adGroups/snapshot' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{}'
    ```
    </details>
5. Successful calls result in a `202` response that includes a `snapshotId` and `status` of `IN_PROGRESS`. Take note of the `snapshotId` for use in the next step. 

    A successful response body should resemble:

    ```
    {
        "snapshotId":"amzn1.clicksAPI.v1.t1.xxxxxxxxxxxxx",
        "recordType":"campaign",
        "status":"IN_PROGRESS",
        "statusDetails":"snapshot is being generated."
    }
    ```

## Checking snapshot status

Once you have made a successful POST call, snapshot generation can take up to 15 minutes. 

You can check the snapshot generation status by using the `snapshotId` returned in step 5 of [Requesting snapshots](#requesting-snapshots) to call the relevant GET snapshot endpoint.

| Ad type | Endpoint | Documentation |
|------------------------|----------------------------|------|
| Sponsored Products | /v2/sp/snapshots/(snapshotId) | [Link](sponsored-products/2-0/openapi#tag/Snapshots/operation/getSnapshotStatus) |
| Sponsored Brands | /v2/hsa/snapshots/{snapshotId} | [Link](reference/sponsored-brands/2/snapshots) |
| Sponsored Display | /sd/snapshots/(snapshotId) | [Link](sponsored-display/3-0/openapi#tag/Snapshots/operation/getSnapshot) |

 <details class="details-bar">
    <summary><strong>Example request</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique snapshot ID.</p>

    ```shell
    curl --request GET 'https://advertising-api.amazon.com/v2/sp/snapshots/amzn1.clicksAPI.xxxxxx' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer Atza|xxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    ```
</details>

Successful calls to the GET status endpoint for Sponsored Brands and Sponsored Products return a `200` status code; all successful calls to the GET status endpoint for Sponsored Display return a `202` status code. 

To check if snapshot generation has completed, check the `status` parameter in the response body. If the report is still generating, `status` is set to `IN_PROGRESS`:

```
{
    "snapshotId":"amzn1.clicksAPI.xxxxxx",
    "recordType":"ad Groups",
    "status":"IN_PROGRESS",
    "statusDetails":"In the process of generating Snapshot."
}
```

When your snapshot is ready to download, the `status` returns as `SUCCESS`:

```
{
    "snapshotId":"amzn1.clicksAPI.xxxxxx",
    "recordType":"ad Groups",
    "status":"SUCCESS",
    "statusDetails":"Snapshot has been successfully generated.",
    "location":"https://advertising-api.amazon.com/sd/snapshots/amzn1.amzn1.clicksAPI.xxxxxx/download","fileSize":371,
    "expiration":1643734785541
}
```

>[TIP] Snapshot generation can take as long as 15 minutes. Repeated calls to check snapshot status may generate a 429 response, indicating that your requests have been throttled. To retrieve snapshots programmatically, your application logic should institute a delay between requests. For more information, see [Retry logic with exponential backoff](reference/concepts/rate-limiting#use-retry-logic-with-exponential-backoff).  

## Downloading snapshots

Once your snapshot is ready, use the `snapshotId` to call the relevant download endpoint:

| Ad type | Endpoint | Documentation |
|------------------------|----------------------------|------|
| Sponsored Products | /v2/sp/snapshots/{snapshotId}/download | [Link](sponsored-products/2-0/openapi#tag/Snapshots/operation/downloadSnapshot) |
| Sponsored Brands | /v2/hsa/snapshots/{snapshotId}/download | [Link](reference/sponsored-brands/2/snapshots) |
| Sponsored Display | /sd/snapshots/{snapshotId}/download | [Link](sponsored-display/3-0/openapi#tag/Snapshots/operation/downloadSnapshot) |

A successful call returns a `307` redirect response. The redirect points you to the S3 bucket where you can download your snapshot file. The snapshot file downloads as JSON compressed in gzip format. 

 <details class="details-bar">
    <summary><strong>Example request</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique snapshot ID.</p>

    ```shell
    curl --request GET 'https://advertising-api.amazon.com/v2/sp/snapshots/amzn1.clicksAPI.xxxxxx/download' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer Atza|xxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    --output "mySnapshot.json"
    ```
</details>

## Using snapshot metadata

Use snapshots in your application logic to improve the efficiency of other requests to the Amazon Ads API. For example, to generate reports most efficiently, we recommend excluding campaign metadata from the report request, then joining report results to snapshot metadata using the unique IDs for entities at the `reportType` level specified.

For more information on the fields returned by each type of snapshot, see [Fields by ad and snapshot type](guides/snapshots/overview#snapshot-types-by-ad-type).