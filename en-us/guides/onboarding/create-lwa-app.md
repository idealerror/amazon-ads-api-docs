---
title: Create a Login with Amazon application
description: Instructions for creating an Amazon Developer account and a Login with Amazon security profile 
type: guide
interface: api
tags:
    - Onboarding
    - Authorization
keywords:
    - register
    - Login with Amazon
    - client application
    - credentials
    - security
---

# Step 1: Create a Login with Amazon application

<div class="breadcrumb-top" style="display: block; font-size: .9em; margin: 0px; margin: -10px 0 20px 0; padding: 10px; background: #f6f6f6; border-left: 8px solid #4f7cb1;">Onboarding: <span style="white-space: nowrap;">[Overview](guides/onboarding/overview)</span> | <span style="white-space: nowrap;">[**1\. Create LwA client**](guides/onboarding/create-lwa-app) | [2\. Apply for API access](guides/onboarding/apply-for-access) | [3\. Assign access](guides/onboarding/assign-api-access)</span></div>

The first step in onboarding to the Amazon Ads API is to create a Login with Amazon (LwA) *client application*, which will later be used to make requests to the API.

Once you have been approved to access the API, you will assign your access to this LwA application in a later step.

To learn more about the role that LwA plays in the Amazon Ads API, see:

- [Amazon Ads API authorization overview](guides/account-management/authorization/overview)
- [Login with Amazon (LwA) concepts](https://developer.amazon.com/docs/login-with-amazon/conceptual-overview.html)

>[TIP:Already have Login with Amazon credentials?]Even if you already have Login with Amazon credentials -- for instance, if you currently use the Selling Partner API -- you will still create a new LwA security profile to use with the Amazon Ads API. If you are already logged in to Amazon Developer, proceed to [Create a new LwA security profile](#create-a-new-lwa-security-profile).

## Register with Amazon Developer

To create a Login with Amazon application, you must register as a developer on [developer.amazon.com](https://developer.amazon.com). 

To register as a developer, click **Sign In**.

From the sign-in screen, either: 

- enter your email address and password to sign in with an existing Amazon account *or*
- choose **Create your Amazon Developer account** to create a new account through Amazon Developer.

You will be redirected to the developer registration page. Complete the form, accept the Amazon Developer Services Agreement, and select **Submit**.

>[WARNING] At this stage, you can register as a developer *either* by using an existing Amazon account or by creating a new account through Amazon Developer. If you're creating an application associated with a business, we recommend you use an email address that is managed by multiple individuals in your organization.<br> <br>This is because the email address associated with your LwA developer registration will also be associated to your Amazon Ads API permissions once you've been approved. Also, once you are approved for API access and accept the license agreement you cannot change the LwA application associated with this email, so it's strongly recommended that all the individuals associated with the email address know which LwA application to associate with the email address.

## Create a new LwA security profile

| [IMAGE] |   |
| ---- | ---- |
| After logging in to the [Amazon Developer](https://developer.amazon.com) site using the account created above, click **Developer Console**.<br> <br>From the developer console, click **Login with Amazon** on the menu bar. | ![Login with Amazon](/_images/setting-up/5.png) |

| [IMAGE] |   |
| ---- | ---- |
| Click **Create a New Security Profile**. | ![Create a new security profile](/_images/setting-up/6.png) |

You're redirected to the **Security Profile Management** page. In the form, enter:

- **Security Profile Name**
- **Security Profile Description**
- **Consent Privacy Notice URL**

For more information on these fields, [see the LwA documentation](https://developer.amazon.com/docs/login-with-amazon/register-web.html#create-a-new-security-profile).

Select the **Save** button when you've entered this information.

>[NOTE]The Consent Privacy Notice URL should be your company’s privacy policy URL, if available. This URL will later be displayed as a link on the LwA authorization site that advertisers will visit to grant permissions to your application. <br><br>For Direct Advertisers accessing their own data, this is not essential for the onboarding process, and any URL may be entered.

## Retrieve your security credentials

| [IMAGE] |   |
| ---- | ---- |
| After creating a new profile, you're redirected to the **Login with Amazon** page. <br><br>This page includes a table that lists your **security profile name** and **OAuth2 credentials**. Locate the name of the security profile you created earlier, and click **Show Client ID and Client Secret**. | ![Show Client ID and Client Secret](/_images/setting-up/8.png) |

Note these values, as they are essential credentials for accessing the Amazon Ads API. The client ID is passed in all requests to the API, and both the client ID and client secret are used to generate access tokens for the API. These values can be retrieved from this panel at any time.

>[TIP:Can I use more than one client ID with the API?]No. As a policy, Amazon Ads enables API access for **one client ID per company**. There is no need for multiple client IDs, as an unlimited number of users can use the same client ID within a company. Additional user permissions can be managed later in the Amazon Developer console. However, **only the *original creator* of the Amazon Developer account** may complete the initial onboarding sequence.

## Next steps

Once you've completed these steps, continue to the next step of the onboarding process: [Apply for Advertising API access](guides/onboarding/apply-for-access).

>[TOPIC:Technical support] If you have difficulty connecting to the Amazon Ads API, please visit our [Technical Support page](support/overview) for information.
