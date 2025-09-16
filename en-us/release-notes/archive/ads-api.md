---
title: Amazon Ads API release notes
description: Amazon Ads API release notes, with information about new features and other changes to the API.
type: release-note
interface: api
keywords:
    - Slack
    - RSS feed
    - updates
---

# Archived release notes for the Amazon Ads API

This document contains release notes 

### December 2022

#### Sponsored Display video creative now supports contextual targeting to better showcase products and brands

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SE, UK, US

We are expanding Sponsored Display's creative offering with video capabilities for campaigns using contextual targeting. Video creatives empower advertisers to showcase their products and brand through immersive storytelling (e.g., tutorials, demos, unboxing, and testimonials). Advertisers using Sponsored Display contextual targeting and audiences will be able to build awareness and consideration of their products and brand on and off Amazon using a video ad format.

Similar to campaigns using the audiences strategy, advertisers using contextual targeting can now choose to customize creatives using a headline and logo, image, or video through Sponsored Display’s self-service display solution that helps grow their business with Amazon. The new video creative capability supports videos up to 45 seconds so advertisers can better tell their stories to customers, helping them understand the product and brand with more engaging content.

If you have insights on creative that helps drive traffic or engagement, you can use that creative for your Amazon Ads campaigns. Ads will link directly to your product detail pages, so audiences can learn more about your product and consider purchasing.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Ad%20groups/createAdGroups), including contextual targeting support for `creativeType` of `VIDEO`.

#### Sponsored Brands launches product targeting API

Regions: CA, DE, ES, FR, IN, IT, JP, MX, UK, US

Sponsored Brands is launching four new API endpoints to enable advertisers to access product targeting information:

