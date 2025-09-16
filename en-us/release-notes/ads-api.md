---
title: Amazon Ads API release notes
description: Amazon Ads API release notes, with information about new features and other changes.
type: release-note
interface: api
keywords:
    - Slack
    - RSS feed
    - updates
---

# Amazon Ads API release notes

> [TIP:Connect your team to Amazon Ads API release notes]You can subscribe to these release notes in your team's workspace via our [RSS feed](https://d3a0d0y2hgofx6.cloudfront.net/rss/en-us/ad-api-rss.xml).<details style="margin-top:8px;"><summary>Connect in Slack</summary><ol><li>[Add the RSS app to your Slack workspace.](https://slack.com/help/articles/218688467-Add-RSS-feeds-to-Slack)</li><li>Send this message in the channel where you want the feed to appear:<br>`/feed subscribe https://d3a0d0y2hgofx6.cloudfront.net/rss/en-us/ad-api-rss.xml`</li></ol></details><details><summary>Connect in Microsoft Teams</summary><ul><li><a href="https://docs.microsoft.com/en-us/connectors/rss">Create a new RSS connector</a> through the **Apps** menu in Teams.</li><li>Provide the following URL:<br>`https://d3a0d0y2hgofx6.cloudfront.net/rss/en-us/ad-api-rss.xml`</li></ul></details>

>[NOTE]This page includes release notes only for the new [Amazon Ads API v1](reference/amazon-ads/overview). For all advanced tools releases, see [Release notes](release-notes/index).

## August 2025

### Campaign management API now available for Sponsored Products, Sponsored Brands, and Amazon DSP

Amazon Ads has launched a single Campaign Management API to unify campaign management into one set of APIs. Built from a [common Amazon Ads data model](reference/amazon-ads/overview), this API standardizes field and enum values across ad products and features, improves error handling, and helps resource consistency across Campaign, Ad Group, Target, and Ad objects. This API includes feature parity with the existing APIs. Previously, there were nearly 200 separate variations of create, read, update, and delete endpoints that will be reduced to 16 with this launch. 

Today, we announce the completion of Sponsored Products, Sponsored Brands, and Amazon DSP for worldwide general availability. All of our ad products will be available through the Amazon Ads Campaign Management API, including Sponsored Display and Sponsored TV later in 2025.

To get started, please see the [campaign management overview](guides/campaign-management/overview) and the [technical specification](amazon-ads/1-0/openapi).

## July 2025

### Account registration API now available in closed beta

The Advertiser Account is Amazon's enhanced workflow for onboarding advertisers and managing their full advertising activity across all ad products (ADSP and Sponsored Ads) and countries. With this launch, customers with an Amazon DSP Seat can use Ads Console to onboard advertisers with a new streamlined account and billing setup. In this account, customers can operate across both ADSP and Sponsored Ads globally across the full advertising lifecycle from campaign management and reporting, to user management and billing. API customers have the same experience on Amazon Ads API, enabling them to onboard advertisers at scale without relying on Ads Console to do so. If you have an ADSP seat and would like to use this feature, please speak with your Amazon ad-tech AE.

Previously, registration and account management functionality for Amazon Ads advertiser accounts was only available through the Ads Console. With this launch, Amazon Ads API customers can execute all account management related actions (create, view, update advertiser accounts etc.) for all Amazon ad products and countries via API without the need to visit Ads Console to do so. Now, customers can register Advertiser Accounts at scale via public API, and they can use the resulting account identifiers to execute campaign management, reporting, billing and user management operations across all countries and ad products (ADSP, SA). Below is the list of new APIs launched:

