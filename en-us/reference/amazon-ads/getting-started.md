---
title: Getting started with Amazon Ads API v1
description:  the Amazon Ads API v1
type: release-note
interface: api
keywords:
    - authorization
    - required headers
    - prerequisites
    - Amazon-Ads-AccountId
    - Amazon-Advertising-API-Scope
    - Amazon-Ads-ClientId
    - common model
---

# Getting started with Amazon Ads API v1

## Prerequisites

All clients who have completed [onboarding to the Amazon Ads API](guides/onboarding/overview) are permitted to call the Ads API v1 generally.

>[TIP:Closed betas]Certain resources may be listed as **closed beta**, in which case you need to submit a request to join the beta program in order to gain access. [Learn more about joining a closed beta program.](developer/betas#how-can-i-join-a-closed-beta)

## Required headers

Just as in the existing API, Ads API v1 requests must provide **Advertising-API-ClientId** and **Authorization** headers to authorize calls. [Learn more about authorization using Login With Amazon.](guides/account-management/authorization/overview)

An additional header is required for most requests, depending on the ad product associated with the request:

- **Amazon-Advertising-API-Scope**: Required for most sponsored ads requests. Use the [Profiles API](reference/2/profiles) to retrieve the profile ID of the desired advertiser and marketplace to use as the value for this header for your Sponsored Ads Seller or Vendor. You can also retrieve profile ID information from the [Manager Accounts API](reference/1-0/managerAccount) for Sponsored Ads Seller and Vendor accounts.
- **Amazon-Ads-AccountId**: Required for ADSP and global Sponsored Products requests.
    - For ADSP requests, either use the [DSP Advertisers API](dsp-advertiser#tag/Current-Seat/operation/ListAdvertiserSeats) and select the `advertiserId` of the desired account or use the [Manager Accounts API](reference/1-0/managerAccount#tag/Manager-Accounts/operation/getManagerAccountsForUser) and select the `dspAdvertiserId` value of a linked account to use as the value for this header.
    - For global Sponsored Products requests (not yet supported), use the [Advertising Accounts API](account-management) and select the `adsAccountId` of the desired advertiser to use as the value for this header.

## OpenAPI specification interface

The Amazon Ads API v1 OpenAPI specification is presented in an enhanced interface featuring a new ad product selection control for select APIs (e.g., campaign management).

![Ad product selector for Ads API v1](/_images/ads-api/product-selector.png)

By default, the page displays a specification common to all ad products, including all available attributes. With any ad product selected, the page displays a specification specific to that ad product, including only those attributes that apply to the given entity for that ad product. These ad product-specific specifications display what is needed for a specific ad product's business rules and validations.

Similarly, the **Download OpenAPI spec** link in the description adjusts depending on whether an ad product is selected.

## Implementation recommendations

### Test accounts

You can use your existing accounts, or you can create a [test account](test-accounts/openapi) for testing. See [Test Account Overview](guides/account-management/test-accounts/overview) for more information on setup.
