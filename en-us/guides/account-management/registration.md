---
title: Account registration
description: Overview of account registration in the Amazon Ads API
type: guide
interface: api
tags:
    - Authorization
    - Account management
keywords:
    - selling account
    - DSP seats
---

# Advertiser accounts API (closed beta)

The Advertiser Account is Amazon’s enhanced workflow for onboarding advertisers and managing their full advertising activity across all ad-products (ADSP and Sponsored Ads) and countries. With this launch, customers with an Amazon DSP Seat can use Ads Console to onboard advertisers with a new streamlined account and billing setup. In this account, customers can operate across both ADSP and Sponsored Ads globally across the full advertising lifecycle from campaign management and reporting, to user management and billing. API customers have the same experience on Amazon Ads API, enabling them to onboard advertisers at scale without relying on Ads Console to do so.

>[WARNING]The advertiser accounts API is currently in closed beta. If you have an ADSP seat and would like to use this feature, please speak with your Amazon ad-tech AE.

Previously, registration and account management functionality for Amazon Ads advertiser accounts was only available through the Ads Console. With this launch, Amazon Ads API customers can execute all account management related actions (create, view, update advertiser accounts etc.) for all Amazon ad products and countries via API without the need to visit Ads Console to do so. Now, customers can register Advertiser Accounts at scale via public API, and they can use the resulting account identifiers to execute campaign management, reporting, billing and user management operations across all countries and ad products (ADSP, SA). Below is the list of new APIs launched:

- [POST /adsApi/v1/create/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/CreateAdvertiserAccount): Register one or multiple advertiser accounts by providing business details or importing them from a selling account token
- [POST /adsApi/v1/query/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/QueryAdvertiserAccount): Fetch account details/attributes (such as business/billing details, selling accounts, alternate identifiers for public APIs etc.) for a given advertiser account, a group of advertiser accounts, or all advertiser accounts that a given user has access to
- [POST /adsApi/v1/update/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/UpdateAdvertiserAccount): Programmatically update account details/attributes for advertiser account(s)
- [POST /adsApi/v1/query/sellingAccounts](amazon-ads/1-0/account-registration#tag/SellingAccounts/operation/QuerySellingAccount): Fetch all Amazon retail selling account (e.g. Seller, Vendor, Author, KDP etc.) that a given user has access to, so that the resulting encrypted selling account token can be used to register an associated advertiser account.

