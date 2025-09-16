---
title: Stores
description: Amazon Ads API version 2 Sponsored Brands Reports reference.
type: guide
interface: api
---

# Stores

Use this interface to request and retrieve store information. This can be used for Sponsored Brands campaign creation, 
to pull the store URL information, and for asset registration for Stores.

| Method | URL | Use case |
| --- | --- | --- |
| GET | `/v2/stores` | List store information for all registered stores under an advertiser. |

## Operations

### listStores

```shell
GET /v2/stores
```

Retrieves a list of stores for a given advertiser.

#### Response

| Status Code | Response object |
| --- | --- |
| `200 - success` | List of [`StoreInfoResponse`](#StoreInfoResponse) |
| `401 - unauthorized` | [`Error`](#Error) |
| `404 - campaign not found` | [`Error`](#Error) |

#### Example Response

```JSON
[
  {
    "code": "SUCCESS",
    "entityId": "ENTITY6SICSOL71XVX",
    "storeName": "Test 1",
    "brandEntityId": "ENTITY6SICSOL71XVX",
    "storePageInfo": [
      {
        "storePageId": "75233FD4-DC27-4D39-A3AE-2DDBEE144AD2",
        "storePageUrl": "https://www.amazon.com/stores/page/75233FD4-DC27-4D39-A3AE-2DDBEE144AD2",
        "storePageName": "Home"
      },
      {
        "storePageId ": "75233FD4-DC27-4D39-A3AE-2DDBEE144AD3",
        "storePageUrl ": " https://www.amazon.com/stores/page/75233FD4-DC27-4D39-A3AE-2DDBEE144AD3",
        "storePageName ": "Subpage title"
      }
    ]
  }
]
```

## Resource representations

### StorePageInfo

```JSON
{
    "title": "StorePageInfo",
    "type": "object",
    "properties": {
        "storePageId": {
            "description": "The ID of the store.",
            "type": "string"
        },
        "storePageUrl": {
            "description": "The store url page. Can be used for SB campaigns as a possible landing page.",
            "type": "string"
        },
        "storePageName": {
            "description": "The page name. Defaults to Home for the main store page.",
            "type": "string"
        }
    }
}
```

### StoreInfoResponse

```JSON
{
    "title": "StoreInfoResponse",
    "type": "object",
    "properties": {
        "code": {
            "description": "The response code.",
            "type": "string"
        },
        "entityId": {
            "description": "The entity ID.",
            "type": "string"
        },
        "storeName": {
            "description": "The name of the store.",
            "type": "string"
        },
        "brandEntityId": {
            "description": "ID used in campaign creation and asset registration.",
            "type": "string"
        },
        "storePageInfo": {
            "description": "The information related to the store.",
            "type": "object",
            "$ref": "#StorePageInfo"
        }
    }
}
```

### Error

See the [error response object](reference/concepts/developer-notes) in Developer notes.