* [GET /sb/targets/categories](sponsored-brands/3-0/openapi/prod#/Product%20Targeting/SBTargetingGetTargetableCategories)
    * Gets a list of all available targetable categories.
* [GET /sb/targets/categories/{categoryRefinementId}/refinements](sponsored-brands/3-0/openapi/prod#/Product%20Targeting/SBTargetingGetRefinementsForCategory)
    * Gets all available refinements for a specific category.
* [POST /sb/targets/products/count](sponsored-brands/3-0/openapi/prod#/Product%20Targeting/SBTargetingGetTargetableASINCounts)
    * Gets the number of targetable products based on the provided category refinements.
* [GET /sb/negativeTargets/brands/recommendations](sponsored-brands/3-0/openapi/prod#/Product%20Targeting/SBTargetingGetNegativeBrands)
    * Gets recommendations for brands to exclude from product targeting.

Though these features were previously available in the advertising console, advertisers can now access them programmatically through the Amazon Ads API. This helps bring the advertiser API experience more in line with the advertising console, and gives advertisers greater flexibility when creating campaigns. 

#### Sponsored Display launches view metrics for video creative

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SE, UK, US

We launched view metrics for reach-optimized Sponsored Display campaigns with video creatives. The new metrics will provide advertisers with a new set of key metrics for measuring and optimizing video campaign performance. The new metrics include: view-through rate (VTR), video first quartile, video midpoint, video third quartile, video complete, and video unmutes, and click-through rate for views (vCTR). View metrics are available in the advertising console, report center, or through the Amazon Ads API.

Video is an important format that advertisers use to tell their brand and product story. When advertisers deliver video creatives, they want to understand how many customers viewed their ad and how they engaged with the video. With this launch, advertisers can now measure the performance of their campaigns across video viewing engagement, providing transparency into the additional value delivered by Sponsored Display video creative. For example, advertisers can use video completion metrics to measure how many customers completed their video ads and better understand what type of content resonates best with customers. Over time, these insights can help advertisers iterate on video quality and optimize for video viewing to help increase customer engagement and improve outcomes of Sponsored Display video creative ad groups.

For full technical details, please see our [reporting documentation](guides/reporting/v2/report-types), including the updated documentation on the new video creative metrics that include `vtr`, `videoFirstQuartileViews`, `videoMidpointViews`, `videoThirdQuartileViews`, `videoCompleteViews`, `videoUnmutes`, and `vctr`. 

#### Sponsored Display is now available for advertisers in Egypt, Saudi Arabia, Turkey, and Poland

Regions: EG, SA, TR, PL

[Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display) contextual targeting and audiences are now available to advertisers in Egypt, Saudi Arabia, Turkey, and Poland.

Vendors and registered sellers in these countries now have access to Sponsored Display, helping them reach relevant audiences throughout their shopping and entertainment journeys with self-service, programmatic display ads that can be created in just a few clicks. Sponsored Display [audiences](https://advertising.amazon.com/library/guides/sponsored-display-audiences) allows you to use Amazon’s rich shopping and streaming signals to help grow your business. Advertisers can use contextual targeting to help generate detail page traffic and then lean into audience strategies to reengage audiences to help secure missed sales opportunities or further cultivate brand loyalty.

Advertisers can add up to 10,000 products to a campaign and benefit from retail awareness so that marketing campaigns will pause if products go out of stock. Advertisers using Sponsored Display can also customize their creatives choosing any combination of video, headline, logo, or lifestyle image to help audiences discover their brands and products.

#### Creative editing API is now available for Sponsored Brands video ad creatives

Regions: All

Advertisers can now update the ad creatives of their live Sponsored Brands video campaigns that are created using ad groups (Sponsored Brands version 4). Ad group is a feature available through the advertising console and Amazon Ads API that enables advertisers to create a new campaign with multiple ad groups to share targeting across different ads formats. For new campaigns that link to a product detail page, advertisers will have the ability to change the videos. For new campaigns that link to a brand Store, advertisers can edit logo, video, headline, and product using this API. This feature allows advertisers to edit a live campaign without needing to create a new campaign that leads to loss of historical information collected on the live campaign. 

Advertisers will also have the option to continue delivering the campaign with the previous creative version while the edited version is reviewed by moderation.

For full technical details, see the [API reference](sponsored-brands/3-0/openapi/prod#/Ad%20Creatives) and the [Managing creatives](guides/sponsored-brands/campaigns/managing-multi-ad-group-campaigns#creatives) guide.

#### Partner Opportunities API v1.1 now available

Regions: All

We’ve released an updated version of the Partner Opportunities, v1.1.  The new version contains new features including:

* *Pagination* of Partner Opportunities list.
* *Metrics Availability:* An API response to indicate Partner Opportunity metric availability and last updated date.
* *Summary of opportunities:* A new API response that will return summarized opportunity information, such as the number of accounts included in available opportunities.

For assistance migrating from v1.0 to v1.1, [please see our documentation](https://advertising.amazon.com/API/docs/en-us/partner-opportunities/#/Partner%20Opportunities).

As a member of the Amazon Ads Partner Network, you can access Partner Opportunities to receive actionable insights for all your linked clients through your Partner Network account. This program curates custom insights for all of the campaigns you manage within your entire book of business.  Partner Opportunities can assist your business by: simplifying your API call process, saving time on managed advertising activities, and can allow you to better support your clients campaigns.  The types of opportunities available are dynamic and generated based on recent information.

1. [Introduction to the Partner Opportunities API](https://advertising.amazon.com/partner-network/resources/10294270-6c73-4cf1-9080-a0b120915505)
2. [Getting started with the Partner Opportunities API](https://advertising.amazon.com/partner-network/resources/99a59988-5042-4400-867e-1dcace45f59e)
3. [FAQs for the Partner Opportunities API](https://advertising.amazon.com/partner-network/resources/a5b8441d-a09e-42f2-9d19-0a0dbc8719b5)

#### Sponsored Display now available in Amazon Marketing Stream (beta) in North America

Amazon Marketing Stream (beta) has now expanded to include Sponsored Display in North America, allowing partners and advertisers to receive performance metrics and information on campaign changes in near real time. Information is pushed to advertisers and partners through the Amazon Ads API, eliminating the need to aggregate metrics over time to understand the changes between hourly performance, and enabling timely notifications on campaign changes. 

Amazon Marketing Stream can help you improve Sponsored Display performance with traffic and performance metrics summarized hourly instead of daily, informing intra-day optimizations. Advertisers and partners will also receive timely notifications on campaign changes that can help you take ** actions in near real time, such as a campaign going out of budget.  

New metrics available through this Sponsored Display launch include new-to-brand metrics and view-based attribution for vCPM (cost-per-thousand viewable impressions) campaigns, summarized hourly. Amazon Marketing Stream can help you understand Sponsored Display campaign KPIs such as impressions, clicks, attributable sales, orders, and new-to-brand sales, broken down by hour of day. For example, you may learn that new-to-brand attributed sales, units and orders are higher during hours 8pm-10pm, informing hourly bid adjustments. You can also determine when views or clicks (vCPM) lead to more conversions, by hour of day.

When you’re ready to get started, visit the Amazon Marketing Stream [onboarding guide](guides/amazon-marketing-stream/onboarding).

### November 2022

#### Awareness: Version 2 of Sponsored Products bid recommendations resource to be deprecated on March 27, 2024

**Note**: This deprecation was [originally announced](release-notes/archive/ads-api#awareness-version-2-of-sponsored-products-bid-recommendations-resource-to-be-deprecated-on-december-31-2022) for December 31, 2022 and was pushed back based on customer feedback. For the latest information, see our [Deprecations page](release-notes/deprecations)

The following Sponsored Products bid recommendations resources will be deprecated on March 27, 2024:

* /v2/sp/adGroups/{adGroupId}/bidRecommendations
* /v2/sp/keywords/{keywordId}/bidRecommendations
* /v2/sp/keywords/bidRecommendations

We recommend that advertisers located in CA, DE, ES, FR, IN, JP, US, or UK migrate to the [new theme-based bid suggestion resource](sponsored-products/3-0/openapi/prod#/ThemeBasedBidRecommendation/GetThemeBasedBidRecommendationForAdGroup_v1) to retrieve bid suggestions. This new resource includes historical impact metrics based on similar products and helps in choosing the right bid for your campaigns.  For full technical details on this new resource, please see the [theme-based bid suggestions documentation](sponsored-products/3-0/openapi/prod#/ThemeBasedBidRecommendation/GetThemeBasedBidRecommendationForAdGroup_v1) or visit the [quick-start guide](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide). Note that for countries in which the theme-based bid suggestion resource is not yet available, we recommend that advertisers use the [/v2/sp/targets/bidRecommendations](sponsored-products/2-0/openapi#/Bid%20recommendations/getBidRecommendations) resource instead.

#### Sponsored Display forecasting for impressions is now available

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, US

Sponsored Display has launched a new API in the Amazon Ads API to help forecast impressions. Impression forecasting is available for Sponsored Display campaigns with contextual targeting and Amazon audiences.

Forecasting helps advertisers understand the impact of their choices during campaign creation. Now advertisers can get the minimum and maximum range of available impressions inventory for a set of selected advertised products, optimization type, targeting, and bid values. This will help advertisers in creating performing campaigns, understanding scale by experimenting with the campaign setup, and forecasting the output of an investment before they get started.

Forecasting will be available for Sponsored Display through the Amazon Ads API. For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Forecasts).

#### Sponsored Brands video support in advertiser test accounts

Regions: All (excluding IN)

[Advertiser test accounts](guides/account-management/test-accounts/overview) will now have the support for Sponsored Brands video campaigns. Users will now be able to create Sponsored Brands video campaigns in a test account. Test accounts do not serve ads or have active billing, allowing integrators to make API requests to test and learn about API functionality in a safe manner. These campaigns will not be served to shoppers and will not incur any advertising charges. For more information on using test accounts for Sponsored Brands video campaigns, please see the [testing Sponsored Brands video campaigns](guides/account-management/test-accounts/use-test-accounts#testing-sponsored-brands-video-campaigns). 

#### Awareness: Sponsored Products versions 1 and 2 APIs deprecation on June 30, 2023

We are announcing plans to deprecate Sponsored Products versions 1 and 2 campaign management APIs, with a target shutdown date of June 30, 2023. Please migrate to the new versions of these endpoints as documented on our [Deprecations page](release-notes/deprecations) and in our [Migration guide](reference/migration-guides/sp-v2-v3). 

**Note**: This deprecation does **not** include the [version 1](reference/1/snapshots) or [version 2](sponsored-products/2-0/openapi#/Snapshots) snapshot endpoints.

The following endpoints and all associated HTTP operations are included in this deprecation:

* Version 1
    * /campaigns/
    * /campaigns/extended/
    * /adGroups/
    * /adGroups/extended/
    * /keywords/
    * /keywords/extended/
    * /negativeKeywords/
    * /negativeKeywords/extended/
    * /campaignNegativeKeywords/
    * /campaignNegativeKeywords/extended/
    * /productAds/
    * /productAds/extended/
* Version 2
    * /v2/sp/campaigns/
    * /v2/sp/campaigns/extended/
    * /v2/sp/adGroups/
    * /v2/sp/adGroups/extended/
    * /v2/sp/keywords/
    * /v2/sp/keywords/extended/
    * /v2/sp/negativeKeywords/
    * /v2/sp/negativeKeywords/extended/
    * /v2/sp/campaignNegativeKeywords/
    * /v2/sp/campaignNegativeKeywords/extended/
    * /v2/sp/productAds/
    * /v2/sp/productAds/extended/
    * /v2/sp/targets/
    * /v2/sp/targets/extended/
    * /v2/sp/negativeTargets/
    * /v2/sp/negativeTargets/extended/
    * /v2/sp/campaignNegativeTargets/
    * /v2/sp/campaignNegativeTargets/extended/

If you would like to share feedback, please use the Amazon Ads API [Service desk](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2) to open a ticket with subject title "SP v2 CRUD Deprecation" by February 15, 2023.

#### Connect with Amazon Ads at AWS re:Invent

Amazon Ads is excited to be attending AWS re:Invent this year. We’ll be kicking off the week with a Lighting Talk on “Using your technical skills with Amazon Ads” on Tuesday 11/29 at 11:45 AM PST. You can stop by Expo booth #2040 to speak with Technical Account Managers and members of the Tech Learning and Developer Relations team, view short technical demonstrations, and pick up some swag. 

To view our full schedule of events, visit the Amazon Ads re:Invent [microsite](https://www.amazonadspartnerevents.com/reInvent2022), and if you haven’t yet [registered for re:Invent](https://reinvent.awsevents.com/register/?trk=www.google.com), tickets are still available.

We hope to see you in Las Vegas!

#### Amazon Brand Stores are now available as click-through destinations for Sponsored Brands video creatives (beta)

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SA, SG, UK, US

We are launching a new feature that lets advertisers use their video campaigns to redirect shoppers to their Brand Store landing pages. Prior to this launch, customers were redirected to product detail pages by default, and with this change advertisers can now choose which landing page customers are redirected to - either the Product Detail Page or the Brand Store. Stores are more discoverable to shoppers as they are the primary method for advertisers to showcase brands and products in a multi-page, immersive shopping experience on Amazon. Brand Stores allow brands to curate content that inspires, educates, and helps shoppers discover the brand’s product selection, taking them from inspiration to action. In addition, video ads with Stores as landing pages offer the opportunity to render in “Top of Search”, Amazon Ads' premium placement. Adding video in this placement will increase the reach that advertisers can achieve with Sponsored Brands video by raising the viewability rate to 100% in top slot.

**Note**: The ad group and ad creation operations are supported for new campaigns using the Amazon advertising console or through the Amazon Ads API using the [Sponsored Brands version 4 beta endpoints](sponsored-brands/3-0/openapi/prod#/Campaigns).

For full technical details, please see the [POST /sb/beta/ads/brandVideo endpoint](sponsored-brands/3-0/openapi/prod#/Ads) and [SB version 4 migration guide](reference/migration-guides/sb-v3-v4).

#### Creative asset library API is now available for videos

The creative asset library API now supports videos as well as images. All features supported for images are now available for video assets. 

To learn more about creative asset library, read the [original release note](release-notes/archive/ads-api#creative-asset-library-api-is-now-available-for-images-only).

For technical details, see the updated [API reference](creative-asset-library).

#### Sponsored Products version 3

Amazon Ads released Sponsored Products version 3 of campaign management create, read, update, and delete (CRUD) APIs. This includes the Sponsored Products campaign, ad group, targeting, negative targeting, keyword, negative keyword, campaign negative keywords, and product ads endpoints.

The Sponsored Products version 3 APIs include usability improvements like token-based pagination, enhanced errors, and response models. The new API is called by the Amazon Ads console, ensuring a consistent experience across the UI and the Amazon Ads API.

Key improvements:

* This version of the API is currently being called by the Amazon Ads console, ensuring feature parity between the UI and API.
* Error responses include additional information developers can use to troubleshoot issues.
* Consolidated extended and list endpoints. Developers can use a parameter in the request to request the extended response.
* Users can request the API returns full objects created or updated in POST and PUT requests versus just the IDs of the objects created or edited.
* Users can filter list operations based on name using broad or exact match.
* Improved latency compared to version 2.

The new endpoints are available to all advertisers and advertising partners on the Amazon Ads API. Please see our [migration guide](reference/migration-guides/sp-v2-v3) for information on how to move from version 2 to version 3, and also check out the updated [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman) for templates to call the version 3 endpoints.

### October 2022

#### Sponsored Display launches video creative capabilities to better showcase products and brands

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SE, UK, US

Sponsored Display has launched video creatives. The new video format empowers advertisers to showcase their products and brand through immersive storytelling (e.g. tutorials, demos, unboxing, and testimonials). Advertisers using [Sponsored Display audiences](https://advertising.amazon.com/library/guides/sponsored-display-audiences) will be able to build awareness and consideration of their products and brand on and off Amazon using a video ad format.

Advertisers can now choose to customize creatives using a headline and logo, image, or video through Sponsored Display’s self-service display solution that helps grow their business with Amazon. The new video creative capability supports videos up to 45 seconds so advertisers can better tell their stories to customers, helping them understand the product and brand with more engaging content. 

Video creatives can help tell your story while serving ads on and off Amazon in your own visual language, helping shoppers get excited about your brand and product experience. If you have insights on creative that helps drive traffic or engagement, you can use that creative for your Amazon Ads campaigns. Ads will link directly to your product detail pages, so audiences can learn more about your product and consider purchasing.

For full technical details, please see our [Sponsored Display documentation](sponsored-display/3-0/openapi#/Creatives) in the advanced tools center, including the updated documentation on video creatives and `creativeType` within [adGroups](sponsored-display/3-0/openapi#/Ad%20groups/createAdGroups) for the `T00030` tactic.

#### Amazon Marketing Stream (beta) now available globally (except IN)

Regions: All (except India)

We are excited to expand Amazon Marketing Stream (beta) to all countries currently supported by the Amazon Ads API, with the exception of India. Amazon Marketing Stream is a new service that delivers Amazon Ads campaign metrics and information to advertisers' or integrators' AWS accounts via a push-based model in near real time.

Learn more about Amazon Marketing Stream in the [original release note](release-notes/archive/ads-api#amazon-marketing-stream-beta) or get started using the [Onboarding guide](guides/amazon-marketing-stream/onboarding).

#### Sponsored Display cost-per-click bid constraints are now identical for vendors and sellers

Regions: UK, DE, CA, US, FR, ES, IT

Bid constraints on Sponsored Display cost-per-click (CPC) campaigns are now identical for sellers and vendors within a given marketplace. Previously, minimum and maximum bids for vendors were different from minimum and maximum bids for sellers in some marketplaces.

For a full list of bid constraints in the Amazon Ads API, see [Bid constraints by marketplace](reference/concepts/limits#bid-constraints-by-marketplace).

#### Sponsored Display is now available for advertisers in Singapore

Regions: SG

[Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display?ref_=a20m_us_wn_sdse_071922_p_sd) contextual targeting and audiences are now available to advertisers in Singapore.

Vendors and registered sellers in Singapore now have access to Sponsored Display, helping them reach customers viewing specific products or categories with contextual targeting, and throughout their customer journey, including the Amazon homepage, with Sponsored Display audiences strategy. Sponsored Display's flexible controls enable advertisers to use self-service display ads to introduce products, engage new customers, and remarket with scale to supplement Sponsored Products and Sponsored Brands campaigns. Advertisers can add up to 10,000 products to a campaign and benefit from retail awareness, meaning campaigns will pause if your products go out of stock, eliminating wasted marketing spend on advertising that cannot lead to a purchase.

For full technical details, please see our [Sponsored Display documentation](sponsored-display/3-0/openapi).

#### Amazon DSP reporting APIs now use updated metric names for off-Amazon conversions

Reports generated in the newest release of the Amazon DSP reporting APIs will support new field names for 20 metrics, bringing the APIs into alignment with Amazon DSP online and downloadable reports. These metrics (formerly 'pixel conversions') have been renamed:

|Legacy API field name	|New API field name	|
|---	|---	|
|productPurchased	|offAmazonPurchases14d	|
|productPurchasedViews	|offAmazonPurchasesViews14d	|
|productPurchasedClicks	|offAmazonPurchasesClicks14d	|
|productPurchasedCVR	|offAmazonPurchaseRate14d	|
|productPurchasedCPA	|offAmazonECPP14d	|
|totalPixel14d	|offAmazonConversions14d	|
|totalPixelViews14d	|offAmazonViews14d	|
|totalPixelClicks14d	|offAmazonClicks14d	|
|totalPixelCVR14d	|offAmazonCVR14d	|
|totalPixelCPA14d	|offAmazonCPA	|
|subscriptionPage14d	|subscribe14d	|
|subscriptionPageViews14d	|subscribeViews14d	|
|subscriptionPageClicks14d	|subscribeClicks14d	|
|subscriptionPageCVR14d	|subscribeCVR14d	|
|subscriptionPageCPA14d	|subscribeCPA14d	|
|signUpButton14d	|signUp14d	|
|signUpButtonViews14d	|signUpViews14d	|
|signUpButtonClicks14d	|signUpClicks14d	|
|signUpButtonCVR14d	|signUpCVR14d	|
|signUpButtonCPA14d	|signUpCPA14d	|

As we improve the conversion tracking experience for Amazon DSP advertisers, these metrics will include conversions from new, non-pixel-based interfaces. Legacy aliases for these metrics are still supported in version 3.0. 

For more information, see the [DSP reporting API reference](dsp-reports-beta-3p/#/Reports/createReportV3). 

### September 2022

#### Awareness: Version 1 of Sponsored Products bid recommendations resources to be deprecated on December 31, 2022

The following Sponsored Products bid recommendations resources will be deprecated on December 31, 2022:

* [/adGroups/{adGroupId}/bidRecommendations](reference/1/bid-recommendations)
* [/keywords/{keywordId}/bidRecommendations](reference/1/bid-recommendations)
* [/keywords/bidRecommendations](reference/1/bid-recommendations)

We recommend that advertisers located in CA, DE, ES, FR, IN, JP, US, or UK migrate to the [new theme-based bid suggestion resource](sponsored-products/3-0/openapi/prod#/ThemeBasedBidRecommendation/GetThemeBasedBidRecommendationForAdGroup_v1) to retrieve bid suggestions. This new resource includes historical impact metrics based on similar products and helps in choosing the right bid for your campaigns.  For full technical details on this new resource, see the [theme-based bid suggestions documentation](sponsored-products/3-0/openapi/prod#/ThemeBasedBidRecommendation/GetThemeBasedBidRecommendationForAdGroup_v1) or visit the [quick-start guide](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide). Note that for countries in which the theme-based bid suggestion resource is not yet available, we recommend that advertisers use the [/v2/sp/targets/bidRecommendations](sponsored-products/2-0/openapi#/Bid%20recommendations/getBidRecommendations) resource instead.

Note: We [previously announced the deprecation](release-notes/archive/ads-api#awareness-version-2-of-sponsored-products-bid-recommendations-resource-to-be-deprecated-on-december-31-2022) of version 2 Sponsored Products bid recommendations. 

See [API deprecations](release-notes/deprecations) for information on all upcoming deprecations.

#### Sponsored Display launches theme-based suggestions for contextual targeting campaigns

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SE, UK, US

Sponsored Display theme-based recommendations are targeting options that empower advertisers with flexibility to choose products that are aligned with their specific campaign strategies, based on price, ratings, and views. These recommendations will now be provided for [Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display?ref_=a20m_us_wn_sd_092021_sd) contextual targeting campaigns within the Amazon Ads API.

Theme-based recommendations offer multiple options to target relevant products without the need for additional research from advertisers. Target products under each theme are curated specific to the use case, with high-quality suggestions based on Amazon's first-party signals. Each theme will have up to 50 individually recommended products, with flexibility to choose products individually or select all products within a theme.

For full technical details, refer to the [API reference](sponsored-display/3-0/openapi#/Targeting%20Recommendations). Note: If you are already specifying a version in the `Accept` request-header field, you must update to `application/vnd.sdtargetingrecommendations.v3.2+json` to access the new features.

#### Sponsored Brands ad groups are now available in open beta through Amazon Ads API

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SA, SG, UK, US

Advertisers using Sponsored Brands can now create ad groups to better organize and manage Sponsored Brands campaigns. Advertisers can use ad groups to organize their ads by brand, keywords, product, category, ad format, landing page, or other classifications that tie to your goals or marketing strategy. Brands can now run goal-based marketing campaigns by grouping different creative types and landing pages under an ad group and monitoring the effectiveness over time. The launch enables brands to evaluate different creatives within the same ad group, helping advertisers compare performance and develop a better understanding of the impact of creatives versions. 

Previously, advertisers could only retrieve ad group information and lacked the support to create or update ad groups. Ad groups are a great way to reduce the cognitive load and complexity, as well as drive consistency when managing campaigns across Sponsored Products and Sponsored Display. With this change, both vendors and sellers can specify multiple ad groups in campaign creation and update operations. 

**Note**: The advertising console will be switching to the new campaign type over the next month. In order to retrieve full details for campaigns created in the ad console, you need to use the new POST /sb/beta/campaigns/list endpoint. To see full reporting data, you will also need to use the `creativeType` filter set to `all` in your Sponsored Brands report requests.

For full technical details, refer to the [API reference](sponsored-brands/3-0/openapi/prod#/Campaigns) and [migration guide](reference/migration-guides/sb-v3-v4).


#### Headline suggestions for Sponsored Display now available through the Amazon Ads API 

Advertisers can now receive suggested headlines for Sponsored Display campaigns through the Amazon Ads API. Suggested headlines are generated based on advertised products selected to show in your creative, and they follow our ad policy guidelines. You can also edit the suggested headlines to best match your campaign goals, but edits to suggested headlines will continue to go through the standard moderation review process along with remaining creative elements (logo, image, products, brand name). 

Learn more with our [reference documentation](sponsored-display/3-0/openapi/prod#/Recommendations/getHeadlineRecommendationsForSD).

#### Branded Searches for Sponsored Brands are now available in all regions

Regions: All

The Sponsored Brands [Branded Searches metric](guides/reporting/v2/metrics#attributedbrandedsearches14d) is now available in all regions supported by the Amazon Ads API. The branded searches metric helps advertisers better understand the impact of Sponsored Brands campaigns by measuring how many times shoppers executed a search that contained an advertiser's brand name after interacting with an ad. 

For more information, see the [original release note](release-notes/archive/ads-api#branded-searches-are-now-available-for-sponsored-brands-through-the-amazon-ads-api). 

### August 2022

#### Updated content for partner opportunities API

Regions: All

We've released a new set of content to walk advertisers through the use of the partner opportunities API. The content begins with an [overview](common-resources/partner-opportunities/overview), follows with a [document to help you get started](guides/recommendations/partner-opportunities/getting-started), and ends with a detailed [how-to guide with troubleshooting](guides/recommendations/partner-opportunities/how-to).

The partner opportunities API provides partners who are registered on the Amazon Ads Partner Network with actionable insights for their linked advertisers with a single call to the Amazon Ads API. Example insights may include: high potential ASINs and excess inventory ASINs to advertise, retail readiness scores below a certain threshold, ASINs without branded search key-terms, and more insights to come.

With a single call, partners can receive the actionable insights available through the partner opportunities API to optimize advertising performance for the advertisers they manage. Rather than calling multiple endpoints for every advertiser they manage, partners can save time by calling the Partner Opportunities API to receive custom insights aimed at improving overall campaign performance for their client accounts.

The partner opportunities API is tailored for partners to use across all the advertisers linked to their Partner Network account. To call the partner opportunities API, partners configure their Amazon Ads API call with their Partner Network Account idenifier, which is available in the Partner Network [user settings](https://advertising.amazon.com/partner-network/settings).

#### Awareness: Sponsored Products version 2 category and brand targeting recommendation endpoint deprecation on October 30, 2022

We are announcing plans to deprecate Sponsored Products version 2 category and brand targeting endpoints on October 30, 2022. Please migrate to the new versions of these endpoints as mentioned in our [previous release notes](release-notes/archive/ads-api#new-and-updated-product-targeting-capabilities-available-for-sponsored-products-on-the-amazon-ads-api).

The following endpoints are included in this deprecation: 

- [/v2/sp/targets/brands](sponsored-products/2-0/openapi#/Product%20targeting/getBrandRecommendations)
- [/v2/sp/targets/categories/refinements](sponsored-products/2-0/openapi#/Product%20targeting/getRefinementsForCategory) 
- [/v2/sp/targets/categories](sponsored-products/2-0/openapi#/Product%20targeting/getTargetingCategories) 

If you have feedback, contact us through the Amazon Ads API [Support webpage](https://amzn-clicks.atlassian.net/servicedesk/customer/portals) with ticket subject title "SP v2 category and brand targeting recommendations deprecation" by September 15, 2022.

To view all upcoming deprecations, see our [API deprecation page](release-notes/deprecations). 

#### Awareness: Sponsored Products version 2 product targeting product recommendations endpoints deprecation on February 28, 2023

We are announcing plans to deprecate a Sponsored Products v2 product targeting recommendation endpoint on February 28, 2022. Please migrate to the new versions of these endpoints as mentioned in our [previous release notes](/release-notes/archive/ads-api#product-targeting-recommendations-with-themes).

The following endpoint is included in this deprecation:

- [/v2/sp/targets/productRecommendations](sponsored-products/2-0/openapi#/Product%20targeting/createTargetRecommendations)

If you have feedback, contact us through the Amazon Ads API [Support webpage](https://amzn-clicks.atlassian.net/servicedesk/customer/portals) with ticket subject title "SP v2 product targeting recommendation deprecation" by November 15, 2022.

To view all upcoming deprecations, see our [API deprecation page](release-notes/deprecations). 

#### Announcing the availability of sponsored ads for traditional authors and changes to Sponsored Brands for authors

We updated how we identify the author brand for new Sponsored Brands campaigns created by authors in United States, United Kingdom, Germany, France, Italy, and Spain. The change provides author support for the Sponsored Brands features of the Amazon Ads API. This will allow users to create and modify Sponsored Brands campaigns for KDP and traditional authors, and draft campaigns through the Amazon Ads API. With this change, authors can use Amazon Ads for more of their catalog, including:

* Titles published outside of Kindle Direct Publishing (i.e., traditionally published titles)
* Co-authored titles

When setting up a new Sponsored Brands campaign, authors will need to first claim the book, KDP or traditionally published, in their Author Central (A2C) account. See [how to claim a book in A2C](https://author.amazon.com/help/G9RVCRGG7Q7TVLAU) for more information.

For more details, refer to the [Sponsored Brands open API documentation](sponsored-brands/3-0/openapi#/).

#### Branded Searches are now available for Sponsored Brands through the Amazon Ads API

Regions: US, UK, DE, FR, IT, ES

Sponsored Brands launched a new Branded Searches metric for US, UK, DE, FR, IT, and ES advertisers. With the release of this metric*,* advertisers can better understand the impact of their Sponsored Brands campaigns in driving branded search queries. Complete branded search information is available from June 21, 2022.  

The branded searches metric helps advertisers better understand the impact of Sponsored Brands campaigns by measuring how many times shoppers executed a search that contained an advertiser's brand name after interacting with a Sponsored Brands ad. The number of branded searches is a strong indicator of brand recall and awareness, and can help advertisers understand the full impact of their campaigns beyond driving sales.

It allows a new value, `attributedBrandedSearches14d`, in the [metrics](guides/reporting/v2/metrics) field for version 2 Sponsored Brands reports. For full technical details, see our [updated documentation](guides/reporting/v2/report-types) including the new `attributedBrandedSearches14d` across campaign, placement, ad group, keyword, target, and ad reports.

#### Awareness: Deprecation of Sponsored Products version 2 reporting endpoints March 30, 2023

Following the release of the new [version 3 reporting endpoints](release-notes/archive/ads-api#announcing-reporting-enhancements-for-sponsored-products-and-sponsored-brands), we are announcing the deprecation the version 2 Sponsored Products reporting by March 30, 2023. This includes the [POST v2/sp/reports/{recordType}](sponsored-products/2-0/openapi#/Reports/requestReport) endpoint. 

Going forward, you can access all version 2 Sponsored Products report types using the [version 3 reporting endpoints](offline-report-prod-3p). For more details on how to upgrade to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3) and [Frequently asked questions](guides/reporting/v3/faq).

If you have feedback, use the Amazon Ads API [Support page](support/overviewview) to submit a ticket with the subject title “SP reports v2 deprecation” by December 1, 2022.


#### Announcing reporting enhancements for Sponsored Products and Sponsored Brands

Regions: All

We are launching the latest version (version 3) of reporting for the Amazon Ads API. This update provides agencies, brands, and service providers with a way to access all their campaign data from a single source, enabling them to spend less time extracting and consolidating data. Additional benefits in this version include: the ability to select date ranges, longer historical data selection, and common request and response schemas for all ad products.

Version 3 reporting currently supports all Sponsored Products version 2 report types, as well as a net new report for Sponsored Brands. The Sponsored Brands purchased products report provides ASIN-level insights for all ad-attributed sales, including promoted products (purchases for your brand's products made by shoppers that clicked on your ad) and brand-halo (sales of any product in your advertised brands made by Amazon or other sellers).

Reporting for the rest of sponsored ads suite will be onboarded in the coming months. 

For full technical details, see our [version 3 overview](guides/reporting/v3/overview) and [migration guide](reference/migration-guides/reporting-v2-v3).

### July 2022 release notes

#### Egypt support

We have added support for Egypt (EG) to the API. For more details, see [API endpoints](reference/api-overview#api-endpoints), [bid constraints by marketplace](reference/concepts/limits#bid-constraints-by-marketplace), the `countryCode` parameter of the [registerProfile and registerBrands operations](reference/2/profiles), and the regional profile time zone codes section of the [profiles reference](reference/2/profiles).

#### Sponsored Display is now available for advertisers in Sweden

Regions: SE

[Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display?ref_=a20m_us_wn_sdse_071922_p_sd) contextual targeting and audiences are now available to advertisers in Sweden.

Vendors and registered sellers in Sweden now have access to Sponsored Display, helping them reach customers viewing specific products or categories with contextual targeting, and throughout their customer journey, including the Amazon homepage, with Sponsored Display audiences strategy. Sponsored Display's flexible controls enable advertisers to use self-service display ads to introduce products, engage new customers, and remarket with scale to supplement Sponsored Products and Sponsored Brands campaigns. Advertisers can add up to 10,000 products to a campaign and benefit from retail awareness, meaning campaigns will pause if your products go out of stock, eliminating wasted marketing spend on advertising that cannot lead to a purchase.

For full technical details, please see our Sponsored Display [documentation](sponsored-display/3-0/openapi).

#### Advertisers can combine a lifestyle image, headline, and logo when customizing their Sponsored Display creatives

Regions: All

With this launch, advertisers using Sponsored Display can now customize their creatives by adding a headline, logo, and lifestyle image either together or individually. Advertisers can choose to add any combination of those assets while creating or editing their creatives. 

Advertisers interested in customizing their creatives now have increased flexibility and control. Every creative choice is optional, and they can choose any combination, allowing advertisers to match their style across creatives or repurpose existing assets. Ads will display alongside a product image, creating consistent branded creatives across ad placements. Ads with lifestyle images will support promotional deals and savings badges to help improve ad performance. When customizing creatives in the console, advertisers will now see a menu of customizable assets that provides real-time feedback and moderation status to quickly provide a holistic status of a creative. This feature applies to new and existing creatives.

For full technical details, please see our updated [Sponsored Display creative documentation](sponsored-display/3-0/openapi#/Creatives) including the ability to choose any combination of `headline`, `brandLogo`, or custom image (`rectCustomImage` and `squareCustomImage`).

#### Announcing partner insights for all linked advertisers

Regions: All

The partner opportunities API provides partners who are registered on the Amazon Ads Partner Network with actionable insights for their linked advertisers with a single call to the Amazon Ads API. Example insights may include: high potential ASINs and excess inventory ASINs to advertise, retail readiness scores below a certain threshold, ASINs without branded search key-terms, and more insights to come.

With a single call, partners can receive the actionable insights available through the partner opportunities API to optimize advertising performance for the advertisers they manage. Rather than calling multiple endpoints for every advertiser they manage, partners can save time by calling the Partner Opportunities API to receive custom insights aimed at improving overall campaign performance for their client accounts.

The partner opportunities API is tailored for partners to use across all the advertisers linked to their Partner Network account. To call the partner opportunities API, partners configure their Amazon Ads API call with their Partner Network Account identifier, which is available in the Partner Network [user settings](https://advertising.amazon.com/partner-network/settings).

To get the list of all available opportunities, partners first call the [GET /partnerOpportunities resource](partner-opportunities/#/Partner%20Opportunities) to obtain descriptive information about the opportunity, such as which advertising products it pertains to. Once you identify an opportunity of interest, you can retrieve the dataset pertaining to the identified opportunity using the [GET /partnerOpportunities/{partnerOpportunityId}/file resource](partner-opportunities/#/Partner%20Opportunities). You will receive a secure URL to the S3 location of the file for each opportunity dataset, which you can download as a CSV file to identify the advertisers, campaigns, or ASINs to which the opportunity applies.

For more information, refer to the [partner opportunities API documentation](partner-opportunities). 

#### To help advertisers understand the impact of their reach and awareness campaigns, Sponsored Display launches branded searches metrics in the reporting API

Regions: US, CA, MX, BR, DE, ES, FR, IT, NL, UK, UAE, AU, JP, IN

Sponsored Display has launched branded searches metrics for vendors and registered sellers across all available regions. With the release of these metrics, advertisers can better understand the impact of advertising campaigns on searches for their brand.

The branded searches metrics help advertisers better understand the impact of Sponsored Display campaigns by measuring how many times customers executed a search that contained an advertiser's brand name after interacting with a Sponsored Display ad. Branded searches works by matching the promoted products within a campaign with searches containing the brand's name on Amazon.

With branded search metrics, brand owners can report on how many shoppers executed a search that included their brand name after viewing or clicking (for awareness campaigns using vCPM) a Sponsored Display ad. This data can be used to help you understand the impact of different Sponsored Display creative and targeting strategies on your brand searches. Complete branded search data will be available from June 21st, 2022. Please allow up to 14 days for full restatement of this data.

For full technical details, please see our updated [Sponsored Display documentation](guides/reporting/v2/report-types) including the new `attributedBrandedSearches14d` for campaigns with CPC as costType and `viewAttributedBrandedSearches14d` for campaigns with vCPM as costType across campaigns, adGroups, targets, and matchedTarget reports.

#### Announcing the availability of performance-based budget rules API for Sponsored Brands

We’ve expanded the [budget rules API](guides/budgets/rules/overview) to enable advertisers to create performance-based budget rules for Sponsored Brands campaigns, using return on advertising spend (ROAS) as the performance metric. Budget rules help you plan budget changes in advance so you can spend less time manually adjusting during a campaign. You can set rules based on specific dates or your ROAS performance goals.

Performance-based budget rules can be set to increase budgets based on campaign performance metrics. With budget rules, advertisers can reduce the likelihood of campaigns running out of budget, especially during peak days or holiday events.

Visit our overview pages to learn more: 

* [Sponsored Products Budget Rules overview](sponsored-products/budget-rules/overview)
* [Sponsored Brands Budget Rules overview](guides/budgets/rules/overview)
* [Sponsored Display Budget Rules overview](sponsored-display/budget-rules/overview)


#### Introducing the validation configuration API 

The validation configuration API supplies commonly-used configuration values that are required to successfully validate Amazon Ads campaigns. These configuration values, such as valid character types, maximum text input lengths, and valid bid and budget ranges, are required fields that are validated when campaigns are submitted or updated. These configuration values are important when launching campaigns in various marketplaces, as the validation values may change due to differing supported languages, as well as marketplace-specific valid bid and budget ranges due to varying currencies. The valdiation configuration API includes two resources that can be partitioned by marketplace code, entity type, and ad program type: 

* [POST /validationConfigurations/campaigns](validation-configuration): returns configurations for campaign level validation
* [POST /validationConfigurations/targetingClauses](validation-configuration): returns configurations for targeting clause level validation

### June 2022 release notes

#### Sponsored Display expands bid optimizations in BR, adding "Optimize for reach"

Regions: BR

We've expanded bid optimization controls for Sponsored Display campaigns to BR. You can now select "Optimize for viewable impressions" and your campaigns will be optimized to drive viewable impressions for all audiences and contextual targeting campaigns within the Amazon Ads console or Amazon Ads API.

Custom bid optimizations allow you to select bid optimizations based on campaign needs and help you evaluate performance based on viewable impressions, clicks, or conversions. This means you can complement your targeting selection and creative customization with appropriate bid optimizations to meet your needs. You now have more controls over your campaigns and are no longer restricted by pre-defined bid optimizations. All existing campaigns in the expanded regions will default to “Optimize for page visits". These new flexible controls enable you to help you more effectively introduce new products, drive traffic, and engage new or existing shoppers.

For full technical details and examples of `costType` and `bidOptimization`, please see our Sponsored Display [developer guide for contextual targeting](guides/sponsored-display/contextual-targeting).

#### Awareness: Version 2 of Sponsored Products bid recommendations resource to be deprecated on September 30, 2023

**Note**: This deprecation date has been extended from **December 31 2022** based on customer feedback. For the latest information, see our [Deprecations page](release-notes/deprecations).

The following Sponsored Products bid recommendations resources will be deprecated on September 30, 2023:

* /v2/sp/adGroups/{adGroupId}/bidRecommendations
* /v2/sp/keywords/{keywordId}/bidRecommendations
* /v2/sp/keywords/bidRecommendations

We recommend that advertisers located in CA, DE, ES, FR, IN, JP, US, or UK migrate to the [new theme-based bid suggestion resource](sponsored-products/3-0/openapi/prod#/ThemeBasedBidRecommendation/GetThemeBasedBidRecommendationForAdGroup_v1) to retrieve bid suggestions. This new resource includes historical impact metrics based on similar products and helps in choosing the right bid for your campaigns.  For full technical details on this new resource, please see the [theme-based bid suggestions documentation](sponsored-products/3-0/openapi/prod#/ThemeBasedBidRecommendation/GetThemeBasedBidRecommendationForAdGroup_v1) or visit the [quick-start guide](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide). Note that for countries in which the theme-based bid suggestion resource is not yet available, we recommend that advertisers use the [/v2/sp/targets/bidRecommendations](sponsored-products/2-0/openapi#/Bid%20recommendations/getBidRecommendations) resource instead.

#### Awareness: Version 2 of Sponsored Brands Campaign resource to be deprecated on December 19 2022

[Version 2 of the Sponsored Brands Campaign resource](reference/sponsored-brands/2/campaigns) is to be deprecated on December 19, 2022. We recommend all clients migrate to version 3 of the [Sponsored Brands Campaign resource](sponsored-brands/3-0/openapi#/Campaigns) as soon as possible. 

We welcome feedback on this deprecation, please contact us by visiting the [Amazon Ads API Support page](https://amzn-clicks.atlassian.net/servicedesk/customer/portals) and create a ticket with the subject title **SB v2 Deprecation** before August 30, 2022.

#### Sponsored Products keyword recommendations version 5.0

We have launched version 5.0 of keyword recommendations, giving advertisers more interpretability and control by providing recommended keywords along with bid suggestions from different themes. The recommended keywords can be sorted on two different dimensions: click volume (clicks) and conversion volume (conversions). The theme being released today is ‘bids to generate sales’ and the response also includes impact metrics at the ad-group level such as weekly clicks and purchase orders received by similar products. Previously, advertisers looking to add additional keywords to their campaigns saw a long list of suggested keywords but were provided no additional context on why Amazon was providing those suggestions. This release allows advertisers to more easily evaluate one suggestion versus another by providing a prioritization for suggested keywords along with an estimated impact of adopting them.

For more information, see the [keyword recommendations resource reference](sponsored-products/3-0/openapi/prod#/Keyword%20Recommendations/getRankedKeywordRecommendation). Note that you must click on the version drop-down box in the request and response body sections of the reference and select `application/vnd.spkeywordsrecommendation.v5+json` to view the documentation for version 5.0 of the resource.

#### Sponsored Display product targeting evolves to contextual targeting, expanding ads off Amazon

We are evolving Sponsored Display's product targeting into [contextual targeting](https://advertising.amazon.com/help?entityId=ENTITY2MBK6T7VH81E1#GNTK7LTYZS3KSTAN), allowing advertisers to reach more of their audiences at more points in their shopping and entertainment journey. The contextual targeting strategy will now allow advertisers to reach audiences both in the Amazon store and outside the Amazon store when customers view content related to targeting strategies. Advertisers do not need to change their targeted products or categories to enable the new feature. Sponsored Display will find the right opportunities to expand advertiser reach beyond the store, to web pages and mobile apps.

Vendors and registered sellers can use Sponsored Display to reach their audiences throughout their shopping and entertainment journeys to drive product consideration and awareness. Previously, contextually targeted campaigns would only show on the Amazon store (e.g., on product pages in the “Fitness and Exercise” category). However, there are many opportunities to reach fitness enthusiasts at other points in their day, as they browse other web pages or use their favorite mobile apps. Our machine learning models will now automatically find more opportunities to deliver campaigns, while still adhering to advertiser bidding strategies and objectives: for instance, when optimizing for page views, we will help find additional opportunities beyond Amazon to serve ads at the same or improved cost per click. Advertisers can pair contextual targeting with page view optimization strategies, alongside a remarketing strategy that can help drive additional reach and conversions from the increased visitors to their detail pages. 

For full technical details, please see our [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting), including the updated documentation to reflect the name change from product targeting to contextual targeting. For additional examples, please see our [contextual targeting developer guide](guides/sponsored-display/contextual-targeting). Please note, no changes will be needed to your existing 'T00020' campaigns to take advantage of this change. 


#### Amazon Marketing Stream (Beta)

Regions: US, CA, MX

Amazon Marketing Stream has launched to open beta, allowing partners and advertisers to receive campaign data in near real time, without needing to repeatedly call the API. Amazon Marketing Stream offers new capabilities compared with an API-based reporting approach, including traffic and conversion data summarized hourly instead of daily, less throttling, additional reporting dimensions, and better insights on deltas instead of aggregates. 

In the past, access to scaled campaign reporting was limited to pull-based API calls that provided daily performance. In addition, the API needed to be called multiple times throughout the day, which led to throttling and a less efficient workflow.  

Amazon Marketing Stream is available for Sponsored Products campaigns and sponsored ads budget usage via the Amazon Ads API for agencies, tool providers, and direct advertisers, including vendors and sellers in Canada, Mexico, and the United States. When you’re ready to get started, visit the Amazon Marketing Stream [onboarding guide](guides/amazon-marketing-stream/onboarding).

#### Announcing general availability for Sponsored Display deny lists, helping brands control ads off Amazon

Regions: US, CA, MX, BR, DE, ES, FR, IT, NL, UK, UAE

Sponsored Display deny lists have launched to general availability in the Amazon Ads console and Amazon Ads API. Sponsored Display ads serve on Amazon and off Amazon on third-party properties, such as websites and mobile apps. The deny list functionality provides you control to manage your brand presence off Amazon on third-party properties that don't align with your brand. You can upload a deny list of websites and mobile apps where you don't want your ads to show across all your Sponsored Display campaigns.

Vendors and registered sellers are now better equipped to engage with audiences off Amazon during their shopping and entertainment journey without concern of their ads showing on websites and mobile apps that are not aligned to their brand. Deny lists provide an additional layer of protection on top of Amazon ads global brand safety policy to allow advertisers to remove websites or mobile apps that are generally brand safe but don't align with the brand's values. For example, a vegan food brand may choose not to advertise on websites that discuss meat products to protect their brand reputation, even though the website does not contain sensitive content.

For full technical details, please see our updated Sponsored Display [brand safety documentation](sponsored-display/brand-safety) in the Amazon Ads API.

#### Learn more about Sponsored Display's dynamic segments available through the Amazon Ads console and API

With Sponsored Display's dynamic segments, advertisers can target additional products that are similar to those being advertised. The products selected by similar product targeting are based on the advertised product and are dynamically generated based on retail data. Dynamic segments help advertisers reach relevant audiences most likely to click and help drive traffic to advertised products. These segments allow advertisers to launch Sponsored Display campaigns quickly and easily to help create product consideration.

For full technical details, please see our updated documentation on Sponsored Display [dynamic segments in the Amazon Ads API](guides/sponsored-display/dynamic-segments).

#### Sponsored Display report request limit is 1 TPS per unique combination of clientId and profileId

Clients may see increased throttles (429 responses) when calling the Sponsored Display [POST report endpoint](sponsored-display/3-0/openapi#/Reports/requestReport) if they are calling the resource more than 1 TPS per unique combination of clientId and profileId. To avoid rate limiting, please reduce the cadence of API reporting calls. Rate limiting, sometimes referred to as throttling, occurs when the Amazon Ads API receives too many requests within a given time period.  

Rate limiting will occur dynamically based on the overall system load. We encourage users to avoid making reporting requests during high-usage times and to smooth their report creation requests throughout the day. For more information, please visit our documentation on [rate limiting in the Amazon Ads API](reference/concepts/rate-limiting).

#### Consolidated recommendations launches for Sponsored Products

Regions: US

Consolidated recommendations are now available through the Amazon Ads API. This new feature identifies up to 5 Sponsored Products campaigns with the most incremental sales opportunities for each advertiser. It provides the suggested bids, budget, and targeting changes created with advanced analytics to maximize sales. You also receive the estimate incremental clicks for each campaign if the recommendations are adopted. The initial launch will cover keyword targeting campaigns and automatic targeting campaigns with one ad-group in the US. 

Before this launch, advertisers received recommendations at the campaign level, and did not know which of their campaigns had the greatest incremental sales opportunity. With this launch, Amazon helps advertisers identify the top Sponsored Products campaigns with the most incremental sales opportunity, and provide the consolidated recommendations across bids, budget, and targeting for these campaigns in one place.

Advertisers can retrieve new recommendations on a daily basis using the [consolidated recommendations resource](sponsored-products/3-0/openapi/prod#/Consolidated%20Recommendations). When recommendations are available, this resource includes the campaign identifier, recommendations, and estimated incremental clicks. Advertisers can then modify the recommendations or accept the recommendations using the [update campaign](sponsored-products/2-0/openapi#/Campaigns/updateCampaigns) or [update targeting clause](sponsored-products/2-0/openapi#/Product%20targeting/updateTargetingClause) operations. The recommendations are refreshed daily.

#### Budget usage percentage now available for real-time pulls in Amazon Ads API

With the new budget usage feature, users worldwide can now pull their budget usage percentage in real-time for Sponsored Products, Sponsored Brands, and Sponsored Display using the Amazon Ads API. You can pull budget usage information for both campaigns and portfolios. 

Your budget usage percentage helps you understand how fast your campaigns and portfolios are spending daily budgets, and can give you the information needed to make quick budget decisions. 

For more information see the `Budget Usage` resource in the [Sponsored Products reference](sponsored-products/3-0/openapi/prod#/Budget%20Usage), [Sponsored Brands reference](sponsored-brands/3-0/openapi/prod#/Budget%20Usage), the [Sponsored Display reference](sponsored-display/3-0/openapi/prod#/Budget%20Usage), or the [Portfolios reference](reference/portfolios#/Budget%20Usage).

#### Sponsored Display bid recommendations expands to additional regions

Regions: AU, IN, MX, and NL

We’ve expanded suggested bids for Sponsored Display to AU, IN, MX, and NL. These recommendations will now be provided for Sponsored Display views remarketing and product targeting campaigns within the ad console and API for those new regions in addition to the existing regions of CA, DE, ES, FR, IT, JP, UAE, UK, and US. These suggested bids will be added by default in the ad console.

Bid recommendations help remove the guesswork when selecting a bid. Now, when you create any CPC-based or vCPM-based Sponsored Display campaigns with product targeting (products, similar products, and category) or views remarketing in AU, IN, MX, or NL, you’ll receive machine learning powered bid recommendations. These recommendations are presented as a suggested bid and a bid range, and both are calculated by analyzing a group of winning bids for recent, similar ads within your category. Suggested bid and bid range update daily, based on the increase or decrease in competing bids and ads in each auction. If you’re unsure which bid to start with, we recommend advertisers select the suggested bid. This is the bid that our algorithms suggest will most likely help deliver impressions for an ad similar to yours.

For full technical details, please see our updated Sponsored Display [documentation](sponsored-display/3-0/openapi#/Bid%20Recommendations) or [recommendations developer guide](guides/sponsored-display/recommendations).

### May 2022 release notes

#### Amazon Ads API webinar for June 2022 

Join Amazon Ads for a monthly webinar series on the Amazon Ads API to learn about recent product releases, product deep dives, and upcoming product releases. 

**North American series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_7OkAr9u5INOxMEK) 

**European series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_3QSzePdHBkfexfM)

#### Sandbox Deprecation on June 28 2022

Today we are announcing plans to deprecate the Amazon Advertising API Sandbox environment. We are targeting end of life on June 28, 2022. We recommend migrating to [Advertiser Test Accounts](guides/account-management/test-accounts/overview), which are a type of advertiser account dedicated to testing. Test accounts do not serve ads or have active billing, allowing integrators to make API requests to test and learn about API functionality in a similar experience to production accounts, as well as access the ads console from a test account to see your API updates reflect in ads console experience. For more information on these test accounts, please see our [test accounts overview](guides/account-management/test-accounts/overview). If you would like to share feedback, please contact us through the [Amazon Ads API Support webpage](https://amzn-clicks.atlassian.net/servicedesk/customer/portals) with ticket subject title "Sandbox Deprecation" by June 14, 2022.

#### Introducing a Postman collection for the Amazon Ads API

Amazon Ads is launching a Postman collection to help get started with the Amazon Ads API. Postman is a third-party tool that allows you to make API calls using a graphical user interface. The Amazon Ads API Postman collection includes common endpoints, example request/responses, and automations that make it easy to test the API and manage authorization. The collection currently supports endpoints related to sponsored ads reports and snapshots, test accounts, and authorization. You can find the collection files in the new [GitHub repository for advanced tools docs](https://github.com/amzn/ads-advanced-tools-docs). 

To use the collection, download the collection and environment files from GitHub, input your Amazon Ads API credentials, and start sending requests--no code necessary. 

For full technical details, please read our [Postman guide](guides/postman).

####  Amazon Ads API GitHub repository

You can now interact with the Amazon Ads API on Github in our [ads-advanced-tools-docs repository](https://github.com/amzn/ads-advanced-tools-docs). This repository will be home to Postman collections, code artifacts, and other resources to supplement the documentation in the advanced tools center. 

Developers are encouraged to [open issues](https://github.com/amzn/ads-advanced-tools-docs/issues/new/choose) on the repository to make suggestions and report problems related to the documentation. Our team will monitor incoming issues to continuously improve the documentation.

To stay up to date on the latest additions, add yourself to the repository as a watcher.

#### Sponsored Display adds new-to-brand metrics to campaigns with view-based attribution 

New-to-brand metrics are now available for Sponsored Display campaigns with view-based attribution (vCPM) through the Amazon Ads API. New-to-brand metrics enable you to measure orders and sales of your products generated from first-time customers of your brand. With new-to-brand metrics, you can better measure and optimize in-flight campaigns and plan future marketing strategies to drive customer acquisition and brand loyalty. New customer orders are a key step in establishing a long-term brand relationship with customers.

An order is considered new to your brand if a shopper hasn't purchased one of the products from your brand in the past 12 months. The following campaign performance metrics are available: new-to-brand sales, new-to-brand orders, and new-to-brand units. Using these metrics, you can calculate the cost to acquire a new-to-brand customer using view-based attribution for campaigns with a `costType` of "vcpm". 

For full technical details and to see the new metrics that include `viewAttributedSalesNewToBrand14d`, `viewAttributedOrdersNewToBrand14d`, and `viewAttributedUnitsOrderedNewToBrand14d`, please see our updated [reporting documentation](guides/reporting/v2/report-types).


#### Upcoming change to Sponsored Display using the legacy 'remarketing' or 'T00001' tactics on the Amazon Ads API

Starting on June 1, 2022, Sponsored Display will no longer support reporting calls for the legacy 'remarketing' or 'T00001' tactics on the Amazon Ads API. All of these campaigns were archived in 2021 and reporting data is only available up to 60 days back from the current date via the API. Going forward, Sponsored Display advertisers should only use reporting calls for the audiences strategy via tactic 'T00030' or product targeting via tactic 'T00020'. 

As a reminder, Sponsored Display audiences and product targeting can be used together to help engage shoppers at all stages of the purchase journey. Used together, these strategies can help drive awareness and consideration for your brand as well as product sales.

For more information, see our updated [reporting documentation](guides/reporting/v2/sponsored-ads-reports).

#### Amazon DSP Product and Whole Foods Market audience creation open beta

Regions: US, UK, DE, JP, FR, IT, ES, CA, MX, AU, AE, BR, NL, IN, SA, SE, TR

Note: Whole Foods Market audience creation is only available in the US.

Amazon Ads API users can now create custom Product and Whole Foods Market audiences for use in Amazon DSP campaigns. To build their audiences, users can choose from the available attributes and use a custom lookback window, listed below. The onboarded audiences are seamlessly available within the advertiser’s Amazon DSP account. 

Attributes:

* Product: Purchases, Views, Similar Product Views, Searches
* Whole Foods Market: In-store purchases

Lookback windows:

* Up to 365 Days: Product Purchases, Whole Foods Market In-store purchases 
* Up to 90 Days: Product Views, Similar Product Views, Product Searches 

This feature is available through the Amazon Ads API. To learn more, please see the [audience creation API documentation](dsp-audiences/#/Audiences).

#### Amazon Ads API now supports domain targeting in Amazon DSP

Regions: BR, CA, MX, US

Amazon DSP now supports the ability to read, add, and remove domain targeting on line items via the Amazon Ads API. This release brings parity for domain targeting with the Amazon DSP console and enables users to specify inventory for their campaigns. Domains can be either included or excluded. 

This API includes four different methods for domain targeting:

1. Inherit existing domain targeting from the advertiser level.
2. Add one or more existing domain lists from the entity level.
3. Add or remove individual domains at the line item level.
4. Upload a comma-separated values (CSV) file of domains using the [file uploads resource](dsp-campaigns/#/FileUploads).

See the [Domain Targeting reference documentation](dsp-campaigns/#/Domain%20Targeting) for more details. 

### April 2022 release notes

#### Bid suggestions are now available in more regions and ad formats

Sponsored Brands bid suggestions for product attribute targeting and keywords campaigns support has been expanded to additional regions and ad formats: 

* Keyword Static campaigns in: BR, NL, SG, SE; 
* Keyword Video campaigns in: CA, MX, JP, AU, UK, IT, ES, FR, DE, AE, IN, NL, SG, SE; 
* Product Targeting Video campaigns in: CA, MX, JP, UK, IT, ES, FR, DE, IN

Bid suggestions are based on historical winning bids of impressions and auction data. This feature provides advertisers with suggested bids as well as a bid range which can help them rank higher in auctions and receive impressions, reducing the complexity associated with manual bidding optimization. This launch expands this feature to smaller marketplaces that previously required Advertisers to use manual bidding optimization. For more information, see the [Sponsored Brands suggested bid and bid range reference](https://advertising.amazon.com/help?entityId=ENTITY281Q1OGLKKHT9#GYZFLBC2FUPH6M36).

Sponsored Brands APIs that previously supported bid recommendations for product collection or Stores spotlight ad formats only now support video formats as well. Previously, bid recommendations US was the only marketplace, that supported bid recommendations for the video creative. This launch extends this feature for many more marketplaces, allowing more advertisers to reduce their efforts when opting for video ads. For full technical details, please refer to our [updated reference documentation](sponsored-brands/3-0/openapi#/Bid%20recommendations).

The following API has been updated to support this launch:

* [/sb/recommendations/bids](sponsored-brands/3-0/openapi#/Bid%20recommendations) marketplace support has expanded to include bid suggestions for video creative ads for additional marketplaces. Specify the adFormat property in your request to specify the video creative type. This ad format was previously available and limited to US marketplace only. 

#### Creative assets expands to Sponsored Display in the Amazon Ads Console

Vendors and Sellers using the Amazon Ads console can now search for and reuse existing [creative assets](https://advertising.amazon.com/resources/whats-new/creative-assets?ref_=a20m_us_wn_sd_041922_wb_sb_071521) to build their Sponsored Display campaigns. This means you can easily reuse creative you’ve already uploaded as a part of their Sponsored Brands, Stores, or Posts efforts within your Sponsored Display campaigns.

Integration with creative assets allows you to build Sponsored Display campaigns quickly, as well as provide a consistent shopping experience across your Amazon Ads campaigns. When adding a logo or custom image to a Sponsored Display campaign, you can select “Choose from assets” instead of “Upload image” to browse and reuse existing creatives assets. To help you browse, you can search, sort and filter by name, tag products, and other attributes to find assets easily. Search results can be further narrowed by filtering and sorting by asset types (logo, custom, or product image), upload date, file format, file size, dimension, moderation status, and more. Any brand content you upload to a Sponsored Display campaign will also be saved to creative assets for easy reuse later on.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Creatives) in the Amazon Ads API and our [Sponsored Display creative developer guide](sponsored-display/creatives) to learn more about uploading assets to creative assets.

#### A preview of Sponsored Display's support for video creatives is now available in the Amazon Ads API

The operation is in **preview only**. We will update our release notes once this functionality becomes available. Sponsored Display will be enhancing its creative offering by allowing video formats, empowering advertisers to showcase their products and brand through immersive storytelling made possible via video (e.g., tutorials, demos, unboxing, and testimonials). Advertisers will be able to build awareness and consideration of their products and brand off Amazon more effectively using a video ad format. 

For full technical details, see our preview of the new `creativeType` for adGroups in our [Sponsored Display documentation](sponsored-display/3-0/openapi#/Ad%20groups/createAdGroups). 

#### Amazon Ads API webinar for April 2022 

Join Amazon Ads for a monthly webinar series on the Amazon Ads API to learn about recent product releases, product deep dives, and upcoming product releases. 

**North American series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_7OkAr9u5INOxMEK) 

**European series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_3QSzePdHBkfexfM)

We also offer a recording of our latest Amazon Ads API product updates webinar in the Partner Network. You can access the recording [here](https://advertising.amazon.com/partner-network/resources) via your Partner Network account and view on demand. 

### March 2022 release notes

#### Amazon Ads API webinar for March 2022 

Join Amazon Ads for a monthly webinar series on the Amazon Ads API to learn about recent product releases, product deep dives, and upcoming product releases. 

**North American series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_7OkAr9u5INOxMEK) 

**European series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_3QSzePdHBkfexfM)

We also offer a recording of our latest Amazon Ads API product updates webinar in the Partner Network. You can access the recording [here](https://advertising.amazon.com/partner-network/resources) via your Partner Network account and view on demand. 

#### Advertiser test accounts for Sponsored Products and Sponsored Brands APIs

Regions: All (excluding IN)

We’re launching Amazon Ads API test accounts for the Sponsored Products and Sponsored Brands APIs. Advertising test accounts are a type of advertising account dedicated to testing. Test accounts do not serve ads or have active billing, allowing integrators to make API requests to test and learn about API functionality. Our first milestone includes test account types based on vendor and author advertisers.

The [test account creation API](test-accounts/openapi) enables users to provision an advertising test account, which can then be used to test API functionality. 

- `POST /testAccounts` allows test account creation
- `GET /testAccounts` retrieves information about created test accounts

For more information on the test accounts API and the use of advertising test accounts, please see our [test accounts overview](guides/account-management/test-accounts/overview). 

#### Sponsored Display launches matched target reporting in the Amazon Ads API

Regions: US, CA, ES, FR, IT, DE, UK, AE, JP, IN, AU, NL, BR, MX

Matched target reporting is now available for Sponsored Display vendors and sellers registered in Amazon Brand Registry. With the release of this report, advertisers can dive deep into performance based on the product detail page where an ad is displayed.

The matched target report gives visibility into which ASINs and related product detail pages your ads appeared on and resulted in at least one click. For example, if you are using Sponsored Display product targeting to help reach audiences within a specific category like “running shoes,” you can not only analyze performance at the targeting clause level (i.e., the “running shoes” category), but also break down performance based on the product detail pages where your ads appeared within that category. You can use this data to harvest new product targeting options and refine your Sponsored Display product targeting strategy. 

Use the `segment` parameter with the value `matchedTarget` to segment campaign, ad group, or target reports to include the ASIN associated to the product page where your ads appeared. Note that the matched target report only includes ASINs of product pages with at least one ad click. Data in the matched target report is available from March 1, 2022; data available before that date may be incomplete. For full technical details, see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Reports/requestReport).

#### Turkey support

We have added support for Turkey (PL) to the API. For more details, see [API endpoints](reference/api-overview#api-endpoints), [bid constraints by marketplace](reference/concepts/limits#bid-constraints-by-marketplace), the `countryCode` parameter of the [registerProfile and registerBrands operations](reference/2/profiles), and the regional profile time zone codes section of the [profiles reference](reference/2/profiles).

#### Sponsored Brands launches new-to-brand and detail page views metrics for video

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SA, SG, UK, US

New-to-brand and detail page view metrics are now available for Sponsored Brands video campaigns for vendors and sellers registered in Amazon brand registry. New-to-brand metrics provide the number of first-time purchases for promoted products within the brand over a one-year lookback window. With new-to-brand metrics, advertisers receive campaign performance metrics such as total new-to-brand purchases and sales, new-to-brand purchase rate, and cost per new-to-brand customer. Detail page view metrics help you understand how many shoppers are visiting your product pages as a result of your sponsored brands campaigns. These metrics are calculated based on the number of detail page views generated within a 14-day attribution window.

With this launch, we are introducing a new advertising report that enables you to query ad-level performance metrics. In addition, there is a new optional value called `all` for `creativeType` that allows you to query both non-video and video campaigns. For the `ads` report type, `creativeType` can only be `all` (null or video is not allowed).

For more information see our updated [Sponsored Brands documentation](sponsored-brands/3-0/openapi#/Reports). These new metrics are available for campaign, ad group, target, and ad reports.

#### New Sponsored Display features availability by marketplace documentation

We released a list of all Sponsored Display features and their availability by marketplace. This content will help you understand what each feature does, what regions it is supported in, and what API resource corresponds to the feature. You can find this list under [Sponsored Display > Features support by marketplace](guides/sponsored-display/features).

### February 2022 release notes

#### Amazon Ads API webinar for March 2022 

Join Amazon Ads for a monthly webinar series on the Amazon Ads API to learn about recent product releases, product deep dives, and upcoming product releases. 

**North American series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_7OkAr9u5INOxMEK) 

**European series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_3QSzePdHBkfexfM)

We also offer a recording of our latest Amazon Ads API product updates webinar in the Partner Network. You can access the recording [here](https://advertising.amazon.com/partner-network/resources) via your Partner Network account and view on demand. 

#### Sponsored Display expands bid recommendations to vCPM-based campaigns

Regions: US, CA, UK, IT, FR, ES, DE, AE, JP

We’ve launched suggested bids for Sponsored Display vCPM-based campaigns. These recommendations will be provided for [Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display?ref_=a20m_us_wn_sd_050321_sd) views remarketing and product targeting through the Amazon Ads API across CA, DE, ES, FR, IT, JP, AE, UK, and the US.

Bid recommendations help remove the guesswork when selecting a bid. Now, when you create any CPC-based or vCPM-based Sponsored Display product targeting or views remarketing campaign, you’ll receive machine learning-powered bid recommendations. These recommendations are presented as a suggested bid and a bid range, and both are calculated by analyzing a group of winning bids for recent, similar ads within your category. Suggested bid and bid range update daily, based on the increase or decrease in competing bids and ads in each auction. If you’re unsure which bid to start with, we recommend advertisers select the suggested bid; this is the bid that our algorithms suggest will most likely help deliver impressions for an ad similar to yours.

For full technical details, please see our updated Sponsored Display [documentation](sponsored-display/3-0/openapi#/Bid%20Recommendations) or [recommendations developer guide](guides/sponsored-display/recommendations).

#### New sponsored ads reporting documentation

We released new overview and tutorial content that covers the reporting capabilities of the Amazon Ads API. This content will help you understand API reporting concepts and request your first report. You can also view a consolidated list of report types and metrics for Sponsored Products, Sponsored Brands, and Sponsored Display. It will also help you differentiate between API and advertising console reports.

- Find all reporting content under [Concepts > Reporting](concepts/reporting/overview).

We also added new documentation that introduces the snapshot feature of the Amazon Ads API. You can use this content to learn about requesting snapshots and how snapshots relate to reports.

- Find all snapshots content under [Concepts > Snapshots](guides/snapshots/overview/overview).

#### Amazon DSP campaign management third-party creative API

Regions: US

We are launching capabilities to manage third-party Amazon DSP display creatives through the Amazon Ads API. These capabilities are launching in beta in the United States. 

The campaign management third-party creative feature enables you to create, read, update, and retrieve HTML used to generate a preview for third-party-served display creatives. You can integrate Amazon DSP creative management capabilities with your existing Amazon Ads API-enabled applications. 

* [POST /dsp/creatives/thirdparty](dsp-campaigns/#/Third%20Party%20Creative/createThirdPartyCreative) creates one or more third-party creatives.
* [GET /dsp/creatives/thirdparty](dsp-campaigns/#/Third%20Party%20Creative/getThirdPartyCreatives) retrieves one or more third-party creatives.
* [PUT /dsp/creatives/thirdparty](dsp-campaigns/#/Third%20Party%20Creative/updateThirdPartyCreative) updates one or more third-party creatives.
* [POST /dsp/creatives/thirdparty/preview](dsp-campaigns/#Third%20Party%20Creative/previewThirdPartyCreative) posts a request to retrieve HTML content that can be used to create a preview for a third-party creative.

For more information on third-party creatives please see the [Help center](https://advertising.amazon.com/dsp/help/ss/en/creatives/dyn-ecomm/?#GRHZG63TKWBCZYLJ).

### January 2022 release notes

#### Theme-based bid suggestions for Sponsored Products now available in open beta

Theme-based bid suggestions, the next-generation bid suggestion service, helps advertisers choose a bid that aligns with their campaign goals. Bid suggestions include historical impact metrics based on similar products, and they help in choosing the right bid for the campaign. 

The first theme released provides suggested bids to generate sales. Metrics included in the response are weekly clicks and purchase orders received by similar products. For example, advertisers looking to generate more sales will call this API endpoint to get bid suggestions from the sales theme. They can then decide which bid to use based on historical results of similar products. 

The theme-based bid suggestion for sales helps you set up performant campaigns by providing bid suggestion transparency. You can now look at historical impact metrics that accompany suggested bids. You can request bid suggestions for new campaigns or for existing ad groups. The suggested bids are returned at the targeting clause level and the impact metrics are aggregated at the ad group level.

For full technical details, please see the [theme-based bid suggestions documentation](sponsored-products/3-0/openapi/prod#/ThemeBasedBidRecommendation/GetThemeBasedBidRecommendationForAdGroup_v1) or view the [quick-start guide](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide).

####  Enabling pre-bid invalid traffic filtering using Pixalate on Amazon owned-and-operated and third-party supply 

Regions: All

You can now enable pre-bid filtering for invalid traffic on Amazon owned-and-operated and third-party supply worldwide using [Pixalate’s](https://www.pixalate.com/) product, on the line item settings page. [Pixalate](https://www.pixalate.com/) can block invalid traffic from IPv4 and IPv6 addresses, user agents and device identifiers on Streaming TV, mobile or web supply, and mobile apps that were removed from the Google Play or Apple App stores in the last six months.

Enabling invalid traffic pre-bid filtering prevents you from running on supply that [Pixalate](https://www.pixalate.com/) deems invalid. This product can filter both general invalid traffic and sophisticated invalid traffic. 

This feature is available in version 3.1 of the line item resource for `STANDARD_DISPLAY`, `AAP_MOBILE_APP`, `AMAZON_MOBILE_DISPLAY`, and `VIDEO` line item types. It can be found in the Pixalate property under the targeting object. For full technical details, please select version 3.1 of the `/dsp/lineItems` resource using the Media type dropdown in our updated [reference documentation](dsp-campaigns/#/LineItem). 

#### Oracle Data Cloud's Pre-Bid by Moat Viewability on third-party supply

Amazon DSP campaign management APIs now support the Media Rating Council definition of viewability targeting using Pre-Bid by Moat Viewability. The objective and purpose of the Media Rating Council can be found on the [Media Rating Council home page](https://www.mediaratingcouncil.org/). This feature is available through the Amazon Ads API in the United States for standard display, mobile app, and video line items running on third-party supply sources. By enabling pre-bid viewability targeting, advertisers can target the appropriate viewability tier (20%+, 30%+, 40%+, 50%+, 60%+, 70%+, 80%+). Driven by Oracle Data Cloud’s industry-leading global measurement footprint, Pre-Bid by Moat Viewability can hep you reach/access viewable inventory down to the ad-size level. 

> [NOTE] The pre-bid viewability targeting is supported only on third-party supply. If it is applied to Amazon owned-and-operated supply and third-party supply, line items disregard this while delivering impressions for Amazon owned-and-operated supply. [Learn more about third-party pre-bid targeting](https://advertising.amazon.com/dsp/help#G692763N6EY3D8BT).

This feature is available in versions 3.0 and 3.1 of the line item resource for `STANDARD_DISPLAY`, `AAP_MOBILE_APP`, and `VIDEO` line item types. It can be found in the `oracleDataCloud` property under the targeting object. For full technical details, please select version 3.0 or 3.1 of the `/dsp/lineItems` resource using the Media type dropdown in our updated [reference documentation](dsp-campaigns/#/LineItem). 

#### Oracle Data Cloud's Standard Predicts on third-party supply

Amazon DSP campaign management APIs now support Oracle Data Cloud’s standard predicts product in the United States. Predicts is a contextual targeting feature that allows brands to capitalize on trending content and the associated inventory in real time, helping to drive greater relevancy and reach. Predicts helps you achieve your reach goals by dynamically expanding static keyword based targeting segments in real time on a daily basis and receiving impressions as the conversation evolves. This feature is available through the Amazon Ads API in the US for standard display, mobile app, and video line items running on third-party supply sources. Oracle’s standard predicts segments are built and maintained by Oracle. [Oracle’s custom predicts](https://advertising.amazon.com/en-us/resources/whats-new/oracle-data-cloud-custom-predicts) segments are custom built and maintained by brands and agencies themselves, or with the help of Oracle.

> [NOTE] The pre-bid standard predicts targeting is supported only on third-party supply. If it is applied to Amazon owned-and-operated supply and third-party supply, line items disregard this while delivering impressions for Amazon owned-and-operated supply. [Learn more about third-party pre-bid targeting](https://advertising.amazon.com/dsp/help#G692763N6EY3D8BT).

This feature is available in version 3.1 of the line item resource for `STANDARD_DISPLAY`, `AAP_MOBILE_APP`, and `VIDEO` line item types. It can be found in the `oracleDataCloud` property under the targeting object. For full technical details, please select version 3.1 of the `/dsp/lineItems` resource using the Media type dropdown in our updated [reference documentation](dsp-campaigns/#/LineItem). You can also refer to the discovery endpoint `/dsp/preBidTargeting/oracleDataCloud/standardPredicts` to retrieve standard predict pre-bid targeting data.

#### Amazon Ads API product updates webinar series

Join Amazon Ads for a monthly webinar series on the Amazon Ads API to learn about recent product releases, product deep dives, and upcoming product releases.

North American series: [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_7OkAr9u5INOxMEK)

European series: [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_3QSzePdHBkfexfM)

We also offer a recording of our latest Amazon Advertising API product updates webinar in the Partner Network. You can access [here](https://advertising.amazon.com/partner-network/resources) via your Partner Network account and view on demand. 

#### Amazon Ads API now supports hashed audiences creation in Amazon DSP

Regions: US, CA, JP, DE, UK, FR, IT, ES, IN, AU

Amazon Ads API users can now use the hashed records feature to ingest their owned audience records and create their own audiences for their Amazon DSP campaigns. Advertisers can further programmatically create audiences by using the [data provider](data-provider/openapi) feature with their hashed audiences.

With the hashed records feature, users can choose from the different attributes to create their own audiences and use them in their Amazon DSP campaigns. The onboarded audiences are seamlessly available within the advertiser’s Amazon DSP account.

This feature is available through the Amazon Ads API. To learn more, please see the [hashed records API documentation](data-provider/hashed-records#/Hashed%20Records).

#### Poland support

We have added support for Poland (PL) to the API. For more details, see [API endpoints](reference/api-overview#api-endpoints), [bid constraints by marketplace](reference/concepts/limits#bid-constraints-by-marketplace), the `countryCode` parameter of the [registerProfile and registerBrands operations](reference/2/profiles), and the regional profile time zone codes section of the [profiles reference](reference/2/profiles).

#### Sponsored Display expands bid optimization to Australia, Netherlands, and Mexico

Regions: AU, NL, MX

We've expanded bid optimization controls for Sponsored Display campaigns to AU, NL, and MX. You can now select "Optimize for viewable impressions", "Optimize for page visits", or "Optimize for conversions" and your campaigns will be optimized to help drive viewable impressions, page visits, or conversions for all audiences and product targeting campaigns across CA, DE, ES, FR, IT, JP, UAE, UK, MX, NL, AU, and the US.

Custom bid optimizations allow you to select bid optimizations based on campaign needs and helps you evaluate performance based on viewable impressions, clicks, or conversions. This means you can complement your targeting selection and creative customization with appropriate bid optimizations to meet your needs. You now have more controls over your campaigns and are no longer restricted by pre-defined bid optimizations. All existing campaigns in the expanded regions will default to "Optimize for page visits". These new flexible controls enable you to help you more effectively introduce new products, drive traffic, and engage new or existing shoppers.

For full technical details, including each `bidOptimization` available through the Amazon Ads API, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Ad%20groups/createAdGroups).


#### Sponsored Brands bidding by placement groups

Regions: US, CA, MX, UK, DE, FR, IT, ES, JP, IN, AU, NL, AE, SG, SA, BR

Sponsored Brands now offers bidding by placement groups controls for advertisers to better execute brand advertising strategies. The new capability gives advertisers more granular bidding controls over the various placements where their ads are shown. Advertisers can now adjust bids for detail pages, home pages, and other placements. You can optimize placement groups to help with your campaign performance and marketing goals. Please note that the video ad format is not supported, at this time.

The following APIs have been updated to support bidding by placement groups for Sponsored Brands campaigns:

* [/sb/campaigns](sponsored-brands/3-0/openapi#/Campaigns) and [/sb/draft/campaigns](sponsored-brands/3-0/openapi#/Drafts) now support bid adjustment at placement group level.
* `/v2/reports` now supports campaign placement level granularity at placement group level.

For full technical details, please refer to our updated [Sponsored Brands Ads API reference documentation](sponsored-brands/3-0/openapi#/Campaigns/createCampaigns).


### December 2021 release notes

#### 5000-record limit on profiles is now enforced

We have started enforcing a 5000-record limit in the response to [GET requests for profiles](reference/2/profiles#/Profiles/listProfiles). This limit was previously documented in our developer notes but was not regularly enforced. We are making this change to improve the performance of this endpoint. While we have already reached out to all application owners impacted by this change, if you have any questions please contact our support team [via our help desk](support/overviewview) or your Amazon representative.

#### Introducing a new version of Amazon DSP reporting APIs and a new experience for API authorization

We are introducing a new version of reporting endpoints for Amazon DSP. Users can now retrieve reports for advertiser accounts linked to their manager accounts. With this change, advertisers can grant an application access to their DSP advertiser accounts without granting access to the corresponding entity. This improvement enables advertisers that leverage Amazon DSP managed service model to retrieve reports via the API. This new version of the DSP reporting API also introduces simplifications to the developer experience. For example, it does not require a scope header and it can be used with profile endpoint. We made this simplification as part of a larger effort to improve the user experience for authorization.

Developers will notice the [new DSP reporting API](dsp-reports-beta-3p/#/Reports) does not require an `Amazon-Advertising-API-Scope` header. The new version of DSP reporting includes an `accountid` URL parameter, and this value can a DSP entity identifier or a DSP advertiser identifier. In the future, the parameter will also support manager account identifiers. Your entity identifier is available from the Amazon DSP web interface.

To learn more about this new experience, see the `accountId` parameter for each of the resources in the [new DSP reporting API documentation](dsp-reports-beta-3p/#/Reports).

#### Amazon DSP campaign management video creative API

Regions: US

We are launching capabilities to manage first party Amazon DSP video creatives through the Amazon Advertising API. These capabilities are launching as a beta in the US, at this time. 
 
The campaign management video creative feature enables you to create, read, update, and retrieve HTML used to generate a preview for first party served video creatives. You can integrate Amazon DSP creative management capabilities with your existing API-enabled applications.

* [POST /dsp/creatives/video](dsp-campaigns/#/Video%20Creative/createVideoCreatives) Create one or more video creatives.
* [GET /dsp/creatives/video](dsp-campaigns/#/Video%20Creative/getVideoCreative) Retrieve one or more video creatives.
* [PUT /dsp/creatives/video](dsp-campaigns/#/Video%20Creative/updateVideoCreatives) Update one or more video creatives.
* [POST /dsp/creatives/video/preview](dsp-campaigns/#/Video%20Creative/previewVideoCreative) Post a request to retrieve HTML content that can be used to create a preview for a video creative.

### November 2021 release notes

#### International localization of products is now available for Sponsored Products campaigns

Sellers and vendors worldwide can now copy and localize advertised products (ASINs/SKUs) from one geography to any combination of countries where Amazon Advertising is available. This is an enhancement to the existing LocalizeProduct API, which was previously only available to localize products across France, Germany, Italy, Spain and the United Kingdom or Canada, Mexico and the United States. This API also checks that the product is eligible for advertising by the advertiser in the target country before localizing the product. 

This feature can be used alongside the existing [keyword localization](localization/#/Keyword%20Localization), [targeting expression localization](localization/#/Targeting%20Expression%20Localization), [currency localization](localization/#/Currency%20Localization), and [profiles](reference/2/profiles#/Profiles/listProfiles) APIs. This will help you create Sponsored Products auto or manually targeted campaigns in different countries with minimal effort or local language knowledge. You can take campaign parameters (product targets, keywords, budget, bids and ASINs/SKUs) from one country and localize the parameters to launch campaigns in other countries. 

Learn more by reading our [reference documentation](localization).

#### International localization of keywords is now available for Sponsored Products campaigns

Sellers and vendors worldwide can now copy and localize keywords from one geography to any combination of countries where Amazon Advertising is available. This is an enhancement to the existing [keyword localization API](localization/#/Keyword%20Localization), which previously was only available to translate keywords across Chinese, English, French, German, Italian, and Spanish, but now works across additional language combinations where Amazon Advertising is available. Please see the [reference documentation](localization) for a full list of available languages. This API previously used a machine translation service to translate keywords, but for enhanced keyword translation it now leverages a machine-learning algorithm that uses a combination of similarity between words and Amazon Advertising and search data to find the best matching keyword in the target country. 

This feature can be used alongside the existing [product localization](localization/#/Product%20Localization), [targeting expression localization](localization/#/Targeting%20Expression%20Localization), [currency localization](localization/#/Currency%20Localization), and [profiles](reference/2/profiles#/Profiles/listProfiles) APIs. This will help you create Sponsored Products manually targeted campaigns in different countries with minimal effort or local language knowledge. You can take campaign parameters (keywords, product targets, budget, bids and ASINs/SKUs) from one country and localize the parameters to launch campaigns in other countries. 

Learn more by reading our [reference documentation](localization).

#### Amazon DSP campaign management responsive eCommerce creatives API and creative moderation API

Regions: US

We are launching capabilities to manage responsive eCommerce creatives, and retrieve your creative moderation status. These capabilities are launching as a beta in the US, at this time. 

The campaign management responsive eCommerce creative feature enabled advertisers to create, read, update, and retrieve HTML used to generate a preview first party served responsive eCommerce creatives through APIs.

The campaign management creative moderation status API enables advertisers to retrieve the moderation status of creatives. Responses include: approved, approved with exceptions, waiting for line item, not approved, and pending.
 
These new capabilities are built to achieve full parity with the Amazon DSP console. They enable you to save time and may increase business efficiency by automating manual creative management tasks. You can integrate Amazon DSP creative management capabilities with your existing API-enabled applications.

* [POST /dsp/creatives/rec/preview](dsp-campaigns/#/Responsive%20eCommerce%20Creative/previewRecCreative): Post a request to retrieve HTML content that can be used to create a preview for a Responsive eCommerce creative
* [POST /dsp/creatives/rec](dsp-campaigns/#/Responsive%20eCommerce%20Creative/createRecCreatives): Create one or more Responsive eCommerce creatives
    
* [GET /dsp/creatives/rec](dsp-campaigns/#/Responsive%20eCommerce%20Creative/getRecCreatives): Retrieve one or more Responsive eCommerce creatives
* [PUT /dsp/creatives/rec](dsp-campaigns/#/Responsive%20eCommerce%20Creative/updateRecCreatives): Update one or more Responsive eCommerce creatives
* [GET /dsp/moderation/creatives](dsp-campaigns/#/Moderation/getCreativeModeration): Retrieves the moderation status of a creative

#### Sponsored Display has added detail page views metrics on the Amazon Ads API

Detail page view metrics are now available for Sponsored Display campaigns for vendors and sellers registered in Amazon Brand Registry. Detail page views help you understand how many shoppers are visiting your product pages as a result of your Sponsored Display campaigns and are calculated based on the number of advertising attributed detail page views generated from a campaign that occurred within a 14-day attribution window.

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Reports), including the newly available metrics of `attributedDetailPageView14d` with click attribution for CPC campaigns and `viewAttributedDetailPageView14d` with click or view attribution for vCPM campaigns. These new metrics are available for campaign, adGroup, productAd, and target reports. 

#### Sponsored Display audiences expands custom lookback windows through the Amazon Ads API

With this launch, we added customizable lookback windows for both views and purchases remarketing audiences. For each audience, advertisers can now specify time periods of 7, 14, 30, 60, or 90 days to include shoppers who performed the action of visiting a detail page or purchasing a product. For remarketing audiences that purchased a product, we’re further extending to time periods of 180 and 365 days so advertisers can reach even more shoppers who purchased exact products, similar products, categories, and refined categories. We recommend using new-to-brand metrics to better measure and optimize in-flight Audiences campaigns to drive customer acquisition and brand loyalty.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting), including the new 'purchases' targeting with custom lookback windows that can be used with Audiences campaigns. For additional examples, please visit our [Audiences developer guide](guides/sponsored-display/audience-targeting).

#### Sponsored Display ad groups now available for vendors through the Amazon Ads API and Ads Console

With this launch, vendors can create ad groups to better organize and manage Sponsored Display campaigns. Advertisers can use ad groups to organize their ads based on brand, product, category, price range, or other classifications like theme or targeting strategy. 

Previously, an error was returned if an account associated with a vendor attempted to create a campaign that included multiple ad groups. With this change, both vendors and sellers can specify multiple ad groups in campaign creation and update operations. 

For more information, see our [Sponsored Display documentation](sponsored-display/3-0/openapi#/Ad%20groups/createAdGroups).

#### Amazon Ads API webinars 

Join Amazon Ads for a monthly webinar series on the Amazon Ads API to learn about recent product releases, product deep dives, and upcoming product releases. 

**North American series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_1EL01GAjf32GRiS) 

**European series** - [Register Now](https://amazonads.au1.qualtrics.com/jfe/form/SV_6FN4KU5xvP5LIjk)

We also offer a recording of our latest Amazon Ads API product updates webinar in the Partner Network. You can access [here](https://advertising.amazon.com/partner-network/resources) via your Partner Network account and view on demand. 

#### Sponsored Brands bid optimization and bid adjustment for new-to-brand customers now available on the Amazon Ads API

Regions: US

Sponsored Brands now offers bidding controls to maximize new-to-brand customers in advertisers’ campaigns. The new bid optimization strategy allows advertisers to choose between maximizing immediate sales, which is the default campaign behavior, or maximizing new-to-brand customers. The new bid adjustment allows advertisers to set higher bids for shoppers that haven’t purchased from their brand before. Please note that video ad format is not supported at this time. 

The following APIs have been updated to version 3.3 to support bid optimization strategy and bid adjustment for new to brand customers on Sponsored Brands: 

* [/sb/draft/campaigns](sponsored-brands/3-0/openapi#/Drafts)
* [/sb/campaigns](sponsored-brands/3-0/openapi#/Campaigns)

For full technical details, please select version 3.3 of each resource using the `Media type` dropdown in our updated [reference documentation](sponsored-brands/3-0/openapi#/Campaigns).

### October 2021 release notes

#### Sponsored Display expands to livestreams on Twitch

Marketplaces: US, CA, FR, IT, DE, ES, UK

We're expanding Sponsored Display on Twitch to help brands better reach audiences throughout the shopping and entertainment journey. In addition to reaching audiences on [Twitch's browse tab and directory pages](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-display-ads-on-twitch?ref_=a20m_us_wn_sd_102921_wn_sd_061421) with display ads, brands that use Sponsored Display will now be able to reach Amazon audiences with display ads on Twitch channels, during a creator's livestream, for the first time. These display ads are integrated seamlessly to Twitch livestreams, allowing brands to engage viewers without disrupting the streaming experience.

Twitch is a video streaming service with a hyper-engaged audience of over 30 million average daily visitors, nearly half of which are ages 18 to 34. Now any brand using Sponsored Display audiences in the US, CA, FR, IT, DE, ES, or UK can better help build their brand with new creative experiences and additional opportunities to engage or re-engage hard-to-reach, relevant Twitch audiences during their video viewing experience. For more information, see our recent [launch annoucement](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-display-expands-to-livestreams-on-twitch/?ref_=a20m_us_wn_gw).

#### Advertisers can now measure brand marketing impact using Amazon Brand Lift

Regions: US

Advertisers can now design, launch, and view reporting of Amazon Brand Lift studies from within the [Amazon DSP](https://advertising.amazon.com/solutions/products/amazon-dsp?ref_=a20m_us_wn_unbxd_dsp_102621_dsp) console and through the Amazon Ads API. Amazon Brand Lift helps advertisers quantify how their Amazon Ads campaigns are driving marketing objectives such as awareness, purchase intent, and ad recall. Amazon Brand Lift is powered by the [Amazon Shopper Panel](https://panel.amazon.com/?ref_=a20m_us_wn_unbxd_dsp_102621), an invitation-only, opt-in program where participants can earn monthly rewards by choosing to share receipts from purchases made outside of Amazon.com or complete short surveys from within a standalone mobile app.

Amazon Brand Lift is an easy, insightful, and privacy-safe way for advertisers to quantify the impact of upper and mid-funnel campaigns. With participation of the sizable, representative, and engaged Amazon Shopper Panel community, Amazon Brand Lift helps provide objective and concrete measurement results. The reporting provides insights such as how much an advertiser’s campaign has impacted the percentage of respondents who report being aware of a brand or interested in purchasing from a brand.

To create, manage, and retrieve results for their Amazon Brand Lift studies, please use the following endpoints:

* Create study: [POST/dsp/measurement/studies/brandLift](dsp-measurement/#Measurement/CreateDSPBrandLiftStudies)
* Update study details: [PUT/measurement/studies/brandLift](dsp-measurement/#Measurement/UpdateDSPBrandLiftStudies)
* Get results: [GET/measurement/studies/brandLift/{studyId}/result](dsp-measurement/#Measurement/GetDSPBrandLiftStudyResult)

Learn more about the Amazon Brand Lift APIs in our [technical documentation](dsp-measurement).


#### Use Brand Metrics to measure customer journey

Regions: US, CA, UK, IT, FR, DE, ES, JP

Brand Metrics is a new measurement solution that quantifies opportunities for your brands at each stage of the customer journey on Amazon, and helps brands understand the value of different shopping engagements that impact stages of that journey. With Brand Metrics you can now access awareness and consideration indices that compare brand performance to peers using models predictive of consideration and sales. Brand Metrics is built at scale to measure all shopping engagements with your brand on Amazon, not just ad-attributed engagements and quantifies the number of shoppers in the awareness, consideration, and purchase stages of the shopping journey with your brand. Brand Metrics also provides Return on Engagement, so you can measure the average historical sales following a consideration event or purchase.

Brand Metrics helps you...

* **Understand** your brand performance. Brand Metrics measures the number of actual on-Amazon shopper engagements.
* **Measure** the impact of your upper and mid funnel tactics to see how they contribute to shoppers moving through the purchase journey.
* **Evaluate** engagement metrics to understand the value of your branded intent, and how brand purchasers go on to generate additional sales in the 12 months following a purchase.
* **Monitor** your performance relative to your category and peers at each stage of the purchase journey and over time.
* **Optimize** your marketing and advertising efforts on Amazon to engage more shoppers and build your brand.

This release features two new endpoints - [/insights/brandMetrics/report](brand-metrics-openapi/#Report) and [/insights/brandMetrics/{reportId}](brand-metrics-openapi/#Report) to generate and retrieve Brand Metrics report in CSV or JSON format.  With these endpoints the developers can now create customizable reports by passing in a date range, a brand name, or by passing in specific metrics from a list of brand metrics.

For full technical details please see our [overview](guides/reporting/brand-metrics/overview) and [Brand Metrics reference documentation](brand-metrics-openapi) in the Amazon Ads API.

#### Announcing general availability of budget rules API for sponsored ads

Advertisers worldwide can now set up budget rules for their Sponsored Products, Sponsored Brands, and Sponsored Display campaigns through the Amazon Ads API. Budget rules help you plan budget changes in advance so you can spend less time manually adjusting during a campaign. You can set rules based on specific dates or performance goals.

Schedule-based budget rules can be set to increase daily budgets for any date range or for recommended events such as holidays or peak shopping events. For shopping events, we will provide recommended budgets based on historical shopping activities. Performance-based budget rules can be set to increase budgets based on campaign performance metrics. With budget rules, advertisers can reduce the likelihood of campaigns running out of budget, especially during peak days or holiday events. 

Visit our overview pages to learn more: [Sponsored Products Budget Rules overview](sponsored-products/budget-rules/overview), [Sponsored Brands Budget Rules overview](guides/budgets/rules/overview), [Sponsored Display Budget Rules overview](sponsored-display/budget-rules/overview).

#### Sweden support

We have added support for Sweden (SE) to the API. For more details see the API endpoints and keyword bid constraints by marketplace sections of the [overview](reference/api-overview), the regional profile time zone codes section of the [profiles reference](reference/2/profiles), and the countryCode parameter of the [registerProfile and registerBrands operations](reference/2/profiles).

#### Sponsored Brands API version bump 3.0 to 3.2

The Sponsored Brands API version has been updated from 3.0 to version 3.2. Currently in 3.0, in GET Store Spotlight campaign API call, Store Spotlight campaigns are returned as Product collection. Now in 3.2, the same GET call for Store Spotlight will return the campaign as Store Spotlight.

With this version bump, the advertisers can now create and manage Store Spotlight campaigns for their accounts. The integrator has a choice to specify in the header which version. In GET Store Spotlight call, if the header is not specified, the 3.2 or latest version will be returned (Store Spotlight will return as Store Spotlight). In the header, if version 3.0 is specified, Store Spotlight will return as Product Collection.

Learn more about the Sponsored Brands API in our [technical documentation](sponsored-brands/3-0/openapi).

#### Sponsored Brands Lifestyle and Store spotlight creatives are now available in open beta

 Two new ad types for Sponsored Brands are now live through the Amazon Ads API: custom images and Stores spotlight.  Advertisers can now use ad formats with custom imagery, and they can promote their Stores subpages and product categories with Stores spotlight. This release expands your ability to tell your brand story. 

With custom images in Sponsored Brands, you can engage shoppers browsing on Amazon with ads that feature rich imagery to tell your brand story. Custom images can include photos representing your brand or your product in use. These new images can help drive engagement. Ads in this beta may display a custom image, brand logo, and products.

With Stores spotlight ads, you can promote Stores subpages and product categories. This lets you feature Amazon Stores pages where shoppers can discover different product collections. You can showcase up to three subpages and will have the ability to customize the headline along with the subpage image and labels.

You can now build custom image and Stores spotlight campaigns through the existing campaigns resources as an optional add-on. Learn more in our Sponsored Brands [reference documentation](https://advertising.amazon.com/resources/ad-specs/ecommerce/?ref_=a20m_us_spcs_ecm) , [Sponsored Brands ads](https://advertising.amazon.com/solutions/products/sponsored-brands/?ref_=a20m_us_search_title)).  Learn more about the Sponsored Brands API in our [technical documentation](sponsored-brands/3-0/openapi#/Campaigns/getCampaign).

#### Creative asset library API is now available for images only

Creative asset library has launched its newest API. Creative asset library is a new centralized digital asset management and content serving solution for brands to store, organize, and reuse content across Amazon Ads, Amazon Retail, and for our partners.  With this release, you can use the CreativeAssets APIs to upload images for Ad Program and Register the uploaded asset to reuse them.

The creative asset library supplies fresh and fully-optimized brand content that powers consistent shopper experiences.  Once moderated and approved, assets in the library can be easily reused for new ad campaigns.

The creative assets library is available through the Amazon Ads API. We introduced three new features to help you manage your creative assets.  Learn more in our technical documentation in the links below: 

* [/assets/upload](creative-asset-library#/Creative%20Assets/getUploadLocation) returns a URL to upload an image or video asset to the chosen Creative assets library.
* [/assets/register](creative-asset-library#/Creative%20Assets/registerAsset) allows you to add custom tags and other metadata to assets to aid in asset management and usage.
* [/assets/search](creative-asset-library#/Creative%20Assets/searchAssets) returns a list of assets that meet the criteria specified in the request.
* [/assets](creative-asset-library#/Creative%20Assets/getAsset) returns detailed information on a single asset, including versions, metadata, and tags.

Learn more about creative assets in our [product documentation](https://advertising.amazon.com/resources/whats-new/creative-assets/?ref_=a20m_us_search_title).


#### Amazon DSP campaign management image creative API

Self-service Amazon DSP customers such as advertisers, agencies, and tool providers can now leverage the campaign management creative image API to programmatically create, read, update, and preview image creatives in the Amazon DSP.

The Amazon DSP campaign management image creative API enables customers to save time and may increase business efficiency by automating manual creative management tasks. Users can also integrate Amazon DSP creative management with the same tools or solutions used to manage other marketing channels, and the seller/vendor business for endemic advertisers.

* [POST/dsp/creatives/image](dsp-campaigns/#/ImageCreative/createImageCreatives): Creates one or more image creatives
* [GET/dsp/creatives/image](dsp-campaigns/#/ImageCreative/getImageCreatives): Get image creative(s)
* [PUT/dsp/creatives/image](dsp-campaigns/#/ImageCreative/updateImageCreatives): Updates one or more image creatives
* [POST/dsp/creatives/image/preview](dsp-campaigns/#/ImageCreative/previewImageCreative) Preview Image creative

For more information on creatives please see the [Help Center](https://advertising.amazon.com/dsp/help/ss/en/creatives/#GML894B56D9DBKU8).

#### Sponsored Display has expanded portfolio support to campaigns with vCPM billing and view attribution through the Amazon Ads API

Sponsored Display support for portfolios enables advertisers to group and organize Sponsored Products, Sponsored Brands, and Sponsored Display campaigns into collections that mirror the structure of their business. This functionality is now extended to Sponsored Display campaigns using the new bid optimization "Optimize for Viewable Impressions". You can measure the success of these new campaigns by leveraging metrics like viewable impressions and vCPM.

With portfolios, you now have the flexibility to arrange campaigns in a way that's meaningful for you, such as by brand, product category, or season across all self-service ad products. A key advantage portfolios offer is control over spend across campaigns. Once your portfolio budget is exhausted, all campaigns in that portfolio will automatically pause until you choose to reactivate them by increasing your portfolio budget.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Campaigns) in the Amazon Ads API, including the 'portfolioId' that can be associated to each campaign.

### September 2021 release notes

#### Sponsored Display audiences expands with purchases remarketing and custom lookback windows through the Amazon Ads API

We are expanding Sponsored Display's custom-built audiences with purchases remarketing to help advertisers drive repeat purchases and cultivate brand loyalty. The purchases remarketing strategy helps reach audiences based on historical purchase behaviors on Amazon. Advertisers can now remarket to shoppers who previously purchased their own products, related products, or products from specific retail categories. Like other custom-built audiences, advertisers can choose to refine targeting based on product insights, such as price and star rating.

With this launch, we also added customizable lookback windows for both views and purchases remarketing audiences. For each audience, advertisers can specify a time period of 7, 14, or 30 days to include shoppers who performed the action (visited a detail page or purchased a product).

Vendors and brand registered sellers are now better equipped to help drive brand and product loyalty using Sponsored Display. Advertisers can now leverage purchases remarketing audiences to encourage repeat purchases by remarketing to existing purchasers. This release also helps drive product consideration by cross-selling to audiences who recently bought related products. 

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting) in the Amazon Ads API, including the new 'purchases' targeting with custom lookback windows that can be used with Audiences campaigns. For additional examples, please visit our [Audiences Developer guide](guides/sponsored-display/audience-targeting).

#### Sponsored Display enables creative editing through the Amazon Ads API

Advertisers using Sponsored Display in US, CA, UK, DE, ES, FR, IT, AE, JP, IN, AU, NL, MX, and BR can now add or edit their headlines, logos, or [custom product images](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-display-custom-product-image-creative?ref_=a20m_us_wn_ftrd2_btn_sd_082421) for existing campaigns. After editing campaign creative, Sponsored Display campaigns will continue to serve the most recent approved creative, so campaign delivery will not be interrupted while new submissions await moderation approval. If your recent edit is rejected, the previously approved version will continue to serve. This allows you to edit and update images, headlines, and logos without worrying about moderation rejections disrupting your campaign delivery.

Advertisers can optimize campaign creative to better tell their story without having to create new campaigns or pause delivery. Rather than create new campaigns to reflect updated imagery, headlines, or logos, advertisers will be able to refresh or edit campaigns with creative elements based on their brand strategy, learnings, or seasonal relevance.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Creatives) in the Amazon Ads API and our [Sponsored Display creative developer guide](sponsored-display/creatives) to learn more about uploading or editing assets.

#### Sponsored Display product targeting and audiences are now available in Brazil through the Amazon Ads API

Sponsored Display product targeting and audiences are now available through the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in Brazil. As a reminder, Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences and product targeting help you reach shoppers while they browse and shop on Amazon, and used together these strategies can help drive awareness, consideration, and conversion. 

We recommend using new-to-brand metrics to better measure and optimize in-flight audiences campaigns to drive customer acquisition and brand loyalty. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories" or Amazon Audiences for In-market, Lifestyle, Interests, and Life Events segments.

For more information, see our [Sponsored Display documentation](sponsored-display/3-0/openapi#/Campaigns) in the Amazon Ads API.

#### Amazon Attribution is now available in the Amazon Ads console

Marketplaces: US, CA, UK, DE, FR, IT, ES.

Amazon Attribution has launched within the Amazon Ads console, so you're able to access Amazon Attribution alongside other self-service advertising products, including Sponsored Products, Sponsored Brands, Sponsored Display, and Stores. Eligible advertisers will automatically have access with no need to sign up or wait for approval. For API-integrated tool providers, this will simplify authorization and onboarding by leveraging the same advertising account profiles as sponsored ads products.
 
As part of this transition, advertisers already using Amazon Attribution should migrate their data to an advertising console account. Vendor advertisers can do so by logging into their Amazon Attribution DSP account and mapping their Amazon Attribution advertisers to their advertising console accounts. Seller accounts were automatically migrated based on the seller account ID associated to Amazon Attribution and advertising console accounts. If you are currently running Amazon Attribution measurement across live campaigns, they are not impacted. We recommend advertisers interested in measuring new campaigns do so using tags retrieved from their ad console profile.
 
[Amazon Attribution API endpoints](amazon-attribution-prod-3p) have not changed. The same endpoints may be used for both DSP-based profiles and advertising console profiles. 
 
For more information, please refer to the [Amazon Attribution API getting started page](guides/amazon-attribution/get-started).

#### Changes to daily sponsored ads budgeting policy

Marketplaces: US, CA, MX, UK, DE, FR, IT, ES, NL, JP, IN, AU, AR, SE, BR, SG

We updated our daily budgeting policy for Sponsored Products, Sponsored Brands, and Sponsored Display, allowing you to spend up to 25% more than the average daily budget on any given day. This allows you to benefit from high traffic days by using budgets from previous days when your spend is below your daily budget. The daily budget amount is averaged over the course of a month.

Visit our Support Center to [learn more](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GTGPQGUXNCTHE2DS).


#### New metrics for custom keyword ranking for Sponsored Products

Marketplaces: US, CA, MX, UK, DE, FR, IT, ES, NL, JP, IN, AU, AR, SE, BR, SG

New metrics and bids for all match types are now available for the Sponsored Products custom keyword ranking feature on the Amazon Ads API. You can now receive search term impression share, search term impression rank, and bid recommendations across all match types (broad, phrase, exact) for each recommended keyword when using custom keyword ranking. These metrics are returned as part of the custom keyword ranking call. Search term impression and rank are only available if your account has generated impressions for search terms matching the recommended keywords. 

These metrics help with making informed keyword selections by identifying potential keywords to target based on your impression share and impression rank of search terms. Search term impression share shows how your account-wide impression share for each search term compares to other advertisers, and the overall percentage of ad impressions you receive compared to other advertisers over last 30 days. For example, if you have a Sponsored Products impression share of 20% for a search term, it means that you won 20% of all Sponsored Products ad impressions for that search term. Similarly, search term rank helps you understand the level of activity on terms that you're targeting. For example, if you have a search term impression rank of 3 on a search term, it means that you received the third most Sponsored Products ad impressions for the same search term over the last 30 days. 

To access these additional metrics, call the API with the Media-Type: `application/vnd.spkeywordsrecommendation.v4+json`. Visit the [reference documentation](sponsored-products/3-0/openapi/prod#/Get%20ranked%20keywords%20recommendations/getRankedKeywordRecommendation) to learn more.

#### International localization of product targeting is now available for Sponsored Products advertising campaigns

Sellers and vendors worldwide can now copy product targeting specifications from one geography to any combination of countries where Amazon Ads is available. With this feature, you can match over categories, category refinements, products, negative targeted products, and negative targeted brands. 

This feature can be used alongside the existing [LocalizeKeyword](localization/#/Keyword%20Localization), [LocalizeProduct](localization/#/Product%20Localization), [LocalizeCurrency](localization/#/Currency%20Localization) and [Profiles](reference/2/profiles#/Profiles/listProfiles) endpoints. This will help you create Sponsored Products manually targeted campaigns in different countries with minimal effort or local language knowledge. You can take campaign parameters (product targets, budget, bids and ASINs/SKUs) from one country and localize the parameters to launch campaigns in other countries. 

Learn more with our reference documentation: [LocalizeTargetingExpression](localization/#/Targeting%20Expression%20Localization).

#### Rule-based bidding is now available for Sponsored Products

Rule-based bidding allows advertisers to optimize their Sponsored Products campaigns by enabling a new bidding control, rule-based bidding. This new control can drive sales while maintaining a ROAS greater than or equal to the ROAS guardrail for the campaign. Once enabled, Amazon Ads will adjust bids on each ad opportunity to drive more sales. Amazon Ads doesn't guarantee to meet ROAS guardrail. If we're not meeting the guardrail and ROAS decreases over a 21-day period (excluding special days like Prime Day) we will disable the rule and restore campaign's original bidding strategy and targeting clause bids. Optionally, in addition to ROAS, an advertiser can provide an average bid guardrail. If there is an average bid guardrail provided, and if the campaign CPC over a 7-day period exceeds the average bid guardrail by 125%, the rule will be disabled.

Advertisers can use the endpoints to create, retrieve, update, delete, and retrieve the status of the rule. Advertisers can also use the eligibility API to get a ROAS and average bid guardrail based on campaign's historical performance and similar advertisers.

See the [overview](guides/sponsored-products/rule-based-bidding/overview) for more information, then follow the [setting up](sponsored-products/rule-based-bidding/setting-up) process before you [get started](guides/sponsored-products/rule-based-bidding/getting-started) with the API.

#### Pre-moderation features now available for Sponsored Brands and Stores on the Amazon Ads API

Advertisers launching Sponsored Brands campaigns and Stores can now receive pre-moderation guidance through the Amazon Ads API. Pre-moderation endpoints provide real-time policy guidance based on creative elements and assist in fixing content that violates policy before being submitted for full moderation review. Guidance will include why a campaign might be rejected if submitted for moderation review, and links to relevant ad policies for additional explanation on the guidelines. Note that this feature is currently available in the US, UK, and Canada. Learn more with our [reference documentation](sponsored-brands/premoderation/openapi#/Pre%20Moderation).

### August 2021 release notes

#### Sponsored Display has expanded bid optimizations to enhance awareness campaigns with vCPM billing and view attribution through the Amazon Ads API

We've expanded our bidding controls for Sponsored Display campaigns; now advertisers and sellers registered in Amazon Brand Registry in the US, UK, CA, FR, ES, IT, DE, AE, JP, and IN can select "Optimize for Viewable Impressions" to help drive product awareness. When selecting this optimization, advertisers will express their bids for maximizing viewable impressions with [vCPM](https://advertising.amazon.com/help#GG44RFW942U9F6F5) charging (cost per thousand viewed impressions) and access view attribution metrics to help understand the value of these awareness campaigns. 

The new bid optimization "Optimize for Viewable Impressions" complements broader targeting options available through product targeting and audiences, helping advertisers reach more potential customers and creating more awareness for products and brands. You can measure the success of these new campaigns by leveraging metrics like viewable impressions and vCPM.

Regardless of targeting strategy, ads with this bid optimization may only serve on Amazon, and Sponsored Display reporting helps these advertisers optimize campaigns and learn more about the addressable audiences. Since these are [vCPM](https://advertising.amazon.com/help#GG44RFW942U9F6F5) campaigns, we recommend starting with bids of at least $5 USD to test, then adjusting based on early learnings. For full technical details, including the new 'reach' bidOptimization through the Amazon Ads API, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Ad%20groups/createAdGroups). 

#### Automated blocking of ineligible products for Sponsored Products and Sponsored Brands ads on the Amazon Ads API

Advertisers who create new Sponsored Products and Sponsored Brands campaigns will now be informed when their ads contain ineligible products. When the ads contains ineligible products, the Amazon Ads API will notify you and prevent creation for these ads. This feature helps advertisers streamline their process when conforming with Amazon Ads's existing product acceptability and ad policy requirements. Prior to this change, ads containing ineligible products were created but required advertisers to manually troubleshoot issues by using Product Ads Extended data. There has been no change to Amazon Ads's product acceptability and ad policy requirements as part of this release. 

Advertisers can use this product eligibility feature to get more information about their products. You can retrieve the Sponsored Products and Sponsored Brands eligibility status for a given list of products associated in your Seller Central or Vendor Central account. If products are ineligible for advertising, a provided status update will explain why the product is ineligible so that you can take action to activate them. For more information, see the [reference documentation](eligibility-prod-3p).

#### Sponsored Display launches portfolio support through the Amazon Ads API 

Portfolio support is now available for Sponsored Display vendors and sellers registered in Amazon Brand Registry in the US, UK, CA, FR, ES, IT, DE, AE, JP, IN, AU, NL, and MX.

Sponsored Display support for portfolios enables advertisers to group and organize Sponsored Products, Sponsored Brands, and Sponsored Display campaigns into collections that mirror the structure of their business. With portfolios, you now have the flexibility to arrange campaigns in a way that's meaningful for you, such as by brand, product category, or season across all self-service ad products. A key advantage portfolios offer is control over spend across campaigns. Once your portfolio budget is exhausted, all campaigns in that portfolio will automatically pause until you choose to reactivate them by increasing your portfolio budget.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Campaigns) in the Amazon Ads API, including the 'portfolioId' that can be associated to each campaign.


#### Sponsored Display product targeting and audiences are now available in Mexico through the Amazon Ads API

Sponsored Display product targeting and audiences are now available on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in Mexico. As a reminder, Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences & product targeting help you reach shoppers while they browse and shop on Amazon. Used together these strategies can help drive awareness, consideration, and conversion. 

We recommend using new-to-brand metrics to better measure and optimize in-flight audiences campaign to drive customer acquisition and brand loyalty. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories" or Amazon Audiences for In-market, Lifestyle, Interests, and Life Events segments.

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Campaigns) in the Amazon Ads API.

#### Branded Search metric is now available for DSP reports through the Amazon Ads API

Marketplaces: US, CA, MX, BR, UK, DE, FR, IT, ES, NL, IN, UAE, JP, AU, and SA

The Branded Search metric is now available for Amazon DSP customers worldwide for campaign reports. This metric allows advertisers to quantify how many customers executed a search queries that contained an advertiser's brand name after seeing an ad for their campaign. This works by matching the brands of the featured ASINs within a campaign with search queries containing those brand names on Amazon. The metric counts a conversion if the user viewed or clicked an ad for the campaign prior to the search.

Refer to the [DSP reports documentation](dsp-reports-beta-3p/#/Reports) for details on how to request these metrics.

#### Video view metrics now available for Sponsored Brands video

Marketplaces: US, UK, DE, FR, IT, ES, IN, JP, CA, MX, AU, SA, SG, UAE, and NL

Advertisers can now measure the performance of their Sponsored Brands video campaigns against a new set of video viewing metrics, available for download in the report center or through the Amazon Ads API. The new metrics include: viewable impressions, view-through rate (VTR), viewable click-through rate (vCTR), 5 second views, 5 second view rate, video first quartile, video midpoint, video third quartile, video completes, and unmutes. Video view metrics are available in downloadable reports by keyword, campaign, and campaign placement.

#### Sponsored Display bid recommendations are now generally available for product targeting and audiences through the Amazon Ads API

Sponsored Display bid recommendations are now available in the Amazon Ads API for vendors and sellers registered within Amazon Brand Registry in the US, CA, DE, UK, FR, IT, ES, JP and UAE. Bid recommendations help remove the guesswork when selecting a bid. Now, when you create any Sponsored Display product targeting or views remarketing campaign, you'll receive machine learning powered bid recommendations. These recommendations are presented as a suggested bid and a bid range, and both are calculated by analyzing a group of winning bids for recent, similar ads within your category. Suggested bid and bid range update daily, based on the increase or decrease in competing bids and ads in each auction. The bid recommendation is provided at the target level using the exact structure of existing targets, which enables you to programmatically reuse targeting clauses and obtain bid recommendations.

For full technical details, please see our updated [Sponsored Display recommendations developer guide](guides/sponsored-display/recommendations) in the Amazon Ads API. You can also refer to the [reference documentation](sponsored-display/3-0/openapi#/Bid%20Recommendations).

### July 2021 release notes

#### Amazon DSP campaign management (beta) functionality is now available in Canada, Mexico and Brazil

Self-service customers can now leverage the Amazon Ads API to manage Amazon DSP ad campaigns in Canada, Mexico and Brazil. Amazon DSP campaign management functionality enables you to: 

* Create, read and update orders and line items
* Retrieve pertinent information required to manage orders and line items such as domain lists, IAB content categories, and supply sources
* Read, add, and remove creatives to and from line items

For more information, see our [reference documentation](dsp-campaigns). 

#### Amazon DSP now supports 'Environment type' breakdowns in reporting 

A new 'environment type' breakdown has been added to the technology dimension for users of the Amazon DSP API. This option allows you to create a report summarized by the environment (i.e. app or web) where customers saw ads. This is in addition to the 'device type', 'browser', 'browser version', and 'operating system' that already exist in the technology dimension.

To request these new reports, you will need to specify TECHNOLOGY in the 'type' object and ENVIRONMENT_TYPE in the dimensions list. The 'environment type' breakdown will help you understand how your campaign performed on apps and websites, which may help inform optimizations for future campaigns. 

You can refer to the [documentation](dsp-reports-beta-3p/#/Reports) for details on how to request these new reports. If you are already specifying a version in the `Accept` header, you must update to `application/vnd.dspcreatereports.v2_2+json` to access the new features. The new features are available by default if you do not specify a version.

#### Planned Maintenance for DSP Reporting API Services in all regions, 6PM UTC to 8PM UTC, July 29, 2021

The DSP Reporting API in all regions will be unavailable for approximately 15- 20 minutes between 6 PM UTC to 8 PM UTC on July 29, 2021 for planned maintenance. DSP Reporting API users will see failures for operations that create and retrieve reports operations during this time period. No other Amazon Ads API services are impacted.

#### Branded Search metric is now available for DSP reports through the Amazon Ads API

The Branded Search metric is now available for Amazon DSP customers worldwide for campaign reports. This metric allows advertisers to quantify how many customers executed a search queries contained an advertiser's brand name after seeing an ad for their campaign. This works by matching the brands of the featured ASINs within a campaign with search queries containing those brand names on Amazon. The metric counts a conversion if the user viewed or clicked an ad for the campaign prior to the search.

You can refer to the [DSP reports documentation](dsp-reports-beta-3p/#/Reports) for details on how to request these metrics.

#### Bid suggestions now available for Sponsored Brands video on the Amazon Ads API

Sponsored Brands video now offers bid suggestions for Product attribute targeting and Keywords campaigns.  

Bid suggestions are based on historical winning bids of impressions and auction data. The new capability provides advertisers with a range of bids they should consider setting to, in order to increase their chances for winning and delivering impressions. As a consequence, advertisers can save time on bid optimizations and increase their return while spending their budget. For more information see the Sponsored Brands Suggested bid and [bid range reference](https://advertising.amazon.com/help?entityId=ENTITY281Q1OGLKKHT9#GYZFLBC2FUPH6M36)

Sponsored Brands APIs that previously only supported bid recommendations for product collection or Store spotlight ad formats, now support video format as well. For full technical details, please refer to our updated [reference documentation](sponsored-brands/3-0/openapi#/Bid%20recommendations).

The following API has been updated to support this launch:

* /sb/recommendations/bids now supports bid suggestions for video creative ads. Use new parameter adFormat in request to specify the ad creative type.

#### DoubleVerify's brand suitability tiers available on third-party supply through the Amazon DSP

Amazon DSP now supports DoubleVerify's (DV) next evolution of brand safety solution: Brand Suitability Tiers. This will allow alignment of brand safety and suitability settings with the advertiser's unique standards. This feature is available for standard display, mobile app, and video line items running on third-party supply sources.

With this release, DV will align brand safety product functionality with approach put forth in the American Association of Advertising Agencies (4A's), Advertising Protection Bureau (APB), World Federation of Advertisers (WFA), and Global Alliance for Responsible Media (GARM) standards, which is designed to transition the industry to a more consistent approach for protecting brand reputation while preserving scale and supporting publishers.

You must update to the version 3 accept headers in order to access the new features. If you do not specify a version, the new features are available by default. For more details, refer to our [reference documentation](dsp-campaigns).

Note that DoubleVerify's brand suitability tiers are supported only on third-party supply. If it is applied on line items targeting both Amazon owned-and-operated supply and third-party supply, line items disregard this for Amazon owned-and-operated supply. For more information on third-party pre-bid targeting, refer to this [help article](https://advertising.amazon.com/dsp/help#G692763N6EY3D8BT).

#### New product targeting capabilities available for Sponsored Brands video on the Amazon Ads API

Sponsored Brands video now offers a new, simple way to execute brand advertising strategies such as category expansion and brand reach strategies. The new product targeting option allows you to target ads by categories and refine by brand, price, or star reviews. With product targeting, you can engage shoppers that are exploring at a category level. Reporting is now available for campaigns that use product targeting expressions. For more information see the Sponsored Brands [product targeting reference](https://advertising.amazon.com/help?entityId=ENTITY281Q1OGLKKHT9#G3XAU5G7C2JTNQTM).

Sponsored Brands APIs that previously only supported product targeting for product collection or Store spotlight creative formats, now support video format as well. For full technical details, please refer to our updated [reference documentation](sponsored-brands/3-0/openapi#/Product%20targeting).

The following APIs have been updated to support product targeting for Sponsored Brands video campaigns:

* `/sb/campaigns` and `/sb/draft/campaigns` endpoints now support managing video format campaigns with product targets.
* `/sb/targets` and `/sb/negativeTargets` endpoints now support managing targets for video campaigns.
* `/v2/reports` endpoints now support product targeted video campaigns.

#### Product targeting recommendations with themes

Sponsored Products advertisers can now receive suggestions for product targeting campaigns grouped by themes via the API. Given an advertised product (ASIN) as input, this API returns a list of ASINs as suggested targets. We generate these suggestions using your ad's historical performance, items that shoppers frequently view and purchase together, and various other techniques. Suggested targets can be retrieved either as a single list, or grouped by theme. See below for currently available themes:

* **Top converting targets** – These ASINs generated conversions for the input ASIN in the past 30 days (e.g. your product appeared as an ad on the detail page of these items, and a shopper clicked and purchased your item). The suggested ASINs under this theme are sorted in decreasing order of sales generated for your promoted item.
* **Similar items (frequently viewed together)** – Items that shoppers frequently view and click along with your advertised item during a shopping session.
* **Complements** – Items that are frequently purchased together as complements. For example, if you are promoting a tennis racquet, you may see tennis balls recommended under this theme.
* **Similar items with low ratings and reviews** – Subset of the 'similar items' theme containing items that are rated lower than 3 stars and/or with fewer than 5 reviews. 
* **Books with similar readership** (currently available for e-books only)– Other e-books that Kindle readers often read along-with your advertised book.

> [NOTE] Availability of themes differs by input ASIN - some ASINs may not have all above themes available.

The feature is available at `/sp/targets/products/recommendations` under the v3 Product Targeting APIs. With this release, support for the v2 product recommendations API (`/v2/sp/targets/productRecommendations`) will be limited to critical bug fixes. The v2 endpoint will be available until Dec 31, 2021. Starting January 1, 2021 API callers will then be required to migrate to v3 to avoid service interruption. We recommend updating to the new endpoints earlier than October 31, 2021 to allow time for testing, and to take advantage of the latest functionality.

For full technical details, please visit [Sponsored Products v3](sponsored-products/3-0/openapi/prod#/Product%20Targeting) documentation. 

#### Sponsored Display creative customization for headline, logo, and images are now available through the Amazon Ads API

Sponsored Display creative customization for headline, logo, and images are now available on the Amazon Ads API in the US, UK, FR, ES, IT, DE, UAE, JP, CA, and IN.

Advertisers can now personalize their Sponsored Display product targeting and audiences ad creatives with custom headlines, brand logos, and images which will help advertisers create more engaging ad creatives to convey their brand message. As a part of this release, Sponsored Display enables custom image creatives, a new option for advertisers to replace default product images with a custom image of their choice. Newly supported custom product images allow for brands to now upload their own graphics, including those with bespoke background colors and art direction, to help convey their brand and product story.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Creatives) in the Amazon Ads API and our [Sponsored Display creative developer guide](sponsored-display/creatives) to learn more about uploading assets to the asset library.

#### Similar product targeting capability is now available for Sponsored Display through the Amazon Ads API

Similar product targeting capability is now available for Sponsored Display campaigns on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in the US, CA, UK, DE, FR, IT, ES, AE, JP, AU, NL and IN. 

Sponsored Display advertisers can now easily broaden their targeting scope and drive more scale by adding similar products to the targeting.  Using a single targeting clause of "similarProduct", advertisers can target additional products that are similar to those being advertised. The products within similar product targeting are based on the advertised product and are dynamically generated using machine learning models that are built on retail data. Similar targeted products have a higher probability of driving traffic (improved CTR) to the advertised product. This helps advertisers launch campaigns faster and improves product consideration by providing a larger number of products to target. 

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting) in the Amazon Ads API.

#### Sponsored Display audiences is now available in Australia and the Netherlands through the Amazon Ads API

Sponsored Display audiences is now available on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in the NL and AU. This strategy helps you reach shoppers on Amazon based on shopping actions. As a reminder, Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences & product targeting help you reach shoppers while they browse and shop on Amazon. Used together these strategies can help drive awareness, consideration, and conversion. We recommend using *new-to-brand metrics* to better measure and optimize in-flight audiences campaign to drive customer acquisition and brand loyalty. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories" or Amazon Audiences for In-market, Lifestyle, Interests, and Life Events segments.

For more information see our updated [Sponsored Display documentation](guides/sponsored-display/audience-targeting) in the Amazon Ads API.

#### Updates to Sponsored Products productAds extended resource

The ad status `NOT_IN_BUYBOX` is no longer supported. The changes to Sponsored Products API are:

* GET `/v2/sp/productAds/extended` serving status field no longer returns `NOT_IN_BUYBOX` as an enum value.

For more information, please refer to the [developer notes](reference/concepts/developer-notes) section and the API [reference documentation](sponsored-products/2-0/openapi#/Product%20ads/getProductAdEx).

### June 2021 release notes

#### Amazon DSP updates to product and pixel conversion tracking

We have released an update to the way product and pixel conversion tracking are associated to an order. The current endpoint [/dsp/orders/{orderId}/conversionTracking](dsp-campaigns/#/Order/GetConversionTrackings) is being replaced by two separate endpoints for products and pixels: 

* [/dsp/orders/{orderId}/conversionTracking/products](dsp-campaigns/#/Order/updateProductsByOrderId) is used for retrieving, adding, and removing tracked products to an order. We increased the maximum number of ASINs you can add to an order in the request payload from 1,000 to 2,000 ASINs. Alternatively, you can upload a csv file with up to 50,000 ASINs. You can now choose to receive a list or a csv file of ASINs when retrieving tracked ASINs for an order.
* [/dsp/orders/{orderId}/conversionTracking/pixels](dsp-campaigns/#/Order/getPixelsByOrderId) is used for associating existing pixels to an order. 

We recommend migrating to the new endpoints to take advantage of these new features, as the existing `/conversionTracking` endpoint will be deprecated by 12/31/2021. For more details, refer to our [reference documentation](dsp-campaigns).

#### Product metadata is now available via the API

With the [/product/metadata](product-metadata/#Product%20selector) API, advertisers can now retrieve product metadata such as inventory status, price, eligibility status, and product details for SKUS or ASINs in their product catalog. Users can leverage this information to launch, manage or optimize their Amazon Ads campaigns. This functionality is available to Sellers, Vendors, and Authors.

#### Insights on overlapping audiences are now generally available with updates on the Amazon Ads API

Amazon DSP and Sponsored Display advertisers can now retrieve insights on top audiences that overlap with a given audience using the Amazon Ads API. Overlapping audiences help surface other audiences that are similar to your intended audience and that you may want to consider adding to your display campaigns. 

A new version of the API is available with three new features to help you generate and apply insights more effectively:

* *maxResults* allows you to specify the number of overlapping audiences up to 500 you'd like returned in one call.
* *forecastedDailyReach* shows an estimate of the number of unique devices that can be reached across all inventory types when the audience is included in a campaign.
* *forecastedDailyImpressions* shows an estimate of the number of available impressions across all inventory types when the audience is included in a campaign. 

To use the API we recommend first integrating with [/audiences/list](audiences/#/Discovery/listAudiences)API. This API will allow you to search for available audiences, which can then be used as an input to generate overlapping audiences.

For more information, please refer to our [reference documentation](insights/#/Audience%20insights/insightsGetAudiencesOverlappingAudiences).

#### Sponsored Display bid optimization controls for product targeting & audiences campaigns are now available through the Amazon Ads API

Sponsored Display now includes additional controls to optimize bids based on campaign objectives. When building campaigns, advertiser can choose between higher consideration or conversion goals. When selecting "clicks", ads will serve to audiences more likely to click on your ad. These campaigns will bid "down only" and are more likely to have a higher click through rate and lower cost per click. Advertisers may want to drive more clicks when building awareness for their products. When selecting "conversions", the bids will be optimized for higher conversion rates, showing your ads to more shoppers more likely to purchase the product. When selected, ads will serve to audiences who are more likely to click on the ad and then purchase the product within 14 days. These campaigns bid "up and down" and are more likely to have a higher click conversion rate. Advertisers may want to drive more conversions to increase sales & build customer loyalty.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Ad%20groups) in the Amazon Ads API.

#### Sponsored Display audiences has expanded to include Twitch display placements 

Sponsored Display audiences campaigns have now launched on [Twitch](https://www.twitch.tv/directory), a live streaming service and global community for content spanning gaming, entertainment, music, sports, and more, in the US, CA, FR, IT, DE, ES, and UK. Now all Sponsored Display audiences campaigns are eligible to serve on the Twitch Browse and Discovery pages to help advertisers reach highly-engaged and relevant audiences watching live-streamed content on Twitch.

#### Sponsored Display audiences is now available in India through the Amazon Ads API

Sponsored Display audiences is now available on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in India. This strategy helps you reach shoppers on Amazon based on shopping actions. As a reminder, Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences & product targeting help you reach shoppers while they browse and shop on Amazon. Used together these strategies can help drive awareness, consideration, and conversion. We recommend using *new-to-brand metrics* to better measure and optimize in-flight audiences campaign to drive customer acquisition and brand loyalty. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories" or Amazon Audiences for In-market, Lifestyle, Interests, and Life Events segments.

For more information see our updated [Sponsored Display documentation](guides/sponsored-display/audience-targeting) in the Amazon Ads API.

#### Updated authorization and authentication content

We've updated the [setting up](guides/onboarding/overview) section of the Amazon Ads Advanced Tools Center that describes authorization and authentication in the Amazon Ads API. The updated content describes how the authorization (OAuth 2.0) flow is implemented, and more clearly aligns each Amazon authorization and authentication API with each step of the flow. We'd appreciate your feedback on this new content, so please click on the "leave feedback" widget in the site header and give us a thumbs up or thumbs down with any comments you have. 

#### Amazon Attribution API now supports product-level reports

Vendors and registered sellers in the US, CA, UK, DE, FR, IT, and ES can now access product-level daily reporting for Amazon Attribution at the campaignId and adgroupId levels through Amazon Ads API. Previously, Amazon Attribution API customers were only able to access daily reporting without visibility into the purchased products included in their conversion and sales reporting.

With the launch of the new Products Report available on the Amazon Attribution API, you can now gain insight into the specific products purchased in Amazon's store, including performance across promoted products and brand halo products as result of non-Amazon marketing. You can use this insight to help optimize campaigns in-flight or plan future marketing strategies, including opportunities to re-market, cross-sell, or promote new products. When combined with insights from other retail and advertising reports, you have a more comprehensive view of your brand's performance on Amazon.

The Products Report can be retrieved by adding an optional reportType parameter to your existing Amazon Attribution reporting request body. You will need to enter PRODUCTS as the parameter value and list the set of desired metrics. If you enter the reportType as PRODUCTS and do not list any metrics, your report will contain all PRODUCTS metrics by default. For a list of PRODUCTS metrics and more information on how you can retrieve product-level reporting, see the [reference documentation](guides/amazon-attribution/how-to).


### May 2021 release notes

#### Sponsored Display product targeting is now available in Australia, Netherlands, and India through the Amazon Ads API

Sponsored Display product targeting is now available in the Amazon Ads API for sellers in AU, NL, and IN for professional sellers enrolled in the Amazon Brand Registry and vendors. Product targeting for Sponsored Display gives sellers and vendors additional opportunities to reach and re-engage shoppers. Product targeting enables advertisers to promote product discovery, measure campaigns more effectively by targeting specific products or relevant product categories on Amazon, and gain greater line of sight into campaign optimization. Sellers and vendors now have the ability to: target multiple ASINs within campaigns, access detailed reporting, leverage flexible bidding options, and broaden category targeting capabilities. 

Product targeting brings audiences closer to the point of purchase and helps brands stay top of mind. We recommend using *new-to-brand* metrics to better measure and optimize in-flight audiences campaign to drive customer acquisition and brand loyalty. These ads may appear alongside product description pages, customer reviews, shopping results pages, or under the featured offer. 

For more information see our updated [Sponsored Display documentation](sponsored-display/product-targeting) in the Amazon Ads API.

#### Sponsored Display bid recommendations (open beta) are now available for product targeting through the Amazon Ads API

Sponsored Display bid recommendations are now available in the Amazon Ads API for vendors and sellers registered within Amazon Brand Registry in the US, CA, DE, UK, FR, IT, ES, JP and UAE. Bid recommendations provides recommended bids for target clauses with median, lower and upper boundaries based on historical bid values for the ASINs in a recent period. These recommendations will help advertiser select a bid which will more likely win impressions and generate clicks for the targeted ad to the shopper. The bid recommendation is provided at the target level using the exact structure of existing target clauses, which enables you to programmatically reuse campaign target clauses and obtain bid recommendations.

For full technical details, please see our updated [Sponsored Display recommendations developer guide](guides/sponsored-display/recommendations) in the Amazon Ads API.


### April 2021 release notes

#### Sponsored Display brand safety (open beta) is now available through the Amazon Ads API

Sponsored Display brand safety is now available in the Amazon Ads API for vendors and sellers registered within Amazon Brand Registry in the US, CA, DE, UK, FR, IT, ES and UAE. Sponsored Display can run ads onsite and offsite on third-party sites and mobile apps. The brand safety functionality gives you more control on where your ads serve to protect your brand and block ads from delivering on third-party sites that don't align with your brand. You can upload a list of domains / mobile applications that you want to prevent ads from showing in your Sponsored Display campaigns. This feature will be available at the advertiser level, allowing each advertiser to control where their ads are shown off-Amazon. 

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/brand-safety) in the Amazon Ads API.

#### Sponsored Display audiences is now available in Japan through the Amazon Ads API

Sponsored Display audiences is now available on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in Japan. This strategy helps you reach shoppers on Amazon based on shopping actions. As a reminder, Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences & product targeting help you reach shoppers while they browse and shop on Amazon. Used together these strategies can help drive awareness, consideration, and conversion. We recommend using *new-to-brand metrics* to better measure and optimize in-flight audiences campaign to drive customer acquisition and brand loyalty. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories". 

For more information see our updated [Sponsored Display documentation](guides/sponsored-display/audience-targeting) in the Amazon Ads API.

#### Amazon DSP campaign management functionality now supports video line items

Support for managing video ads is now available on the Amazon Ads API for Amazon DSP customers in the US. With this launch, you can manage your streaming TV ads and Online video ads to engage your audiences at scale. 

* With Amazon streaming TV ads video ads, you can showcase your brand alongside premium, brand-safe streaming content, including the latest movies and TV shows on IMDb TV, across broadcast and network apps, and alongside live sports and news. 
* With online video ads, you can reach audiences with both in-stream and out-stream video ads on Amazon affiliated sites such as IMDb.com and Twitch, and across the web on leading publishers' sites through Amazon Publisher Services and third-party exchanges.

This new functionality enables you to create, read and update video line items. Additionally, you can read video creatives, and add or remove video creatives to and from line items. For more information, see our [reference documentation](dsp-campaigns).

#### New and updated product targeting capabilities available for Sponsored Products on the Amazon Ads API

We have introduced three new features to help you create and manage product targeting campaigns more effectively: 

* [/sp/targets/categories](sponsored-products/3-0/openapi/prod#/Product%20Targeting/getTargetableCategories) returns all categories available to target an advertiser's country. The information is returned in a hierarchy, with each parent category (e.g. "shoes") linked to its child categories (e.g. "running shoes", "dress shoes", "hiking shoes" etc.).
* [/sp/targets/products/count](sponsored-products/3-0/openapi/prod#/Product%20Targeting/getTargetableASINCounts) returns an approximate number of products within each category. You can use this while creating category targets to get a sense of how many underlying products will be targeted.
* [/sp/negativeTargets/brands/recommendations](sponsored-products/3-0/openapi/prod#/Product%20Targeting/getNegativeBrands) provides suggested brands to negatively target when targeting a category.

In addition, we have released new versions of existing product targeting APIs: 

* [/v2/sp/targets/brands](sponsored-products/2-0/openapi#/Product%20targeting/getBrandRecommendations) endpoint has been updated to [/sp/negativeTargets/brands/search](sponsored-products/3-0/openapi/prod#/Product%20Targeting/searchBrands). Endpoint returns brands for a given search keyword.
* [/v2/sp/targets/categories/refinements](sponsored-products/2-0/openapi#/Product%20targeting/getRefinementsForCategory) has been updated to [/sp/targets/category/{categoryId}/refinements](sponsored-products/3-0/openapi/prod#/Product%20Targeting/getRefinementsForCategory). Endpoint returns available options to refine a category target - e.g. brands, genre, age range, etc.
* [/v2/sp/targets/categories](sponsored-products/2-0/openapi#/Product%20targeting/getTargetingCategories) endpoint has been updated to [/sp/targets/categories/recommendations](sponsored-products/3-0/openapi/prod#/Product%20Targeting/getTargetableCategories). Endpoint returns suggested categories to target based on the input product.

The updated endpoints provide more intuitive path and field names, improved version management to minimize breaking changes on your end, and improved error codes with more details. With the release of the v3 endpoints, support for v2 endpoints will be limited to critical bug fixes. The v2 endpoints will be available until October 31, 2021. Starting November 1, 2021 API callers will then be required to migrate to v3 to avoid service interruption. We recommend updating to the new endpoints earlier than October 31, 2021 to allow time for testing, and to take advantage of the latest functionality.

### March 2021 release notes

#### Singapore support

We have added support for Singapore (SG) to the API. For more details see the API endpoints and keyword bid constraints by marketplace sections of the [overview](reference/api-overview), the regional profile time zone codes section of the [profiles reference](reference/2/profiles), and the countryCode parameter of the [registerProfile and registerBrands operations](reference/2/profiles).

#### Amazon Audience explorer is now available for Amazon DSP and Sponsored Display customers worldwide on Amazon Ads API 

Advertisers, agencies, and tool providers can now leverage the Amazon Ads API to discover audiences for their advertising campaigns. You can use this API to search audiences by the audience name, or audience Id. You can also apply filters to the audiences to narrow down your search.  The API returns all audiences with the specified filters of name, ID, and category. The valid values for category filter are 'In-market', 'Lifestyle', 'Life event', 'Interest', 'Demographics', 'Custom-built', 'Contextual', and 'Advertiser audiences'. You can now also find the daily reach and impressions forecast for the audiences using this API.

This enables you to discover relevant audiences faster. For example, to find all audiences who are in the in-market category for shoes, you can set the 'category' filter to 'In-Market' and use the 'audienceName' filter to 'shoes'. This functionality is available worldwide to all Amazon Ads API integrators. 

For full technical details, please visit list [Audiences reference](audiences/#/Discovery/listAudiences).

#### New insights showing overlapping audiences now available in beta on the Amazon Ads API

Amazon DSP and Sponsored Display advertisers can now retrieve insights on the top audiences that overlap with a given audience using the Amazon Ads API. Advertisers can use any audience available to them as the input, and the API will display the top audiences that the input audience is also likely to be a part of. Overlapping audiences can be filtered by audience type, size, or affinity score.

Overlapping audiences are used to help surface other audiences that are similar to your target audience and that you may want to consider adding to your display campaigns. You can also use the insights to deepen your audience understanding through discovering other interests and shopping behaviors that your your brand's current or prospective audience shares. This API functionality allows advertisers to view insights within the tools they use to build and manage campaigns in Sponsored Display or Amazon DSP, as well as other marketing channels. 

For example, if you are planning a campaign to drive consideration for your storage boxes, you may want to view the overlapping audiences to plan your audience strategy. You can pull these insights for an audience in-market for storage boxes. The API might share that the top overlapping audiences are lifestyle - recent movers (1 month), in-market for standing shelf units, and in-market for tool utility shelves. You may include one or more of these audiences to get further reach in your display campaigns. 

To use the API we recommend first integrating with [/listAudiences](audiences/#/Discovery/listAudiences) API. This API will allow you to search for available audiences, which can then be used as an input to generate overlapping audiences. 

For more information, please refer to our [reference documentation](insights/#Audience%20insights).

#### Sponsored Display has expanded Amazon audiences through the Amazon Ads API

Sponsored Display audiences expands to support Amazon audiences for vendors and sellers registered in CA, DE, ES, FR, IT, JP, the UAE, the UK and the US on the Amazon Ads API. This launch gives advertisers access to audiences built using Amazon's first-party shopping & streaming signals. Sponsored Display audiences empowers advertisers of all sizes to reach audiences across their purchase journey—both on and off-Amazon—to help grow their Amazon business.

Amazon audiences can be used to help engage new customers and help them learn about your brand on Amazon. You can now select audiences in a similar way to how you would describe your brand's core customer—for example, "Outdoor Enthusiasts" or "Environmentally Conscious Shoppers", and connect the language used in your overall brand marketing strategy to your Sponsored Display campaigns. Consider mixing and matching Amazon audiences segments with custom-built views remarketing audiences within your ad group.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting) on the Amazon Ads API.

#### Audience taxonomy browser for Sponsored Display and Amazon DSP now available worldwide on Amazon Ads API

The Amazon Ads API supports audience exploration within the taxonomy category. You can now browse by top-level category, sub-category, and see the number of relevant Amazon Ads audiences in each sub-category making it easier and faster to select relevant audiences. This API can be used to filter relevant audiences in sub-categories creating a more seamless audience discovery experience. 

For more details, please see our [Audience Taxonomy documentation](audiences/#/Discovery/fetchTaxonomy) on the Amazon Ads API.

#### Sponsored Display category recommendations now available through the Amazon Ads API

Sponsored Display category recommendations for product targeting and audiences is now available on the Amazon Ads API in the US, CA, DE, UK, FR, IT, ES and UAE.

When Sponsored Display advertisers submit a list of products to advertise for their Sponsored Display campaigns, they will now receive a list of category recommendations, alongside their existing product recommendations. These categories are ranked by their probability of driving higher click-through rate (CTR) for the advertised products selected and also includes a range of products that can help improve the scale of your campaigns. This helps you launch campaigns faster and can help improve product consideration by providing a larger number of products to target.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting%20Recommendations) on the Amazon Ads API.

#### Update to Planned Maintenance for Sponsored Brands Services in the EU region

An earlier release note announced that Sponsored Brands operations in the Advertising API EU Region would be unavailable for approximately 15-20 minutes beginning at 7PM UTC on March 9, 2021 for planned maintenance. This date and time has been updated and is now March 25th beginning at 18:00 UTC. The duration remains 15-20 minutes. Sponsored Brands operations will see API failures for create, read, update, and delete during this planned maintenance event. Sponsored Brands operations in the NA and FE regions are not impacted. No other Advertising API services are impacted in the EU region.

#### New filter in Sponsored Products reporting endpoint

Users of the API can now filter Sponsored Products report requests by state and can exclude inactive campaigns. The new filter can help users reduce the size of reports requests and the time it takes to generate a report. We made this improvement based on user feedback.  For more information see the `stateFilter` description in [Sponsored Product reports](sponsored-products/2-0/openapi#/Reports/requestReport). 

#### Sponsored Display has added new-to-brand metrics for product targeting campaigns on the Amazon Ads API

New-to-brand metrics are now available for Sponsored Display product targeting campaigns on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in the US, UK, FR, ES, IT, DE, UAE, JP, and CA. New-to-brand metrics enable you to measure orders and sales of your products generated from first-time customers of your brand. With new-to-brand metrics, you can better measure and optimize in-flight campaigns, as well as plan future marketing strategies, to drive customer acquisition and brand loyalty. A new customer order is a key step in establishing a long-term brand relationship with a customer.

An order is considered new to your brand if a shopper hasn't purchased one of the products from your brand in the past 12 months. If a shopper has not purchased from your brand within the 12-month look back window, the order is considered new-to-brand. The following campaign performance metrics are available: new-to-brand orders, new-to-brand sales, and new-to-brand units. Using these metrics, you can calculate the cost to acquire a new-to-brand customer. Additionally, based on your objectives and campaign performance, you can optimize your campaigns by broadening or refining your targeting.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Reports/requestReport) on the Amazon Ads API.

#### Amazon DSP API now supports technology, geography, and product reports, deals dimension, and orderID filter

Amazon DSP advertisers now have access to new reports and report dimensions through the Amazon DSP API. You can now pull reports based on Technology, Geography, and Products. Additionally, the inventory report now has deals as a dimension to break out private marketplace deals. 

These new reports help you get more granular analytics about Amazon DSP campaigns. Each report provides details that can help you learn about performance and optimize your campaign strategies and tactics. 

1. **Technology report:** The technology report has been added to aid campaign optimization based on attributes such as device type, browser, or operating system. 
2. **Geography report:** The geography report has been added to help you optimize campaigns based on geographical attributes such as country, city, or ZIP/postal code. 
3. **Products report:** The product report has been added to help you measure conversions for your campaign at an product level. The report includes the top promoted and brand halo products for your selected campaign. You can see conversions at a child product level, as well as parent product roll-ups in the same report.
4. **Deals dimension.** The deals dimension has been added to the inventory report type to help customers using private marketplaces (PMP) deals to see a breakdown of performance and delivery metrics by deal name and identifier. 
5. **OrderId filter.** The orderID filter has been added so you can now generate reports for specific orders under a given profile and advertiser. This has been added to help customers reduce the number of rows returned in a single report. 

See the reference documentation for [Amazon DSP reports](dsp-reports-beta-3p/#/Reports/createReportV2) for details on how to request these new reports. If you are already specifying a version in the accept header, you must update to application/vnd.dspcreatereports.v2_1+json to access the new features. If you are not specifying a version, the new features are available by default. 

#### Upcoming change to Sponsored Display suggested products on the Amazon Ads API

Starting on May 10, 2021, [Sponsored Display suggested products](sponsored-display/3-0/openapi#/Suggested%20products%20%5BDeprecating%20soon%5D) will no longer be supported through the Amazon Ads API and ASIN data will no longer be refreshed. This feature is currently only available in the US for the legacy tactic 'remarketing'. Sponsored Display advertisers should use the [Sponsored Display recommendations API](guides/sponsored-display/recommendations) to guide campaign creation and optimization.

The new Sponsored Display recommendations API provides advertisers with product targeting recommendations for their campaigns by submitting a list of products being featured in a campaign. It is available in the US, UK, FR, ES, IT, DE, UAE, CA and JP. This new functionality in the Amazon Ads API helps you launch campaigns faster by providing relevant product recommendations at scale, which removes friction in discovering the optimal products to target. Recommendations are ranked by their probability of driving higher click through rate (CTR) and are ranked based on the frequency of similar detail page views between the recommended and advertised product .

For more information, see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting%20Recommendations) in the Amazon Ads API.

#### Upcoming change to Sponsored Display campaign creation or campaign managment using the 'remarketing' tactic on the Amazon Ads API

Starting on May 17, 2021, Sponsored Display will no longer support campaign creation or campaign management for the legacy 'remarketing' tactic in the Amazon Ads API. Going forward, Sponsored Display advertisers should use the audiences strategy via tactic 'T00030'. In addition to the "advertised products" and "similar to advertised products" targeting functionality previously available in the 'remarketing' tactic, 'T00030' advertisers will gain access to new controls including the ability to remarket off of "categories" and leverage "category refinements". As a reminder, Sponsored Display audiences and product targeting can be used together to help engage shoppers at all stages of the purchase journey. Audiences helps you reach shoppers while they browse on Amazon and across the web, while product targeting helps you reach customers as they shop on Amazon. Used together, these strategies can help drive awareness and consideration for your brand as well as product sales.

For more information, see our updated [Sponsored Display documentation](guides/sponsored-display/audience-targeting) in the Amazon Ads API.

#### Planned Maintenance for Sponsored Brands Services in the EU region

Sponsored Brands operations in the Advertising API EU Region will be unavailable for approximately 15-20 minutes beginning at 7PM UTC on March 9, 2021 for planned maintenance. Sponsored Brands operations will see API failures for create, read, update, and delete operations starting at 7PM UTC and lasting approximately 15-20 minutes. Sponsored Brands operations in the NA and FE regions are not impacted. No other Advertising API services are impacted.

#### New request filters for the Profiles resource

We've released two new request filters for the Profiles resource. The first is a profile type filter that enables you to filter the response to any combination of Seller, Vendor, or Agency profile types. The second is a valid payment filter that enables you to filter the response to include only profiles that have valid payment methods. These improvements make it easier and faster to retrieve specific profiles. For more information, see the [Profiles resource reference](reference/2/profiles#/Profiles/listProfiles).

### February 2021 release notes

#### New Advertising program filter for the Profiles resource 

We've released a new Advertising program filter for the [Profiles resource](reference/2/profiles#/Profiles/listProfiles). This filter enables you to filter the response to include profiles that are associated with the specified advertising program only. 

Currently, the only supported advertising program is [Billing](invoices), and this filters the [GET Profiles operation](reference/2/profiles#/Profiles/listProfiles) response to include only those profiles to which the user account and application associated with the access token have permission to view [billing](invoices) data.

For more information, see [permissions management for Billing](billing/permissions).

#### Upcoming change to Sponsored Brands product optimization support

Starting on March 25th, 2021, the `shouldOptimizeAsins` flag, also known as **product optimization**, for Sponsored Brands campaigns will no longer be supported through the Amazon Ads API. This feature is currently available in the US and UK. Existing Sponsored Brands campaigns with product optimization enabled will no longer have the products in the creative automatically optimized. Campaigns with product optimization enabled will be converted to standard Sponsored Brands product collection campaigns with the default selected products showing in the creative. 

The changes to the [Sponsored Brands API](sponsored-brands/3-0/openapi) are:

* [POST](sponsored-brands/3-0/openapi#/Campaigns/createCampaigns) or [PUT](sponsored-brands/3-0/openapi#/Campaigns/updateCampaigns) `/sb/campaigns` will no longer honor the `shouldOptimizeAsins` field in the Sponsored Brands `campaign` resource. The value of this field will always be `false`.
* [GET](sponsored-brands/3-0/openapi#/Campaigns/listCampaigns) `/sb/campaigns` will return `false` as the value of the `shouldOptimizeAsins` field for all campaigns. 

And starting on September 25th, 2021, the `shouldOptimizeAsins` property will be removed from the Sponsored Brands `campaign` resource, and will no longer be part of the [GET](sponsored-brands/3-0/openapi#/Campaigns/listCampaigns) `/sb/campaigns` response.

#### Sponsored Display has added new-to-brand metrics for audiences campaigns on the Amazon Ads API

New-to-brand metrics are now available for Sponsored Display audiences campaigns on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in the US, UK, FR, ES, IT, DE, UAE, and CA. New-to-brand metrics enable you to measure orders and sales of your products generated from first-time customers of your brand. With new-to-brand metrics, you can better measure and optimize in-flight campaigns, as well as plan future marketing strategies, to drive customer acquisition and brand loyalty. A new customer order is a key step in establishing a long-term brand relationship with a customer.

An order is considered new to your brand if they haven't purchased one of the products from your brand in the past 12 months. If the customer has not purchased from your brand within the 12-month look back window, the order is considered new-to-brand. The following campaign performance metrics are available: new-to-brand orders, new-to-brand sales, and new-to-brand units. Using these metrics, you can calculate the cost to acquire a new-to-brand customer. Additionally, based on your objectives and campaign performance, you can optimize your campaigns by broadening or refining your targeting.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Reports/requestReport) on the Amazon Ads API.

#### Multi-language support for keywords available for Sponsored Products and Sponsored Brands

Advertisers now have the ability to automatically translate keywords for their Sponsored Products and Sponsored Brands campaigns through the Amazon Ads API. Using these new capabilities, you can provide a list of keywords in your preferred language and receive the localized versions for each marketplace's default language. This helps you expand your reach and run campaigns across geographies where you may be unfamiliar with the local languages. For example, an English-speaking advertiser from UK can use this feature to translate all keywords into Spanish and French and run campaigns in Spain and France. The feature also works in the reverse direction, giving you the ability to retrieve translations for all keywords in a campaign, in an advertiser's preferred language.

You can provide lists of keywords during the campaign creation and management process. The list will be translated to the marketplace default language using the [Keyword Localization capability](localization/#/Keyword%20Localization). Both translated and original untranslated keywords, along with the advertiser's locale, can be added to the campaign. The translated keywords added to the campaign will be used to target shopper's queries in the default language for the country. The untranslated keywords remain retrievable as a reference of your original input. This feature is available today for both [Sponsored Products](sponsored-products/2-0/openapi#/Keywords) and [Sponsored Brands](sponsored-brands/3-0/openapi#/Keywords).

### January 2021 release notes

#### Category benchmarks for Sponsored Brands is now available on the Amazon Ads API

Advertisers can now compare their Sponsored Brands campaign performance to peers using category-level benchmarks. Using the Amazon Ads API, you can pull benchmark data that shows performance compared to the median, lower performing quartile (Bottom 25%), and top performing quartile (Top 25%) values within specific categories. You can use this information to monitor the health of live Sponsored Brand campaigns and as a tool to create effective brand strategies for new campaigns. Benchmarks are available for impressions, click-through rate (CTR), return on ad spend (ROAS), and advertising cost of sales (ACOS). Brand category benchmarks are available for a 90-day look-back window. Learn more in our [reference documentation](sponsored-brands/3-0/category-benchmark#/Category%20Benchmark).

#### KENP read and Estimated KENP royalties metrics are now available for KDP Authors on the Amazon Ads API

To help authors better measure the impact ads are having on their Kindle eBooks enrolled in Kindle Unlimited, we've added Kindle Edition Normalized Pages (KENP) read and estimated royalties to the Amazon Ads API. The KENP read metric is the estimated number of pages read by Kindle Unlimited customers attributed to the ads. The KENP estimated royalties metric estimates the royalties attributed to your Sponsored Products campaigns from customers who borrow your book from Kindle Unlimited or Kindle Owners' Lending Library.

At this time, estimated Kindle Edition Normalized Pages (KENP) Royalties is available to KDP authors advertising in the US, CA, UK, DE, FR, IT, ES, and AU. Keep in mind that this metric is an estimate. If the reporting date range includes days before the KDP Select Global Fund is announced, we will use the previous month's KENP rate to estimate the royalties. Data is reported as of July 15, 2020.

So what exactly does KENP Royalties tell you? Let's say a customer clicks a Sponsored Product ad featuring your Kindle eBook and then borrows it through Kindle Unlimited. If the customer reads 120 normalized pages on day 1, we'll attribute associated royalties to day 1 within your campaign dashboard and downloadable reports. If the customer reads the rest of your book—let's say 100 pages—on day 2, we'll report associated royalties on day 2.

If you build a client-facing interface, you are required to inform the account owner (KDP Author) that the KENP Royalties metrics are an estimate. You can find more information about the KENP metrics in the [Amazon Ads Support Center](https://advertising.amazon.com/help/?entityId=ENTITY2PKTRKXGCEJ6D#GSJCXUYD4UGQP2MX). For more information on how to consume these metrics, please refer to our updated [reference documentation](sponsored-products/2-0/openapi#/Reports).

#### Response size increases for Sponsored Brands target and negative target resources

The POST operations to retrieve a list of [target](sponsored-brands/3-0/openapi#/Product%20targeting/listTargets) and [negative target](sponsored-brands/3-0/openapi#/Negative%20product%20targeting/listNegativeTargets) resources for Sponsored Brands have increased the maximum number of results from 100 to 5000. For more information, see the `maxResults` request parameter for the [POST /sb/targets/list](sponsored-brands/3-0/openapi#/Product%20targeting/listTargets) and [POST sb/negativeTargets/list](sponsored-brands/3-0/openapi#/Negative%20product%20targeting/listNegativeTargets).


#### International marketplace localization is now generally available for Sponsored Products advertising campaigns for sellers and vendors in North America and Europe

Sellers and vendors in US, CA, MX, UK, DE, FR, IT, and ES can now create localized Sponsored Products campaigns within their region. This feature makes it easier to create campaigns in different languages with minimal effort or local language knowledge. You can choose which advertising account to use and then localize product ASINs/SKUs (LocalizeProduct), currencies (LocalizeCurrency), or keywords (LocalizeKeyword) from one marketplace to another. Learn more with our reference documentation: [profiles resource](reference/2/profiles#/Profiles/listProfiles), [localize product](localization/#Localize%20Product), [localize currency](localization/#Localize%20Currency), and [localize keyword](localization/#Localize%20Keyword).

#### Cross-country marketplace campaign copying is now available for Sponsored Products auto-targeted advertising campaigns to sellers and vendors in North America and Europe

Sellers and vendors in US, CA, MX, UK, DE, FR, IT, and ES can now copy Sponsored Products auto-targeting campaigns from one marketplace to another within their region. Copying a campaign transfers all settings and localizes them for the target marketplace. Additionally, you can check the status of campaigns which you previously copied. This functionality may help you save time by quickly copying campaigns without needing to translate keywords and currencies or re-add products you want to advertise for each marketplace campaign. Learn more with our reference documentation: [campaign copy](sponsored-products/3-0/campaign-management/openapi#/campaigns%20copy) and [campaign copy status](sponsored-products/3-0/campaign-management/openapi#/campaign%20status).

#### Netherlands support

We have added support for the Netherlands (NL) to the API. For more details see the API endpoints and keyword bid constraints by marketplace sections of the [overview](reference/api-overview), the regional profile time zone codes section of the [profiles reference](reference/2/profiles), and the countryCode parameter of the [registerProfile and registerBrands operations](reference/2/profiles).

#### Sponsored Display snapshots functionality is now available in on the Amazon Ads API

Sponsored Display snapshots functionality is now available on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in the US, UK, FR, ES, IT, DE, UAE, CA and JP. The snapshots resource is already available for Sponsored Products and Sponsored Brands and can be used to retrieve a record of your campaigns, ad groups, products ads, or targets using a single API call. This interface can be used to download bulk account snapshots files asynchronously.

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Snapshots) in the Amazon Ads API.

#### Advertisers can now request translations of recommended keywords for Sponsored Brands using the Amazon Ads API

Translations are available for default languages that are supported in the country a product is being advertised in. This makes it easier for you to understand recommended keywords for countries in which you do not know the default language. Visit the [reference documentation](sponsored-brands/3-0/openapi/prod#/Keyword%20Recommendations) for more information, including which countries and languages are supported with translations.

#### Sponsored Display product targeting recommendations is now available in new countries on the Amazon Ads API

Sponsored Display advertisers can now submit a list of products and receive product targeting recommendations for their product targeting campaigns in the UK, FR, ES, IT, DE, AE, CA and JP. This new functionality in the Amazon Ads API helps you launch campaigns faster by providing higher number of relevant product recommendations at scale. Recommendations are ranked by their probability of driving higher clickthrough rate (CTR) and are based on the similarity of the recommended products to advertised products with similar detail page views.

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/) in the Amazon Ads API.

#### Reporting rate limit enhancements

We are implementing changes to reduce the number of failed and timed-out reports in the Sponsored Brands and Sponsored Products reporting endpoints. [Users](sponsored-products/2-0/openapi#/Reports/requestReport) will now have higher rate limits at low usage times and lower rate limits at high usage times. We will determine usage levels at the endpoint region level.  We encourage users to smooth their report creation requests throughout the day, such as pulling older data periods later in the day, and using [exponential backoff](https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/) when encountering 429 errors. 

#### Custom invoicing identifiers are now available for Sponsored Brands on the Amazon Ads API

Advertisers can now reconcile booking data with invoices they receive from Amazon Ads using campaign tags for Sponsored Brands. Advertisers can now add custom identifiers when setting up or updating a campaign. The identifiers will be available within invoicing data, allowing for easier reconciliation against internal records and budgets. This is a follow up to the invoicing release from September, which provided continuous visibility into your billing information. For partners or vendors with multiple accounts, this removes the need to manually match campaigns on your advertising invoices to your own campaign booking systems. Once an invoice has been issued, you will be able to get data for any invoice that is due, has been paid, or was written off, at any time and now see your campaign tags represented. Learn more by visiting our [reference documentation](sponsored-brands/3-0/openapi#/Campaigns).

### December 2020 release notes

#### New manager account operations available on the Amazon Ads API

Sponsored ads and Amazon DSP advertisers worldwide can now create and update manager accounts using the Amazon Ads API to maintain a single point of access for all of their advertising accounts.

New operations have been added to the `/managerAccount` resource to support creating manager accounts. You can also link or unlink advertising accounts to/from the manager account. When new accounts are added or removed from your manager account, the updates will automatically be reflected in the responses from the `/profile` and `/managerAccount` resources, which allows both developer and client-facing teams to be in sync on their Amazon Ads accounts. 

Manager account helps advertisers and agencies save time and make data-driven decisions when working across several advertising accounts. Manager account users will be able to see detailed performance metrics on the manager account overview page in the Amazon Ads console with a single user invitation. For more information, please refer to our [blog post](https://advertising.amazon.com/resources/whats-new/unified-manager-account?ref_=a20m_us_wn_ftrd_btn_mgr_acct_dsp) on manager account and [API documentation](reference/1-0/managerAccount#/Manager%20Accounts).

#### Brazil support

We have added support for Brazil (BR) to the API. For more details see the API endpoints and keyword bid constraints by marketplace sections of the [overview](reference/api-overview), the regional profile time zone codes section of the [profiles reference](reference/2/profiles), and the countryCode parameter of the [registerProfile and registerBrands operations](reference/2/profiles).

#### Mexico support

We have added support for Mexico (MX) to the API. For more details see the API endpoints and keyword bid constraints by marketplace sections of the [overview](reference/api-overview), the regional profile time zone codes section of the [profiles reference](reference/2/profiles), and the countryCode parameter of the [registerProfile and registerBrands operations](reference/2/profiles).

#### Amazon DSP campaign management functionality (open beta) now available on the Amazon Ads API in the US

Advertisers, agencies, and tool providers can now leverage the Amazon Ads API to manage Amazon DSP display ad campaigns in the US. You can: 1) create, read, and update orders and line items; 2) add and remove creatives to and from line items; and 3) read domain lists, audiences, IAB content categories, product categories, supply sources, pixels, and creatives. This enables you to automate campaign management operations and reduce the time you spend on manual efforts. Additionally, you can include Amazon DSP campaign management into your centralized tools where you manage all of your marketing. This functionality is available to self-service Amazon DSP customers for display ad campaigns in the US. For existing Amazon Ads API customers, check out our [reference documentation](guides/dsp/overview). For new customers, follow the steps outlined in the [setting up](guides/onboarding/overview) guide to get started.

#### Sponsored Display audiences is now available in new countries on the Amazon Ads API

Sponsored Display audiences is now available on the Amazon Ads API for vendors and sellers registered in Amazon Brand Registry in the UK, FR, ES, IT, DE, and CA. This strategy helps you reach shoppers on and off Amazon based on shopping actions. As a reminder, Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences helps you reach shoppers while they browse on Amazon and across the web while product targeting helps you reach shoppers as they shop on Amazon. Used together these strategies can help drive awareness, consideration, and conversion. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories".

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting) in the Amazon Ads API.

#### Budget rules API open beta         

Advertisers now have the ability to set campaign budgets in advance using budget rules (beta) using the Amazon Ads API. Budget rules help reduce manual effort spent on adjusting budgets. You can set budget rules for specific dates or based on performance goals. 

Schedule based budget rules can be set to increase daily budgets for any date range or recommended events, such as Black Friday Cyber Monday. During shopping events, we will provide recommended budgets based on historical shopping activities. Performance based budget rules let you set rules to increase budgets based on campaign performance metrics. With budget rules, advertisers can reduce the likelihood of campaigns running out of budgets, especially during peak days or holiday events. 

Benefits of using budget rules:

* Reduces likelihood of campaigns going out of budget preventing missed opportunities 
* Provides budget recommendations for special events 
* Gives more control over budget
* Reduces manual effort and time spent adjusting budgets

Advertisers can now set up budget rules for their campaigns through the Amazon Ads API. Budget rules (beta) helps you plan budget changes in advance so you can spend less time manually adjusting during a campaign. You can set rules based on specific dates or performance goals.

Budget rules based on scheduling can increase daily budgets for any date range or for recommended events like Black Friday or Cyber Monday. For shopping events, we will provide recommended budgets based on historical shopping activities. Budget rules also lets you automate daily budget changes based on performance metrics. This new feature will help reduce the likelihood of campaigns running out of budgets, especially during peak days or holiday events. Learn more in the  [Sponsored Products Budget Rules reference](sponsored-products/3-0/openapi/prod#/BudgetRules) and [Sponsored Brands Budget Rules reference](sponsored-brands/3-0/openapi/prod#/BudgetRules) or the [Sponsored Products Budget Rules overview](sponsored-products/budget-rules/overview) and [Sponsored Brands Budget Rules overview](guides/budgets/rules/overview).

#### Custom invoicing identifiers are now available for Sponsored Products on the Amazon Ads API

Advertisers can now reconcile booking data with invoices they receive from Amazon Ads using campaign tags for Sponsored Products. Advertisers can now add custom identifiers when setting up or updating a campaign. The identifiers will be available within invoicing data, allowing for easier reconciliation against internal records and budgets. This is a follow up to the invoicing release from September, which provided continuous visibility into your billing information. For partners or vendors with multiple accounts, this removes the need to manually match campaigns on your advertising invoices to your own campaign booking systems. Once an invoice has been issued, you will be able to get data for any invoice that is due, has been paid, or was written off, at any time and now see your campaign tags represented. Learn more by visiting our [reference documentation](sponsored-products/2-0/openapi#/Campaigns).

#### International marketplace localization is now available for Sponsored Products advertising campaigns for sellers and vendors in North America and Europe

Sellers and vendors in US, CA, MX, UK, DE, FR, IT, and ES can now create localized Sponsored Products campaigns within their region. This feature makes it easier to create campaigns in different languages with minimal effort or local language knowledge. You can choose which advertising account to use and then localize product ASINs/SKUs (LocalizeProduct), currencies (LocalizeCurrency), or keywords (LocalizeKeyword) from one marketplace to another. Learn more with our reference documentation: [v2Profiles](reference/2/profiles#/Profiles/listProfiles), [LocalizeProduct](localization/#Localize%20Product), [LocalizeCurrency](localization/#Localize%20Currency), and [LocalizeKeyword](localization/#Localize%20Keyword).

### November 2020 release notes

#### Product moderation information for Sponsored Products now included in ads management data on the Amazon Ads API

You can now receive product moderation information as part of Product Ads Extended data on the Amazon Ads API. Possible product moderation information may include if an item violates advertising policy or has a prohibited attribute. You can access this new information as part of Sponsored Products ad management via the ProductAdsEx endpoint under a new servingStatusDetails field. To provide more information, links to related Support Center topics are included in the call response for select product moderation statuses. To learn more, visit our [reference documentation](sponsored-products/2-0/openapi#/Product%20ads/listProductAdsEx).

#### Restricted keywords: prescription medications

Updated keyword restrictions for advertisers. Amazon Ads does not allow the targeting of prescription medicine products or the use of prescription medicine names as targeting keywords or phrases.

For example, prescription medicine keyword or products, such as "Lipitor" or "prednisone".

As a reminder, keywords or products targeted through Amazon Ads must be relevant to the product being promoted. Ads must not target keywords or products which could result in offensive, insensitive, or undesirable shopper experience. 

#### Product moderation information for Sponsored Products and Sponsored Brands now available on the Amazon Ads API

You can now receive product moderation information as new product eligibility statuses on the Amazon Ads API. Possible product moderation information may include if an item violates advertising policy or has a prohibited attribute. Additionally, you can now retrieve both product moderation and product eligibility information with a single call for a given list of products (ASINs or SKUs) associated with your Seller Central or Vendor Central accounts. To provide more information about product moderation statuses, links to related Support Center topics are included in the call response. Product eligibility supports Sponsored Products and Sponsored Brands. To learn more, visit our [reference documentation](eligibility-prod-3p).

Note that information regarding the content moderation of a campaign is not available via this endpoint. Visit our [reference documentation](eligibility-prod-3p) for instructions on retrieving this information for each ad product.


#### Sponsored Display audiences is now available for vendors and professional sellers in the US on the Amazon Ads API

Sponsored Display audiences is now available on the Amazon Ads API for vendors and professional sellers in the US. This strategy helps advertisers reach shoppers on and off-Amazon based on shopping signals. Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences helps you reach shoppers while they browse on Amazon and across the web while product targeting helps you reach shoppers as they shop on Amazon; used together these strategies can help drive awareness, consideration and conversion. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories".

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting) in the Amazon Ads API.

#### Updates to compatibility and versioning in the Amazon Ads API
 
As part of our commitment to ensuring ease of use in the Amazon Ads API, we've updated our compatibility and versioning policy. Resources in the Amazon Ads API are now versioned independently. Clients request a specific version of a resource using the `Accept` request-header field. If the service supports the exact version, the exact version of the resource is always returned. Clients that do not request a specific version receive the most recent compatible version. Clients that request a deprecated version receive the most recent compatible version. We also guarantee resources are backwards compatible within a minor version. See the [Amazon Ads API compatibility and versioning policy](get-started/compatibility-versioning-policy) for more information.

#### New! The Amazon Attribution API is now available for vendors and professional sellers enrolled in Brand Registry in CA, DE, FR, IT, and ES.

The Amazon Attribution API is now available for vendors and professional sellers enrolled in Brand Registry in CA, DE, FR, IT, and ES. This functionality was previously only available to US and UK vendors, as well as professional sellers enrolled in Brand Registry in the US.  

Now tool providers working with vendors and professional sellers enrolled in Brand Registry in the US, CA, UK, DE, FR, IT, and ES can make Amazon metrics, including detail page views, add to carts, and purchases, conveniently available to their clients without having to log into the Amazon Attribution console to manually create tags and download reports. Amazon Attribution reporting through the API is available for paid non-Amazon (i.e. Google or Facebook) media campaigns that drive clicks to an Amazon product or Stores page.  

Now tool providers who integrate with Amazon Attribution's API can help their clients optimize their non-Amazon media based on how shoppers discover, research, and buy products on Amazon. Through access to on-demand Amazon reporting, tool providers can offer their clients a unified view of campaign performance, with Amazon conversions displayed alongside existing digital marketing metrics. Tool providers can also leverage this reporting to inform their algorithms in order to help improve their clients' campaign performance through multi-channel recommendations and automated bid/budget optimizations.  

The Amazon Attribution API does not require campaign management (CREATE or POST) calls and is comprised of 5 resources. For more information about Amazon Attribution, and how to register your clients, visit the [Amazon Attribution product page](https://advertising.amazon.com/solutions/products/amazon-attribution). For more details about how to integrate with Amazon Attribution, create API tags for measurement, or retrieve reporting, visit the [Amazon Attribution reference documentation](guides/amazon-attribution/overview).


#### Search term impression rank on the Amazon Ads API

You can now understand how your Sponsored Brands campaigns compare to other advertisers with search term impression rank, now available on the Amazon Ads API. This metric helps you understand how your campaigns perform compared to other advertisers in relation to ad impressions. It shows the overall rank of ad impressions you receive compared to other advertisers for the same search term over a selected time period. Visit our [reference documentation](sponsored-brands/3-0/openapi#/Reports) to learn more.

### October 2020 release notes

#### Budget recommendations and missed opportunities for Sponsored Products now available on the Amazon Ads API

Advertisers in the US can now receive recommended daily budgets for their Sponsored Products campaigns. This feature provides a budget estimate for your campaign to deliver for a full day. Additionally, you can get more information about campaigns that went out of budget in the past. For those campaigns, you can receive "percent of time in budget", "estimated missed impressions", "estimated missed clicks", and "estimated missed sales" metrics. These estimates are based on the advertising opportunities your campaign missed while out of budget and may help inform your future budget allocation decisions. Visit the [reference documentation](sponsored-products/3-0/openapi/prod#/BudgetRecommendation/getBudgetRecommendations) to learn more.

#### Sponsored Display product targeting recommendations is now available in the US on the Amazon Ads API
 
Sponsored Display advertisers can now submit a list of products and receive product targeting recommendations for their product targeting campaigns. This new functionality in the Amazon Ads API helps you launch campaigns faster by providing higher number of relevant product recommendations at scale. Recommendations are ranked by their probability of driving higher clickthrough rate (CTR) and are based on the similarity of the recommended products to advertised products with similar detail page views.

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting%20Recommendations/getTargetRecommendations) in the Amazon Ads API.

#### Sponsored Display audiences is now available for vendors in the US on the Amazon Ads API

Sponsored Display audiences is now available on the Amazon Ads API for vendors in the US. This strategy helps advertisers reach shoppers on and off-Amazon based on shopping signals. Sponsored Display audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences helps you reach shoppers while they browse on Amazon and across the web while product targeting helps you reach shoppers as they shop on Amazon; used together these strategies can help drive awareness, consideration and conversion. You can help increase your reach with Sponsored Display audiences by including audience segments from "advertised products", "similar to advertised products", and "categories".

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Targeting) in the Amazon Ads API.

#### Manager account is now available on the Amazon Ads API. 

You can use manager account to create a single point of access for all your advertising across sellers, vendors, and countries. After initial setup in the console, manager account on the API will return a list of all accounts that are linked by advertiser. When new accounts are added or removed from your manager account, they will automatically be returned to you via `/profile` and `/managerAccount` endpoints. This allows both developer and client-facing teams to always be in sync on their Amazon Ads accounts. It also enables access to all accounts for an advertiser or agency with a single user invitation. To get started, go to advertising console to [create your first manager account](https://advertising.amazon.com/help?ref_=AAC_gnav_support_center#G69CDSR9MNSWJH95) and invite your email address to the manager account. For more information, refer to our [blog post](https://advertising.amazon.com/blog/manage-multiple-advertising-accounts-in-one-dashboard) and [API documentation](reference/1-0/managerAccount#/Manager%20Accounts).

### September 2020 release notes

#### Invoicing is now available on the Amazon Ads API

Advertisers can now retrieve invoice information for their sponsored ads and DSP campaigns through the Amazon Ads API. You can now match your Amazon Ads campaign billing with your internal records in an automated manner. This also provides you with continuous visibility into your billing information. For partners or vendors with multiple accounts, this removes the need to manually match campaigns on your advertising invoices to your own campaign booking platforms. Once an invoice has been issued, you will be able to get data for any invoice that is due, has been paid, or was written off, at any time. There will be two endpoints for invoicing: one that will return a list of invoices for a given advertiser, and the other will return details for a specific invoice. Learn more by visiting our [reference documentation](invoices).

#### Amazon DSP API Reporting Now Generally Available

Today we are moving Amazon's DSP API reporting functionality out of beta and into general availability status. Advertisers, agencies and tool providers can leverage the Amazon Ads API to retrieve reports for their Amazon DSP campaigns worldwide. Users can generate campaign, audience, and inventory reports similar to custom reports in the DSP console. Metrics are available at the order, line item, creative, site and supply levels. This API functionality will make it easier for advertisers to retrieve reports and view campaign performance in the tools they use to manage other marketing channels. This functionality is available to advertisers, tool providers and agencies that have self-service access to Amazon DSP. To get started, follow the steps outlined in [setting up](guides/onboarding/overview).

#### Manager account will be available on the Amazon Ads API starting this week

Starting this week, [manager account](https://advertising.amazon.com/help?ref_=AAC_gnav_support_center#GU3YDB26FR7XT3C8) will be available on the Amazon Ads API. This update will include a change in the behavior of `/profiles` for sellers. With the update, when an application does `GET /profiles` the list of profiles that are returned will include both the seller accounts that the user has access to (as granted via Seller Central) and the seller accounts that are included because of access granted via manager accounts. If you want to differentiate between the two types of access, you can call manager accounts on the API.

#### Starting this week, we will begin rolling out an update to simplify and secure authorization for sellers

With this change, access is in sync to the permissions of the user who granted you LWA access, i.e. you can perform operations on the same set of profiles as the seller. If the user loses permission to the seller account, the integrators to whom the user authorized will no longer be able to access the seller profile. If a user gains access to a new seller account via user invitation or their manager account, the integrators the user authorized will automatically be able to access the new seller profile. This change requires no new integration and will automatically be reflected in your existing integrations.

If you were granted access to any of your user accounts by a user that has since lost permission, you will need to resend consent to the advertiser to regain access. Follow the steps [here](guides/get-started/retrieve-access-token) to gain access. It also automates your access to a new profile when a seller starts advertising with a new country or accounts.

#### New! Amazon Attribution API modified tag format

The Amazon Attribution API macro enabled and non-macro Attribution tags have changed. Existing campaigns with the deprecated tags will still enable Amazon Attribution measurement, however all new campaigns should use the new tag. The method for calling for Attribution tags has not changed. More details on the updated Amazon Attribution API tag are available in the [Attribution tags reference documentation](amazon-attribution-prod-3p).


#### Custom keyword ranking is now available for Sponsored Products in the US on the Amazon Ads API

Advertisers in the US can now customize how their keywords are ranked for their Sponsored Products through the Amazon Ads API. You can use custom keyword ranking in keyword targeted campaigns to rank keywords by: estimated clicks; estimated conversions; or a default that combines multiple metrics. You can rank your targeted keywords, recommended keywords, or a combination of both. Ranked keyword recommendations include keyword match type and recommended bid. When you submit your targeted keywords for ranking, you have the option to provide a match type and bid, and those values will be used for ranking. If you do not provide match type or bid, we will include the values used to rank with the results. Recommended keywords can also be translated into a language that is supported in a user's country. Visit the [reference documentation](sponsored-products/3-0/openapi/prod#/Get%20ranked%20keywords%20recommendations/getRankedKeywordRecommendation) home to learn more.

#### Sponsored Brands video now available on the Amazon Ads API 

[Sponsored Brands video](https://advertising.amazon.com/en-us/resources/ad-specs/sponsored-brands-video) is now available to vendors and sellers registered in Amazon brand registry in the US on the Amazon Ads API. You can upload video creatives, create and manage video campaigns, and access reporting. Use Sponsored Brands video to stand out on desktop and mobile shopping pages, and help customers discover your brand and products as they shop on Amazon. Ads are keyword targeted, cost-per-click, and link customers directly to the product detail page where they can learn more and purchase. To get started with the API, see the [reference documentation](sponsored-brands/3-0/openapi).

**Draft campaign support for Sponsored Brands video on the Amazon Ads API**

Advertisers in the US can now create and manage draft campaigns for Sponsored Brands video on the Amazon Ads API. Draft campaigns allow you to create and save a campaign before submitting for approval. For more information, see the [reference documentation](sponsored-brands/3-0/openapi).

**Sponsored Brands video preview URL now available on the Amazon Ads API**

Advertisers in the US can now get a preview URL for Sponsored Brands video campaigns on the Amazon Ads API. This allows you to review the encoded media being uploaded. For more information, see the [reference documentation](sponsored-brands/3-0/openapi#/Media). 

**Get Sponsored Brands product collection and video campaigns in a single call on the Amazon Ads API**

Advertisers in the US can now get a list of their Sponsored Brands product collection and Sponsored Brands video campaigns in a single call on the API. You can also filter the campaigns by `adFormat` ("productCollection" or "video"). Learn more about getting a campaign list and using adFormat filter in the [reference documentation](sponsored-brands/3-0/openapi#/Campaigns/listCampaigns).

#### Amazon DSP API adds advertiser read functionality in open beta

The Amazon Ads API now enables customers who have self-service access to Amazon DSP to retrieve advertiser details for their Amazon DSP profiles. ADSP API customers can now get advertiser ID, advertiser name, currency, URL, country, and time zone for the advertisers within their entity. More details on the Advertiser read functionality are available in the [reference documentation](dsp-advertiser/#Advertiser).

### August 2020 release notes

#### History now available on the Amazon Ads API

Advertisers can now pull a detailed summary of changes made to their Sponsored Products and Sponsored Brands campaigns on the Amazon Ads API. You can view change history for up to 90 days at the individual campaign and ad group levels, and across all Sponsored Products and Sponsored Brands campaigns in the same view. Learn more in our [reference documentation](change-history).

#### New! Tool providers who are integrated with the Amazon Ads API can now seamlessly enable Amazon conversion and sales measurement on their client's non-Amazon campaigns.

Amazon Attribution is available to US and UK vendors, as well as US seller brand owners. Amazon Attribution is a free measurement solution that provides visibility into how advertisers' non-Amazon digital marketing tactics impact their brand's shopping activity on Amazon. Now available through the Amazon Ads API, tool providers can make Amazon metrics, including detail page views, add to carts, and purchases, conveniently available to their clients without having to log into the Amazon Attribution console to manually create tags and download reports. Amazon Attribution reporting through the API is available for paid media campaigns that drive clicks to an Amazon product or Stores page.

Now tool providers who integrate with Amazon Attribution's API can help their clients optimize their non-Amazon media based on how shoppers discover, research, and buy products on Amazon. Through access to on-demand Amazon reporting, tool providers can offer their clients a unified view of campaign performance, with Amazon conversions displayed alongside existing digital marketing metrics. With Amazon reporting now available through the API, tool providers can also leverage their algorithms to help improve their clients' campaign performance through multi-channel recommendations and automated bid/budget optimizations.

Amazon Attribution's API is comprised only of GET calls; it does not require campaign management (CREATE or POST) calls. For more information about Amazon Attribution, and how to register your clients, visit the [Amazon Attribution product page](https://advertising.amazon.com/solutions/products/amazon-attribution). For more details about how to integrate with Amazon Attribution, create tags for measurement through the API, or retrieve reporting, visit the [reference documentation](amazon-attribution-prod-3p).


#### Sponsored Display product targeting is now available in the UK, FR, ES, IT, DE, AE, and JP for professional sellers and vendors on the Amazon Ads API

Sponsored Display product targeting is now available in the Amazon Ads API for sellers in the UK, FR, ES, IT, DE, AE and JP for professional sellers enrolled in the [Amazon Brand Registry](https://brandservices.amazon.com/) and vendors. This functionality was previously only available in the US & CA through the API. Product targeting for Sponsored Display gives sellers and vendors additional opportunities to reach and re-engage shoppers. Product targeting enables advertisers to promote product discovery, measure campaigns more effectively by targeting specific products or relevant product categories on Amazon, and gain greater line of sight into campaign optimization. Sellers and vendors now have the ability to: target multiple ASINs within campaigns, access detailed reporting, leverage flexible bidding options, and broaden category targeting capabilities. Product targeting in Sponsored Display brings audiences closer to the point of purchase and helps brands stay top of mind. Product targeting ads may appear alongside product description pages, customer reviews, shopping results pages, or under the featured offer.

For more information see our [Sponsored Display documentation](sponsored-display/3-0/openapi#) in the Amazon Ads API.

#### Amazon DSP API now supports Audience and Inventory reports, additional currencies and changes to default metrics behavior

Today we are announcing several updates to the Amazon DSP API reporting functionality. Updates include the addition and deprecation of several new features for beta customers. Please note some of these may be breaking changes which will go into effect **8/31**. 

**New! Audience and Inventory Reports:** Amazon DSP API customers can now generate AUDIENCE and INVENTORY reports, in addition to CAMPAIGN report types. Audience reports enable you to generate campaign metrics summarized by audience segment targeting method. Inventory reports enable you to generate campaign metrics summarized by site and supply source. More details about how to generate Audience and Inventory reports, and which fields are included in each report type, are available in the [reference documentation](dsp-reports-beta-3p/#/Reports/createReportV2).

**New! Longer date ranges:** You can now generate Amazon DSP reports for up to 31 days by specifying a startDate and endDate. The startDate can be up to 90 days from the report request date. By default, metrics will be at the SUMMARY time unit and aggregated over the specified time period. To generate a report with a daily breakdown, you can specify DAILY in the timeUnit object. For more details about how to generate reports across longer date ranges, see the [reference documentation](dsp-reports-beta-3p/#/Reports/createReportV2). With the introduction of startDate and endDate, **we will no longer support reportDate after 8/31.**

**Changes to default metrics when no Metrics are specified.** Currently reports generated through the API contain all available metrics for a given report type and dimension if no metric names are specified in the metrics object. **Starting 8/31, only the default fields will be returned when metrics are empty in the report request.** For a list of default metrics by reporting dimension, and all available metric fields to include on your report request, see the [reference documentation](dsp-reports-beta-3p/#/Reports/createReportV2).

**New! Support for Additional Currencies and Updated Sales Fields**: Campaign reports now include an `orderCurrency` field by default, which returns the currency used for the order. With the addition of the `orderCurrency` field, we added four new fields: `sales14d`, `totalSales14d`, `newToBrandProductSales14d`, and `totalNewToBrandProductSales14d`. The values reported in these and spend-related fields will be based on the currency value returned in the `orderCurrency` field. With the addition of the new Sales fields mentioned above, **we will deprecate the currency-specific sales fields: salesUSD14d, salesGBP14d, salesJPY14d, etc. on 8/31.**

**New! Additional Metadata:** You can now specify `advertiserCountry` and `advertiserTimezone` in the metrics field when you create a report request to have these fields returned in the report. In addition, `orderCurrency` is added by default when ORDER is specified in the dimensions object.

#### Preview of Sponsored Display audiences is now available on the Amazon Ads API

[Sponsored Display documentation](sponsored-display/3-0/openapi#/) has been updated to include a preview of Sponsored Display audiences functionality which allows advertisers to reach audiences based on customer shopping signals. Audiences and product targeting can be used together to help reach shoppers at all stages in the shopping journey. Audiences help you reach shoppers while they browse on Amazon and across the web and helps to increase awareness for your brand and products, while product targeting helps you reach shoppers as they shop on Amazon.

We will notify developers in a release note when audiences functionality transitions from preview to generally available.

#### Negative product targeting for Sponsored Product auto targeting now available on the Amazon Ads API
 
Advertisers can now add negative product targeting to Sponsored Products auto targeting campaigns through the API. You can use the search term report to find products to add as negative targets based on performance. Learn more in our [reference documentation](reference/sponsored-products/2/product-attribute-targeting).

#### Product eligibility for Sponsored Products and Sponsored Brands now available for sellers and vendors on the Amazon Ads API

Product eligibility helps advertisers understand which products are eligible for advertising and what actions to take to resolve issues, such as when a product is not the featured offer, is out of stock, or is in a restricted category. You can retrieve the Sponsored Products and Sponsored Brands eligibility status for a given list of products (ASINs or SKUs) associated in your Seller or Vendor Central account. If products are ineligible for advertising, a status update will be provided explaining why the product is ineligible so that you can take action to activate them. For more information, see the [reference documentation](eligibility-prod-3p).

### July 2020 release notes

#### 'Read-only' Support for Sponsored Products Custom Text is now available in the Amazon Ads API

Developers can now retrieve custom text as a read-only field from any applicable Sponsored Products (SP) ad. Custom Text for SP provides book advertisers in the United States with a 150 character text string to describe their book beyond just their title and format. This text is displayed in the creative treatment for certain SP placements and provides shoppers with additional information about the advertised book. 

#### Sponsored Display product targeting is now available in the US & CA for professional sellers and vendors on the Amazon Ads API

Sponsored Display (beta) product targeting is now available in the Amazon Ads API for sellers in the US & CA for professional sellers enrolled in the [Amazon Brand Registry](https://brandservices.amazon.com/) and vendors. Product targeting for Sponsored Display gives sellers and vendors additional opportunities to reach and re-engage audiences in combination with views audiences. Product targeting enables advertisers to promote product discovery, measure campaigns more effectively by targeting specific products or relevant product categories on Amazon, and gain greater line of sight into campaign optimization. Sellers and vendors now have the ability to: target multiple ASINs within campaigns, access detailed reporting, leverage flexible bidding options, and broaden category targeting capabilities. Product targeting in Sponsored Display brings audiences closer to the point of purchase and helps brands stay top of mind. Product targeting ads may appear alongside product description pages, customer reviews, shopping results pages, or under the featured offer.

For more information see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#) in the Amazon Ads API.

#### Sponsored Display now supports negative targeting when using product targeting in the US & CA for professional sellers and vendors on the Amazon Ads API

Advertisers can now negatively target products or brands when using Sponsored Display (beta) product targeting through the Amazon Ads API. Adding products or brands as negative targets prevent ads from displaying when a shopper browses product pages that match those negative targets. For more information, see the [reference documentation](sponsored-display/3-0/openapi#/Negative%20targeting/listNegativeTargetingClauses).

#### Sponsored Brands Title Authority Validation is now available in the Amazon Ads API

Title Authority Validation, which prevents repetitive images from appearing in Sponsored Brands (SB) book campaigns, is now available in the SB API for advertisers in the United States. SB campaigns require advertisers to select three ASINs, and for book campaigns, those ASINs must be associated with different titles to avoid displaying repetitive products in the same ad. For more information, see the reference documentation.

#### Sponsored Brands video now available on the Amazon Ads API in open beta
 
[Sponsored Brands video](https://advertising.amazon.com/en-us/resources/ad-specs/sponsored-brands-video) is now available in open beta to vendors and seller brand owners in the US on the Amazon Ads API. You can upload video creatives, create and manage video campaigns, and access reporting. Use Sponsored Brands video to stand out on desktop and mobile shopping pages, and help customers discover your brand and products as they shop on Amazon. Ads are keyword targeted, cost-per-click, and link customers directly to the product detail page where they can learn more and purchase. To get started with the API, see the [reference documentation](sponsored-brands-video/3-0/openapi).

### June 2020 release notes

#### Product eligibility for Sponsored Products and Sponsored Brands now available in beta for sellers and vendors on the Amazon Ads API

Product eligibility helps advertisers understand which products are eligible for advertising and what actions to take to resolve issues, such as when a product is not the featured offer, is out of stock, or is in a restricted category. You can retrieve the Sponsored Products and Sponsored Brands eligibility status for a given list of products (ASINs or SKUs) associated in your Seller or Vendor Central account. If products are ineligible for advertising, a status update will be provided explaining why the product is ineligible so that you can take action to activate them. For more information, see the [reference documentation](eligibility-prod-3p).

#### KDP Author Account Subtype support and Sponsored Products ASIN Validation for KDP Authors are now available in the Amazon Ads API

Support for a new Kindle Direct Publishing (KDP) author identifier is now available as a vendor subtype in the Amazon Ads API. With this release, it is possible to identify Authors programmatically, providing developers with the ability to control access to features or differentiate the customer experience. We also added KDP Author ASIN Validation for Sponsored Products, already enforced in the Amazon Ads Console, which prevents KDP Authors from promoting ASINs outside of their KDP catalogs. This implementation moves us closer to achieving parity between the Advertising Console UI and API for books advertisers. For more information, see the [reference documentation](reference/2/profiles).

### May 2020 release notes

#### Sponsored Products product targeting is now available for Australia and United Arab Emirates on the Amazon Ads API

We added support for Australia and United Arab Emirates for Sponsored Products product targeting on the API. For more information see our updated [Sponsored Products product targeting documentation](reference/sponsored-products/2/product-attribute-targeting).

#### Amazon DSP reporting API functionality now available in open beta 

Advertisers, agencies and tool providers can now leverage the Amazon Ads API to retrieve reports for all their Amazon DSP campaigns worldwide. Users can filter results by advertiser IDs and specify a list of metrics from over 400 available. Metrics are available at the order, line item, and creative levels. This API functionality saves agencies and advertisers hours spent retrieving reports manually. Additionally, it will make it easier for advertisers to retrieve reports in the tools they use to manage other marketing channels. This functionality is available to advertisers, tool providers and agencies that have self-service access to Amazon DSP. To get started, follow the steps outlined in [setting up](guides/onboarding/overview).

### April 2020 release notes

#### Product eligibility now available in beta to sellers on the Amazon Ads API

Product eligibility helps advertisers understand which products are eligible for advertising and what actions to take to resolve issues, such as when a product is not the featured offer, is out of stock, or is in a restricted category. Advertisers can retrieve the Sponsored Products eligibility status for a given list of products (ASINs or SKUs) associated in your Seller Central account. If products are ineligible for advertising, a status update will be provided explaining why the product is ineligible so that you can take action to activate them. For more information, see the [reference documentation](eligibility-prod-3p).

#### Product views and similar product views targeting is now available for Sponsored Display on the Amazon Ads API

Sponsored Display now enables you to separately manage the two default audience segments for views campaign - those who viewed your advertised products (exactProduct) and those who viewed similar products (similarProduct). Advertisers are now able to: add separate bids for each audience segment, and pause or restart delivery to either of the two audience segments.

The feature is now available for all vendors and professional sellers enrolled in the [Amazon Brand Registry](https://brandservices.amazon.com/) in the US for views campaigns created after October 29. For more information see the [Sponsored Display targeting resource](sponsored-display/3-0/openapi#/Targeting) in the Amazon Ads API. We plan to support targeting level reporting with future API functionality.

### March 2020 release notes

#### Suggested products and product readiness status now available for Sponsored Display on the Amazon Ads API

Sponsored Display has launched new features for suggested products and product readiness status for sellers using views targeting on the Amazon Ads API. The new functionality will provide a readiness score for each product of high, medium, or low, based on estimated reach and performance. A high, medium, or low score will indicate products that have a high, medium, or low amount of product detail page visits in the last 28 days. When creating audiences based on views, we recommend selecting products with high product detail page views to reach a larger audience. With the new option, you can now quickly launch a campaign with products that have the recommended product detail page views to help achieve effective scale.

This functionality is now available in the US for all sellers creating new Sponsored Display views campaigns. For more information see the [Sponsored Display suggested products reference](sponsored-display/3-0/openapi#/Suggested%20products) in the Amazon Ads API.

#### Sponsored Brands product targeting draft campaigns
 
The Amazon Ads API now allows users to create, get, update, and delete draft Sponsored Brands campaigns created with product targeting. The API can also be used to submit product targeting draft campaigns for moderation approval. This capability allows advertisers to make changes and save them as drafts. To learn more, see the Sponsored Brands draft campaign [reference documentation](https://advertising.amazon.com/API/docs/en-us/sponsored-brands/3-0/openapi#/Drafts).

### January 2020 release notes

#### Updates to daily budget calculation

In October 2019, we changed the way we calculate daily budgets for Sponsored Brands campaigns. With this update, budgets for new Sponsored Brands campaigns are calculated as an average over the course of the calendar month. This allows you to spend up to 10% more than your average daily budget while ensuring you don't spend more than the monthly budget (daily budget you've set, multiplied by the number of days in that month).

Starting February 2020, Sponsored Brands campaigns created on or before October 8, 2019, with a daily budget will now be calculated as an average over the course of the calendar month. Visit our [Support Center](https://advertising.amazon.com/help?ref_=a20m_us_blog_whtsnewspnsbrdsstrs_012820#GTGPQGUXNCTHE2DS) to learn more.

#### Preview of Sponsored Display product targeting is now available

[Sponsored Display documentation](sponsored-display/3-0/openapi#/) has been updated to include a preview of Sponsored Display product targeting functionality. This functionality was previously available for vendors and will be available for professional sellers enrolled in the [Amazon Brand Registry](https://brandservices.amazon.com/). Product targeting for Sponsored Display gives sellers additional opportunities to find and re-engage audiences in combination with Views audiences. Product targeting enables advertisers to promote product discovery, measure campaigns more effectively by targeting specific products or relevant product categories on Amazon, and gain greater line of sight into campaign optimization. Sellers now have the ability to: target multiple ASINs within campaigns, access detailed reporting, leverage flexible bidding options, and broaden category targeting capabilities. Product targeting in Sponsored Display brings audiences closer to the point of purchase and helps brands stay top of mind. We are sharing this documentation to help developers plan their implementation. When this functionality becomes available, we'll notify users in a release note on this page.

#### United Arab Emirates support

We have added support for United Arab Emirates (AE) to the API. For more details see the *API endpoints* and *keyword bid constraints by marketplace* sections of the [overview](get-started/overview), the *regional profile time zone codes* section of the [profiles reference](reference/2/profiles), and the `countryCode` parameter of the [registerProfile and registerBrands operations](reference/2/profiles). 

### December 2019 release notes

#### Keyword Placement report is now available for Sponsored Brands

Use the keyword placement report to get better insight into your keyword performance across different placement types. For more information, see the **segmentation section** of [keyword placement reporting](https://advertising.amazon.com/API/docs/en-us/reference/sponsored-products/2/reports).

#### Additional reporting functionality for Sponsored Display now available in US, CA, UK, FR, ES, IT, DE, and JP

Reporting for Sponsored Display campaigns that use interest and product audiences is now available. This functionality allows users to retrieve reports for recently created Sponsored Display campaigns that use these audiences and for Sponsored Display campaigns that were previously Product Display Ads (PDA) campaigns. This functionality is only relevant for vendors as we have not rolled out Sponsored Display interest and product audiences for sellers yet. For more information, see the [Sponsored Display reporting documentation](sponsored-display/3-0/openapi#/Reports/requestReport). Advertisers can use this functionality in US, CA, UK, FR, ES, IT, DE, and JP.

### November 2019 release notes

#### Search term report is now available for Sponsored Brands

Search term reports give visibility into the search terms entered by shoppers searching on Amazon that resulted in at least one click. You can use this report to identify high performing searches from shoppers and to create negative keywords for search terms that don't meet your goals. For more information, see the [search term reports documentation](sponsored-brands/3-0/openapi#/Reports). 


#### Additional reporting functionality for Sponsored Display now available in the US and CA

Reporting for Sponsored Display campaigns that use interest and product audiences is now available. This functionality allows users to retrieve reports for recently created Sponsored Display campaigns that use these audiences and for Sponsored Display campaigns that were previously Product Display Ads (PDA) campaigns. This functionality is only relevant for vendors as we have not rolled out Sponsored Display interest and product audiences for sellers yet. For more information, see the [Sponsored Display reporting documentation](sponsored-display/3-0/openapi#/Reports). Advertisers can use this functionality in US and CA. We will also be rolling out this functionality in UK, FR, ES, IT, DE, JP, and IN soon and will make an announcement in a release note on this page, so check back regularly.

#### Amazon Ads API for data providers

Data providers can now use the Amazon Ads API to create and update audiences that advertisers can use in the Amazon DSP. This provides advertisers with more choice in their audience management. Using new endpoints, API users can edit audience descriptions and add or remove records from an existing audience. To use this functionality, follow the [getting started process](guides/onboarding/overview) and review the [documentation](data-provider/openapi).

### October 2019 release notes

#### Version 3 of the Amazon Ads API is now available 

Version 3 of the API includes functionality that was released in open beta earlier this year with no additional changes. Version 3 of API introduces elements that are specific to Sponsored Brands and changes that improve the ease of use of the API. For Sponsored Brands specific elements, the new version of the API introduces the ability to create a Sponsored Brand campaign via the API, upload and retrieve creative assets, retrieve the brand associated with a specific profile, and access creative and landing page information in the Sponsored Brands campaign object. The new version of the API adheres to OpenAPI standards, a widely adopted standard for documenting APIs. This functionality is available to all developers that have access to the Amazon Ads API. Note we are not announcing plans to deprecate version 1 or 2 at this time. To learn more, visit the [reference documentation](sponsored-brands/3-0/openapi) section or the [FAQ](info/faq) section in the developer portal and make sure to see this section on [Sponsored Brands moderation](sponsored-brands/3-0/openapi#/Moderation) and [campaign life-cycle status](reference/concepts/developer-notes).

#### RSS feed added to the developer portal

We've added an RSS feed to the developer portal. The RSS feed allows developer portal users to receive notifications any time there are changes to the API, making it easier to stay up to date. To subscribe to the RSS feed, right-click the RSS icon at the top right of this page, copy the link, and add it to your RSS aggregator.
 
#### Added "latest" to the version picker

We've made changes to the default view of the content. The documentation version picker drop-down box now displays the latest version of the reference for each product in the Amazon Ads API in a single view by default. This makes it easier to navigate through the latest reference documentation for all products. If you would like to see previous versions, select "previous" from the drop-down box above the table of contents. You can get back to the latest version by selecting "latest" from the same drop-down box. 


#### Ad groups now available for vendors

Ad groups are a way to organize and manage Sponsored Products ads within a campaign. Advertisers can use ad groups to group their ads by brand, product, category, price range, or other classifications like theme or targeting strategy. Previously, an error was returned if an account associated with a vendor attempted to create a campaign that included multiple ad groups. With this change, vendors can specify multiple ad groups in campaign create and update operations. See the [Amazon Ads support center](https://advertising.amazon.com/help#GKPA6T8WW3AYKV4Q) for more information on ad groups, and the [create and update operations](reference/sponsored-products/2/ad-groups) in the Developer Portal for more information about using ad groups with the API.

#### Moderation outcomes now available for Sponsored Brands campaigns

We have released functionality to provide moderation outcomes for Sponsored Brands campaigns though the Amazon Ads API. New moderation endpoints provide guidance to users to assist them in fixing non-approved Sponsored Brands campaigns. The guidance includes information about the reasons the campaign was rejected and links to relevant ad policies for additional explanation. Note that this release is currently available only in the US marketplace. For more information see the [Sponsored Brands moderation reference](sponsored-brands/3-0/openapi#/Moderation).

#### Product targeting is now available for Sponsored Brands

Sponsored Brands now offers a new, simple way to execute brand advertising strategies such as category expansion and brand reach strategies. The new product targeting option allows advertisers to target ads for categories they can refine by brand, price or star reviews. With product targeting, advertisers can engage more shoppers still exploring at a category level. With this release we are also making reporting available for campaigns that use product targeting expressions. Product targeting is currently in open beta. For more information see the [Sponsored Brands product targeting reference](sponsored-brands/3-0/openapi#/Product%20targeting).

#### Sponsored Brands bid recommendations for keywords and product targeting now available using the API

[Sponsored Brands bid recommendations](sponsored-brands/3-0/openapi#/Bid%20recommendations) for keywords and product targeting are now available using the API. Recommended bids are generated for keywords, targeting expressions, or campaigns. We are currently rolling out targeting expressions across locales. Bid recommendations are currently available in the FE and NA marketplace, and while the API functionality is available in EU, recommendations are not yet available. We'll update this release note when roll out of this functionality is completed.

#### Additional reporting functionality for Sponsored Display now available in sandbox

Reporting for Sponsored Display campaigns that use interest and product audiences is now available in the sandbox endpoint. This functionality allows users to retrieve reports for recently created Sponsored Display campaigns that use these audiences and for Sponsored Display campaigns that were previously Product Display Ads campaigns. This functionality is only relevant for vendors as we have not rolled out Sponsored Display interest and product audiences for sellers yet. For more information, see the [Sponsored Display reporting documentation](sponsored-display/3-0/openapi#/Reports). Advertisers can use the sandbox endpoint for testing, and will soon be available in an open beta in US, CA, UK, FR, ES, IT, DE, JP, and IN. We'll make the announcement in a release note on this page, so check back regularly.

#### Monthly recurring budget added to the API

Advertisers can now specify a monthly budget at the Portfolio level that is automatically renewed at the beginning of each month. A recurring budget can be set when a Portfolio is created or updated. For more information about the change to the API, see the [createPortfolios](reference/2/portfolios) and [updatePortfolios](reference/2/portfolios) operations. For more information about budgets, see [Portfolio budgets](https://advertising.amazon.com/help#GU7E77PGF96FMW3X) in the [Amazon Ads support center](https://advertising.amazon.com/help).

#### Daily Budget for Sponsored Brands has changed

Daily Budget for Sponsored Brands has changed. New campaigns with a daily budget are calculated as an average over a calendar month, just like Sponsored Products campaigns. Learn more at the [Amazon Ads support center](https://advertising.amazon.com/help#GTGPQGUXNCTHE2DS).

#### Sponsored Brands campaign names are now editable

Advertisers can now edit the name field for Sponsored Brands campaigns. For more details, see the [updateCampaign](sponsored-brands/3-0/openapi#/Campaigns) operation.

### September 2019 release notes

#### Sponsored Display now available through the API

Sponsored Display, a new self-service advertising solution that helps advertisers grow their business by reaching relevant audiences both on and off Amazon, is now available through the API. Functionality available today includes create, read, update, delete, and report functionality for Sponsored Display [views audiences](https://advertising.amazon.com/help#GPKPWTLW6B8T683N). Sponsored Display campaigns that use views audiences reach shoppers off Amazon who visited a product detail page or detail pages of similar products but didn't make a purchase.  For more information, see the release announcement in the [blog](info/blog), [FAQ](info/faq), and the [reference documentation](sponsored-display/3-0/openapi). 

#### Sponsored Display product and interest audience reporting documentation preview

Earlier today we announced that Product Display Ads campaigns are now part of Sponsored Display, included as campaigns that use product and interest audiences. While this functionality is not yet available through the API, we've published preview documentation for reporting of Sponsored Display campaigns that use product and interest audiences. We're sharing this documentation to help developers plan future implementation. When this functionality becomes available,  we'll notify users in a release note on this page. 

### August 2019 release notes

#### Keyword recommendations for Sponsored Brands Campaigns

Keyword recommendations for Sponsored Brands Campaigns are now available using the API. Advertisers can get keyword recommendations for ASINs that are associated with either a custom landing page, a Store page, or for vendors, a custom landing page. For a simple landing page, ASINs are passed as an array, while just the URL is passed for either a Store page or a custom landing page. To learn more, see the [keywords recommendation documentation](sponsored-brands/3-0/openapi#/Keyword%20recommendations). 

### July 2019 release notes

#### Sponsored Products now offers three new category targeting refinements

We have added three new category targeting refinements to Sponsored Products: shipping eligibility, genre, and product age range. Each new refinement should help advertisers expand reach and target ads more granularly. With shipping eligibility advertisers can target Prime or non-Prime shipping eligible products, and with genre and product age range targeting advertisers can easily refine targeting for nuanced categories. To learn more, see the [product targeting documentation](sponsored-brands/3-0/openapi#/Product%20targeting).

#### Sponsored Brands functionality through the Amazon Ads API now supports negative keywords

Negative keywords prevent ads from displaying when a shopper's search terms match negative keywords. Negative keywords can be used with broad, phrase, and exact match types to prevent ads from being displayed.

#### New metrics added to Sponsored Brands

We added new metrics to Sponsored Brands reports. The new metrics include units, detail page views, and orders. To learn more, see the Sponsored Brands [metrics documentation](sponsored-brands/3-0/openapi#/Reports.md).

#### Sponsored Brands draft campaigns

The Amazon Ads API now allows users to create, get, update, and delete campaigns that are in a draft state. The API can also be used to submit draft campaigns for moderation approval. To learn more, see the Sponsored Brands draft campaign [reference documentation](sponsored-brands/3-0/openapi#/Drafts).

#### New creative optimization feature available for Sponsored Brands 
This week we'll release functionality advertisers can use to engage shoppers with dynamically optimized Sponsored Brand ads. The new product optimization feature automatically selects the most contextually relevant ASINs from a Store or landing page so ads change to match each search. Users can set one campaign for an entire Store or landing page and let Amazon find the relevant products to show in the ad that best match the shoppers search. For more details see [relevant documentation here](sponsored-brands/3-0/openapi#/Campaigns).

### June 2019 release notes

#### Retrieve Stores associated with a profile

We added functionality enabling users to get Stores information associated with a [profile](reference/2/profiles). Users can now get the name and URL for both a Store and its sub pages. This makes it easy for users to find and use Store data when creating a Sponsored Brands campaign.

### May 2019 release notes

#### Sponsored Brands campaign creation open beta

We launched the ability to create Sponsored Brands campaigns via the Amazon Ads API.  This release also introduces version 3 of the API.  This release will bring the Amazon Ads API close to parity with the advertising console for sponsored ads. To learn more visit the [blog post](info/blog), [relevant documentation](sponsored-brands/3-0/openapi#/Campaigns) section or the [FAQ section](info/faq) in the developer portal. Also, make sure to see the developer notes on Sponsored Brands [moderation](reference/concepts/developer-notes)
and campaign life-cycle [status](reference/concepts/developer-notes).

#### Brand and category names added to target expression

Target object will now include a ResolvedExpression element for product target methods like getTargetingClause, listTargetingClauses, getNegativeTargetingClause, and listNegativeTargetingClauses. The added element will display the brand or category name for each brand and category included in a target expression. We implemented this improvement based on customer feedback.. To learn more about targeting expression see documentation [here](reference/sponsored-products/2/product-attribute-targeting).

#### New Sponsored Brands broad match modifiers

Sponsored Brands now offers broad match modifiers. Users that want a certain word to always appear in any broad matched keyword can add a broad match modifier by adding a '+' symbol before the word. e.g., if using a keyword "+men shoes" with broad match, then the ad will only match to queries that contain the word 'men'. The ad may match to "men sneakers", or "running shoes for men" etc. but not to "running shoes".

Sponsored Brands broad match now include words that are variations such as plurals, synonyms or other words related to the keyword. e.g., an advertiser's ad targeting 'shoes' using broad match may show on shopper query for 'sneakers'. 
This helps advertisers reach even more shoppers who may be interested in your products. 

Users can add the modifiers through API by adding a '+' symbol before the word. To learn more about Sponsored Brands keywords see documentation [here](reference/sponsored-products/2/ad-group-keywords).

#### Sponsored Products auto targeting (API-only feature) 

Advertisers that use the API to create Sponsored Products campaigns can set different bids per different automatic targeting groups (close match, loose match, substitutes, complements) when they create a campaign. This feature is only available through the API, in the advertising console users can only modify bids per targeting group once a campaign has already been created, for auto targeting documentation see [here](reference/sponsored-products/2/product-attribute-targeting)

#### Bid suggestions for target expressions

We launched the ability to retrieve bid recommendations for target expressions. With this functionality users can get recommendations for keywords, product targeting and automatic targeting expressions. For more details see documentation [here](reference/sponsored-products/2/bid-recommendations)

#### Australia support

We added support for Australia via the API. For more details see these developer portal sections: [endpoints and supported countries](get-started/overview.md), [keyword bid constraints](get-started/overview.md), [regional profile time zone codes](reference/2/profiles) and [register profile country list](reference/2/profiles).

### March 2019 release notes

#### New-to-brand metrics now available

New-to-brand metrics for Sponsored Brands are now available in the Amazon Ads API. New-to-brand metrics determine whether an ad-attributed purchase was made by an existing customer or one buying a brandâ€™s product on Amazon for the first time over the prior year. With new-to-brand, advertisers receive campaign performance metrics such as total new-to-brand purchases and sales, new-to-brand purchase rate, and cost per new-to-brand customer. See 
documentation [here](sponsored-brands/3-0/openapi#/Reports).


### New or changed objects - January 2019
| Entity or service level | Detail |
| --- | --- |
| [Profiles](reference/2/profiles) and [Supported features](get-started/overview) | **Availability of new marketplace, Japan (JP)**. Added JP to regional profile time zone codes and JPY currency code to resource representation in [Profiles](reference/2/profiles.md). See additions in [Supported Features](get-started/overview) for Far East (FE) API endpoint and [keyword bid constraints](get-started/overview) for JP marketplace. |


## Advertising API version advance to v2

### Advertising terminology and name changes 

We [recently announced](https://advertising.amazon.com/blog/amazon-advertising-simplified?ref=_int-email) that we are now simply, Amazon Ads. With this, we retired the names Amazon Media Group (AMG), Amazon Marketing Services (AMS), and Amazon Ads Platform (AAP), and announced a simplified product portfolio. This change impacts API users because of syntax built into the structure of the API. The documentation for v2 will continue to reference the older product names in the technical reference sections. A key is provided below to assist in transition to the new terminology.

| New terminology| Retired terms |
| --- | --- |
| Amazon Ads |-	Amazon Marketing Services (AMS)<span class="break"> </span>-	Amazon Media Group (AMG)<span class="break"> </span>-	Amazon Ads Platform (AAP)	 |
| Sponsored Brands (SB) | Headline Search Ads (HSA) |
| Amazon DSP | Amazon Ads Platform (AAP) |

### Requirements for API v2

-	All API calls must be routed to the new v2 endpoints. 
-	All API calls to v2 endpoints must contain the Amazon-Advertising-API-ClientId header. 

### Migrating from v1 to v2 

Migrating to v2 means changing your application to use the new v2 endpoints. The API v1 will continue to be supported. Applications that call to v1 endpoints will not require modification to how they currently operate. Integration with v2 is required to access the new features available in v2 and is recommended preparation for future releases.

### New or changed objects - October 2018

| Entity or service level | Detail |
| --- |
| Authorization | Client ID is **now a required header** for all requests to v2 endpoints. The Client ID header must match the case-sensitive Client ID encapsulated in the access token. This is the same Client ID found in the Web Settings section of the Login with Amazon App Console.<span class="break"> </span>The header of your request must contain: **Amazon-Advertising-API-ClientId** and your unique Client ID.|
| Profiles | URL endpoint for profiles changed to `/v2/profiles`. |
| Profiles | There is a new field for Profile objects which are now differentiated by the filter: `type`. This filter has possible values of: `vendor` or `seller` |
| Profiles | There is a new field for Profile objects called `id` that replaces `sellerStringId` and `brandEntityId` |
| Profiles | Removed the fields: `sellerStringId` and `brandEntityId` |
| Profiles | The field `brandName`has been renamed to `name` |
| Campaigns | URL endpoint for campaigns changed to `/v2/sp/campaigns` for Sponsored Products. Endpoint changed to `/v2/hsa/campaigns` for Sponsored Brands. |
| Campaigns | POST method is not supported for Sponsored Brands, new campaigns must be created through the user interface. |
| Ad groups | URL endpoint for ad groups changed to `/v2/sp/adGroups`. Ad groups can only be managed for Sponsored Products and do not apply to Sponsored Brands. |
| Ad group biddable keywords | URL endpoint for keywords changed to `/v2/sp/keywords` for Sponsored Products. Endpoint changed to `/v2/hsa/keywords` for Sponsored Brands. |
| Ad group biddable keywords | Keywords operations support up to 1000 keywords per request for Sponsored Products. |
| Ad group biddable keywords | Keywords operations support up to 100 keywords per request for Sponsored Brands. |
| Ad group biddable keywords | The `adGroupId` is a required field for updating keywords for Sponsored Brands through the API. There is only one ad group allowed per campaign and it acts like a placeholder. You will receive the `adGroupId` as part of the response object for a GET keywords request. |
| Ad group negative keywords | URL endpoint for ad group negative keywords changed to `/v2/sp/negativeKeywords`. Negative keywords are not used with Sponsored Brands. |
| Campaign negative keywords | URL endpoint has been changed to `/v2/sp/campaignNegativeKeywords` for Sponsored Products. Campaign negative keywords are not used for Sponsored Brands. |
| Suggested keywords | URL endpoint for Sponsored Products suggested keywords operations has been changed to: `/v2/sp/AdGroups/{adGroupId}/suggested/keywords`. The change is to use /sp/ to specify Sponsored Products. Suggested keywords are not used for Sponsored Brands. |
| Product ads | URL endpoint for product ads changed to `/v2/sp/productAds` for Sponsored Products. Not used with Sponsored Brands. |
| Bid recommendations | URL endpoint for bid recommendations changed to `/v2/sp/{record type}/{entity id}/bidRecommendations` for Sponsored Products. Not used with Sponsored Brands. |
| Reports | Separate URL endpoints for Sponsored Products and Sponsored Brands for reports operations. Changed to: `/v2/{sp or hsa}/` |
| Reports | URL endpoint for asins reports changed to: `/v2/asins/report`. |
| Entity snapshots | Separate URL endpoints for Sponsored Products and Sponsored Brands for snapshots POST operations. Changed to: `/v2/{sp or hsa}/` |