- [POST /adsApi/v1/create/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/CreateAdvertiserAccount): Register one or multiple advertiser accounts by providing business details or importing them from a selling account token
- [POST /adsApi/v1/query/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/QueryAdvertiserAccount): Fetch account details/attributes (such as business/billing details, selling accounts, alternate identifiers for public APIs etc.) for a given advertiser account, a group of advertiser accounts, or all advertiser accounts that a given user has access to
- [POST /adsApi/v1/update/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/UpdateAdvertiserAccount): Programmatically update account details/attributes for advertiser account(s)
- [POST /adsApi/v1/query/sellingAccounts](amazon-ads/1-0/account-registration#tag/SellingAccounts/operation/QuerySellingAccount): Fetch all Amazon retail selling account (e.g. Seller, Vendor, Author, KDP etc.) that a given user has access to, so that the resulting encrypted selling account token can be used to register an associated advertiser account.

## June 2025

### Manage Brand Stores programmatically through the Amazon Ads API

Brand Stores has launched a new set of APIs that offer brands and advertisers programmatic access to manage their Amazon Brand Stores more efficiently. These APIs, now available through the Amazon Ads API, provide two key functions: 1) retrieve Brand Store page content and update the product content and selection across Brand Stores pages and 2) submit store updates for moderation.

Advertisers can use these APIs to programmatically manage their Brand Stores pages. These functionalities can be combined with Brand Home APIs to automate and maintain product information freshness, creating a more efficient and streamlined management process for Brand Stores on Amazon. 

The Brand Stores management APIs are available in beta to all Brand Store owners through the Amazon Ads API. To learn more, see the [Brand Stores management overview](guides/brand-store/user-guide) and the [technical specifications](amazon-ads/1-0/brand-stores).

### New API to query account links available in closed beta 

We have introduced a new POST endpoint /adsApi/v1/query/accountLinks that returns information about linked accounts in a paginated manner. Manager Account users can retrieve details about all accounts (Sponsored Ads accounts, ADSP advertiser accounts, and Manager Accounts) linked to their Manager Accounts. Additionally, advertiser account users can find all Manager Accounts linked to their advertising account. This helps users better understand the structure and access relationships across their advertising setup.

This enhancement helps customers with complex account structures better understand their organizational account structure. The new paginated API fixes two key limitations: it removes the previous 50-linked-account limit for Manager Accounts in the GET /managerAccounts API, and it gives advertiser account users the ability to query linked Manager Accounts via API, a feature not previously available. Customers can now retrieve complete information on their account linking structure, helping them make better decisions about their organization's advertising spend and performance.

To learn more, see the [linked accounts API user guide](guides/account-management/linked-accounts) and the [technical specification](amazon-ads/1-0/permissions).

### Amazon Ads Forecasting API (beta) is now available

Amazon Ads has launched a new forecasting API for Amazon DSP (ADSP) campaigns. Built on the Amazon Ads data model, this API provides access to all real-time forecasts currently available on ADSP Campaign Manager.

In this launch, the API offers campaign-level performance, delivery, and spend forecasts currently available on forecasting widgets. This includes campaign flight level forecasts for spend, reach, CPC, CPA, and ROAS. The API provides forecasts both for new orders and existing orders that are spending budget.

Throughout 2025, we will expand the capabilities of the API to include forecasts for other features on the campaign manager such as line-level forecasts, additional spend potential, and creative forecasts. These metrics will also be available using datasets through Amazon Marketing Cloud for easier integrations. For more information, please see the [technical specification](amazon-ads/1-0/forecasts) and the [Forecast API overview](guides/dsp/forecasts).

## May 2025

### Amazon Ads Campaign Management API (beta) is now available

Amazon Ads has launched a single Campaign Management API for all ad products, starting with Sponsored Products, Sponsored Brands, and Amazon DSP. Built from a common Amazon Ads data model, this API will ensure field naming, enumerated values, and resource consistency across Campaign, Ad Group, Target, and Ad objects. 

Today, there are over 190 separate variations of create, read, update, and delete endpoints, which will be reduced to 16 with this launch. With the Amazon Ads Campaign Management API developers can expect to see complete feature parity, improved error handling, consistent request and response patterns, and higher pagination thresholds than previous versions.

Throughout 2025, all of our ad products will become available through the Campaign Management API, including Sponsored Display and Sponsored TV. We will also be launching consistent campaign datasets through Amazon Marketing Stream, built from the same underlying Amazon Ads domain model, for further consistency across all products. 

Although we do not anticipate updates to the contract, the contract may be subject to change during the beta period. 

For more information, please see the [technical specification](amazon-ads/1-0/openapi) and the [campaign management overview](guides/campaign-management/overview).
