---
title: Postman tutorial
description: Understand the steps to get started using the Amazon Ads API Postman collection and environment file. 
type: guide
interface: api
tags:
    - Account management
    - Authorization
---

# Get started with Postman

[Postman](https://www.postman.com/) is a tool that allows developers to make API calls using a user interface. Postman can also store variables and perform basic automations that simplify API testing. Amazon Ads provides a Postman collection that includes some of our most used endpoints so that developers can easily test out the API. Postman also enables developers to automatically [generate code samples](https://learning.postman.com/docs/sending-requests/generate-code-snippets/) in a number of programming languages. 

This tutorial describes how to set up the Amazon Ads API Postman collection. 

## Before you begin

Decide what version of Postman you want to use. If you don't have a Postman account, you can [download the desktop application](https://www.postman.com/downloads/). You can also [create a free account](https://identity.getpostman.com/signup) and use the web application. 

**This tutorial assumes you have already [onboarded to the API](guides/onboarding/overview) and [generated access and refresh tokens](guides/get-started/overview).**

## Tutorial

**File import and environment setup**

1. Download the Postman [environment file](https://raw.githubusercontent.com/amzn/ads-advanced-tools-docs/main/postman/Amazon_Ads_API_Environment.postman_environment.json) and [collection file](https://raw.githubusercontent.com/amzn/ads-advanced-tools-docs/main/postman/Amazon_Ads_API.postman_collection.json) from GitHub.
2. [Import both files into Postman](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/).
    1. In the **Collections** tab, you should see the **Amazon Ads API** collection. 
    2. In the **Environments** tab, you should see the **Amazon Ads API** environment. 
3. From the **Environments** tab, select the **Amazon Ads API Environment**. 
4. Add your auth credentials to the environment. Enter your credentials into the **Current Value** column for the following variables:

    | Variable | Description |
    |----------|-------------|
    | `client_id` | The client ID associated with your Login with Amazon application. |
    | `client_secret` | The client ID associated with your Login with Amazon application. |
    | `refresh_token` | Your refresh token used to get a fresh access token. |

5. (Optional) Update the URL prefixes. In the downloaded file, `api_url` is populated with the North America URL. If the advertising account you are working with located outside North America, update the `api_url`. See the [API overview](reference/api-overview#api-endpoints) for available URLs by geography. 

6. Save your changes to the environment.

**Get a fresh access token**

7. From the Postman **Collections** tab, navigate to the **Amazon Ads API** collection.
8. [Set the active environment](https://learning.postman.com/docs/sending-requests/managing-environments/#selecting-an-active-environment) to **Amazon Ads API**.
9. Open the **`POST` Access token from refresh token** endpoint. Hover over the `{{refresh_token}}`, `{{client_id}`, and `{{client_secret}}` values to check that they are pulling in the right values from your environment. 
10. Send the request. You should get a `200 OK` response code. 
11. Navigate back to your **Amazon Ads API Environment**. You should see a new **Current Value** for `access_token`, along with a timestamp in **Current Value** for `token_expires_at`. 
    
    >[TIP] `token_expires_at` keeps track of when your token expires. By default, each time you make a call within the collection, Postman checks if your token has expired. If it has expired, Postman automatically refreshes the token before making the call.

**Get your profile ID**

12. To make other calls using the API, you need a [profile ID](guides/account-management/authorization/profiles). If you know your profile ID, you can enter it directly into the `profileId` variable in the **Amazon Ads API Environment**. 

    >[TIP:Don't know your profile ID?] If you don't know your profile ID, use the **Profiles** > **`GET` Profiles** endpoint to return profiles associated to your account. The first profile returned is added to your environment automatically. If you have multiple profile IDs, make sure to copy and paste the desired profile ID into your environment.

Now that your environment has a valid access token and profile ID, you are ready to make calls to the Amazon Ads API!

## Next steps

Explore one of our tutorials: 

* [Make your first call](guides/get-started/first-call)
* [Request a report](guides/reporting/v2/sponsored-ads-reports)
* [Request a snapshot](guides/snapshots/get-started)

>[TIP:Brand new to Amazon Ads?] Use Postman to [create a test account](guides/account-management/test-accounts/create-test-accounts) where you can practice building campaigns without incurring ad spend.

## Currently supported

The Amazon Ads API postman collection currently supports the following features:

- Authentication
- Sponsored ads reporting
- Sponsored ads snapshots 
- Test accounts
- Account management
- Amazon Marketing Stream
- Sponsored Products campaign management (version 3)
- Product metadata
- Sponsored Display (for non-Amazon sellers)
- Budget usage
- Budget rules
- Exports
- Partner opportunities

>[NOTE] There is a separate collection available for **Sponsored Display** endpoints. If you want to test Sponsored Display endpoints, follow the [Sponsored Display Postman tutorial](guides/sponsored-display/tutorials/postman).

## Environment variable definitions

This table contains descriptions for all variables in the environment file. 

| Variable | Description |
|----------|-------------|
| `client_id` | The client ID of an authorized Login with Amazon application. |
| `client_secret` | The client secret of an authorized Login with Amazon application. |
| `permission_scope` | The scope used to determine permissions for your account. For general API use, keep the default value as `advertising::campaign_management`. |
| `redirect_uri` | Enter the URL included in the **Allowed Return URLs** configuration of a Login with Amazon application. Set to `https://amazon.com` by default; enable this URL in Login with Amazon. |
| `access_token` | The access token used to authenticate your API calls. Each token is valid for 60 minutes. |
| `refresh_token` | The token used to refresh your access token. |
| `token_expires_at` | The timestamp at which an access token expires. |
| `profileId` | The profile ID associated with your Amazon Ads account. |
| `auth_grant_url` | The URL prefix used for auth calls. |
| `token_url` | The URL prefix used for token calls. |
| `api_url` | The URL prefix used for general API calls. |

>[TOPIC: Support] If you have feedback or questions about the Postman collection, open an issue in our [GitHub repository](https://github.com/amzn/ads-advanced-tools-docs).


