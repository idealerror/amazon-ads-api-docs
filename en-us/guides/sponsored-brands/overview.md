---
title: Sponsored Brands overview
description: An overview of Sponsored Brands, with links to information about campaign management and reporting.
type: guide
interface: api
tags: 
    - Sponsored Brands
keywords: 
    - Overview
---

# Sponsored Brands overview

Sponsored Brands gives advertisers the ability to use different ad formats to showcase a brand. Sponsored Brands is available to vendors and sellers. Depending on your campaign objectives and ad format, you can choose between product collection, store spotlight, or video.

[Learn more about how Sponsored Brands can help you meet your business objectives.](https://advertising.amazon.com/help?#GGWFYHL27MFXLHS6)

## Campaign management

>[TIP:New campaign management APIs]You can now manage campaigns across ad products using a common model and a single set of API endpoints. To get started with campaign management in the Amazon Ads API v1, see the [campaign management overview](guides/campaign-management/overview).

To learn more about using the product-specific Sponsored Brands API to create and manage Sponsored Brands campaigns, visit the pages below:

* [Campaign structure](guides/sponsored-brands/campaigns/structure)
* [Creating a campaign](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-1-create-a-campaign)
* [Full API reference](sponsored-brands/3-0/openapi/prod)
* [Postman collection](https://github.com/amzn/ads-advanced-tools-docs)

## Reporting

The Ads API reporting functionality provides a variety of reports to help you retrieve historical impression, click, cost, and conversion data for your Sponsored Brands campaigns.

Learn more: 

* [Report types](guides/reporting/v2/report-types)
* [Getting started with reports](guides/reporting/v2/sponsored-ads-reports)

## Campaign status mapping between Sponsored Brands v4 API and advertising console

This table provides mapping of the sponsored brand v4 campaign creation `servingStatus` and how it correlates with the campaign `status` in the advertising console.

>[NOTE] The `servingStatus` can only be retrieved via the [POST /sb/v4/campaigns/list](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns) endpoint. It won't be available in the response during updating/creating a campaign.


| SBv4 serving status                              | Description                                                               | Advertising console status |
|--------------------------------------------------|---------------------------------------------------------------------------|-------------------|
| ADVERTISER\_STATUS\_ENABLED                        | The advertiser's status is enabled.                                        | enabled           |
| ADVERTISER\_POLICING\_PENDING\_REVIEW               | The advertiser is pending review because of a policing reason.             | paused            |
| ADVERTISER\_POLICING\_SUSPENDED                    | The advertiser's status is suspended because of a policing reason.         | paused            |
| ADVERTISER_PAUSED                                | The advertiser's status is paused.                                         | paused            |
| ADVERTISER_ARCHIVED                              | The advertiser's status is archived.                                       | archived          |
| ADVERTISER\_PAYMENT\_FAILURE                       | The advertiser's internal status is suspended.                             | paused            |
| ADVERTISER\_ACCOUNT\_OUT\_OF\_BUDGET                 | The advertiser is out of budget for all Sponsored Ads campaigns.           | paused            |
| ADVERTISER\_OUT\_OF\_PREPAY\_BALANCE                 | The advertiser is out of prepay balance for all Sponsored Ads campaigns.   | paused            |
| ADVERTISER\_EXCEED\_SPEND\_LIMIT                   | The advertiser spends over the daily limit.                                | paused            |
| CAMPAIGN\_STATUS\_ENABLED                          | The campaign's status is enabled.                                          | enabled           |
| CAMPAIGN_PAUSED                                  | The campaign's status is paused.                                           | paused            |
| CAMPAIGN_ARCHIVED                                | The campaign's status is archived.                                         | archived          |
| CAMPAIGN_INCOMPLETE                              | The campaign does not contain any ads or targeting clauses.                | paused            |
| CAMPAIGN\_OUT\_OF\_BUDGET                           | The campaign is out of budget.                                             | paused            |
| PORTFOLIO\_STATUS\_ENABLED                         | The portfolio's status is enabled.                                         | paused            |
| PORTFOLIO_PAUSED                                 | The portfolio's status is paused.                                          | paused            |
| PORTFOLIO_ARCHIVED                               | The portfolio's status is archived.                                        | archived          |
| PORTFOLIO\_OUT\_OF\_BUDGET                          | The portfolio is out of budget.                                            | archived          |
| PORTFOLIO\_PENDING\_START\_DATE                     | The portfolio's start date is in the future.                               | paused            |
| PORTFOLIO_ENDED                                  | The portfolio's end date is in the past.                                   | paused            |
| INELIGIBLE                                       | The ad offer is ineligible.                                                | paused            |
| ELIGIBLE                                         | The ad offer is eligible.                                                  | enabled           |
| ENDED                                            | The campaign's end date is in the past.                                    | paused            |
| PENDING_REVIEW                                   | The campaign is pending review.                                            | paused            |
| PENDING\_START\_DATE                               | The campaign's start date is in the future.                                | paused            |
| REJECTED                                         | The campaign is rejected by the moderation process.                        | paused            |
| UNKNOWN                                          | The serving status is unknown. Please contact us for support.              | paused            |
