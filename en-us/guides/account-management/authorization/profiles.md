---
title: Profiles
description: Overview of profiles in the Amazon Ads API, with information about the GET /profiles endpoint and how a profile identifier is used to authorize requests to the API.
type: guide
interface: api
tags:
    - Authorization
    - Account management
keywords:
    - access
    - headers
    - profileId
    - regions
    - Amazon-Advertising-API-Scope
    - unauthorized
    - agency
---

# Profiles

Profiles in the Amazon Ads API represent an advertiser's account in a specific marketplace. Advertisers may have a single profile if they advertise in only one marketplace, or a separate profile for *each* marketplace if they advertise regionally or globally.

>[TOPIC:Profiles, accounts, and identity]Profiles in the Amazon Ads API correspond to _accounts_ in the Amazon Ads console. A separate _account_ is established for each marketplace. A single _user_ (the advertiser) may manage multiple accounts in the console or through the API via the same Amazon login credentials.

Profiles play a crucial role in the Amazon Ads API by determining the management scope for a given call. A profile ID is a required credential to access an advertiser's data and services in a specific marketplace.

## Authorization

Calls to the Amazon Ads API require authorization based on OAuth 2.0. The following must be included in the request headers for any successful call:

* The **client ID** of the application authorized to access the API
* The **access token** representing the application's permission to access data and services for a given advertiser

In addition, nearly all resources available through the Amazon Ads API require passing a specific **profile identifier** in the request header. This value is passed as the `Amazon-Advertising-API-Scope` parameter. 

Even for advertisers with only one profile, this header is required. If the `Amazon-Advertising-API-Scope` header is not included or is incorrect, a `401 Unauthorized` or `400 Bad Request` response is returned. 

- For a general summary of authorization in the Amazon Ads API, see the [Authorization Overview](guides/account-management/authorization/overview).

## Retrieving profiles

A list of profiles for a given advertiser can be retrieved via a `GET` request at the `/v2/profiles` endpoint. This call does *not* require the `Amazon-Advertising-API-Scope` header. 

The retrieved list includes profiles associated with the advertiser whose permission is represented by the access token. Each profile in the retrieved list represents the advertiser's account in a specific marketplace.

[Manager accounts](guides/account-management/authorization/manager-accounts) can be linked to multiple advertiser accounts and can have multiple profiles. For manager accounts, only the advertiser accounts in the API host's region are returned. See [Regions](#regions).

The `profileId` of a given profile is passed in subsequent calls to the API to provide access to the advertiser's resources in that profile's marketplace.

- For more information, see the [technical specifications for the Profiles resource](reference/2/profiles).

>[NOTE]The response from `GET /v2/profiles` includes a maximum of 5000 items.

## Regions

The Profiles resource returns *only* those profiles whose marketplace falls within the region covered by the API host to which the call is addressed. 

For example, an advertiser with accounts only in the JP and SG marketplaces will receive `[ ]` as the response body for a `GET` request at the North American host via `https://advertising-api.amazon.com/v2/profiles`. This advertiser should call the Far East host via `https://advertising-api-fe.amazon.com/v2/profiles` to retrieve profiles from that region.

The `profileId` for a profile in a marketplace outside the region covered by a given host does *not* provide authorization for other operations on the host when passed as the `Amazon-Advertising-API-Scope` header. A `4XX - Unauthorized` response is returned.

