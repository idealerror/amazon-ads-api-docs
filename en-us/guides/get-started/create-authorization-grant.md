---
title: Getting started - Create an authorization grant
description: Step-by-step instructions for retrieving an LwA authorization grant code, including how to generate an authorization grant URL
type: guide
interface: api
tags:
    - Onboarding
    - Authorization
keywords:
    - Login with Amazon
    - access
    - permission
    - return URL
    - scope
    - Amazon Developer
    - redirect_uri
---

# Step 1: Create an authorization grant

<div class="breadcrumb-top" style="display: block; font-size: .9em; margin: 0px; margin: -10px 0 20px 0; padding: 10px; background: #f6f6f6; border-left: 8px solid #4f7cb1;">Getting started : <span style="white-space: nowrap;">[Overview](guides/get-started/overview) </span> | <span style="white-space: nowrap;">[**1\. Create auth grant**](guides/get-started/create-authorization-grant) | [2\. Generate access token](guides/get-started/retrieve-access-token) | [3\. Retrieve profiles](guides/get-started/retrieve-profiles)</span></div>

As described in the ["Getting Started" overview](guides/get-started/overview), an approved *client application* may make calls to the Amazon Ads API on behalf of an Amazon *user account* with access to Amazon Ads accounts.

The relationship between client application and user account is administered by Login with Amazon (LwA). The user account must explicitly grant authorization to the client application through [LwA's Authorization Code Grant process](https://developer.amazon.com/docs/login-with-amazon/authorization-grants.html) to generate an *authorization code* as described below.

- To learn more about authorization in the Amazon Ads API, see the [Authorization overview](guides/account-management/authorization/overview).

>[TIP:Getting started with our Postman collection]The Amazon Ads API Postman collection offers a simplified approach to getting started. [Learn more about the Amazon Ads API Postman collection.](guides/get-started/using-postman-collection)

There are two scenarios for requesting and granting authorization to access the advertising data and services associated with a user account:

- **Direct Advertiser** : You want to use the API to access the advertising data and services associated with your **own** Amazon account.
- **Partner** : A third party wants to authorize you to access the advertising data and services associated with **their** Amazon account.

For either scenario, you will create an authorization URL.

## Create an authorization URL

To retrieve an authorization code, create an authorization URL that enables a user account to grant authorization to your client application. This process involves three steps:

- [Allow a return URL](#Allow-a-return-URL)
- [Determine the URL prefix for your region](#Determine-the-URL-prefix-for-your-region)
- [Determine the values for the required query parameters](#Determine-the-values-for-the-required-query-parameters)

### Allow a return URL

Log in to the [Amazon Developer console](https://developer.amazon.com/dashboard) using your Amazon Developer account and select **Login with Amazon** from the top menu. 

Choose the security profile you used to apply for API access, move your mouse to the "gear" icon under **Manage** on the right side of the screen, and select **Web Settings** from the menu.

![screenshot of Login with Amazon console](/_images/setting-up/LwA_console.png)

From the Web Settings panel, click **Edit**. Add a valid address in the **Allowed Return URLs** field and click **Save**. 

> [TOPIC:What is an Allowed Return URL?] Any URL you add to the "Allowed Return URLs" list may be used as the `redirect_uri` parameter in "Determine the values for the required query parameters" below. After an advertiser grants authorization to your LwA client, the advertiser will be redirected to the specified URL, with the authorization code appended as a query parameter. Either of these approaches may prove best for your use case:<br /> <br /><ol><li>**Partners** seeking authorization from third parties may eventually wish to create a custom return URL or use LwA in conjunction with their own application. For more information on this method, consult the <a href="https://developer.amazon.com/docs/login-with-amazon/documentation-overview.html">Login with Amazon documentation</a>.</li><li>**Direct Advertisers** may use any valid URL -- `https://amazon.com`, for instance -- and then manually retrieve the authorization code as described below.</li></ol>The remainder of this walkthrough follows the second method.

### Determine the URL prefix for your region

Login with Amazon implements a different authorization URL prefix for each region. Select the URL for your region from the table below.

| Region | URL prefix | 
|--------|------------|
| North America (NA) | `https://www.amazon.com/ap/oa` |
| Europe (EU) | `https://eu.account.amazon.com/ap/oa` |
| Far East (FE) | `https://apac.account.amazon.com/ap/oa` |

>[NOTE] An authorization code retrieved from any of these URLs can be used to access the advertising API in *any* region.

>[WARNING] If this authorization grant is for the **Data Provider API**, recall that you created a separate DSP account for each region. You are required to repeat this process three times, once for each of the regions described above. 

### Determine the values for the required query parameters

The authorization URL has the following query parameters:

| Parameter | Description |
|-----------|-------------|
| client_id | The **Client ID** associated with your Login with Amazon client application. For information on locating your client ID, see [Create a Login with Amazon application](guides/onboarding/create-lwa-app). |
| scope | The OAuth 2.0 permission scope used to limit the application's access to an advertiser's account.<br/> <br />For the DSP, Sponsored Brands, Sponsored Display, Sponsored Products, and Amazon Attribution APIs, set `scope` to `advertising::campaign_management`. For the Data Provider API, set `scope` to `advertising::audiences`.|
| response_type | The type of response. Always set to `code`. |
| redirect_uri | The value from the **Allowed Return URLs** field of your Login with Amazon security profile, as set above. |

>[NOTE]For information about optional parameters, see [Authorization grants](guides/account-management/authorization/authorization-grants).

To generate an authorization code in the North America (NA) region, replace `YOUR_LWA_CLIENT_ID` and `YOUR_RETURN_URL` in the URL below with your values:

```shell
https://www.amazon.com/ap/oa?client_id=YOUR_LWA_CLIENT_ID&scope=advertising::campaign_management&response_type=code&redirect_uri=YOUR_RETURN_URL
```

For instance, given the following values:

- **Client ID:** `amzn1.application-oa2-client.12345678901234567890`
- **Return URL:** `https://amazon.com`

The authorization grant URL would resemble:

```shell
https://www.amazon.com/ap/oa
    ?client_id=amzn1.application-oa2-client.12345678901234567890
    &scope=advertising::campaign_management
    &response_type=code
    &redirect_uri=https://amazon.com
```

>[NOTE] LwA clients that were approved to access the API **before October 2020** may need to set `scope` to `cpc_advertising:campaign_management`.

## To grant access to your *own* Amazon Ads data

*For instructions on gaining access to a third party's data, [see "To gain access to a third party's Amazon Ads data" below](#To-gain-access-to-a-third-partys-Amazon-Ads-data).*

If you are granting permission to access your own advertising data and services, follow these steps:

1. Paste the authorization URL determined above into your browser's address bar. Navigate to the URL.

2. Sign in **using an Amazon user account with access to the Amazon Ads accounts** you want to manage through the API. This account may not be the same as the Amazon Developer account you used to create the LwA client.

3. You will be redirected to a consent form listing the specific data and services included in the authorization grant. To grant access, select **Allow**.

4. You will be redirected to the `redirect_uri` that you specified previously, with query parameters appended to the URL. Copy the address from your browser's address bar, and note the value of the `code` parameter (the `xxxx` in the example below):

```shell
https://www.amazon.com/?code=xxxxxxxxxxxxxxxxxxx&scope=advertising%3A%3Acampaign_management
```

>*To continue, see ["Next steps" below](#Next-steps).*

## To gain access to a third party's Amazon Ads data

If you are requesting that a third party grant access to their advertising data and services, follow these steps: 

1. Send the authorization URL determined above to the third party, with instructions to paste the URL into their browser's address bar and navigate to the URL.

2. Instruct the third party to sign in using the Amazon user account that has access to the Amazon Ads accounts for which they wish to grant authorization.

3. The third party will be redirected to a consent form listing the specific data and services included in the authorization grant. Instruct the third party to select **Allow** if they consent to sharing these data and services with you.

4. They will be redirected to the `redirect_uri` that you specified previously, with query parameters appended to the URL. Instruct the third party to send this URL to you.

Note the value of the `code` parameter (the `xxxx` in the example below):

```shell
https://www.amazon.com/?code=xxxxxxxxxxxxxxxxxxx&scope=advertising%3A%3Acampaign_management
```

## Next steps

The `code` parameter in the redirect URL is an **authorization code**, which you can now use in the next step of the onboarding process: [Retrieve access and refresh tokens](guides/get-started/retrieve-access-token).

>[WARNING] Authorization codes expire after 5 minutes. A new code may be generated by repeating the steps above.

>[TOPIC:Technical support] If you have difficulty connecting to the Amazon Ads API, visit our [Technical Support page](support/overview) for information.
