---
title: Assign API access to a Login with Amazon application
description: How to assign access to your LwA application after approval 
type: guide
interface: api
tags:
    - Onboarding
keywords:
    - register
    - email
    - technical support
    - Login with Amazon
    - scope
---

# Step 3: Assign API access to a Login with Amazon application

<div class="breadcrumb-top" style="display: block; font-size: .9em; margin: 0px; margin: -10px 0 20px 0; padding: 10px; background: #f6f6f6; border-left: 8px solid #4f7cb1;">Onboarding: <span style="white-space: nowrap;">[Overview](guides/onboarding/overview)</span> | <span style="white-space: nowrap;">[1\. Create LwA client](guides/onboarding/create-lwa-app) | [2\. Apply for API access](guides/onboarding/apply-for-access) | [**3\. Assign access**](guides/onboarding/assign-api-access)</span></div>

If your request for API access is approved by Amazon, you'll receive an email from Amazon Ads. Click the link in this email to open a web page where you can assign API access to the Login with Amazon (LwA) application [you created in step 1](guides/onboarding/create-lwa-app).

>[WARNING]Before clicking on the link, it's important that you **log out of all Amazon user accounts** (including your personal shopping account) **except the Amazon account you used to apply for access in [step 2](guides/onboarding/apply-for-access)**. If anyone clicks on the link while logged into the wrong account, the link will be invalidated and will need to be reset by the Amazon API support team.

## Select your LwA application

After clicking the link, you'll see a message reminding you to create a client application through Login with Amazon as described in step 1 of this onboarding walkthrough. If you already completed step 1, click **Continue** to see a list of the LwA applications associated with the logged-in account. 

Select the Login with Amazon application you created in step 1 and click **Submit**. 

Once you've clicked the **Submit** button, you will see a confirmation page showing your LwA application's **client ID** along with the **scopes** that your application can now use to request permission from user accounts to access data and services:

- `advertising::campaign_management`: The required scope for most requests to the Amazon Ads API.
- `advertising::test:create_account`: The required scope for creating [test accounts](guides/account-management/test-accounts/overview).

You will primarily use `advertising::campaign_management` to access data and services through the API.

>[NOTE]If you have been approved for access to the data provider API, you will also see the `advertising::audiences` scope.

### Additional recommended steps for members of the Amazon Ads Partner Network

| [IMAGE] |   |
| ---- | ---- |
| If you indicated in the form from step 2 which LwA application you would like to link and have now assigned scope, you should see that the application now shows as linked in your Partner Network Account under **API applications**. | ![Screenshot of Partner Network applications interface](/_images/setting-up/partner-network-applications.png) |

If for any reason you did not request to link the relevant LwA application in the form from step 2, you can still choose to link an LwA application (with Ads scope) at this stage by navigating to **API applications** and then selecting **Link application**.

## Next steps

Your LwA client application for the Amazon Ads API has now been established. Any Amazon user account can now authorize your LwA application to access that account's advertising data and services through the API.

From this point, you will use this Login with Amazon application to create and manage authorization credentials, which must be passed in the headers of any request to the API.

To begin, see [Getting started](guides/get-started/overview).

>[TOPIC:Technical support] If you have difficulty connecting to the Amazon Ads API, please visit our [Technical Support page](support/overview) for information.
