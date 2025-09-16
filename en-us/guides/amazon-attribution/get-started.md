---
title: Amazon Attribution API get started
description: How to get started with the Amazon Attribution API
type: guide
interface: api
tags:
    - Amazon Attribution
keywords:
    - getting started
---

# Getting started with the Amazon Attribution API

In order to get started with Amazon Attribution API, you must first:

1. Set up your Advertising API developer account.
2. Obtain authorization or access for an eligible advertiser's Amazon Ads account. 

Here are three ways to obtain permission:

* **Recommended approach:** You create an [OAuth Login With Amazon application](guides/onboarding/create-lwa-app) and the advertiser uses it to grant you access to their profile.
* Advertiser invites you to their ad console account via the email address associated with your API developer account.
* Advertiser invites you to a manager account linked to their ad console account via the email address associated with your API developer account.

>[TOPIC:Authorization and access] For more information about authorization credentials for the Amazon Ads API, please see [our onboarding overview](guides/onboarding/overview).

Once you have completed the above requirements, you can use the Amazon Attribution API to read the following resources associated with your advertiser:

* Profiles
* Publishers
* Attribution tags
* Reports

## Notes on Amazon Attribution migration from DSP to advertising console

Advertisers eligible for Amazon Attribution can access it through the advertising console alongside other self-service advertising products, including Sponsored Products, Sponsored Brands, Sponsored Display, and Stores. 

Legacy Amazon Attribution accounts are no longer accessible through a web browser interface but still function as usual through the API. Advertisers with legacy accounts should have already migrated their data to their ad console accounts. For assistance with account migration, advertisers should reach out to Amazon Ads support using the ad console help center. 

To ease migration for API integrators, the Amazon Attribution API uses the same API endpoints for both legacy profiles and advertising profiles. Note that while legacy profiles could contain more than one Amazon Attribution advertiser, this is no longer true for advertising console profiles, which have a one-to-one relationship with advertisers. The Advertisers resource remains functional on either type of profile for full backward compatibility, but `advertiserId` values will be different on each profile.

An Amazon Attribution legacy profile can be identified by the `subType` property with value `AMAZON_ATTRIBUTION`. New integrators should filter out these outdated profiles in their user account onboarding process to prevent confusion.

## Next steps

Learn [how to use the Amazon Attribution API](guides/amazon-attribution/how-to).
