---
title: Profiles
description: Amazon Ads API version 1 Profiles reference.
type: guide
interface: api
---

# Profiles

>[WARNING] These endpoints are deprecated and have an expected shutoff date of **May 28, 2024**. Use [version 2 profiles endpoints](reference/2/profiles) going forward. See the [Deprecations page](release-notes/deprecations) for more information.

Marketplaces include associated properties that are specific to the region-- for example, `countryCode`, `currencyCode`, or `timezone`. These properties dictate the behavior of the Advertising API such as scheduling, budget, and billing. For this reason, Advertisers with accounts associated with more than one marketplace (for example: Amazon.com, Amazon.co.uk, Amazon.co.jp) will have only one profile associated with each marketplace. Each profile must be associated with a unique Login with Amazon account. Each Login with Amazon account requires a unique email address. When generating API authentication tokens you must use the correct profile for the international marketplace where you manage your campaigns

Authentication tokens are used to obtain all the profiles associated with the advertiser. The profile ID is passed in the header for calls to the API. Reports and all entity management operations are associated with a single profile. Advertisers cannot have more than one profile for each marketplace.

Profiles are associated with a unique Profile ID. A management scope is required in the header for all API calls. Use your Profile ID as the value for the management scope (Amazon-Advertising-API-Scope) in the header. To retrieve your profile ID, make a call to list profiles associated with the access token you are using.

```shell
GET /v1/profiles

    Host: advertising-api.amazon.com
    Authorization: Bearer Atza|IQEBLjAsAhRmHjNgHpi0U...
    Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.a8358a60…
    Amazon-Advertising-API-Scope: 1834670349683089580748 
    Content-Type: application/json 
```

The following methods are used to read and update profiles. The
following table describes the service behavior in terms of the URL space
and HTTP methods supported by the service-managed resources.

| Method | URL                   | Use Case                                              |
|--------|-----------------------|-------------------------------------------------------|
| GET    | `/profiles`             | Retrieve all profiles associated with the auth token. |
| GET    | `/profiles/{profileId}` | Retrieve a profile by ID                              |
| PUT    | `/profiles/`            | Update one or more profiles                           |

#### Regional profile time zone codes

Use the exact text and format in the ‘Time zone’ column to designate the correct
time zone.

| Region | Time zone |
| --- | --- |
| US | America/Los\_Angeles |
| CA | America/Los\_Angeles |
| UK | Europe/London |
| DE | Europe/Paris |
| FR | Europe/Paris |
| ES | Europe/Paris |
| IT | Europe/Paris |
| JP | Asia/Tokyo |
| AU | Australia/Sydney |
| AE | Asia/Dubai |

## Operations

### listProfiles

```shell
GET /v1/profiles
```

Retrieves profiles associated with an auth token.

#### Parameters

None

#### Response


| Status Code | Response object |
| --- | --- |
| `200 - success` | List of [`Profile`](#Profile)  |
| `401 - unauthorized` | [`Error`](#Error) |

### getProfile

```shell
GET /v1/profiles/{profileId}
```

Retrieves a single profile specified by identifier.

#### Parameters

| Parameter Name | Type | Description |
| --- | --- | --- |
| `profileId` | string | The profile identifier. |

#### Response

| Status Code | Response object |
| --- | --- |
| `200 - success` | List of [`Profile`](#Profile)  |
| `401 - unauthorized` | [`Error`](#Error) |
| `404 - profile not found` | [`Error`](#Error) |

### updateProfiles

```shell
PUT /v1/profiles
```

Updates one or more profiles. Advertisers are identified using their profileId.

#### Parameters

| Type | Specified In | Description |
| --- | --- | --- |
| List of [`Profile`](#Profile)  | body | A list of updates containing profile IDs and the mutable fields to be modified.<span class="break"> </span>Mutable fields: `dailyBudget` |

#### Response

| Status Code | Response object |
| --- | --- |
| `207 - multi-response` | List of [`ProfileResponse`](#ProfileResponse) reflecting the same order as the input |
| `401 - unauthorized` | [`Error`](#Error) |

### registerProfile

```shell
PUT /v1/profiles/register
```

Registers a profile in sandbox. If this call is made to a production endpoint you will receive an error.

#### Parameters

| Type | Specified In | Description |
| --- | --- | --- |
| `countryCode` | Body | The country in which you wish to register the profile. Can be one of: US, CA, UK, DE, FR, ES, IT, JP, AU, AE |

#### Response

| Status Code | Response object |
| --- | --- |
| `200 - Success` | Profile registration succeeded. |
| `401 - Unauthorized` | [`Error`](#Error) |
| `404 - Not Found` | [`Error`](#Error). This will be returned if making the call to a production endpoint. |

### registerBrand

```shell
PUT /v1/profiles/registerBrand
```

Registers a brand in sandbox. If this call is made to a production endpoint you will receive an error.

#### Parameters

| Type | Specified In | Description |
| --- | --- | --- |
| `countryCode` | Body | The country in which you wish to register the profile. Can be one of: US, CA, UK, DE, FR, IT, ES, JP, AU, AE |
| `brand` | Body | REQUIRED. Brand name. |

#### Response

| Status Code | Response object |
| --- | --- |
| 200 - Success | Profile registration succeeded. |
| 401 - Unauthorized | [`Error`](#Error) |
| 404 - Not Found | [`Error`](#Error). This will be returned if making the call to a production endpoint. |

## Resource representations

### Profile

```JSON
{
    "title": "Profile",
    "type": "object",
    "properties": {
       "profileId": {
           "description": "The ID of the profile",
           "type": "number"
       },
       "countryCode": {
           "description": "The country code identifying the publisher(s) on which ads will run",
           "type": "string",
           "oneOf": ["US"]
       },
       "currencyCode": {
           "description": "The currency used for all monetary values for entities under this profile",
           "type": "string",
           "oneOf": ["USD"]
       },
       "dailyBudget": {
           "description": "The optional budget shared by all entities created under this profile",
           "type": "number",
           "minimum": 1.00
       },
       "timeZone": {
           "description": "The tz database time zone used for all date-based campaign management and reporting.",
           "type": "string"
       },
       "accountInfo": {
           "type": "object",
           "properties": {
               "marketplaceStringId": {
                   "description": "The string identifier for the marketplace associated with this profile. This is the same identifier used by MWS",
                   "type": "string"
               },
                "sellerStringId": {
                   "description": "The string identifier for the seller associated with this profile. This is the same identifier used by MWS",
                   "type": "string"
               }
           }
       }
    }
 }
```

### ProfileResponse

```JSON
 {
    "title": "ProfileResponse",
    "type": "object",
    "properties": {
       "profileId": {
           "description": "The ID of the profile that was updated, if successful",
           "type": "number"
       },
       "code": {
           "description": "An enumerated success or error code for machine use.",
           "type": "string"
       },
       "details": {
           "description": "A human-readable description of the error, if unsuccessful",
           "type": "string"
       }
    }
}

```

### Error

```JSON
{
    "title": "Error",
    "type": "object",
    "properties": {
       "code": {
           "description": "An enumerated error code for machine use",
           "type": "string"
       },
       "details": {
           "description": "A human-readable description of the error. ",
           "type": "string"
       }
    }
}
```
 