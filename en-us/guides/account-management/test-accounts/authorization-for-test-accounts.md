---
title: Authorization for test accounts
description: Setup instructions for test account permission scope in the Amazon Ads API, with information about prerequisites
type: guide
interface: api
tags:
    - Testing
    - Account management
    - Onboarding
    - Authorization
keywords:
    - advertising::test:create_account
    - auth grant URL
    - permission scopes
---

# Authorization for test accounts

Authorization in the Amazon Ads API is handled by [Login with Amazon](https://developer.amazon.com/docs/login-with-amazon/documentation-overview.html) according to an OAuth 2.0 call flow. To access data and services for a standard advertising account, requests to the API must include the client ID for an approved Login with Amazon application as well as an access token representing permission from an Amazon user account to access data and services within the `advertising::campaign_management` scope.

- For an overview of authorization in the Amazon Ads API, see [Authorization](guides/account-management/authorization/overview).

Test account creation requires the additional `advertising::test:create_account` permission scope. This scope can be requested by any Login with Amazon client application approved to use the Amazon Ads API. 

>[WARNING]We recommend that this permission scope *not* be requested for existing user accounts, but rather only from an Amazon user account specifically created for testing as described below. User accounts that manage existing advertising accounts are blocked from creating test accounts in the API.

## Prerequisites

The following prerequisites are required to access the test accounts API:

1. **A Login with Amazon client application approved for access to the Amazon Ads API.** Approved clients are permitted to request the `advertising::test:create_account` permission scope. If you have not been approved for API access, please begin with our [onboarding overview](guides/onboarding/overview).
2. **An Amazon user account that *does not* currently manage Amazon Ads accounts.** We recommend creating a *new* Amazon user account for this purpose using a valid email address to which your development team has access. Note the login credentials for this account, and ensure that the Amazon account is valid in the marketplace in which you plan to use test accounts.

## Authorization grant

The process for granting authorization for test accounts is similar to that described in [Authorization grants](guides/account-management/authorization/authorization-grants).

### Authorization grant URL

The simplest approach is to request _both_ permission scopes from the test user account via a single authorization grant URL. 

As described in the [Login with Amazon documentation](https://developer.amazon.com/docs/login-with-amazon/requesting-scopes-as-essential-voluntary.html), multiple scopes can be requested from a single authorization grant URL by separating the scopes with a URL-encoded space character (`%20`), as in the following example:

```shell
advertising::test:create_account%20advertising::campaign_management
```

For example, the URL for an authorization grant with both scopes (split into multiple lines for legibility) would resemble the following:

```shell
https://www.amazon.com/ap/oa
    ?client_id=YOUR_LWA_CLIENT_ID
    &scope=advertising::test:create_account%20advertising::campaign_management
    &response_type=code
    &redirect_uri=YOUR_RETURN_URL
```

The strings `YOUR_LWA_CLIENT_ID` and `YOUR_RETURN_URL` should be replaced with appropriate values for your Login with Amazon application. For information on determining these values and selecting the appropriate regional host, see [Authorization grants](guides/account-management/authorization/authorization-grants).

## Retrieving access tokens

Visit the URL determined above in a browser that is _not_ signed in to any Amazon user account. Sign in using the account described in prerequisite 2 above, then allow permissions via the Login with Amazon user interface. 

You will be redirected to the specified redirect URL with the authorization grant code appended as the `code` query parameter. See [Retrieving authorization codes](guides/account-management/authorization/authorization-grants#retrieving-authorization-codes).

The retrieved code is valid for 5 minutes and can be used to retrieve access and refresh tokens as described in [Access tokens](guides/account-management/authorization/access-tokens).

## Using access tokens

An access token retrieved by the method described above will grant the client application permission to:

- Create test accounts as described in [Creating test accounts](guides/account-management/test-accounts/create-test-accounts) through the `advertising::test:create_account` scope
- Use the created test accounts to test requests to the API as described in [Using test accounts](guides/account-management/test-accounts/use-test-accounts) through the `advertising::campaign_management` scope

>[NOTE]It is possible to request permission for each scope individually by repeating the authorization grant and token retrieval process. For more information, see [Authorization grants](guides/account-management/authorization/authorization-grants) and the [Authorization overview](guides/account-management/authorization/overview).

As with other access tokens for the Amazon Ads API, access tokens expire in 60 minutes. A new access token can be generated as needed [using a refresh token](guides/account-management/authorization/access-tokens#generating-an-access-token-using-a-refresh-token).

To get started, see [Creating test accounts](guides/account-management/test-accounts/create-test-accounts).
