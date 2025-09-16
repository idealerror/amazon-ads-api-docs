---
title: Snapshots
description: Amazon Ads API version 1 snapshots reference.
type: guide
interface: api
---
# Entity snapshots

>[WARNING] Snapshots APIs are deprecated and will be shut off on October 15, 2024. For replacement functionality, see the [exports](guides/exports/overview) API. To learn more, view the [migration guide](reference/migration-guides/snapshots-exports). 

Used to retrieve a record of your campaigns, ad groups, ads, and keywords in bulk. This
interface is used to download bulk account snapshots files
asynchronously. **Note:** Sponsored Brands performance data is not available in snapshots for the API v1.

| Method | URL | Use Case   |
|--------|-----|------------|
| POST   | /{recordType}/snapshot  | Request a file-based snapshot of all entities of the specified type in the account satisfying the filtering criteria |
| GET    | /snapshots/{snapshotId} | Retrieve status, metadata and location of previously requested snapshot  |

## Operations

### requestSnapshot

```shell
POST /{recordType}/snapshot    

{                               
"campaignType":{campaignType},  
"stateFilter":{stateFilter}     
}
```

Request a snapshot report for all entities of a single type

#### Parameters

| Parameter Name | Type   | Specified In | Description |
|----------------|--------|--------------|-------------|
| recordType     | String | URL          | The type of entity for which the snapshot should be generated. This must be one of: `campaigns`, `adGroups`, `keywords`, `negativeKeywords`, `campaignNegativeKeywords`, or `productAds`      |
| campaignType   | String | POST body    | The type of campaign for which snapshot should be generated. Must be: `sponsoredProducts`, `sponsoredBrands` |
| stateFilter    | String | POST body    | Restricts results to entities with state within the specified comma-separated list. Must be one of: `enabled`, `paused`, `archived`. Default behavior is to include enabled and paused. |

#### Response

| Status Code        | Response object  |
|--------------------|------------------|
| 202 - success      | [`SnapshotResponse`](#SnapshotResponse) |
| 401 - unauthorized | [`Error`](#Error)            |

The [`SnapshotResponse`](#SnapshotResponse) will contain snapshot status. Additional metadata
will be populated when the snapshot has completed and the location
header will specify the location of the generated report.

## Resource representations

### SnapshotResponse

```JSON
{
    "title": "SnapshotResponse",
    "type": "object",
    "properties": {   
      "snapshotId": {
           "description": "The ID of the for the snapshot that was requested.",
           "type": "string"
       },
      "recordType": {
           "description": "The record type of the report. It can be campaign, adGroups, productAds, keywords, negativeKeywords or campaignNegativeKeywords.",
           "type": "string"
       },
       "status": {
           "description": "The status of the generation of the snapshot, it can be IN_PROGRESS, SUCCESS or FAILURE.",
           "type": "string"
       },
       "statusDetails": {
           "description": "Description of the status.",
           "type": "string"
       },
       "location": {
           "description": "The URI for the snapshot. It's only available if status is SUCCESS.",
           "type": "string"
       },
       "fileSize": {
           "description": "The size of the snapshot file in bytes. It's only available if status is SUCCESS.",
           "type": "number"
       },
       "expiration": {
           "description": "The epoch time for expiration of the snapshot file. It's only available if status is SUCCESS.",
           "type": "number"
       }
    }
}
```

### Error

See [error object return](reference/concepts/developer-notes) format as described in Developer notes.
