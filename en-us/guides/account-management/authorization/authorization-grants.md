---
title: Amazon Ads API Authorization Grants
description: How to create authorization grants for the Amazon Ads API using Login with Amazon.
type: guide
interface: api
tags:
    - Authorization
    - Account management
keywords:
    - consent request
    - authorization URL
    - authorization code
    - redirect URI
    - scope
    - permission
---

# Authorization grants

As explained in the [Authorization Overview](guides/account-management/authorization/overview), a successful call to the Amazon Ads API requires an advertiser to explicitly grant authorization to a client application to access the advertiser's data. 

This authorization process is handled by the Login with Amazon (LwA) service in multiple stages. The first manifestation of this is an **authorization code**.

Let's briefly review these identities:

- **Advertiser**: An Amazon Ads customer account.
- **Client**: A Login with Amazon client application approved to call the Amazon Ads API.

Let's also recall that in the following diagram, the authorization code represents a specific advertiser's consent for a specific client to access the advertiser's data and services through the Amazon Ads API.

![OAuth 2.0 in the Amazon Ads API](/_images/setting-up/advertising_api_auth_flow.png)

The authorization code is generated through a consent request enabled by Login with Amazon.

## Login with Amazon consent requests

Login with Amazon enables advertisers to grant authorization to clients through the LwA user interface. 

Upon visiting a specific URL in their web browser, the advertiser is prompted to log in using their Amazon credentials. Then the advertiser may choose to confirm their consent for the client that created the request to access the advertiser's Amazon Ads data and services.

- For more general information about authorization grants in LwA, see [Authorization Grants](https://developer.amazon.com/docs/login-with-amazon/authorization-grants.html "Login with Amazon documentation").

## The consent request URL

The consent request page is served by Amazon from one of three regional hosts:

| Region | URL prefix | 
|--------|------------|
| North America (NA) | `https://www.amazon.com/ap/oa` |
| Europe (EU) | `https://eu.account.amazon.com/ap/oa` |
| Far East (FE) | `https://apac.account.amazon.com/ap/oa` |

The request is associated with a specific *client application* and *scope* via query parameters in a unique URL. 

| Parameter | Description |
|-----------|-------------|
| client_id | The **client identifier** of the client application. |
| scope | The OAuth 2.0 permission scope describing the extent of the application's access to an advertiser's account.<br/>For most uses of the Amazon Ads API, set `scope` to `advertising::campaign_management`. For the Data Provider API, set `scope` to `advertising::audiences`. |
| response_type | The type of response. Always set to `code`. |
| redirect_uri | A URL to which the advertiser will be redirected after allowing access. |
| state | *OPTIONAL.* An opaque value used to maintain state between this request and the response. This value is included in the redirect. |
| code_challenge | *OPTIONAL.* Used to secure authorization code grants via Proof Key for Code Exchange (PKCE). |
| code\_challenge\_method | *OPTIONAL.* The method used to encode the `code_verifier` for the `code_challenge` parameter. |

Example:

```shell
https://www.amazon.com/ap/oa?client_id=YOUR_LWA_CLIENT_ID&scope=advertising::campaign_management&response_type=code&redirect_uri=YOUR_RETURN_URL
```

Once the advertiser confirms consent, Login with Amazon generates an authorization code representing the advertiser's permission for the client to access the advertiser's data within the given scope.

>[TIP]For security reasons, Login with Amazon recommends using `state`, `code_challenge`, and `code_challenge_method` for application integrations. For more information about these parameters, see [Authorization Requests](https://developer.amazon.com/docs/login-with-amazon/authorization-code-grant.html#authorization-request "Login with Amazon documentation").

## Retrieving authorization codes

After confirming consent, the advertiser is redirected to the `redirect_uri` specified in the request URL. The authorization code is appended as a query parameter. 

For a `redirect_uri` of `https://www.amazon.com`, for instance, the full redirect will resemble the following:

```
https://www.amazon.com/?code=xxxxxxxxxxxxxxxxxxx&scope=advertising%3A%3Acampaign_management
```

The `code` parameter is the authorization code.

>[TOPIC:Choosing a redirect URI]The `redirect_uri` may be any URL allowed for your Login with Amazon client. Allowed return URLs can be adjusted in the Amazon Developer console by editing the "Allowed Return URLs" field in the "Web Settings" for the client. To follow step-by-step instructions for setting a return URL, see our [Onboarding Walkthrough](guides/onboarding/overview).<br> <br>Because the authorization code is returned via the advertiser's web browser as a query parameter, some API callers may wish to redirect the advertiser to a page that handles the authorization code programmatically. For more information, see [Dynamically Redirecting Users](https://developer.amazon.com/docs/login-with-amazon/dynamically-redirect-users.html "Login with Amazon documentation").

## Using authorization codes

An authorization code can be used to retrieve an access token and refresh token from Login with Amazon. The access token is then used as a credential for calls to the Amazon Ads API.

Authorization codes are currently valid for 5 minutes. 

Authorization codes are valid globally across LwA hosts. 

- Learn more about [retrieving access and refresh tokens](guides/account-management/authorization/access-tokens).

## Authorization grant concepts

- An authorization code represents an advertiser's explicit grant of permission for a specific client to access the advertiser's data and services within a specific scope.
- A client creates a consent request by including parameters in a URL served from a Login with Amazon host.
- The advertiser can confirm consent through the Login with Amazon user interface displayed at that URL.
- The advertiser will then be redirected to a URL specified by the client, with the authorization code appended as a query parameter.

## Learn more

Learn more about concepts from this document:

- [Authorization Overview](/guides/account-management/authorization/overview)
- [Access Tokens](guides/account-management/authorization/access-tokens)
- [Authorization Grants (Login with Amazon documentation)](https://developer.amazon.com/docs/login-with-amazon/authorization-grants.html)

To follow a step-by-step guide to establishing authorization in the Amazon Ads API, see our [Onboarding Walkthrough](guides/onboarding/overview).
