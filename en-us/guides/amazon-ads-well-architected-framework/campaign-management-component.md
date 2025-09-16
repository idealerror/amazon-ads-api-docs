---
title: campaign management component
description: string
type: guide # (one of: guide)
interface: general # (one of: api, bulk-operations, amazon-marketing-stream)
---
## Campaign management component

Campaign management component focuses on design principles and best practices for building Campaign Management (CM) solutions using Amazon Ads API. The scope of is component is limited to Sponsored Products, Sponsored Brands, Sponsored Display and Amazon DSP ad products, which allow you to create campaigns tailored to specific campaign objectives.

### Definitions

The definitions presented here are tailored to the context of Amazon's advertising platforms and services, and may have a narrower scope compared to more general definitions of these concepts.

* **Campaigns**: A top-level container for advertising activities, including ad groups, targeting settings, budget allocation, and campaign goals.
* **Ad group**: A campaign entity that contains specific targeting criteria, keywords, ads, and ad types. Ad groups provide a logical grouping to organize, manage, and track performance metrics within your campaign. Each ad group is associated to a single campaign and can have settings for bids, budgets, creatives.
* **Keyword:** A word or phrase used for targeting and bidding in an ad group. Keywords can have different match types like broad, phrase, and exact. Negative keywords can also be added to exclude certain search terms. Keywords are also referred as targets**.**
* **Audience:** An audience refers to a specific group of potential shoppers that advertisers aim to reach with their ads. Audiences can be segmented based on various characteristics, including demographics, interests, shopping behaviors, and purchasing patterns.
* **Target:** A target refers to the specific audiences, keywords or products an advertiser wants to reach with their ads. Targeting can include or exclude these conditions.
* **Budget**: An ad budget refers to the amount of money allocated to an ad campaign.
* **Bid**: The amount an advertiser is willing to pay for a click or impression.
* **Cost Per Click (CPC):** CPC determines how much advertisers pay for the ads they place on websites or social media, based on the number of clicks the ad receives.
*  **Cost Per Mille (CPM):** CPM also called cost per thousand impressions; refers to the total ad spend for every 1,000 impressions an ad receives.
* **Ad products**: Amazon ads products include Sponsored Brands, Sponsored Products, Sponsored Display, Sponsored TV, and Amazon DSP.  Different ad types have specific creative requirements and specifications for assets like images, videos, and logos.
* **Creative assets**: The visual elements used in an ad, such as logos, images, and videos. These assets must adhere to specific creative specifications for each ad type.
* **Personas**: Personas are detailed profiles of an advertiser’s ideal customers. Advertisers use these personas to create targeted campaigns that speak directly to the needs and behaviors of their target.
* **Third party (3P) Amazon DSP deals**: These are deals that advertisers have negotiated and executed in Amazon DSP (ADSP) so you can use those deals to place ads through Amazon DSP platform.
* **Historic reach curve**: The [historic reach API](guides/media-planning/historic-reach/overview) provides visibility into the historic reach of Amazon’s supply sources for media planning purposes. It allows you to pass input parameters, such as supply package, audience, demographic and date range and return back a historic reach curve that matches the input criteria.
* **Forecasting reach curve**: The [reach forecasting API](guides/media-planning/reach-forecasting/overview) allows integrators to generate reach forecasts against inventory that is available to buy, while taking into account brand and campaign parameters.
* **First party (1P) signals:** 1P signals are information directly collected and owned by an advertiser about their customers.

### Campaign management design principles

* **Create campaign components after verifying existence of dependencies:** Campaigns follow a hierarchical structure: campaigns contain ad groups, which contain ads. To create a successful campaign, start by creating the campaign first, then add ad groups to the campaign, followed by creating ads within each ad group. For manual campaigns, add targeting options to each ad group. Always verify each component's successful creation before adding subsequent components.

### Campaign management  best practices

