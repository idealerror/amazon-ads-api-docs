---
title: Amazon Ads API for Sponsored Display overview 
description: An overview of the Amazon Ads API for Sponsored Display.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - overview
    - campaign management
---


# Amazon Ads API for Sponsored Display overview

>[TIP: New: Sponsored Display for advertisers that don't sell on Amazon] Non-Amazon sellers can now create campaigns to direct customers to an off-Amazon landing page that advertises their goods or services. [Get started](guides/sponsored-display/non-amazon-sellers/get-started). 

Sponsored Display is a self-service, retail aware display marketing solution that helps you grow your business and brand on Amazon by engaging shoppers across the purchase journey on and off Amazon.

The Amazon Ads API for Sponsored Display enables integrators to programmatically access Sponsored Display (SD) to build applications to automate, scale, and optimize SD campaigns. The API is described using an [OpenAPI specification](https://swagger.io/specification/), an industry standard for describing API services. The OpenAPI document for Sponsored Display is published to the [developer portal](sponsored-display/3-0/openapi) and can also be [downloaded](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/3-0/openapi.yaml) for local use. 

## Campaign management

The Sponsored Display API is based on the HTTP protocol, with GET, POST, PUT, and DELETE operations. The API aligns with RESTful design principals to align HTTP methods with corresponding actions. For example, the HTTP GET operation retrieves resources, which can potentially be cached for performance benefits. The HTTP POST operation typically creates resources, while the PUT operation updates resources. The HTTP DELETE operation archives resources. 

The API provides resource representations using a consistent URI hierarchy. For example, the top-level URI is `/sd/campaigns`. To work with individual resources, the API includes a path parameter to specify an identifier. For example, in the GET `/sd/campaigns/{campaignId}` URI,  `{campaignId}` is the identifier of a single campaign that can be retrieved, deleted, or modified. 

For efficiency, most resources include a minimal set of fields and associated values. However, if you require the full set of fields and values, include the `/extended` path parameter at the end of the URI. For example, GET `/sd/campaigns/extended` or GET `/sd/campaigns/extended/{campaignId}` include the `servingStatus`, `creationDate`, and `lastUpdatedDate` fields, while GET `/sd/campaigns` or GET `/sd/campaigns/{campaignId}` does not.

Sponsored Display campaigns are created by generating and associating child resources. There is an order in which resources must be created:

![Sponsored display call flow](/_images/sponsored-display/call-flow.jpg)

* **Campaign** is the top-level resource used to define your campaign. It requires a unique campaign identifier and name to differentiate it from other campaigns. This resource includes fields such as budget, campaign start date, campaign end date, and the like.
* **Ad group** provides a logical grouping of common product ads with associated bid amounts. Each ad group is associated to a single campaign.
* **Product ad** is a specific advertisement run as part of your campaign. Each product ad is associated with a single ad group.
* **Targeting clauses** are targeting expressions used to target products, categories, or audiences. The target expression you specify is based on the type of campaign and applies at the ad group level.

>[NOTE] Advertisers that don't sell on Amazon can also make use of Sponsored Display locations, which allows you to target campaigns toward specific geographic regions. Locations are not available for advertisers that sell on Amazon. 

## Tactics

Sponsored Display [campaigns](sponsored-display/3-0/openapi#tag/Campaigns) are differentiated by **tactic**. A tactic defines the targeting strategy for the campaign. 

Sponsored Display campaigns focus on two campaigns types: 

1. Contextual targeting.
2. Audiences.

These campaign types are available to both **seller brand owners** and **vendors**. Note that advertisers that don't sell on Amazon can only use Audience targeting. 

The following targeting strategies are supported:

| Targeting strategy | Identifier in the API | Advertiser type |
|--------------------|-----------------|--------|
| Contextual targeting | `T00020` | Seller, vendor | 
| Audiences | `T00030`  | Seller, vendor, advertisers that don't sell on Amazon. | 

## Bid optimization

Sponsored Display's [bid optimization](https://advertising.amazon.com/help?entityId=ENTITY1X6FK5RDHNB96#GDRFG6U7TRD72EFW) strategies allow you to specify the metrics against which Sponsored Display should optimize when bidding for impressions. We’ll display ads to different audiences depending on the metric that you want to optimize. Within the Sponsored Display [campaign](sponsored-display/3-0/openapi#tag/Campaigns/operation/createCampaigns), you can choose different `costType` of either "vcpm" or "cpc". Within an [adGroup](sponsored-display/3-0/openapi#tag/Ad-groups/operation/createAdGroups), you can then choose the `bidOptimization` for the ad group. Default behavior is to optimize for clicks.

| [IMAGE] | |
| --- | --- |
| Regardless of your bid optimization selection, if we find an opportunity where your ad is more likely to lead to viewable impressions, page visits, or conversions based on the bid optimization you selected, we may increase your bid for that auction by up to 300%.<br><br>Similarly, if we find an opportunity that looks less likely to lead to your bid optimization goal, we might lower your bid for that auction. For example, when bidding for ad placements, Amazon can currently adjust a bid of $1 up to a maximum of $4 when there are opportunities for higher expected result. | ![Bid optimization strategies](/_images/sponsored-display/bid-optimization.png) |

There are 3 bid optimization strategies for Sponsored Display campaigns:

| bidOptimization | costType | Description |
|--------------------|-----------------|--------|--------|
| clicks | `cpc` | [Default] Optimize for page visits and reporting |
| conversions | `cpc`  | Optimize for conversion |
| reach | `vcpm`  | Optimize for viewable impressions. We recommend starting with $5 USD bids for each target to begin testing with this awareness strategy. $1 is the minimum bid for vCPM. |

## Security

The Amazon Ads APIs uses the Amazon Identity Provider to authenticate and authorize requests. This security scheme is consistent across all Amazon Ads products.

The Amazon Ad API requires three mandatory headers for each request:

* **Amazon-Advertising-API-ClientId**
* **Amazon-Advertising-API-Scope**
* **Authorization**

The `Amazon-Advertising-API-ClientId` header contains the client identifer that you registered with the Amazon Ads API platform. It represents a unique identity for your application. This value should not be shared with anyone else since it represents entitlement to your Amazon Ads API.

The `Amazon-Advertising-API-Scope` header contains the profile Id of your advertiser. You can retrieve this Id using the [profiles resource](reference/2/profiles#tag/Profiles).

The `Authorization` header is an industry-standard header that includes an OAuth access token. It follows the format: `Authorization: Bearer <access_token>`.

To learn more about creating an authorization token, see [creating API authorization and refresh tokens](guides/get-started/retrieve-access-token).

**Next steps**

If you've already [set up your account](guides/onboarding/overview), you're ready to learn [create Sponsored Display campaigns using Postman](guides/sponsored-display/tutorials/postman).

To learn more about all the features that are available by marketplace, see [features](guides/sponsored-display/features).