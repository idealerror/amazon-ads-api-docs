---
title: Onboarding Step 6 - Retrieve and use a profile ID
description: Walkthrough of retrieving profiles during onboarding, including an explanation of how profiles relate to advertising accounts and marketplaces.
type: guide
interface: api
tags:
    - Onboarding
    - Authorization
    - Account management
keywords:
    - advertising account
    - region
    - Amazon-Advertising-API-Scope
    - unauthorized
    - access
---


# Step 3: Retrieve a profile ID

<div class="breadcrumb-top" style="display: block; font-size: .9em; margin: 0px; margin: -10px 0 20px 0; padding: 10px; background: #f6f6f6; border-left: 8px solid #4f7cb1;">Getting started : <span style="white-space: nowrap;">[Overview](guides/get-started/overview) </span> | <span style="white-space: nowrap;">[1\. Create auth grant](guides/get-started/create-authorization-grant) | [2\. Generate access token](guides/get-started/retrieve-access-token) | [**3\. Retrieve profiles**](guides/get-started/retrieve-profiles)</span></div>

As described in the ["Getting Started" overview](guides/get-started/overview), there are two essential authorization credentials for calling the Amazon Ads API:

- The **client ID** of a Login with Amazon (LwA) client application that is approved for API access.
- An **access token** representing permission for that client to access resources on behalf of an *Amazon user account* that manages Amazon Ads accounts.

In addition, nearly all requests to the API require a **profile ID** representing the user's advertising account in a _specific_ marketplace. Once you have [generated an access token](guides/get-started/retrieve-access-token), follow the steps below to retrieve a profile ID for use in requests to the API.

- To review the steps so far, see the ["Getting Started" overview](guides/get-started/overview#steps-in-this-process).
- For conceptual information regarding profiles in the Amazon Ads API, see [Profiles](guides/account-management/authorization/profiles).

>[TIP:Retrieve profiles with our Postman collection]The Amazon Ads API Postman collection makes retrieving profiles easier, and automatically includes a profile ID in the headers of subsequent requests. [Learn more about the Amazon Ads API Postman collection.](guides/get-started/using-postman-collection)

## Access the Profiles resource

To retrieve a list of available profiles, make a `GET` request to the `/v2/profiles` endpoint in a region where the user account manages advertising accounts.

- For a list of regional hosts, see [API Endpoints](reference/api-overview#api-endpoints).
- For technical specifications, see the [Profiles OpenAPI specification](reference/2/profiles).

For a user account that manages an advertising account in the MX marketplace, for instance, the request should be directed to the host for North America: 

```
https://advertising-api.amazon.com/v2/profiles
```

The request has two required headers:

- `Amazon-Advertising-API-ClientId`: The client identifier of the LwA client application.
- `Authorization`: The string `Bearer ` prepended to the access token.

For a call to the North American host using cURL, substitute your values in the following request:

```
curl  \
    -H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.bb48851ca..."\
    -H "Authorization: Bearer Atza|IQEBLjAsAhRmHjNgHpi0U-Dme37rR6CuUpSR..." \
    https://advertising-api.amazon.com/v2/profiles
```

### Profiles response

The response to a successful call at this endpoint includes a list of profiles associated with the user account in the API host's region.

For example, the response from the North American host for a user account that manages exactly one advertising account (in the MX marketplace) would resemble the following:

```json
[
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
  }
]
```

>[TIP:Do you see multiple profiles?]Each profile represents an advertising account in a different marketplace. Note the `countryCode` value to determine the marketplace for a given profile.

>[TIP:Don't see any profiles?]An empty array (`[ ]`) in the response body indicates that authorization was successful but the user account does not have **View and Edit** permissions for any advertising accounts in this region. See [API Endpoints](reference/api-overview#api-endpoints) for region information.<br /><br />You can use optional parameters in your request to display profiles for which you have view-only permission. [Learn more about permissions](guides/account-management/permissions), or see the `accessLevel` and `apiProgram` parameters in the [Profiles specification](reference/2/profiles#tag/Profiles/operation/listProfiles).<br /><br />You can also use your approved LwA client to generate test accounts for the API, even if you do not have access to advertising accounts in any region. [Learn more about test accounts.](guides/account-management/test-accounts/overview)

>[TIP:Using a manager account?]If the user account authorizing the request is a manager account, the response will include all accounts in the current region for which the manager account has **Editor** access. For accounts with only **Viewer** access, use the `GET /managerAccounts` endpoint to retrieve profile IDs. [Learn more about manager accounts.](guides/account-management/authorization/manager-accounts)

## Pass the profile ID in subsequent requests

Aside from the `/v2/profiles` endpoint, requests to the Amazon Ads API can access resources for *only one* profile. The selected profile is determined by the profile ID passed as the `Amazon-Advertising-API-Scope` header.

For subsequent requests to the API, you'll pass your client ID, your access token, and a profile ID as the values for three required headers:

- `Amazon-Advertising-API-ClientID`: Your client ID.
- `Authorization`: The string `Bearer ` prepended to the access token.
- `Amazon-Advertising-API-Scope`: The profile ID for an advertising account in a specific marketplace.

>[NOTE]Access tokens expire after 60 minutes, and a request using an expired access token will receive an `Unauthorized` response. To generate a new token when needed, see [Generating an access token using a refresh token](guides/account-management/authorization/access-tokens#generating-an-access-token-using-a-refresh-token).

## Next steps

To get started using your credentials, see [Making your first call](guides/get-started/first-call).

>[TOPIC:Technical support]If you have difficulty connecting to the Amazon Ads API, visit our [Technical Support page](support/overview) for information about assistance.
