---
title: Getting started - Using the Amazon Ads API Postman collection
description: Walkthrough of setting up API access using the Amazon Ads API Postman collection to handle authorization and make your first call
type: guide
interface: api
tags:
    - Onboarding
    - Authorization
    - Account management
keywords:
    - Postman
    - getting started
    - how to
    - GitHub
    - environment
---


# Quickstart guide: Getting started using the Amazon Ads API Postman collection

The [Amazon Ads API Postman collection](guides/postman) includes scripts to ease the management of authentication and authorization credentials for calling the API, as well as pre-built requests for demonstrating common uses of the API.

As an alternative to our [getting started walkthrough](guides/get-started/overview), many first-time callers may find Postman useful for understanding the API. 

The quick setup below offers step-by-step instructions for configuring the Postman collection to make requests to the API.

>[TIP:Video walkthrough]Demonstration videos are embedded for each step below. You can also [view the entire walkthrough on YouTube](https://www.youtube.com/watch?v=SWqOPN33phw).

## Before you begin

To complete this guide, you will need:

- The **client ID** and **client secret** of a Login with Amazon client application approved to use the Amazon Ads API. See [Onboarding](guides/onboarding/overview) if you have not yet been approved for access.
- Login credentials for an Amazon user account that manages Amazon Ads accounts.
- [Postman](https://www.postman.com/), a third-party tool that allows developers to make API calls via a user interface. You can use either the [desktop](https://www.postman.com/downloads/) or [web-based](https://identity.getpostman.com/signup) application.

>[TIP:The Postman user interface]Terms for Postman UI elements used in this document are aligned with those used by Postman. Please refer to [Postman's interface documentation](https://learning.postman.com/docs/getting-started/navigating-postman/) for more information and visual aids.

## Quick setup

### Import the collection and environment files

<details class="details-bar">
    <summary><b>Video</b></summary>

    <iframe sandbox="allow-scripts allow-same-origin" width="720" height="405" src="https://www.youtube.com/embed/rZkS8m3EAro" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

</details>

1. Download the Postman [environment file](https://raw.githubusercontent.com/amzn/ads-advanced-tools-docs/main/postman/Amazon_Ads_API_Environment.postman_environment.json) and [collection file](https://raw.githubusercontent.com/amzn/ads-advanced-tools-docs/main/postman/Amazon_Ads_API.postman_collection.json) from GitHub.
2. [Import both files into Postman](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/). To verify the import:
    - Select the **Collections** icon in the left sidebar. You should see the **Amazon Ads API** collection. 
    - Select the **Environments** icon in the left sidebar. You should see the **Amazon Ads API Environment**. 
3. From the [**Environments** selector](https://learning.postman.com/docs/getting-started/navigating-postman/#environment-selector-and-environment-quick-look), select the **Amazon Ads API Environment** to activate the environment. 

### Configure the environment

<details class="details-bar">
    <summary><b>Video</b></summary>

    <iframe sandbox="allow-scripts allow-same-origin" width="720" height="405" src="https://www.youtube.com/embed/bVJnA6mVgLc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

</details>

1. From the left sidebar, select **Environments**, then select the **Amazon Ads API Environment**.
2. Manually set the **Current Value** for the following variables:

    | Variable | Description |
    | --- | --- |
    | `client_id` | The client ID of the Login with Amazon client application approved to use the API. |
    | `client_secret` | The client secret of the Login with Amazon client application approved to use the API. |
    | `redirect_uri` | A URL included in the "Allowed Return URLs" configuration of your Login with Amazon application.<br> <br>In the environment, this is set to `https://amazon.com` by default. You will still need to [enable this URL in Login with Amazon](guides/get-started/create-authorization-grant#allow-a-return-url) or alter this variable to reflect another allowed URL. |

    >[NOTE]The default environment is configured to access the North American host for the API. For additional changes to access the API in other regions, see [Regions](#regions).

3. Save the changes to your environment.

### Generate an authorization grant code 

<details class="details-bar">
    <summary><b>Video</b></summary>

    <iframe sandbox="allow-scripts allow-same-origin" width="720" height="405" src="https://www.youtube.com/embed/uZaoQCZ1NX4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

</details>

1. From the Postman left sidebar, go to **Collections** > **Amazon Ads API**.
2. Select the **Auth** folder in the collection, then locate the **`GET` Auth grant login** request.
3. Open the **Console** in the [Postman footer](https://learning.postman.com/docs/getting-started/navigating-postman/#footer).
4. **Send** the request. This action tests the URL in Postman's preview window and logs the appropriate URL to the Postman console.
5. Copy the authorization grant URL from the console, visit the location in a web browser, and log in with an Amazon user account that manages Amazon Ads accounts. 

    >[WARNING]Ensure that you are logged out from any other Amazon user accounts before visiting the authorization URL.

6. You are redirected to the `redirect_uri` that you set in your environment file. An authorization grant code is appended to this URL as the `code` query parameter. 

The address in your browser's URL bar should resemble the following, where `XXXXX` is the authorization grant code:

```
https://amazon.com/?code=XXXXX&scope=advertising%3A%3Acampaign_management
```

Note this code for use in the next step. The authorization code represents permission from the user account for your client application to access that account's advertising data and services.

>[NOTE]Authorization grant codes expire in 5 minutes. A new code can be generated by repeating the authorization grant process.

### Retrieve access and refresh tokens

<details class="details-bar">
    <summary><b>Video</b></summary>

    <iframe sandbox="allow-scripts allow-same-origin" width="720" height="405" src="https://www.youtube.com/embed/K5Jc2DjCmyo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

</details>

1. From the **Auth** folder of the collection, select the **`POST` Access token from auth grant** request.
2. In the **Body** of this request, find the `code` parameter and enter the code retrieved in the previous step.
3. **Send** the request.

A successful request sets the `access_token` and `refresh_token` variables in your environment. 

>[TIP:Token expiry]Access tokens expire in 60 minutes. Refresh tokens do not expire.<br> <br>For subsequent requests to the API, a script in the Amazon Ads API Postman collection uses the refresh token to generate new access tokens automatically as needed.

### Retrieve a profile ID

<details class="details-bar">
    <summary><b>Video</b></summary>

    <iframe sandbox="allow-scripts allow-same-origin" width="720" height="405" src="https://www.youtube.com/embed/ZD1gbIPKiX4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

</details>

1. Select the **Accounts** folder in the collection.
2. Select the **`GET` Profiles** request and **Send** the request.

The response to this request is a list of _profiles_, each of which represents the user account's Amazon Ads account in a particular marketplace.

By default, a script in the collection sets the `profileId` variable in the environment to the _first_ profile returned in the response. This value is used in subsequent calls. To learn more about profiles, see [Profiles](guides/account-management/authorization/profiles).

## Next steps

Your Postman environment now has the required credentials to make other requests to the Amazon Ads API:

- Your client ID
- An access token
- A profile ID

The Postman collection is configured to include these values in the headers for subsequent requests.

To test a typical first call to the API, see [Making your first call](guides/get-started/first-call).



>[TOPIC:Technical support] If you have difficulty connecting to the Amazon Ads API, visit our [Technical support page](support/overview) for information.<br><br>If you believe the issue is related to this documentation or to the Postman collection itself, you can open an issue in our [GitHub repository](https://github.com/amzn/ads-advanced-tools-docs).

## Optional changes

### Regions

The provided environment is configured to call the North American host for the Amazon Ads API. To access advertising data and services in Europe or Asia, change the `api_url` , `auth_grant_url`, and `token_url` variables in the environment to the appropriate regional hosts.

| Region | `api_url` | `auth_grant_url` | `token_url` |
| ------ | --------- | ---------------- | ----------- |
| NA | `https://advertising-api.amazon.com` | `https://www.amazon.com/ap/oa` | `https://api.amazon.com/auth/o2/token` |
| EU | `https://advertising-api-eu.amazon.com` | `https://eu.account.amazon.com/ap/oa` | `https://api.amazon.co.uk/auth/o2/token` |
| FE | `https://advertising-api-fe.amazon.com` | `https://apac.account.amazon.com/ap/oa` | `https://api.amazon.co.jp/auth/o2/token` |

For a list of marketplaces in each region, see [API endpoints](reference/api-overview#api-endpoints).