* **Use pre-moderation to increase** **moderation approval rate**: The [unified pre-moderation API](pre-moderation#tag/Pre-Moderation/operation/preModeration) offers real-time policy guidance based on the technical, creative, and policy components of an ad. Use it before moderation to reduce punctuation, grammar, spelling errors, capitalization, and spacing errors that may cause your ads to get rejected.
* **Adhere to marketplace bid thresholds:** Implement validation checks to ensure campaign bids align with marketplace-specific thresholds before API submission. For current marketplace limits, refer [here](reference/concepts/limits#bid-constraints-by-marketplace).
* **Track campaign component states and dependencies:** Track campaign component states and dependencies during creation through distinct creation states (created, pending, failed) and operational states (enabled, archived, paused). This tracking enables automatic retry of failed operations, process resumption, and sequential component advancement.
* **Export campaigns metadata:** While reports contain performance data, [exports](exports) provide metadata about your campaigns at the time of the request. Use exports instead of list operations to retrieve metadata in bulk for all configured campaigns, ad groups, targeting expressions, and more. Access metadata infrequently as campaign configuration data generally remains stable.
* **Use list operations to retrieve entities in bulk:** Utilize batch endpoints (for instance [list Sponsored Products campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredProductsCampaigns), [list Sponsored Brands ad groups](sponsored-brands/3-0/openapi/prod#tag/Ad-groups/operation/ListSponsoredBrandsAdGroups), etc) when fetching multiple entities instead of retrieving one entity individually.
* **Implement pagination handling:** Build an abstraction layer that handles pagination automatically for list operations. Most endpoints use nextToken and maxResults for these.

### Implementation guidance

* **Use target exclusion list for brand safety**: Amazon Ads has global brand safety policies that takes a variety of measures to prevent your ads from appearing on webpages and apps containing sensitive content, products, and services. Use [brand safety list](sponsored-display/3-0/openapi#tag/Brand-Safety-List) for Sponsored Display and [domain targeting](dsp-campaigns#tag/Domain-Targeting/operation/getDomainTargeting) for Amazon DSP to support brand safety use cases.
* **Select appropriate mechanisms for external audiences:** Amazon Ads products like Sponsored Display and Amazon DSP offer prebuilt audience segments called [Amazon Audiences](https://advertising.amazon.co.uk/help/GXTPQ3LLAFJZ8SYS) to help advertisers reach prospective shoppers, both on and off Amazon. Generate audience insights using [persona builder API](persona-builder) and reach curve API during the audience planning phase for Amazon DSP and Sponsored Display.

  Amazon Marketing Cloud further lets you define [rule-based audiences](guides/amazon-marketing-cloud/audiences/rule-based-audiences) through which you can augment Sponsored Display and Amazon DSP audience targeting, and also adjust bids for such audiences on Sponsored Brands and Sponsored Products. Amazon DSP also allows you to bring external shopper profiles for audience targeting. These external profiles can be grouped into the two categories defined below.
    * **Advertiser 1P audiences:** Amazon Ads allows the capability to use an advertiser’s own set of shopper profiles for audience targeting. These profiles represent a direct relationship that an advertiser has with their shoppers. You can ingest advertiser owned audiences using [AMC Advertiser Audiences](guides/amazon-marketing-cloud/audiences/audience-management-service) or [AMC Advertiser Data Upload](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview).
        * Use AMC Advertiser Audiences when ingesting the shopper profiles directly into an advertiser specific AMC instance.
        * Use AMC Advertiser Data Upload when uploading any form of 1P signals into an advertiser specific AMC instance, including shopper profiles.
    * **3P audiences:** Amazon DSP also provides access to third-party audiences that complement Amazon audiences and your 1P audiences. Partners who act as data providers and sell their third-party audiences should use [Data Provider (DP) API](guides/dsp/data-provider) to create and distribute 3P audiences. You can upload shopper profiles once and associate it with multiple audiences and advertisers.

