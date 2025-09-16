---
title: Get started with Marketing Mix Modeling
description: How to get started with MMM
type: guide
interface: api
tags:
   - Reporting
   - Marketing Mix Modeling
---
# Get started

To get started with the MMM API, you need to have:

1. Onboard with the Amazon Ads API.
1. Registered with a manager account for the MMM service.

## Onboard to Amazon Ads API

To onboard to the Amazon Ads APIs, complete the following steps:

1. **Create a [Login with Amazon application](guides/onboarding/overview) and get Amazon Ads API access.** Requests to the Amazon Ads API are made by a client application administered by Login with Amazon.
2. **[Create an authorization grant](guides/get-started/create-authorization-grant).** To generate the necessary access tokens to successfully perform an API request:
   1. Follow the section “[To grant access to your *own* Amazon Ads data](guides/get-started/create-authorization-grant#to-grant-access-to-your-own-amazon-ads-data),” as you are granting *your* LwA app permission. Before pasting the authorization URL (step 1 of that section), ensure that you are signed out of all Amazon accounts *or* paste the authorization URL in an incognito/private window of your browser. Sign in using an Amazon user account that is associated with your LwA application. Ensure that this user has the **Admin** role in your manager account.
      ** Use `advertising::campaign_management` as the scope when creating the authorization code URL.
   2. [Retrieve access and refresh tokens](guides/get-started/retrieve-access-token).

>[NOTE] You may skip “[Step 3: Retrieve a profile ID](guides/get-started/retrieve-profiles),” as the MMM API does not require this parameter.

## Registering for MMM
To work with MMM programmatically, you must first [register for MMM](https://advertising.amazon.com/resources/whats-new/self-service-marketing-mix-model) in the Amazon Ads Console.

1. **Create or sign in to your [Amazon Ads](https://advertising.amazon.com) US account.**
2. **[Register for MMM](https://advertising.amazon.com/choose-account?destination=/mmm-portal/requests/) in the Amazon Ads console.** You may select an existing manager account or create a new one. You will need your manager account ID to call the API. This can be found at the [Manager Account page](https://advertising.amazon.com/mn) in the top left corner.

   ![Ads console screenshot](/_images/abvp-console-screenshot.png)

### Brand group access

Once your registration has been approved, contact <mmm-support@amazon.com> to help you set up brand access.

## Next steps
- Learn more about the [MMM data feed](guides/reporting/marketing-mix-modeling/mmm-data-feed).
- [Make your first call](guides/reporting/marketing-mix-modeling/call-the-api) to the MMM API.
