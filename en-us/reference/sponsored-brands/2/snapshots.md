---
title: Snapshots
description: Amazon Ads API version 2 Sponsored Brands snapshots reference.
type: guide
interface: api
---
# Entity snapshots

>[WARNING] Snapshots APIs are deprecated and will be shut off on October 15, 2024. For replacement functionality, see the [exports](guides/exports/overview) API. To learn more, view the [migration guide](reference/migration-guides/snapshots-exports). 

>[WARNING] Currently, the Ads API does not support snapshots for Sponsored Brands video campaigns or campaigns created using the [version 4 endpoints](sponsored-brands/3-0/openapi/prod#/Campaigns). Snapshots include records for [version 3](sponsored-brands/3-0/openapi#/Campaigns), non-video campaigns only. To get entities from all versions, use the [Exports](exports) functionality.

Used to retrieve a record of your campaigns and keywords in bulk. This interface is used to download bulk account snapshots files asynchronously.

| Method | URL  | Use Case |
|--------|------|----------|
| POST   | `/v2/hsa/{recordType}/snapshot`  | Request a file-based snapshot of all entities of the specified type in the account satisfying the filtering criteria. |
| GET    | `/v2/hsa/snapshots/{snapshotId}` | Retrieve status, metadata and location of previously requested snapshot. |

## Operations

### requestSnapshot

```shell
POST /v2/hsa/{recordType}/snapshot    

{                                
  "stateFilter":{stateFilter}     
}
```

Request a snapshot report for all entities of a single record type for Sponsored Products or Sponsored Brands.

#### Parameters

| Parameter Name | Type   | Specified In | Description |
|----------------|--------|--------------|-------------|
| recordType     | String | URL          | The type of entity for which the snapshot should be generated. This must be one of: `campaigns`, or `keywords`.  |
| stateFilter    | String | POST body    | Restricts results to entities with state within the specified comma-separated list. Must be one of: `enabled`, `paused`, `archived`. Default behavior is to include enabled and paused. |

#### Response

| Status Code        | Response object  |
|--------------------|------------------|
| 202 - success      | [`SnapshotResponse`](#SnapshotResponse) |
| 401 - unauthorized | [`Error`](#Error) |

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
           "description": "The ID of the snapshot that was requested.",
           "type": "string"
       },
      "recordType": {
           "description": "The record type of the report.",
           "type": "string",
           "oneOf": ["campaigns","keywords"]
       },
       "status": {
           "description": "The status of the generation of the snapshot.",
           "type": "string",
           "oneOf":["IN_PROGRESS","SUCCESS","FAILURE"]
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

See the [error response object](reference/concepts/developer-notes) in Developer notes.
