---
title: Migrating to version 4 Sponsored Brands campaigns
description: A migration guide for moving to Sponsored Brands version 4 campaign resources 
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - Migration guide
    - Version 4
    - Multi-ad group campaigns
---

# Migrating to version 4 Sponsored Brands campaigns

## Overview

Sponsored Brands advertisers can now create ad groups to better organize and manage their campaigns. Ad groups are a way to organize and manage your campaign by brand, keywords, product, category, ad format, landing page, or other classifications that tie to your goals or marketing strategy. Brands can test different creatives within the same ad group, enabling advertisers to compare the performance and develop a better understanding of Amazon shopper behaviors.

## Key benefits

1. **Structure your campaigns in a way that works for you** by creating ad groups that align with your marketing strategies and goals.
2. **Monitor performance of ad group campaigns with reports**. You can easily retrieve reports based on keywords, campaign placement, search terms, attributed purchases, and more. Plus, you’ll no longer need to run separate reports for different campaign types and manually calculate the performance of your ad spend for Sponsored Brands. 
3. **Make the most of testing based on your goals**, whether that’s building brand awareness, driving consideration, or increasing sales. You can test targeting against different ad formats or landing pages and measure the performance. Plus, you can test different versions of the same ad formats within the same ad group to get a better understanding of shopper behavior. 
4. **Use consistent strategies across Amazon Ads sponsored ads products** to run your campaigns more effectively. The ad groups feature is also available for both [Sponsored Products](https://advertising.amazon.com/solutions/products/sponsored-products/) and [Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display/).

## Endpoint equivalencies

| Version 3 | Version 4 beta | Version 4 general availability |
|-----------|------------|-------------|
| POST /sb/campaigns | POST /sb/beta/campaigns | POST /sb/v4/campaigns |
| GET /sb/campaigns <br> GET /sb/campaigns/{campaignId} | POST /sb/beta/campaigns/list | POST /sb/v4/campaigns/list |
| PUT /sb/campaigns | - | PUT /sb/v4/campaigns |
| DELETE /sb/campaigns/{campaignId} | - | POST /sb/v4/campaigns/delete |
| GET /sb/adGroups <br> GET /sb/adGroups/{adGroupId} | POST /sb/beta/adGroups/list | POST /sb/v4/adGroups/list |
| - | POST /sb/beta/adGroups | POST /sb/v4/adGroups |
| - | POST /sb/beta/adGroups/delete | POST /sb/v4/adGroups/delete |
| - | PUT /sb/beta/adGroups | PUT /sb/v4/adGroups |
| - | POST /sb/beta/ads/list | POST /sb/v4/ads/list |
| - | POST /sb/beta/ads/video | POST /sb/v4/ads/video |
| - | POST /sb/beta/ads/brandVideo | POST /sb/v4/ads/brandVideo |
| - | POST /sb/beta/ads/productCollection | POST /sb/v4/ads/productCollection |
| - | POST /sb/beta/ads/storeSpotlight | POST /sb/v4/ads/storeSpotlight |
| - | PUT /sb/beta/ads | PUT /sb/v4/ads |
| - | POST /sb/beta/ads/delete | POST /sb/v4/ads/delete |


## Frequently asked questions

### How much time do I have to migrate from the beta endpoints to the general availability endpoints?

The version 4 beta end points will be available until August 31, 2023. After August 31th, all request calls that use the beta endpoints will be rejected. 

See [Deprecations](release-notes/deprecations) for more information. 

### Is there any difference between version 4 beta and general availability endpoints?

No, there are no functional or contractual changes between version 4 general availability and open beta endpoints.

### How are multi-ad group campaigns different from the current campaign structure?

The [current campaign endpoints](sponsored-brands/3-0/openapi/prod#tag/Campaigns) don't allow you to create or update ad groups, or associate ad creatives to ad groups. The current campaigns contain read-only ad groups where creatives are associated with campaigns instead of ad groups. 

In this guide, we'll refer to the current campaigns without multi-ad group support as **legacy campaigns** whereas campaigns with multi-ad group support are referred to as **multi-ad group campaigns**. The new multi-ad group campaigns contain three entities: campaigns, ad groups, and ads. Refer to diagram below to understand campaign hierarchy.

* **Campaigns**: Campaigns group your ads by their advertising budgets and dates. 
* **Ad groups:** Ad groups let you organize and manage your ads by brand, keywords, product, category, ad format, landing page, or other classifications that tie to your goals.
* **Ads:** Use ads to manage different variations of your creatives. Associate multiple ads with each ad group that share the same landing page but different ASINs, headlines, life style images, product category, or videos.

![Campaign entity diagram](/_images/sponsored-brands/v4-campaigns-diagram.png)

### What API changes are required to support multi-ad group campaigns?

We recommend you migrate to the [newest version of Sponsored Brands campaign management APIs](sponsored-brands/3-0/openapi#/Campaigns), which are now generally available. The new version lets advertisers create and manage multi-ad group campaigns, as well as read any previously created Sponsored Brands campaigns. The [list campaigns](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns) response includes a new field `isMultiAdGroupsEnabled` that returns `true` if the campaign is built on the newest version. 

You can view our [Creating multi-ad group campaigns tutorial](guides/sponsored-brands/campaigns/get-started-with-campaigns) to view the steps and examples for using the new endpoints. 

>[NOTE] The existing [version 3 GET campaigns endpoint](sponsored-brands/3-0/openapi#tag/Campaigns/operation/listCampaigns) returns the first ad group and ad from a multi-ad group campaign. If you want to see all ad groups and ads available, you need to use the [new list campaigns endpoint](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns). 

### What's the impact of using the existing [version 3 GET campaigns endpoint](sponsored-brands/3-0/openapi#tag/Campaigns/operation/listCampaigns)?

The current version 3 GET endpoint provides you with limited support for multi-ad group campaigns. The GET response will only include the first creative from the multi-ad group campaign. 

If you are using the version 3.3 GET campaigns endpoint, then the response includes optional fields `isMultiAdGroupsEnabled`, `adGroupCount`, and `adCount`. These fields will help you determine whether campaign is multi-ad group enabled and the number of ad groups and ads in the campaign. **We recommend that you use the [version 4 campaigns list](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns) endpoint to retrieve campaign-level details for all campaign types.**

>[WARNING] The version 3.3 GET campaigns endpoint will **not** return [brand video ads](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsBrandVideoAdsBeta). In order to get creative details for brand video ads, use the version 4 [POST sb/v4/ads/list](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/ListSponsoredBrandsAds) endpoint. 

>[NOTE] If you are using any ad format or creative type filters in your version 3 GET campaigns call, the response will not include multi-ad group campaigns. In version 4 campaigns, the ad format and creative type filters belong to the ad entity instead of the campaign entity.

>[WARNING] The advertising console will be switching to the new campaign type in September 2022. In order to retrieve full details for campaigns created in the ad console, you need to use the new POST /sb/v4/campaigns/list endpoint. You will also need to use the `creativeType` filter set to `all` in reports to get data for campaigns created in the console. 

### Can I add multiple ad groups to legacy campaigns?

Adding new ad groups to legacy campaigns is not allowed. The ad groups for these campaigns only support GET operations. Ad group creation and management are only supported for new multi-ad group campaigns through new version 4 endpoints. If you try to update a legacy ad group using the version 4 endpoints, you will receive an `INVALID_ARGUMENT` error.

### How do I update or archive multi-ad group campaigns?

PUT and DELETE endpoints are available through the version 4 campaign management APIs.

>[NOTE] The **`NameFilter`** for the `POST sb/v4/adGroups/list` endpoint will not work for legacy campaigns since the ad group name is auto-generated based on campaign name, so we don't recommend using this filter for legacy campaigns.

View our [Managing multi-ad group campaigns guide](guides/sponsored-brands/campaigns/managing-multi-ad-group-campaigns) for more information.

### What changes are required to retrieve reporting data using reporting API?

To retrieve reporting data for multi-ad group campaigns, you must include `"creativeType": "all"` as part of your report request. Reports using this filter return reporting data from both multi-ad group and legacy campaigns.

>[NOTE] If you don't use this filter, your reports **will not** contain data related to multi-ad group campaigns.

For [Ads reports](guides/reporting/v2/report-types#ad-reports), multi-ad group campaigns will support the `adId` metric so you can track the performance of your ads. For legacy campaigns, you will not receive `adId` in the ad report.

### Is there any impact to targeting and recommendation APIs?

There is no change to the targeting or recommendation endpoints. The existing endpoints are compatible with all types of campaigns. There is no moderation for keywords in new campaigns. The keywords become active as soon as they are added to ad groups.

### Are there any changes to ad creative moderation?

For multi ad group campaigns, only ads get moderated for the ad policy. If your ad gets rejected due to an ad policy violation, then your campaign status remains unchanged. In legacy campaigns, the creatives are linked to campaigns so if the creative gets rejected due to ad policy your campaigns status will change to rejected. You can use the [moderation API](moderation) to check the moderation status of your ads. 

>[NOTE] The new campaigns can continue to deliver impressions if there is one active ad.

### What ad formats are supported?

The new APIs support ad format [Product Collection](sponsored-brands/3-0/openapi/prod#/Ads/CreateSponsoredBrandsProductCollectionAds), [Store Spotlight](openapi/prod#tag/Ads/operation/CreateSponsoredBrandStoreSpotlightAds), [Video](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsVideoAds) and [Brand Video](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsVideoAds). You can take advantage of our new ad format brand video to link your branded video to store pages. 

>[WARNING] You cannot create, update, or get brand video ads using existing version 3 APIs.

### Are multi-ad group campaigns included in [snapshots](reference/sponsored-brands/2/snapshots)?

No, campaigns created using the version 4 endpoints will not be included in snapshots. Use [Exports](exports) to return version 4 campaigns structure instead.

### Is there any change to creatives in version 4?

Yes, you need to use the [Asset library API](creative-asset-library) upload both video and image creatives and generate an `assetId`. You can then use the `assetId` when creating ads. 

### Do ads created as part of version 3 campaigns have an adId?

No. Any ads created as part of a [version 3 campaign](sponsored-brands/3-0/openapi#tag/Campaigns) will not have an associated `adId` or `name`. To learn more, see [Managing ads](guides/sponsored-brands/campaigns/managing-multi-ad-group-campaigns#ads).

### Why are some of my creatives missing fields in [POST sb/v4/ads/list](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/ListSponsoredBrandsAds) and [GET sb/campaigns](sponsored-brands/3-0/openapi#tag/Campaigns/operation/listCampaigns)?

Some advertisers have experienced a known issue where older creatives are missing creative-level fields. To ensure you receive all creative-level fields, we suggest uploading a more recent version of the creative asset.

### Why is my campaign `state` showing as `ENABLED` in version 4 for legacy campaigns?

The campaigns `state` for legacy campaigns differs in the response from version 3 and version 4 GET campaigns endpoints. The campaign state in version 3 includes a combination of action taken by the user and campaign serving status. In the version 4 GET response, we have standardized state management for legacy and multi-ad group campaigns where campaign `state` field represents user action, and campaign `servingStatus` in the extended data fields will help you identify the serving state of the campaign. This approach brings similarity with Sponsored Products and Sponsored Display campaign state management. If you are looking to maintain similar state response for legacy campaigns, we recommend that you start using a combination of campaign state and campaign service status.

## Learn more

- [Campaigns version 4 API reference](sponsored-brands/3-0/openapi/prod#tag/Campaigns)
- [Ad groups version 4 API reference](sponsored-brands/3-0/openapi/prod#tag/AdGroups)
- [Ads version 4 API reference](sponsored-brands/3-0/openapi/prod#tag/Ads)
- [Creating campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns)
- [Managing multi-ad group campaigns](guides/sponsored-brands/campaigns/managing-multi-ad-group-campaigns)

## June 2023 update

## Overview

Sponsored Brands is changing the backend data storage for [legacy version 3 APIs](sponsored-brands/3-0/openapi#tag/Campaigns) for consistency across all sponsored ads programs. Sponsored Brands version 3 campaign management APIs now create new campaigns and drafts using version 4 multi-ad group [campaign structure](guides/sponsored-brands/campaigns/structure). There is no change to the API contracts or request/response objects. All version 3 campaign management APIs will continue to work as expected without any changes. 

### Who is affected?

Advertisers, agencies, and tool providers currently using Sponsored Brands (SB) legacy version 3 APIs to create campaigns or drafts. This backend change was completed in the US marketplace on May 22, 2023, followed by all other supported marketplaces on August 10, 2023.

### What is the impact?

|API	|Description	|
|---	|---	|
|[`POST /sb/campaigns`](sponsored-brands/3-0/openapi#tag/Campaigns/operation/createCampaigns)	|Any new campaigns created using this endpoint will create campaigns with one ad groups and one ad. The name of the ad group and ad will be the same as the name of campaign. **WARNING:** If multiple ads or ad groups are added to the campaign, the version 3 GET /sb/campaigns endpoint will return the creative/adFormat of only the first ad from the campaign.	|
|[`POST /sb/drafts/campaigns`](sponsored-brands/3-0/openapi#tag/Drafts/operation/createDraftCampaigns)	|New drafts will be saved as a version 4 campaign in the backend.	|
|[`POST /sb/drafts/campaigns/submit`](sponsored-brands/3-0/openapi#tag/Drafts/operation/submitDraftCampaign)	|If you have an existing draft or brand new draft, upon submission, the campaign will be converted into a multi-ad group campaign.	|

### Why are we making this change?

All Amazon sponsored ads programs are moving towards a common domain model that uses an industry-standard ad groups data structure. With this change, advertisers get a standard experience for new campaigns across legacy version 3 and version 4 APIs. The new campaigns created after 05/23 will have [ad groups campaign structure](guides/sponsored-brands/campaigns/structure) and support campaign management through both version 3 and [version 4](sponsored-brands/3-0/openapi/prod#tag/Campaigns).

### Will there be any impact to my Amazon advertising console experience?

The advertising console will show campaigns in the [ multiple ad groups](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-brands-ad-groups/?ref_=a20m_us_wn_gw) format going forward. You can drill down further into the hierarchy across campaign, ad groups, and ads. These workflows and information cards have been available in the advertising console since October 2022. If you are already using the advertising console to create campaigns, you will now see consistent behavior for campaigns created using the version 3 APIs. 

A campaign details page for legacy campaigns is shown in the screenshot below. On the screen, there are no ad group features.

![Console for legacy campaigns](/_images/sponsored-brands/console1.png)

The image below depicts an example of a campaign detail page for a multi-ad group campaign. On-screen ad group features are emphasized 

![Console for new campaigns](/_images/sponsored-brands/console2.PNG)

### What if I create additional ad groups or ads in the Amazon advertising console? 

If you create additional ad groups or ads in a campaign created using version 3 APIs, you will see only the first ad group. If there are other ad groups or ads in the campaign, they will not be visible in version 3 APIs. Legacy Sponsored Brands version 3 APIs will not get any new features. We encourage our partners to migrate to Sponsored Brand version 4 APIs to take advantage of new features and create campaigns with multiple ad groups.

### What changes are required to retrieve reporting data using reporting API?

There will be no impact to reporting. The performance data for all campaigns created via the advertising console, bulksheets, legacy version 3 APIs, and version 4 APIs will be included in the [version 2 reporting APIs](sponsored-brands/3-0/openapi#tag/Reports). 

### Are there any changes to ad creative moderation?

In legacy campaigns, the creatives were linked to campaigns, so if the creative was rejected due to ad policy, then your entire campaign status was changed to rejected. In the new ad groups based campaigns, individual ads are moderated for ad policy adherence. Your campaign status will remain unchanged even if one of the ads in the ad groups hierarchy is rejected. You can use the [moderation API](moderation) to check the moderation status of your ads. 

### Need help?

There should be no changes in response other than seeing improvement in response time. Please open a [developer support ticket](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2) if you see any issues in response. 

