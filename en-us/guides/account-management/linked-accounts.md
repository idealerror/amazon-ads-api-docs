---
title: Linked accounts
description: Overview of the linked accounts API
type: guide
interface: api
tags:
    - Authorization
    - Account management
keywords:
    - access
    - headers
    - Amazon-Advertising-API-ClientId
    - Amazon-Advertising-API-Scope
    - Manager accounts
---

# Linked accounts API (closed beta)

The Linked Accounts API provides paginated access to relationship data between advertising accounts. Users with Manager Account access can retrieve information about all accounts directly linked to their Manager Account, while advertiser account users can discover which Manager Accounts are associated with their account. This API helps users gain a clearer view of account structure and access relationships within their advertising environment.

[View the technical specification for the linked accounts API.](amazon-ads/1-0/permissions)

>[WARNING]The linked accounts API is currently in closed beta. To request to join this beta program, reach out to the API support team as described on the [Support page](support/overview).

## Account linking model

Both Sponsored Ads and Amazon DSP (ADSP) accounts follow a one-to-many linking model, where a single Manager Account can be associated with multiple advertiser accounts. In both cases, the links are directional from the Manager Account to the advertiser account and support visibility and access management across accounts.

The key difference lies in how these links are structured:

* For Sponsored Ads, each link is defined by the Manager Account, the advertiser account, the type of access being granted, and the countries where that access is applicable. To ensure consistency and prevent redundancy, the system disallows multiple links between the same pair of accounts for the same country.
* For ADSP accounts, the link is determined by the advertiser’s identity and the region in which it operates. While ADSP accounts currently rely on a legacy identifier for account-level operations, this identifier will soon be replaced by a standardized format consistent with Sponsored Ads accounts. This change will not affect existing links, and the legacy identifiers will continue to appear in API responses for backward compatibility.

Manager Accounts can also be linked to multiple other Manager Accounts. This one-to-many model allows hierarchical account structures, where a Manager Account may act as both a parent and a child in different relationships. As with advertiser accounts, the links between Manager Accounts are directional and subject to authorization controls.

The linked accounts API returns only direct children of the account specified in the request. To view the accounts linked to a child manager account, send an additional request using that account's ID as `Amazon-Ads-Manager-AccountId`.

## Listing linked accounts

The `POST /query/accountLinks` endpoint supports two operations: listing **child accounts** linked to a Manager Account, and listing **parent Manager Accounts** associated with a given account. Child account queries can only be performed using a Manager Account as input, while parent account queries are supported for any account type, including Sponsored Ads accounts, ADSP advertiser accounts, and Manager Accounts. In both cases, the user must be authorized to access the specified account.

### Required headers

Please follow the [getting started guide for the Amazon Ads API](guides/get-started/overview) to provide correct credentials for the following required headers:

- `Amazon-Advertising-API-ClientId`
- `Authorization`

In addition, the Linked Accounts API requires one of two account identification headers, depending on the type of query:

- For **child account queries**, include the `Amazon-Ads-Manager-AccountId` header. This should be set to the Manager Account ID for which you want to retrieve linked child accounts.
- For **parent account queries**:
  - To retrieve the parents of an Advertising Account, include the `Amazon-Ads-AccountId` header with either a Global Advertising Account ID or an ADSP Advertiser ID.
  - To retrieve the parents of a Manager Account, include the `Amazon-Ads-Manager-AccountId` header.

### Pagination

The Linked Accounts API supports paginated responses. If a response includes a `nextToken`, you can retrieve the next page of results by including that token in the request body of a subsequent call to the same endpoint.

### Request parameters

The request body for `POST /query/accountLinks` supports the following parameters:

- `relationshipTypeFilter.include`

    This required parameter specifies the direction of the account relationship to query. It accepts one of the following values:

    * `"CHILD"` – returns all accounts linked *under* the specified Manager Account (i.e., child accounts).
    * `"PARENT"` – returns all Manager Accounts linked *above* the specified account (i.e., parent accounts).

    Only one direction should be included per request.

- `maxResults`

    This optional parameter sets the maximum number of results to return in a single response page. The default value is system-defined. If the number of matching links exceeds the specified limit, the response includes a `nextToken`, which can be used to retrieve additional pages.

**Sample request body**

```json
{
    "relationshipTypeFilter": {
        "include": [
            "CHILD"
        ]
    },
    "maxResults": 10
}
```

### Response fields

The response from `POST /query/accountLinks` contains an `accountLinks` array. Each element in this array represents a link between two accounts and includes metadata about the linked account and the nature of the relationship.