- For a list of marketplaces available through each regional host, see our [API Overview](reference/api-overview#api-endpoints).

## Permissions

By default, the `GET /profiles` endpoint displays only profiles with permission to view and edit campaigns. This can be adjusted using the `apiProgram` and `accessLevel` query parameters in the request.

- [Learn more about permissions.](guides/account-management/permissions)

For example, the following request would return a list of profiles in North America for which the authorizing account has **View** permissions for reports:

```
https://advertising-api.amazon.com/v2/profiles?accessLevel=view&apiProgram=report
```

## Profile types

The Profiles resource includes an `accountInfo` object that includes metadata about the account associated with the profile. The `type` property describes the account type, and there are currently three types:

| Type property value | Account type | Description |
|--------------|-----------------------|-------------|
|  `vendor` | Vendor| The account is a vendor account. Vendor accounts are used with Sponsored Products, Sponsored Brands, and Sponsored Display only. |
|  `seller` | Seller  | The account is a seller account. Seller accounts are used with Sponsored Products, Sponsored Brands, and Sponsored Display only. Note that for Sponsored Brands and Sponsored Display, the account must be [enrolled in the Brand Registry](https://brandservices.amazon.com/brandregistry). |
| `agency`  | Agency | The account is an agency account. Agency accounts are used with the DSP and Data Provider APIs only. |

>[TIP]The response can be filtered to include only certain profile types using the `profileTypeFilter` parameter.

## Examples

### Retrieving profiles

In this example, an advertiser operates accounts in the CA, MX, and US marketplaces. The advertiser has granted authorization to access their data and services to a **client** with ID `amzn1.application-oa2-client.XXX`, and the client has retrieved an **access_token** `Atza|zZzZzZzZ`.

The following call to the API at the North American host will retrieve a list with three profiles:

```bash
GET /v2/profiles HTTP/1.1
Host: advertising-api.amazon.com
Content-Type: 'application/json'
Authorization: 'Bearer Atza|zZzZzZzZ'
Amazon-Advertising-API-ClientId: 'amzn1.application-oa2-client.XXX'
```

Response body:

```json
[
  {
    "profileId": 777777777,
    "countryCode": "CA",
    "currencyCode": "CAD",
    "timezone": "America/Los_Angeles",
    "accountInfo": {
      "marketplaceStringId": "A2EUQ1WTGCTBG2",
      "id": "ENTITY2Plqweuieorf",
      "type": "vendor",
      "name": "Name of the Account",
      "validPaymentMethod": false
    }
  },
  {
    "profileId": 888888888,
    "countryCode": "MX",
    "currencyCode": "MXN",
    "timezone": "America/Los_Angeles",
    "accountInfo": {
      "marketplaceStringId": "A1AM78C64UM0Y8",
      "id": "ENTITY2Ihjasdjkeru",
      "type": "vendor",
      "name": "Name of the Account",
      "validPaymentMethod": false
    }
  },
  {
    "profileId": 999999999,
    "countryCode": "US",
    "currencyCode": "USD",
    "timezone": "America/Los_Angeles",
    "accountInfo": {
      "marketplaceStringId": "ATVPDKIKX0DER",
      "id": "ENTITYZIbbbbbrrr",
      "type": "vendor",
      "name": "Name of the Account",
      "validPaymentMethod": true
    }
  }
]
```

### Using profile IDs

For this advertiser, each additional call to the API will be able to access resources associated with *one* of the above profiles. 

For instance, to access the [Sponsored Products Campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredProductsCampaigns) resource for this advertiser's account in MX, the `profileId` from the second profile above will be included as the `Amazon-Advertising-API-Scope` header.

```bash
POST /sp/campaigns/list HTTP/1.1
Host: advertising-api.amazon.com
Content-Type: 'application/vnd.spCampaign.v3+json'
Accept: 'application/vnd.spCampaign.v3+json'
Authorization: 'Bearer Atza|zZzZzZzZ'
Amazon-Advertising-API-ClientId: 'amzn1.application-oa2-client.XXX'
Amazon-Advertising-API-Scope: 888888888

{
    "maxResults": 10
}
```

To access the accounts for US or CA, an additional request must be made for each profile, with the corresponding `profileId` passed as the `Amazon-Advertising-API-Scope` header.

## Profiles concepts

- For Sponsored Ads, a profile represents an advertiser's account in a specific marketplace.
- For Amazon DSP, a profile represents an entity in a specific marketplace or region.
- A client application authorized by a user account to access data and services can retrieve a list of profiles via a `GET` request at the `/v2/profiles` endpoint.
- The `profileId` is included in most requests to the API to enable actions on that profile. The `profileId` is passed in the `Amazon-Advertising-API-Scope` header.

## Learn more

Learn more about concepts from this document:

- [Authorization Overview](guides/account-management/authorization/overview)
- [Authorization Grants](guides/account-management/authorization/authorization-grants)
- [Access Tokens](guides/account-management/authorization/access-tokens)
- [Profiles (OpenAPI specification)](reference/2/profiles)

To follow a step-by-step guide to establishing authorization in the Amazon Ads API, see our [Onboarding Walkthrough](guides/onboarding/overview).
