---
title: Get started with the bid adjustments API
description: How to get started using the bid adjustments API
type: guide
interface: api
---

# Get started with the bid adjustments API

## Amazon Ads API onboarding

To use the bid adjustments API, advertisers will first need to onboard the Amazon Ads API. See [steps to onboard the Amazon Ads API](guides/onboarding/overview). If you have any general questions about onboarding to or using Amazon Ads API, you may reach out for support via the [API Support team](support/overview).

### Authorize the client ID to new DSP accountId (advertiser id)

Bid adjustments API access is through [client ID](https://advertising.amazon.com/API/docs/en-us/guides/get-started/retrieve-access-token#retrieve-your-client-id-and-client-secret); therefore, you do not need to request additional access if you are a partner trying to work with a different DSP customer. You will need your DSP customer to invite you to access their DSP account. This can be done via **Account access and settings** on the [Amazon DSP](https://advertising.amazon.com/solutions/products/amazon-dsp) console. Once this is done, you will be granted account access via both API and the [Amazon DSP](https://advertising.amazon.com/solutions/products/amazon-dsp) console.

## Bid adjustments API access

When making any bid adjustments API requests, please ensure that the below headers ([Amazon Ads API required headers](guides/account-management/authorization/overview#required-authorization-headers)) are correctly specified. **All three headers below are required for ALL API calls.**

|Header	|Purpose	|
|---	|---	|
|Amazon-Advertising-API-ClientId	|The identifier of a client associated with a "Login with Amazon" account.	|
|Amazon-Advertising-API-Scope	|The identifier of a profile associated with the advertiser account.	|
|Amazon-Ads-AccountId	|The Account identifier you use to access the DSP. This is your Amazon DSP Advertiser ID.	|