| Field | Description |
| ----- | ----- |
| `accountId` | A unique identifier for the linked account. For advertiser accounts, this may be a legacy numeric ID or a standardized Amazon Ads account ID in the format `amzn1.ads-account.g.*`. |
| `accountName` | The display name of the linked account, as defined by the account owner. |
| `accountType` | The type of the linked account. Supported values include: <ul><li>`ADVERTISER_ACCOUNT` – for Sponsored Ads or ADSP advertiser accounts</li><li>`MANAGER_ACCOUNT` – for Manager Accounts</li></ul> |
| `adProductGroups` | A list of advertising product groups associated with the linked account. This may include: <ul><li>`"SPONSORED_ADS"` – for Sponsored Products, Sponsored Brands, or Sponsored Display</li><li>`"ADSP"` – for Amazon DSP</li></ul> Manager Accounts may return an empty list if they are not explicitly associated with a specific ad product. |
| `alternateIds` | A list of legacy or region-specific identifiers associated with the account. For ADSP accounts, this includes: <ul><li>`dspAdvertiserId` – the original ADSP ID used before account unification</li><li>`region` – the ADSP region code where the account operates (e.g., `"NA"`)</li></ul>Sponsored Ads and Manager Accounts may return an empty list. |
| `countryCode` | A list of ISO 3166-1 alpha-2 country codes representing the countries where the linked relationship applies. A single link may span multiple countries. |
| `permissionSet` | An object describing the access granted to the Manager Account over the linked account. This may include: <ul><li>`role` – a high-level access role such as `ADMIN` or `EDITOR`</li><li>`customPermissionSet` – a list of specific permission names (e.g., `DATA_SHARING`, `REPORTS_LIMITED`) that define fine-grained access controls</li></ul>Accounts may include both a role and custom permissions, depending on the configuration. |
| `relationshipType` | Indicates the direction of the link relative to the queried account. Possible values are:<ul><li>`CHILD` – the returned account is a child of the queried Manager Account</li><li>`PARENT` – the returned account is a parent Manager Account of the queried account</li></ul> |

### Usage examples

#### Listing child accounts of manager account 

Request header:

`Amazon-Ads-Manager-AccountId` = `amzn1.ads1.ma1.5sqxxxx`

Request body:

```json
{
    "relationshipTypeFilter": {
        "include": [
            "CHILD"
        ]
    },
    "maxResults": 10
}
```

Response:

```json
{
  "accountLinks": [
    {
      "accountId": "593471182699685596",
      "accountName": "johnblod_TEST_1711555896143",
      "accountType": "ADVERTISER_ACCOUNT",
      "adProductGroups": [
        "ADSP"
      ],
      "alternateIds": [
        {
          "dspAdvertiserId": "593471182699685596",
          "region": "NA"
        }
      ],
      "countryCode": [
        "US"
      ],
      "permissionSet": {
        "customPermissionSet": [
          {
            "permissionName": "DATA_SHARING"
          },
          {
            "permissionName": "REPORTS_LIMITED"
          }
        ]
      },
      "relationshipType": "CHILD"
    },
    {
      "accountId": "amzn1.ads-account.g.f31d5c3iosl1pmbwa8sq022f7",
      "accountName": "aindani_test_SA",
      "accountType": "ADVERTISER_ACCOUNT",
      "adProductGroups": [
        "SPONSORED_ADS"
      ],
      "alternateIds": [],
      "countryCode": [
        "US"
      ],
      "permissionSet": {
        "role": {
          "roleName": "EDITOR"
        }
      },
      "relationshipType": "CHILD"
    }
  ]
}
```

#### Listing parent accounts of global account

Request header:

`Amazon-Ads-AccountId` = `amzn1.ads-account.g.1xxxx`

Request body:

```json
{
    "relationshipTypeFilter": {
        "include": [
            "PARENT"
        ]
    },
    "maxResults": 10
}
```

Response:

```json
{
  "accountLinks": [
    {
      "accountId": "amzn1.ads1.ma1.c596p0x40tpz4o1a0bdj9vprt",
      "accountName": "TestManager-27/03/2024 16:10:57",
      "accountType": "MANAGER_ACCOUNT",
      "adProductGroups": [],
      "alternateIds": [],
      "countryCode": [
        "US"
      ],
      "permissionSet": {
        "role": {
          "roleName": "ADMIN"
        }
      },
      "relationshipType": "PARENT"
    },
    {
      "accountId": "amzn1.ads1.ma1.bm3k0yx1po2gxkee2cxn8ti8o",
      "accountName": "aindani_test_ma_parent",
      "accountType": "MANAGER_ACCOUNT",
      "adProductGroups": [],
      "alternateIds": [],
      "countryCode": [
        "US"
      ],
      "permissionSet": {
        "customPermissionSet": [
          {
            "permissionName": "DATA_SHARING"
          },
          {
            "permissionName": "REPORTS_LIMITED"
          }
        ]
      },
      "relationshipType": "PARENT"
    }
  ]
}
```

#### Listing parent accounts of ADSP advertiserId

Request header:

`Amazon-Ads-AccountId` = `5817981xxxx` (dspAdvertiserId)

Request body:

```json
{
    "relationshipTypeFilter": {
        "include": [
            "PARENT"
        ]
    },
    "maxResults": 10
}
```

Response:

```json
{
  "accountLinks": [
    {
      "accountId": "amzn1.ads1.ma1.c596p0x40tpz4o1a0bdj9vprt",
      "accountName": "TestManager-27/03/2024 16:10:57",
      "accountType": "MANAGER_ACCOUNT",
      "adProductGroups": [],
      "alternateIds": [],
      "countryCode": [
        "US"
      ],
      "permissionSet": {
        "role": {
          "roleName": "ADMIN"
        }
      },
      "relationshipType": "PARENT"
    },
    {
      "accountId": "amzn1.ads1.ma1.bm3k0yx1po2gxkee2cxn8ti8o",
      "accountName": "aindani_test_ma_parent",
      "accountType": "MANAGER_ACCOUNT",
      "adProductGroups": [],
      "alternateIds": [],
      "countryCode": [
        "US"
      ],
      "permissionSet": {
        "customPermissionSet": [
          {
            "permissionName": "DATA_SHARING"
          },
          {
            "permissionName": "REPORTS_LIMITED"
          }
        ]
      },
      "relationshipType": "PARENT"
    }
  ]
}
```
