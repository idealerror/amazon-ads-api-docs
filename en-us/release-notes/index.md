---
title: Amazon Ads advanced tools release notes
description: Amazon Ads advanced tools release notes, with information about new features and other changes to the tools.
type: release-note
interface: api
keywords:
    - Slack
    - RSS feed
    - updates
---
# All releases

> [TIP:Connect your team to Amazon Ads API release notes]You can subscribe to these release notes in your team's workspace via our [RSS feed](https://d3a0d0y2hgofx6.cloudfront.net/rss/en-us/ad-api-rss.xml).<details style="margin-top:8px;"><summary>Connect in Slack</summary><ol><li>[Add the RSS app to your Slack workspace.](https://slack.com/help/articles/218688467-Add-RSS-feeds-to-Slack)</li><li>Send this message in the channel where you want the feed to appear:<br>`/feed subscribe https://d3a0d0y2hgofx6.cloudfront.net/rss/en-us/ad-api-rss.xml`</li></ol></details><details><summary>Connect in Microsoft Teams</summary><ul><li><a href="https://docs.microsoft.com/en-us/connectors/rss">Create a new RSS connector</a> through the **Apps** menu in Teams.</li><li>Provide the following URL:<br>`https://d3a0d0y2hgofx6.cloudfront.net/rss/en-us/ad-api-rss.xml`</li></ul></details>

### September 2025

#### International expansion of OLV Ads on Alexa

We’ve expanded online video (OLV) ads on Alexa from [US](https://advertising.amazon.com/resources/whats-new/online-video-ads-on-alexa-home-screen) to JP, MX, CA, UK, BR, IN, DE,  IT, FR, and ES marketplaces. This expansion will provide advertisers an opportunity to expand their reach with incremental customers on Echo Show devices, while driving awareness and consideration for their brand, with a voice-enabled OLV placement.

Note: A click is recorded when the customer engages, via touch or voice, on the auto-playing online video ad on the Alexa Home Screen to launch the video ad in the full screen view.

### August 2025

#### Share frequency capping across manager accounts with Amazon DSP

Amazon DSP now allows advertisers to share frequency groups between manager accounts, providing broader control over reach and frequency across their brand portfolio. This enhancement enables advertisers to manage frequency caps across different manager accounts while maintaining individual account control and visibility.

Key endpoint updates in the API include:

- ListFrequencyGroupsV1: Added `frequencyGroups` list parameter and `entityName` field
- ListFrequencyGroupAdvertiserAssociationsV1: Added `entityName` and `entityId` attributes
- ListFrequencyGroupCampaignAssociationsV1: Added `entityName` and `entityId` attributes

For full technical details, see the specifications for the [frequency groups](dsp-frequency-group) and [frequency group associations](dsp-frequency-group-association) APIs.

#### Introducing new audiences for bid adjustment for Sponsored Brands 

Sponsored Brands introduces two new Amazon-built audiences to help advertisers re-engage with valuable shopper segments: "Purchased brand's product" and "Clicked or Added brand's product to cart". These audiences complement the existing "New-to-brand shoppers" audience, giving advertisers more options to optimize their campaigns through audience bid adjustments.

Advertisers can utilize audience bid boosting on both new and existing Sponsored Brands campaigns. To assess the performance of their bid boosted audiences, advertisers can access the [Audience Report](guides/reporting/v3/report-types/audience#sponsored-brands) through the SB API Reporting. 

To retrieve Amazon-built audiences, use the [Discovery API ListTargetableEntities](targetable-entities#operation/ListTargetableEntities) with the following parameters: `adProduct=SPONSORED_BRANDS, targetTypeFilter=AUDIENCE, and pathsFilter = [["Audience Category", "Custom-built", "Product"]]`. The response will include the `audienceId` for these audiences, which can then be used with [Sponsored Brands Campaign Management API](sponsored-brands/3-0/openapi/prod#tag/Campaigns) to create or adjust the bids for the audience. Amazon-built audiences retrieved using these filters are usable only with `AudienceSegmentType` of `BEHAVIOR_DYNAMIC` when using the campaign management API. For full technical details, see our [updated Sponsored Brands documentation](guides/sponsored-brands/bidding/shopper-cohort).

#### Reach Alexa customers with full screen ads through Responsive eCommerce creative on the Amazon DSP in more marketplaces

We’ve expanded Responsive eCommerce on Alexa ads to FR, ES, IN, and BR. Now ADSP-Self Service buyers can include Alexa supply in their campaigns and deliver engaging Responsive eCommerce ads to new customers. 

#### New Performance+ Insights on Amazon DSP now available in beta

Amazon DSP has launched Performance+ Insights, a new feature that provides advertisers with detailed transparency into their machine learning-generated audience targeting through two key components: (1) Shopper Insights that reveal the behavioral patterns and characteristics of customers reached by automated targeting (such as "premium sports shoes purchasers" or "enjoy sports content") and (2) Time-to-Convert metrics that display the median time between ad interaction and the attributed conversion. 

The Performance+ Insights feature is important for marketers because it addresses a critical trust deficit by providing transparency into ML-generated audience composition and performance. By revealing detailed audience behavioral traits and conversion metrics, marketers gain visibility into previously "black box" targeting mechanisms, allowing them to validate that their ads are reaching relevant audiences and converting effectively.

For more information about the new campaign insights API endpoint, [view the technical specification](dsp-campaign-insights).

#### Streamline your brand management with new APIs that enable automated access to global brand identifiers

Amazon has launched two new APIs that enable access to global brand identifiers through the Advertising API Gateway. Global Brand IDs streamline brand identification by reducing multiple marketplace-specific IDs into a single identifier, enabling consistent brand recognition across all marketplaces. This transforms the multi-marketplace IDs into a single identifier, such as Adidas going from 38 brand IDs to just a single global brand. The introduction of Brand status attributes allows customers to filter and interact with only valid, active brand information while eliminating duplicate or merged entries.

Two new APIs are accessible through the Amazon Ads API:

- QueryBrandAdvertiserAssociation API (POST/adsApi/v1/query/brandAdvertiserAssociations) is a paginated API that authorizes on Global Advertiser IDs and returns Global Brand IDs associated with Global Advertiser Account. 
- RetrieveBrands API (POST/adsApi/v1/retrieve/brands) returns Global Brand IDs metadata, such as brand name, brand status, the list of websites and logos. The API accepts 100 brands in the request at a time.

For more information, see the [technical specification for the brand associations APIs](amazon-ads/1-0/brand-associations).

#### Campaign management API now available for Sponsored Products, Sponsored Brands, and Amazon DSP

Amazon Ads has launched a single Campaign Management API to unify campaign management into one set of APIs. Built from a [common Amazon Ads data model](reference/amazon-ads/overview), this API standardizes field and enum values across ad products and features, improves error handling, and helps resource consistency across Campaign, Ad Group, Target, and Ad objects. This API includes feature parity with the existing APIs. Previously, there were nearly 200 separate variations of create, read, update, and delete endpoints that will be reduced to 16 with this launch. 

Today, we announce the completion of Sponsored Products, Sponsored Brands, and Amazon DSP for worldwide general availability. All of our ad products will be available through the Amazon Ads Campaign Management API, including Sponsored Display and Sponsored TV later in 2025.

To get started, please see the [campaign management overview](guides/campaign-management/overview) and the [technical specification](amazon-ads/1-0/openapi).

### July 2025

#### Supporting Global Stores on Global Campaign API-based advertisers 

With the launch of Sponsored Product Ads on Amazon Global store, API advertisers can now drive more discoverability and improve sales internationally. In this launch phase, we enabled this capability for advertisers in US, UK, and Germany. Log into your seller account to [learn more about Global Store](https://sellercentral.amazon.com/help/hub/reference/202139180). 

* **Product Localization [/products/localize](localization#tag/Product-Localization/operation/getLocalizedProducts)** 
Sponsored Products Ads now extends its product localization capabilities to include GlobalStore products, allowing products to be mapped from a source marketplace to one or more target marketplaces. This enhancement enables advertisers to expand their reach across multiple marketplaces through both local and GlobalStore products. To support GlobalStore products, the API response includes additional fields:
  * `matchType`: Indicates whether the product is `LOCAL` (sold locally in the marketplace) or `GLOBAL_STORE` (sold through the GlobalStore Program).
  * `catalogSourceCountryCode`: For GlobalStore products, this field in the Product Metadata shows the country code of the source marketplace where the seller originally listed the product (possible values: `US`,` UK`, `DE`, `JP`, `AE`).
* **Campaign Creation [/global/productAds](global-cm#tag/Product-Ads/operation/CreateProductAdsOperation)**
SP API or product ads now support the global store specific features for the Global campaign creation. Global Sponsored product ad creation API accepts the global store specific attribute `globalStoreSetting` where advertisers can provide the source catalog country of the offer being used for ads creation. Eligibility check of the products for campaign creation would be done using the `catalogSourceCountryCode` information provided using the `globalStoreSetting` under `CountryAdSubjectSettings`.
* **Global Bid Recommendation [/sp/targets/bid/recommendations](sponsored-products/3-0/openapi/prod)**
The Global Bid recommendations API now support the features of the bid recommendation for the Global Store listings. The Global Bid recommendation API for new Ad group supports a new attribute called `products` that takes the ASIN details along with its source country code. Bidding system uses the `globalStoreSetting` attribute and the `catalogSourceCountryCode` information to check the eligibility of the ASINs that are required for the bid recommendation.
* **Global Keyword Recommendation [/sp/global/targets/keywords/recommendations](sponsored-products/3-0/openapi/prod)**
The Global Keywords recommendation API now support the features of the keyword recommendation for the Global Store listings. The Global Keywords recommendation API for ASINs supports a new attribute called `products` that takes the ASIN details along with its source country code. Global Keywords recommendation system uses the `globalStoreSetting` attribute and the `catalogSourceCountryCode` information to get the bid suggestions for the keywords.

#### DSP frequency groups are now available through the Amazon Ads API

In September 2023, we launched frequency groups for self-service entities in the Amazon advertising console, enabling advertisers to set a frequency cap across multiple orders. With this launch, we are expanding frequency groups to the Amazon DSP Public API, so advertisers can easily create and update frequency groups via the Amazon Ads API.

Brands need broader frequency capping controls to limit the number of times a unique user is exposed to their brands across multiple orders in Amazon DSP, as users could be overexposed to ads when different orders target similar or overlapping audiences. With Hybrid Frequency Caps, advertisers can better manage frequency across their brand portfolio while optimizing campaign performance, such as setting streaming TV frequency caps across specific brands by selecting only streaming TV orders within their frequency groups, helping prevent ad overexposure and maximize budget efficiency, with advertisers seeing an average 6% lift in unique user reach when using Frequency groups to manage frequency across multiple orders in Amazon DSP.

For full technical information, see the API specifications for [frequency groups](dsp-frequency-group) and the [frequency group associations](dsp-frequency-group-association).

#### Automatically share unspent campaign budget with budget exhausted campaigns in a portfolio

We're introducing a smarter way to manage your campaign budgets within portfolios. Now, when some campaigns don't use their full daily budget, that unspent campaign budget won't go to waste. Instead, it can automatically flow to other campaigns in your portfolio that consistently reach their daily spending limits.

This intelligent budget sharing system allows high-performing campaigns to spend up to twice their daily budget by tapping into unspent campaign budget from previous days in the month. This all happens while staying within your total monthly portfolio budget.

This enhancement helps you capitalize on high-traffic days and maintain momentum when certain campaigns are performing well. Your successful campaigns can keep running longer, generating more clicks, impressions, and sales opportunities that might otherwise have been missed.

Implementing this feature is straightforward. Simply access your **budgetControls** settings and set the **campaignUnspentBudgetSharing** feature to **ENABLED** or **DISABLED**, depending on your preference. Whether you're a vendor, registered seller, managed-service customer, self-service customer, or author, this feature is available to help optimize your advertising performance.

For step-by-step guidance on setting up and managing this feature, please refer to the [User Guide](guides/portfolios/sharing-leftover.md).  

The launch is available in the following geographical areas. 

•	North America: United States, Canada, Mexico
•	South America: Brazil
•	Europe: Germany, Spain, France, Italy, United Kingdom, Belgium, Switzerland, Poland, Turkey, Austria, Netherlands, Sweden, Finland, Norway, Ireland, Denmark, Luxembourg
•	Middle East: Saudi Arabia, United Arab Emirates, Israel, Egypt, Morocco, Bahrain, Kuwait
•	Asia Pacific: Australia, India, Japan, New Zealand, China, Singapore

#### Alexa+ tests product discovery through conversational AI 

Sponsored products campaigns will now be automatically integrated into shopping conversations when customers chat with Alexa+ about products. Customers can discover relevant products when asking Alexa for product recommendations, receiving a mix of organic and sponsored options. Advertisers with active Sponsored Products campaigns may be eligible to appear on Alexa+ as part of Amazon's expanded advertising surfaces, helping you connect with customers wherever they shop on Amazon with no additional requirements. Performance metrics will be consolidated within existing reporting dashboards. At this time, Sponsored Products on Alexa+ is only available on the Echo Show. Following the exciting announcement about [Alexa+](https://www.amazon.com/dp/B0DCCNHWV5), our next-generation conversational AI assistant, Alexa+ brings a more natural, context-aware conversation experience to users, and this opens up new opportunities for advertisers to engage with customers during their shopping journey.

#### Updated Deprecation Date: Amazon DSP Legacy APIs (Campaign, Creative and Reporting) will shut off on September 30, 2025

As part of our continued enhancements to the Amazon DSP APIs, we are deprecating several legacy endpoints across Campaign Management, Reporting, and Creatives Management. The deprecation date for the Campaign Management API has been extended to September 30, 2025 (previously June 30, 2025), while the legacy ADSP Reporting API and the legacy Creatives Management API will also be deprecated on September 30, 2025. You can find the full list of deprecated and replacement endpoints on our [Deprecations page](https://advertising.amazon.com/API/docs/en-us/release-notes/deprecations). For upgrade guidance, refer to the various migration guides for Campaign, Creative and Reporting. If you have questions or need support, submit a ticket via the [Amazon Ads API Support page](https://advertising.amazon.com/API/docs/en-us/support/overview) with the subject “ADSP APIs Migration” or contact your account team or use the standard support channels. We recommend migrating early to ensure uninterrupted access and to take advantage of the latest API capabilities.

#### Account registration API now available in closed beta

The Advertiser Account is Amazon's enhanced workflow for onboarding advertisers and managing their full advertising activity across all ad products (ADSP and Sponsored Ads) and countries. With this launch, customers with an Amazon DSP Seat can use Ads Console to onboard advertisers with a new streamlined account and billing setup. In this account, customers can operate across both ADSP and Sponsored Ads globally across the full advertising lifecycle from campaign management and reporting, to user management and billing. API customers have the same experience on Amazon Ads API, enabling them to onboard advertisers at scale without relying on Ads Console to do so. If you have an ADSP seat and would like to use this feature, please speak with your Amazon ad-tech AE.

Previously, registration and account management functionality for Amazon Ads advertiser accounts was only available through the Ads Console. With this launch, Amazon Ads API customers can execute all account management related actions (create, view, update advertiser accounts etc.) for all Amazon ad products and countries via API without the need to visit Ads Console to do so. Now, customers can register Advertiser Accounts at scale via public API, and they can use the resulting account identifiers to execute campaign management, reporting, billing and user management operations across all countries and ad products (ADSP, SA). Below is the list of new APIs launched:

- [POST /adsApi/v1/create/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/CreateAdvertiserAccount): Register one or multiple advertiser accounts by providing business details or importing them from a selling account token
- [POST /adsApi/v1/query/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/QueryAdvertiserAccount): Fetch account details/attributes (such as business/billing details, selling accounts, alternate identifiers for public APIs etc.) for a given advertiser account, a group of advertiser accounts, or all advertiser accounts that a given user has access to
- [POST /adsApi/v1/update/advertiserAccounts](amazon-ads/1-0/account-registration#tag/AdvertiserAccounts/operation/UpdateAdvertiserAccount): Programmatically update account details/attributes for advertiser account(s)
- [POST /adsApi/v1/query/sellingAccounts](amazon-ads/1-0/account-registration#tag/SellingAccounts/operation/QuerySellingAccount): Fetch all Amazon retail selling account (e.g. Seller, Vendor, Author, KDP etc.) that a given user has access to, so that the resulting encrypted selling account token can be used to register an associated advertiser account.

#### Set ad frequency in 10-minute increments for more granular campaign management

We've launched minute grain frequency caps in Amazon DSP, allowing advertisers to set frequency caps in 10-minute increments (for example, 10, 20, 30, 40, and 50 minutes). This enhancement provides significantly finer control than the previous hourly or daily options, addressing a critical request from customers seeking to prevent ad repetition with greater precision.

Minute grain frequency caps help prevent ad fatigue by controlling how frequently users see ads within shorter time intervals. This granular control is especially valuable for high-impact streaming TV campaigns and time-sensitive promotions where multiple impressions in quick succession can diminish effectiveness. By distributing impressions more optimally, advertisers can improve budget efficiency and brand engagement.

Minute grain frequency caps are accessible through existing campaign management API endpoints. The feature maintains backward compatibility with all existing frequency cap implementations. For full technical details, read our [API documentation](dsp-ad-group-and-campaign).

#### Sponsored Brands: Now available for single-book authors

Sponsored Brands is now available to all authors, including those with a single title. Create engaging custom image and video ads to drive awareness and boost your book's visibility. To learn more about Sponsored Brands, see the [Sponsored Brands overview](guides/sponsored-brands/overview).

#### DSP Bid Adjustments API supports supply dimensions

Customers using the DSP Bid Adjustments APIs can now target specific supply sources, source type, and deals to upscale or downscale their bids. This enables customers to more finely tune the parameters of their bidding to their specific use case and increase their campaign performance.

For more details, see the three new dimensions -- dealId, supplySource, supplySourceType -- in the [bid adjustment dimensions guide](guides/dsp/bid-adjustments/dimensions).

#### Automatically share unspent campaign budget with budget exhausted campaigns in a portfolio

Your portfolios can now automatically share their unspent campaign budget, accumulated from previous days in the month, with other campaigns in the same portfolio that have already spent their full daily budget, to enable them to spend more (up to 100% over their daily budget). This system allows you to benefit from high traffic days by keeping campaigns in budget longer. 

This launch automatically uses unspent campaign budget on campaigns that consistently run out of their own daily budget, enabling them to keep delivering ads and earning you more clicks, impressions, and sales, all without increasing your overall total monthly campaign budget. Users can now set the unspent campaign budget sharing option to `on` or `off` by setting featureState for `campaignUnspentBudgetSharing` as `DISABLED/ENABLED` inside **budgetControls**. Reference the user guide for more details. 

This feature is available to customers who are vendors, registered sellers, customers from managed-service, self-service, and authors.  

The launch is available in the following geographical areas:

* North America: United States, Canada, Mexico
* South America: Brazil
* Europe: Germany, Spain, France, Italy, United Kingdom, Belgium, Switzerland, Poland, Turkey, Austria, Netherlands, Sweden, Finland, Norway, Ireland, Denmark, Luxembourg
* Middle East: Saudi Arabia, United Arab Emirates, Israel, Egypt, Morocco, Bahrain, Kuwait
* Asia Pacific: Australia, India, Japan, New Zealand, China, Singapore

### June 2025

#### Manage Brand Stores programmatically through the Amazon Ads API

Brand Stores has launched a new set of APIs that offer brands and advertisers programmatic access to manage their Amazon Brand Stores more efficiently. These APIs, now available through the Amazon Ads API, provide two key functions: 1) retrieve Brand Store page content and update the product content and selection across Brand Stores pages and 2) submit store updates for moderation.

Advertisers can use these APIs to programmatically manage their Brand Stores pages. These functionalities can be combined with Brand Home APIs to automate and maintain product information freshness, creating a more efficient and streamlined management process for Brand Stores on Amazon. 

The Brand Stores management APIs are available in beta to all Brand Store owners through the Amazon Ads API. To learn more, see the [Brand Stores management overview](guides/brand-store/user-guide) and the [technical specifications](amazon-ads/1-0/brand-stores).

#### Sponsored ads and Stores launch in Ireland

Sponsored ads have now launched in Ireland. Eligible sellers and vendors can now leverage Sponsored Products, Sponsored Brands and Stores on Amazon.ie and via the Amazon Ads API.

#### New API to query account links available in closed beta 

We have introduced a new POST endpoint /adsApi/v1/query/accountLinks that returns information about linked accounts in a paginated manner. Manager Account users can retrieve details about all accounts (Sponsored Ads accounts, ADSP advertiser accounts, and Manager Accounts) linked to their Manager Accounts. Additionally, advertiser account users can find all Manager Accounts linked to their advertising account. This helps users better understand the structure and access relationships across their advertising setup.

This enhancement helps customers with complex account structures better understand their organizational account structure. The new paginated API fixes two key limitations: it removes the previous 50-linked-account limit for Manager Accounts in the GET /managerAccounts API, and it gives advertiser account users the ability to query linked Manager Accounts via API, a feature not previously available. Customers can now retrieve complete information on their account linking structure, helping them make better decisions about their organization's advertising spend and performance.

To learn more, see the [linked accounts API user guide](guides/account-management/linked-accounts) and the [technical specification](amazon-ads/1-0/permissions).

#### Customize bids for top of search and rest of search placements with updated placement bid adjustment controls 

Advertisers can now apply bid adjustments up to 900% for the top search placements in their Sponsored Brands campaigns. Bid adjustments for rest of search placements, which were previously limited to up to 99% can now also be adjusted up to 900%. These updates are available for all ad formats, including video, and all targeting types (keyword and product), and can be used with existing bidding controls (manual bidding, cost controls, audience-level bid adjustments). 

The updated placement-level bid adjustment controls will allow advertisers to optimize campaign performance and execute different strategies across search placements. Advertisers will be able view placement-level metrics in the advertising console or can leverage existing Sponsored Brands Placement reports to understand how their campaigns are performing in specific placements, then use the top of search and rest of search placement bid adjustment controls to fine-tune bids to meet their campaign goals. 

Top of search and rest of search placement bid adjustment controls will be available for Sponsored Brands campaigns via the Amazon Ads API. For full technical details, see the updated [Sponsored Brands API specification](https://advertising.amazon.com/API/docs/en-us/sponsored-brands/3-0/openapi/prod#tag/Campaigns).

#### Exclusively reach business shoppers with Amazon Business exclusive campaigns for Sponsored Brands

Advertisers using the Amazon Ads API can now create Sponsored Brands campaigns that exclusively reach business shoppers on Amazon Business, available in Amazon Business Marketplaces - US, CA, MX, DE, IT, FR, ES, UK, IN, JP. To create Amazon Business exclusive campaigns, advertisers will need to set the site Restrictions field in the campaign setting to AMAZON_BUSINESS. Advertisers can drive brand awareness across business shoppers and customize their B2B advertising strategy by setting a B2B budget, using B2B keywords, B2B creatives, and B2B-specific store pages, and adjusting bids to increase sales on Amazon Business. All campaign settings apply exclusively to Amazon Business, and ads will only appear on the Amazon Business store. The Sponsored Brands reports will reflect performance solely on Amazon Business, enabling advertisers to evaluate results and optimize their B2B campaign strategies. See our [reference documentation](https://advertising.amazon.com/API/docs/en-us/sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredBrandsCampaigns) for more information.


#### Long term sales metrics are now available in campaign reporting

Long term sales metrics are available in campaign reporting. Long term sales (LTS) and long term sales ROAS (LTS ROAS) are ad-attributed measures that estimate the incremental sales value your brand can expect to generate over the next 12 months, based on how effectively your advertising campaigns are able to move new-to-brand shoppers further down your purchase funnel. This is based on the historical 12-month return of a given customer engagement with your brand. Customer engagements include detail page views, branded searches, add-to-carts, and purchases. Shoppers are new to brand if they haven't completed a conversion with a brand in the last 12 months.

For more information, see our updated [campaign reports overview](guides/reporting/v3/report-types/campaign).

#### Amazon Ads Forecasting API (beta) is now available

Amazon Ads has launched a new forecasting API for Amazon DSP (ADSP) campaigns. Built on the Amazon Ads data model, this API provides access to all real-time forecasts currently available on ADSP Campaign Manager.

In this launch, the API offers campaign-level performance, delivery, and spend forecasts currently available on forecasting widgets. This includes campaign flight level forecasts for spend, reach, CPC, CPA, and ROAS. The API provides forecasts both for new orders and existing orders that are spending budget.

Throughout 2025, we will expand the capabilities of the API to include forecasts for other features on the campaign manager such as line-level forecasts, additional spend potential, and creative forecasts. These metrics will also be available using datasets through Amazon Marketing Cloud for easier integrations. For more information, please see the [technical specification](amazon-ads/1-0/forecasts) and the [Forecast API overview](guides/dsp/forecasts).

#### Posts are deprecated as of June 3, 2025

The Posts API will be discontinued as of June 3, 2025. New users will no longer be able to make requests to the Posts API; existing Posts users should expect new Posts creation to be discontinued after June 16, 2025, and a full shut-off date of July 31, 2025. We encourage customers to explore our suite of sponsored advertising solutions as we continue to build and experiment with new creative formats.

### May 2025

#### Greater transparency for Twitch campaigns with Content Type, Rating, and Title reporting

We have expanded our content transparency reports on Streaming TV to report ad-impressions delivered on Twitch. Advertisers can now see which Content Type, Rating and Title their Twitch ad impressions were delivered on to now assess if their ads were contextually relevant and use the reporting data to plan their future media investments and adjust their campaign settings.

Content Type, Rating and Title traffic reporting are now available in ADSP downloadable reports under the Audio and Video dimension. For full technical details, please see the [audio and video reports guide](guides/reporting/v3/report-types/audio-and-video).

#### Amazon Marketing Cloud launches Audience creation using Amazon Retail Purchases dataset

The Amazon Retail Purchases (ARP) dataset gives AMC customers 5 years (60 months) of historical Amazon store purchase signals across the advertiser's catalog of products, regardless of media investment against those products. AMC customers in US/CA/JP/AU regions can now use this AMC dataset for audience creation purposes. To get started, see the [Instructional Query (AMC UI access required)](https://advertising.amazon.com/marketing-cloud/instructional-queries/80a886500a4fe9f27e419f410d4f89d1de53ccee201cc7cd2a5e872062b5b8c2?tab=template).

#### New "Betas" section in the advanced tools center

We have added a new content section to the advanced tools center: Betas. The new section will host information about the forthcoming Amazon Ads API v1 as well as specifications and guides for new resource APIs in both open and closed beta stages. To learn more, visit [Betas](developer/betas).

#### Amazon Ads Campaign Management API (beta) is now available

Amazon Ads has launched a single Campaign Management API for all ad products, starting with Sponsored Products, Sponsored Brands, and Amazon DSP. Built from a common Amazon Ads data model, this API will ensure field naming, enumerated values, and resource consistency across Campaign, Ad Group, Target, and Ad objects. 

Today, there are over 190 separate variations of create, read, update, and delete endpoints, which will be reduced to 16 with this launch. With the Amazon Ads Campaign Management API developers can expect to see complete feature parity, improved error handling, consistent request and response patterns, and higher pagination thresholds than previous versions.

Throughout 2025, all of our ad products will become available through the Campaign Management API, including Sponsored Display and Sponsored TV. We will also be launching consistent campaign datasets through Amazon Marketing Stream, built from the same underlying Amazon Ads domain model, for further consistency across all products. 

Although we do not anticipate updates to the contract, the contract may be subject to change during the beta period. 

For more information, please see the [technical specification](amazon-ads/1-0/openapi) and the [campaign management overview](guides/campaign-management/overview).

#### Unlock greater flexibility in Partner Opportunities API with advertiser filtering

We’ve extended the Partner Opportunities API so you can filter for specific advertisers you manage. Partner Opportunities continues to provide you scaled access to insights and recommendations for your advertising clients like unlaunched ASINs and click credit promotions. With this launch, you can provide an advertiser ID or profile ID when listing opportunities (`GET/partnerOpportunities`) and quickly see which opportunities have results for the specified advertisers. 

Whether you want to determine which cohorts of your advertising clients have opportunities available or pinpoint results for a single target advertiser, can structure your call pattern to find the right balance for you. Now, you can use the optional `advertiserId` or `profileId` filters, see which partner opportunities align with the provided advertisers, then access the detailed recommendation and insights as you always have. For more information, see our [API documentation](partner-opportunities#tag/Partner-Opportunities/operation/partnerOpportunitiesListOpportunities).

Partner Opportunities API is available worldwide to all registered partners. See the [overview guide](guides/recommendations/partner-opportunities/overview) for more information. 

#### Negative targets can now be saved on campaigns and ad group regardless of targeting type in US marketplace

We’ve expanded negative targeting so every campaign and ad group has the same options, regardless of targeting type. Now you can negatively target a keyword, ASIN, or brand during campaign creation or on any existing campaign or ad group. Negatively targeting a keyword prevents your ad from showing on search results pages for that keyword, while ASIN and brand negative targeting prevents your ad from showing on the product detail page for that ASIN or ASINs with that brand.

Advertisers might have certain business rules they want to implement but execution was inconsistent across different targeting types. Now, advertisers will have consistent and clear control of negative targets, helping reduce confusion and operational overhead. 

**Bulksheets**: Negative product targeting uses product targeting expressions to also negatively target brands. Learn more: [Get started with bulksheets for Sponsored Products](bulksheets/2-0/get-started-with-sp-bulksheets).

**API**: Now you can succeed saving negative keywords, products, and brands on campaigns and ad groups regardless of targeting type. Learn more: [Negative keyword targeting](guides/sponsored-products/negative-targeting/keywords) and [Sponsored Products negative product and brand targeting overview](guides/sponsored-products/negative-targeting/product-brand).

#### Request Amazon’s Marketing Mix Modeling (MMM) feed via API

We have launched a new API solution that enables programmatic access to Amazon’s Marketing Mix Modeling (MMM) data feed. This new API is available in 13 countries: US, CA, IT, ES, FR, DE, UK, MX, BR, JP, AU, UAE, and KSA. This launch allows advertisers and their partners to seamlessly access the advertising and retail signals needed to derive accurate insights from their MMM solutions. 

This API access streamlines data delivery for advertisers and their MMM partners, reducing manual data exchange efforts, and enabling more frequent, reliable and accurate MMM analysis. 

Three API operations are available for MMM management: 

* Request the list of brands you can pull data for: `POST /mmm/v1/brandGroups/list`
* Request the MMM feed for a given brand: `POST /mmm/v1/reports`
* Check the status of a MMM feed request: `GET /mmm/v1/reports/{reportId}`

For full technical details, see the [MMM API user guide](guides/reporting/marketing-mix-modeling/overview) and the [technical specifications](marketing-mix-modeling).

#### Amazon DSP SSP seat IDs now available in the Amazon Ads API

We’ve launched the ability to create and assign SSP Seat IDs to individual Amazon DSP advertisers worldwide. With this release, Amazon DSP users can request seats to be created for any advertiser they manage. The SSP Seat IDs are instantly usable to ingest advertiser-specific deals (wherever Amazon DSP automatically ingests them), and are applied within 24 hours to existing line items for the purpose of spend tracking.

SSP Seat IDs by advertisers make it easier for Amazon DSP users to manage scenarios where deals created for specific advertisers must be kept separate from other advertisers (for example, to protect advertiser negotiated CPM rates). They in turn enable spend tracking IDs at the advertiser level, allowing SSPs more granular spend reporting and buyer validation.

For more information, see the technical specifications for the [seats API](dsp-seats) and the [seat IDs API](dsp-seat-ids).

#### Amazon Marketing Cloud launches Prime Video Channel Insights

The new Prime Video Insights (PVCI) dataset in AMC allows channel partners to join their own datasets with the Prime Video Channel viewership data associated with their channels. This new dataset can be used for measurement and activation use cases. To get started, see the [dataset documentation](guides/amazon-marketing-cloud/datasources/prime_video_channel_insights_paid) and the IQ [First-streamed titles for Prime Video Channel subscription starts (AMC UI access required)](https://advertising.amazon.com/marketing-cloud/use-cases/72fca9f61c45afe35c4bf7b548bca14a041790df12fe35ec95ffaccb7c0e640e).

### April 2025

#### Exclusively reach business shoppers with Amazon Business exclusive campaigns for Sponsored Products

Advertisers using the Amazon Ads API can now create Sponsored Products campaigns that exclusively reach business shoppers on Amazon Business, available in Amazon Business Marketplaces - US, CA, MX, DE, IT, FR, ES, UK, IN, JP. To create Amazon Business exclusive campaigns, advertisers will need to set the **siteRestrictions** field in the campaign setting to **AMAZON_BUSINESS**. Advertisers can customize their business-to-business (B2B) advertising strategy by setting a B2B budget, promoting ASINs they want to showcase to B2B shoppers, using B2B keywords, and adjusting bids as well as bidding strategies to increase sales on Amazon Business. All campaign settings are uniquely applied to Amazon Business and these campaigns will only be shown on the Amazon Business store. All existing Sponsored Products reports will reflect performance only on Amazon Business. Advertisers can leverage these reports to review campaign performance on Amazon Business. See our [reference documentation](sponsored-products/3-0/openapi/prod#tag/Campaigns) for more information.

#### Amazon Marketing Stream expands ADSP Performance dataset with new conversion and video metrics

Amazon Marketing Stream has expanded the **ADSP Performance dataset** to provide advertisers and developers with richer, and more actionable insights:

- **ADSP Conversions dataset** now includes Brand Halo metrics, enabling measurement of downstream purchases attributed to brand exposure, even if the purchase occurred in a different product line.
- **ADSP Clickstream dataset** has been enhanced to include Off-Amazon conversions and Video metrics, offering a more comprehensive view of customer engagement beyond Amazon owned and operated properties.

These updates help advertisers better understand the full impact of their campaigns and optimize spend based on more complete performance signals.
To learn more, visit the [ADSP Performance dataset](guides/amazon-marketing-stream/datasets/adsp-performance) section of the Amazon Marketing Stream Developer Guide.

#### Amazon Ads Well-Architected Framework Now Available

Amazon Ads has introduced the Well-Architected Framework, enabling developers to build secure, high-performing, and scalable advertising solutions. This framework addresses common integration challenges and accelerates the development of reliable advertising solutions.

The framework includes three key components:

- **Design Principles**: Core architectural guidelines for robust solutions
- **Best Practices**: Proven API integration patterns for optimal performance
- **Implementation Guidance**: Step-by-step technical solutions for seamless integration

**Getting Started**: To get started with the Well-Architected Framework, visit our [technical documentation](guides/amazon-ads-well-architected-framework/introduction).

> **Note**: The framework is available for all Amazon Ads API integrations and is recommended for both new and existing implementations.


#### Optimize headlines with dynamic creative optimization on Sponsored Brands

Sponsored Brands has introduced new headline capabilities allowing advertisers to use dynamic creative optimization (DCO) to improve ad personalization. Advertisers can consent to leveraging DCO during a campaign to enhance or generate new headlines to help better match customer intent. Based on internal testing, headline text optimization can drive up to 1.6% CTR lift vs. non-optimized campaigns.

Advertisers can access headline optimization via the Amazon Ads API by specifying the optional property `creativePropertiesToOptimize` with a value of `[ "HEADLINE" ]` on Product Collection or Store Spotlight creatives in Sponsored Brands.

The feature is enabled for four operations in the Sponsored Brands API. For more information, see the technical specifications for the following:

- [POST /sb/v4/ads/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsExtendedProductCollectionAds) 
- [POST /sb/v4/ads/storeSpotlight](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandStoreSpotlightAds)
- [POST /sb/ads/creatives/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateExtendedProductCollectionCreative)
- [POST /sb/ads/creatives/storeSpotlight](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateStoreSpotlightCreative)

#### Introducing Sponsored Brands V3 (legacy) migration API to migrate campaigns to V4

Sponsored Brands has launched a migration API to help advertisers transition from V3 (legacy) campaigns to V4 (ad group-based)campaigns, which address the complexity of campaign management. The API provides strategic migration pathways for campaigns with no impressions over 180 days, mandating migration or archival by June 30, 2025, otherwise Amazon will archive these campaigns by July 31, 2025. The impression-generating campaigns are required to migrate by September 30, 2025, or face automatic migration by Amazon by December 31, 2025. 

The V4 campaigns enable advertisers to leverage advanced machine learning, the theme targeting feature automatically generates and categorizes keywords based on historical performance and brand interactions, offering two pre-configured targeting groups (keywords related to landing pages and brand) that simplify campaign optimization and improve campaign performance.

The migration API is available in US, CA, MX, BR, JP, SG, AU, UK, DE, FR, IT, ES, NL, AE, IN, SA, SE, PL, TR, EG, SG, and ZA. For more information, see the [updated Sponsored Brands API specification](sponsored-brands/3-0/openapi/prod#tag/V3-Campaign-Migration) or refer to our [developer guide for the migration API](reference/migration-guides/sb-v3-v4-api).

#### Maximize impact with Amazon DSP bid adjustments

Amazon DSP Bid adjustments support both simple adjustments and complex bidding algorithms through single and multi-dimensional specifications. This means advertisers can now maintain nuanced and tailored bidding within a single line item, while still taking full advantage of Amazon DSP’s automated bid optimization.

With this launch, we’ve also released bid adjustments reporting, now available through the reporting API. Availability in the ADSP Report Center will follow shortly. In the console, you can also bulk apply bid adjustments to up to 10 line items from the campaign table on Advertiser and Order page. 

You can also access bid adjustments through our API. For full technical details, see our [bid adjustments API], detailed [developer guide](guides/dsp/bid-adjustments/overview), and new [`dspBidAdjustment` report type](guides/reporting/v3/report-types/bid-adjustment) available through the reporting API.

#### Register once and manage ads worldwide with a global seat

With global seats, Amazon DSP users can now register once and buy ads in every country where Amazon DSP operates. Users can create country specific advertiser accounts, receive invoices from a single Amazon legal entity for their global spend, and can create consolidated reports for their spend regionally.

Global Manager Accounts will be available in three regions: NA, EU, and FE. The Calling Profiles API returns only the Profile ID of the manager account for the regional endpoint you are calling. Choose the region you would like to make changes in and select the corresponding API endpoint:

* NA → https://advertising-api.amazon.com
* EU → https://advertising-api-eu.amazon.com
* FE → https://advertising-api-fe.amazon.com

For more information, see [Account management>Profiles](guides/account-management/authorization/profiles).

#### Sponsored Display updates all existing campaigns to optimize for Conversion opportunities

Sponsored Display is enhancing the machine learning algorithm to find improved conversion opportunities and help drive incremental sales for Reach and Page Visit campaigns, moving these campaigns to align with the optimization objective of Sponsored Display Conversion campaigns. With this change, advertisers who have active Reach and Page Visit campaigns are likely to observe increases in performance metrics associated with these campaigns. For more information, see [Sponsored Display: Bid optimization](guides/sponsored-display/overview#bid-optimization).

#### Amazon Marketing Cloud launches the Amazon Retail Purchases dataset 

The new Amazon Retail Purchases (ARP) dataset in AMC gives AMC customers 5 years (60 months) of historical Amazon store purchase signals across the advertiser's catalog of products,  regardless of media investment against those products. This new dataset can only be used for measurement use cases. To get started, see the [technical documentation](guides/amazon-marketing-cloud/datasources/amazon_retail_purchase_paid) and the [Instructional Query](https://advertising.amazon.com/marketing-cloud/instructional-queries/fa970ad5ac5aeb3ab84b5e4cc3a1393647d1d01da7bd59defcfe4978d87080e6). 


### March 2025

#### Fire TV Inline Display Banner now available for Amazon DSP self-service advertisers

Amazon DSP self-service advertisers in US can now book Fire TV Inline Display Banner campaigns across eligible unguaranteed placements on Home, Live, and Games tabs. These banners expand to "Mini-Details" view when users scroll past, taking up half the screen. They support 10-30s video with sound and various click-through destinations helping increase reach, awareness, and consideration. For full technical details, please read [our API documentation](dsp-ad-group-and-campaign#tag/Ad-Group).

#### Deprecation announcement: Amazon Ad Server tables in Amazon Marketing Cloud

We will deprecate Amazon Ad Server tables in AMC on June 30, 2025. After these tables and fields are deprecated, any workflows using these tables/fields will fail. For more details, see the [deprecation notice of Amazon Ad Server tables in AMC](release-notes/deprecations#deprecation-of-amazon-ad-server-tables-in-amc).  

#### Sponsored TV simplifies CPM updates for advertisers that don't sell in Amazon store

Sponsored TV has simplified cost per thousand impressions (CPM) updates for advertisers that do not sell in the Amazon store. Advertisers will now choose a maximum average CPM at the ad group level rather than for each targeting selection. For technical details, see the updated [Sponsored TV API specification](sponsored-tv-open-beta#tag/TargetingClauses).

#### Introducing Amazon-built “New-to-brand shoppers” audience for bid boosting through Sponsored Brands worldwide 

Sponsored Brands is introducing a new Amazon-built audience segment - "New-to-brand shoppers" - that advertisers can leverage for bid boosting to acquire new customers. This audience includes shoppers who have not purchased from the brand in the last 12 months. 

Advertisers can utilize the NTB audience bid boosting on both new and existing Sponsored Brands campaigns. To assess the performance of their bid-boosted NTB audience, advertisers can access the [Audience report](guides/reporting/v3/report-types/audience#sponsored-brands) through the reporting API. 

To retrieve Amazon-built audiences, you will use the Discovery API [ListTargetableEntities](targetable-entities#operation/ListTargetableEntities) with the following parameters: adProduct=SPONSORED\_BRANDS, targetTypeFilter=["AUDIENCE"], and pathsFilter = [["Audience Category", "Custom-built", "Product"]]. The response will have the "audienceId" for new-to-brand shoppers, which can then be used with [Sponsored Brands campaign management API](sponsored-brands/3-0/openapi/prod#tag/Campaigns) to create and adjust the bids for the audience. Note that Amazon-built audiences retrieved using these filters are only usable with "AudienceSegmentType" of "BEHAVIOR_DYNAMIC" when using the campaign management API. For more information, see our guide to [shopper cohort bidding](guides/sponsored-brands/bidding/shopper-cohort).

#### Amazon Marketing Cloud launches the Keyword clustering playbook

By blending SEO best practices with rich customer insights, this playbook helps advertisers identify high-impact keywords, optimize budget allocation, simplify management, and drive brand growth. To get started, see [Keyword clustering playbook](guides/amazon-marketing-cloud/playbooks/keyword-clustering). 

#### A tutorial outlining how Advertiser Data Upload (ADU) APIs enable you to create first-party audience segments in Amazon Marketing Cloud is now available 

By uploading hashed customer information through the ADU APIs, you can create rule-based audiences and activate them directly in Amazon DSP for highly targeted advertising campaigns. For more details, see [Create advertiser audiences with Amazon ADU APIs](guides/amazon-marketing-cloud/audiences/advertiser-audience-using-adu).

Also, see our updated [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman), to test **AMC administration, reporting (workflow management), and AMC audiences endpoints**.

#### Preview: Introducing Amazon-built audiences for bid boosting in Sponsored Products 

This capability is in preview only. We will update our release notes once this functionality is made available. Sponsored Products is introducing three new Amazon-built audience segments - "Purchased brand's products", "Clicked or added brand's product to cart," and "Affinity for similar products" - that advertisers can leverage for bid boosting to increase their engagement with these highly relevant cohorts of shoppers.

Advertisers can utilize the audience bid boosting on both new and existing Sponsored Products campaigns. To assess the performance of their bid boosted audience, advertisers can access the [Audience Report](guides/reporting/v3/report-types/audience#sponsored-products) through the reporting API.

To retrieve Amazon-built audiences, you will use the Discovery API [ListTargetableEntities](targetable-entities#operation/ListTargetableEntities) with the following parameters: adProduct="SPONSORED\_PRODUCTS", targetTypeFilter=["AUDIENCE"], and pathsFilter=[["Audience Category", "Custom-built", "Product"]]. The response will include an "audienceId" and "audienceResolved" (the name) for each of the three new audiences, which can then be used with the [Sponsored Products campaign management API](sponsored-products/3-0/openapi/prod#tag/Campaigns) to create and adjust the bids for the audience. Note that Amazon-built audiences retrieved using these filters are only usable with "audienceSegmentType" of "BEHAVIOR_DYNAMIC" when using the campaign management API. Also note that "audienceSegmentType" has been made optional in campaign creation, and the API will add the correct audience segment type based on the audience ID.

#### Preview: Introducing Amazon-built "New-To-Brand Shoppers" audience for bid boosting through Sponsored Brands 

>[NOTE]This capability is no longer in preview.

This capability is in preview only. We will update our release notes and API documentation once this functionality is made available. Sponsored Brands is introducing a new Amazon-built audience segment - "New-to-brand shoppers" - that advertisers can leverage for bid boosting to acquire new customers. This audience includes shoppers who have not purchased from the brand in the last 12 months. 

With this new feature, advertisers will have the ability to apply bid adjustments on the New-To-Brand Shoppers audience to increase reach in this valuable segment and drive orders from new customers. Advertisers can utilize the NTB audience bid boosting on both new and existing Sponsored Brands campaigns. To assess the performance of their bid boosted NTB audience, advertisers can access the [Audience Report](guides/reporting/v3/report-types/audience#sponsored-brands) through the SB API Reporting. 

To retrieve Amazon-built audiences, you will use the Discovery API [ListTargetableEntities](targetable-entities#operation/ListTargetableEntities) with the following parameters: adProduct="SPONSORED\_BRANDS", targetTypeFilter=["AUDIENCE"], and pathsFilter = [["Audience Category", "Custom-built", "Product"]]. The response will have the "audienceId" for new-to-brand-shoppers which can then be used with [Sponsored Brands Campaign Management API](sponsored-brands/3-0/openapi/prod#tag/Campaigns) to create and adjust the bids for the audience. Note that Amazon-built audiences retrieved using these filters are only usable with "AudienceSegmentType" of "BEHAVIOR_DYNAMIC" when using the campaign management API. For full technical details, see our updated [Sponsored Brands documentation](guides/sponsored-brands/overview).

### February 2025

#### New implementation guide for integrating Amazon Marketing Stream data with Snowflake

We have published a step-by-step guide and IaC templates to help you stream Amazon Marketing Stream data to Snowflake. You can implement the solution across AWS, Azure, or Google Cloud with our ready-to-use code templates. To learn more, see the [implementation guide](https://advertising.amazon.com/API/docs/en-us/guides/usage-examples/2025-01-ams-and-snowflake-integration).

#### Ads data manager now supports Amazon Marketing Cloud and conversion events 

Ads data manager expands support for advertiser first-party data uploads through Conversions API (C-API) and adds Amazon Marketing Cloud (AMC) as a destination for all supported datasets. The enhancement unifies data management across C-API and Advertiser Audience APIs, and includes partner-facilitated uploads, with all datasets accessible through the [Ads data manager console](adm/1_ads-data-manager-console-overview) and the [Ads data manager APIs](guides/ads-data-manager/adm-overview). 

Advertisers can leverage audience data via Amazon DSP, and C-API events will be available for conversion attribution through the Events Manager. All audience and events datasets managed through Ads data manager will be available for access in AMC when creating a destination linked to your AMC instance within Ads data manager.

#### AMC announces automated approvals for AMC instance requests

AMC is excited to announce the launch of automated approvals for instance creation and advertiser inclusion requests in their instances. This launch will allow any eligible advertiser or partner to self-provision AMC through the instance management APIs, reducing onboarding time from a 2-day SLA to instant approval.

Read [automated instance approval and advertiser inclusion](guides/amazon-marketing-cloud/admin/instance-management#single-advertiser-addition-approval) process to learn more.

#### Audience bid adjustment now available in Amazon Marketing Stream datasets and exports APIs

Following [the launch of audience bid adjustments](#help-increase-engagement-with-advertiser-defined-audiences-through-sponsored-ads-bidding-controls) for Sponsored Products and Sponsored Brands, integrators can now view audience bid adjustment details for campaigns in both exports APIs and Amazon Marketing Stream.

For exports, the new fields are available using [POST/campaigns/export](exports#tag/Exports/operation/CampaignExport) as part of the `optimizations.shopperSegmentBidAdjustment` object. See [Common models](reference/common-models/campaigns) for more information.

For Stream, the new fields are available in the [campaigns](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management) dataset as part of the `bidSetting.shopperCohortBidAdjustment` object.

### January 2025

#### Amazon Marketing Cloud - instance-level API migration to Amazon Ads is now complete

The [migration from the AMC instance-level APIs](#deprecation-announcement-reminder-that-amc-instance-level-apis-will-shut-off-on-august-1-2024) to the new AMC APIs on the Amazon Ads API is now complete and the resources to help you migrate have been consolidated and moved to the [Deprecated resources](guides/amazon-marketing-cloud/amc-migration-hub/migration) section of the Advanced tools center portal.

#### Amazon Ad Server datasources documentation deprecated

Amazon Ad Server was sunset in Q4 2024, and all resources describing the relevant datasources have been moved to the [Deprecated resources](guides/amazon-marketing-cloud/datasources/ad_server_conversions) section of the Advanced tools center portal.

#### Audience bid boosting reports now supported for Sponsored Products and Sponsored Brands

Advertisers who use [audience bid boosting](release-notes/index#help-increase-engagement-with-advertiser-defined-audiences-through-sponsored-ads-bidding-controls) can now access audience bid boosting reports via the Amazon Ads API. Using bid boosting, advertisers can create custom audiences, such as shoppers who have not previously purchased their product and shoppers who were exposed to their streaming TV campaigns. Advertisers can adjust their bids for these audiences to help drive greater product discovery and sales. For more information on audience bid boosting reports, see the [Audience report type documentation](guides/reporting/v3/report-types/audience).

#### Deprecation of Amazon DSP legacy inventoryTypes

The Amazon DSP campaign management APIs are deprecating several enum values for the `inventoryType` field on [POST/dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPostAdGroupV1). After June 30, 2025, users will no longer be able to create ad groups where `inventoryType` equals STANDARD\_DISPLAY, AAP\_MOBILE\_APP, AMAZON\_MOBILE\_DISPLAY, or VIDEO. Instead users should use the newer options below:

|Endpoint	|Legacy inventoryType	|New inventoryType	|Legacy inventoryType deprecation date	|
|---	|---	|---	|---	|
|[POST/dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPostAdGroupV1)	|STANDARD\_DISPLAY	|DISPLAY	|6/30/2025	|
|[POST/dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPostAdGroupV1)	|AMAZON\_MOBILE\_DISPLAY	|DISPLAY	|6/30/2025	|
|[POST/dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPostAdGroupV1)	|AAP\_MOBILE\_APP	|DISPLAY	|6/30/2025	|
|[POST/dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPostAdGroupV1)	|VIDEO	|ONLINE\_VIDEO or STREAMING\_TV	|6/30/2025	|

Please contact our [support team](support/overview) with any questions or feedback.

#### Understand deduplicated reach and frequency across multiple orders in frequency groups

The launch of frequency group-level reach allows advertisers the ability to set a single frequency cap to control ad exposure across multiple orders in a campaign. Complementing this feature, new reporting provides combined deduplicated reach and frequency metrics at the frequency group-level.

Cross-order reach and frequency reporting provides advertisers with a unified, deduplicated view of frequency metrics across all orders within a frequency group. This consolidated reporting eliminates double-counting of audiences exposed to ads from different orders, enabling advertisers to analyze the impact of the frequency group.

You can now access the metric through the reach and frequency report through V3 reporting. For full technical details, please see our [v3 reporting documentation](guides/reporting/v3/report-types/reach).

#### Measure ADSP reach and frequency at the advertiser level

Amazon DSP advertisers now gain a unified view of reach and frequency, deduplicating audiences exposed to ads across different publishers, channels, and devices.

Advertiser-level reach and frequency metrics provide unified insights that help advertisers understand their total unique reach across all of their Amazon Ads investments. These metrics eliminate overlapping audience counts across multiple devices and ad formats, revealing the true size of the delivered audience.

You can now access the metric through the reach and frequency report through V3 reporting. For full technical details, please see our [v3 reporting documentation](guides/reporting/v3/report-types/reach).

#### Updated Deprecation Date: Amazon DSP legacy campaign management APIs will shut off on June 30, 2025

With the launch of our new [Amazon DSP campaign management APIs](reference/dsp/dsp-campaign-management-overview), we are deprecating the [previous version](dsp-campaigns) of the campaign management APIs. Previously we announced a deprecation date of March 31, 2025. We are extending this deprecation date to June 30, 2025. The full list of APIs being deprecated can be found on our [deprecations page](release-notes/deprecations#amazon-ads-api-deprecations).

#### Amazon DSP campaign and creative APIs are now generally available

In October, we [announced the launch](release-notes/index#amazon-dsps-new-campaign-management-endpoints-are-now-available-in-open-beta) of Amazon DSP campaign and creative management APIs in beta. These endpoints are now generally available worldwide for Amazon DSP users and their technology partners.

The campaign and creative management resources enable users to create, read and update their Amazon DSP campaigns and creatives via a programmatic interface. Technology providers and advertisers can use these resources to develop new trader experiences within their own applications and manage Amazon DSP campaigns within their existing workflows. To enable end-to-end campaign management, these new resources can be used together with the [audience](dsp-audiences) and [deal](dsp-create-update-deals) endpoints.

With this release, we’re adding a targetId field to [/dsp/v1/targets](dsp-universal-targeting#tag/Targets):

* When creating a new target via [POST/dsp/v1/targets](dsp-universal-targeting#tag/Targets/operation/CreateDspTargetsV1), every successfully created target will have a `targetId`.
* When listing targets via [POST/dsp/v1/targets/list](dsp-universal-targeting#tag/Targets/operation/ListDspTargetsV1), all listed targets will contain a `targetId`.
* A new endpoint, [POST/dsp/v1/targets/delete](dsp-universal-targeting#tag/Targets/operation/DeleteTargetsByIdV1), enables users to remove targets from ad groups using `targetId`. This endpoint can be used instead of [POST/dsp/v1/targets/deleteByObject](dsp-universal-targeting#tag/Targets/operation/DeleteDspTargetsV1) for a more efficient delete operation.

For more information, refer to these links:

|Campaign Management 	|Creative Management	|Creative Associations	|
|---	|---	|---	|
|[Developer guide](guides/dsp/developer-guide)	|[Developer guide](guides/dsp/creative-management)	|[Developer guide](guides/dsp/creative-associations)	|
|[Migration guide](reference/migration-guides/adsp-campaign-management)  	|[Migration guide](reference/migration-guides/dsp-creatives)	|[Migration guide](reference/migration-guides/dsp-creatives)	|
|[Reference documentation](reference/dsp/dsp-campaign-management-overview) 	|[Reference documentation](dsp-ad-creative)	|[Reference documentation](dsp-ad-creative-association)	|

#### v3 reporting now supports Amazon DSP managed service advertisers

The [version 3 reporting API](offline-report-prod-3p) has a new workflow that enables pulling reports at the individual advertiser level. This new workflow supports these use cases:

* Advertisers who utilize Amazon managed service and wish to pull performance reporting for their managed service campaigns.
* Agencies who want to give reporting access for a subset of advertisers they manage instead of all their advertisers.

For more information on this workflow, please review our [guide to pulling Amazon DSP reports](guides/reporting/dsp/creating-reports).

### December 2024

#### New API that enables the retrieval of advertising billing promotion data and its corresponding consumption metrics across invoices

We have introduced a new API to facilitate the retrieval of advertising billing promotion data and its corresponding consumption metrics across invoices. This new functionality enhances our existing billing API, enabling customers to programmatically retrieve their promotions data and gain visibility on which invoices have consumed it.

This enhancement is crucial for customers to enhance visibility and transparency into promotions. The new API eliminates the need for customers to manually review each individual invoice to determine if a specific promotion has been consumed. Instead, customers can now access this information programmatically, empowering them to make more informed decisions regarding their advertising expenditures and performance.

For more information, see our [API documentation](billing#tag/Billing-Promotions/operation/ListBillingPromotions).

#### Amazon Marketing Cloud launches a playbook to derive enhanced scoring for overlapping AMC audiences

The playbook **Enhanced scoring for overlapping AMC audiences** will guide you through the process of integrating the powerful capabilities of Amazon Marketing Cloud (AMC) with the [persona builder API](persona-builder#persona-builder-api) to deliver comprehensive audience insights. It guides you through the process of identifying AMC audiences and leveraging the persona builder API to identify additional audience attributes across top retail categories, top overlapping audiences, estimated size, top demographics, and Prime Video insights. All the results are accompanied with an affinity score that refers to the likelihood and a percentage score that refers to the percentage of the customers on the above categories. The derived insights will help you build a holistic approach of audience performance.

To get started, see [Enhanced scoring for overlapping AMC audiences](guides/amazon-marketing-cloud/playbooks/enhanced_scoring_for_overlapping_audiences).

#### AMC on AWS Clean Rooms can now create multi-party collaboration instances

AMC on AWS Clean Rooms self-serve instance creation APIs now allow you to create a **multi-party collaboration instance**. With this update, advertisers or partners with data stored across multiple AWS accounts can include up to two additional AWS account IDs in the collaboration instance. This enables AMC to query data or create audiences from these associated AWS accounts.

For more details, see [Create an AWS Clean Rooms backed AMC instance](guides/amazon-marketing-cloud/acr/4_setup_collab#create-an-aws-clean-rooms-backed-amc-instance).

#### Deprecation Announcement: Amazon DSP legacy report endpoints will shut off on June 30, 2025

Following the release of Amazon DSP reports to the [v3 offline report API](offline-report-prod-3p), we are announcing the deprecation of the legacy Amazon DSP report endpoints with a shutoff date of June 30, 2025. You can find a full list of legacy and new endpoints on the [Deprecations page](release-notes/deprecations).

For general guidance, refer to the [v3 reporting overview.](guides/reporting/v3/overview) For more details on how to upgrade, see our [migration guide](reference/migration-guides/dsp-reporting).

For questions, use the [Amazon Ads API Support page](support/overview) to submit a ticket with the subject title "ADSP Report API Migration".

#### Media performance optimization playbook now available for Amazon Marketing Cloud users

This playbook guides you through optimizing ad spend distribution by maximizing a specified KPI. It uses historical performance data to create diminishing returns curves, which project expected outcomes at various budget levels allowing you to visualize performance expectations and identify the point where additional investment yields diminishing returns. To get started, see [Media performance optimization playbook](guides/amazon-marketing-cloud/playbooks/media_performance_optimization).

#### Self-serve instance creation APIs now available for AMC on AWS Clean Rooms users

AMC on AWS Clean Rooms now offers self-serve instance creation and advertiser modification APIs. This new feature enables all AMC account holders, including advertising partners and direct advertisers, to create collaboration instances independently, without requiring assistance from an Amazon representative. For more details, see [Create an AWS Clean Rooms backed AMC instance](guides/amazon-marketing-cloud/acr/4_setup_collab#create-an-aws-clean-rooms-backed-amc-instance).

#### AMC Advertiser data upload - S3 bucket object tagging now allows tagging multiple instances

When defining your bucket policy for Advertiser data uploads, you can now associate multiple instance IDs with a single tag. This allows all the specified instances to access the designated S3 bucket or object. For more details, see [Define tags](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket#define-tags).

#### Unlock Exclusive Reach Insights with STV Inventory Reporting

With the launch of Amazon Exclusive Reach Rate for STV Inventory reporting, advertisers can now isolate and analyze the unique, unduplicated reach delivered by individual Amazon 1P streaming TV inventory sources like Twitch and Prime Video within their total STV investment. This granular reporting provides insights into the exclusive reach generated by Amazon's 1P video channels.

Advertisers are seeking deeper insights into the performance of Amazon's 1P STV inventory. This new Amazon Exclusive Reach Rate allows advertisers to validate how effective their Amazon STV investment is at extending total campaign reach to net-new users. Additionally, they can measure the total reach delivered by each individual STV inventory, and adjust their advertising strategies and engage this exclusive audience beyond linear tv.

For full technical details, please read our [full API documentation](guides/reporting/v3/report-types/reach).

#### Amazon Advertising Payment Public API Introduces Cross-Border and Multi Seller Payable Payment Method Registration

We have introduced a new feature that allows advertisers to select and set any of their owned seller accounts as the seller payable payment method globally. They can now also choose one or more seller payable payment methods during registration. This functionality is available for both registration and payment method updates.

This launch enables public API clients to retrieve eligible seller accounts and seamlessly register or update advertiser payment methods, allowing the advertiser to set one or multiple preferred seller accounts.

[POST /billing/paymentMethods/list](billing#tag/Get-Payment-Methods/operation/GetCustomerPaymentMethods)

* This API now provides a list of seller payable payment methods contains seller account and country code that an advertiser is eligible to register.

[POST /billing/paymentAgreements/](billing#tag/Create-Payment-Agreement/operation/CreatePaymentAgreements)

* This API now accepts seller account and country code that can be used to register or update the advertiser payment method.

[POST /billing/paymentAgreements/list](billing#tag/Get-Payment-Agreements/operation/GetPaymentAgreements)

* This API now returns seller account and country code that registered by the advertiser.

[POST /billing/paymentProfiles/](billing#tag/Create-Payment-Profiles/operation/CreatePaymentProfiles)

* This API now allows advertiser to select specific seller account and country code to set as their default payment method.

#### Deprecation Announcement: Amazon DSP legacy creative and association management endpoints will shut off on May 31, 2025.

Following the release of the new ADSP [Creatives](guides/dsp/creative-management) and [Association](guides/dsp/creative-associations) Management endpoints, we are announcing the deprecation of the [legacy endpoints](dsp-campaigns) with a shutoff date of May 31, 2025. You can find a full list of legacy and new endpoints on the [Deprecations page](release-notes/deprecations).

Going forward, you can access all creative, moderation and association data via the new ADSP [Creatives](guides/dsp/creative-management) and [Association](guides/dsp/creative-associations) endpoints. For more details on how to upgrade, see our [Migration Guide](reference/migration-guides/dsp-creatives).

For questions, use the [Amazon Ads API Support page](support/overview) to submit a ticket with the subject title "ADSP Creative API Migration".

#### Amazon DSP launches contextual keyword targeting

Regions: US, UK, CA, AU

Contextual keyword targeting enables advertisers to connect with audiences by targeting content based on relevant freeform keywords or phrases - without relying on identifiers. Powered by our latest AI models, Amazon DSP places ads on Amazon properties and third-party sites, ensuring ads appear on pages containing or closely related to targeted keywords.

This product is now available in the US, UK, CA, AU to Amazon DSP self-service and managed-service advertisers and is supported on standard display and Amazon mobile display line items.

Contextual keyword targeting overcomes the limitations of category or product retail-based approaches by offering advertisers the flexibility to target freeform keywords, enabling increased reach and performance. Up until now, the existing contextual targeting options available on Amazon DSP restricted advertisers to predefined Amazon retail Categories (Browse Nodes) or Products (ASINs), so advertisers could target ‘picnic baskets’ but not an event like ‘4th of July’ or ‘summer.’ The flexibility of contextual keyword targeting is a universal fit for all of our customers, enabling advertisers increased opportunities to achieve their goals on Amazon DSP.

Relevant API endpoints:

- Create target: [/dsp/v1/targets](dsp-universal-targeting#tag/Targets/operation/CreateDspTargetsV1)
- List targets: [/dsp/v1/targets/list](dsp-universal-targeting#tag/Targets/operation/ListDspTargetsV1)
- Delete target: [/dsp/v1/targets/deleteByObject](dsp-universal-targeting#tag/Targets/operation/DeleteDspTargetsV1)

#### History and preferred language now available for Amazon Ads Status

[Amazon Ads Status](https://status.ads.amazon.com/) provides advertisers with the latest status information on Amazon Ads services. With the new 'History' page, advertisers can now review communications and status updates shared by Amazon Ads spanning the past 6 months. In addition, advertisers can now select a language of preference for status information.

For information about programmatically accessing status information, visit the [Amazon Ads Status](support/status) page in the advanced tools center.

#### ADSP campaign management APIs now support inventory groups

The Amazon DSP campaign management APIs now support creating, reading, and updating inventory groups. Inventory groups are reusable groupings of deals into one singular group that can be added to ad groups.

New endpoints:

* POST /dsp/v1/inventory/inventoryGroups/list: List all inventory groups.
* POST /dsp/v1/inventory/inventoryGroups: Create an inventory group.
* PUT /dsp/v1/inventory/inventoryGroups: Edit an inventory group.
* POST /dsp/v1/inventory/eligibility/inventoryGroups/list: List inventory groups eligible to be added to an ad group.

To add an inventory group to an ad group (line item), use POST /dsp/v1/targets to create an inventorySourceTarget with inventorySourceType=INVENTORY_GROUP.

For more information, see the [API specification for inventory groups](dsp-inventory-groups).

### November 2024

#### Amazon Brand Store insights datasets now available in Amazon Marketing Cloud

Amazon Marketing Cloud (AMC) adds Amazon Brand Store insights datasets to its Paid features suite, allowing users visibility into Brand Store performance through two comprehensive AMC datasets that include page renders and interactions. Available for both trial and subscription enrollments in the following marketplaces: US, Canada, Japan, Australia, France, Italy, Spain, UK, and Germany.

To learn more, visit [Amazon Brand Store insights](guides/amazon-marketing-cloud/datasources/brand_store_insights_paid).

#### Manage creatives with Creative management APIs

Amazon DSP Creative team will launch the Creative Management APIs into open beta. This release includes the ability to create, update, read, preview creatives; create, delete and update creative ad group association; and get moderation details for Amazon DSP creatives.

This launch allows us to grow our integration offering to more customers using the latest version of the creative APIs. This set of APIs offer the functionality available on the Amazon DSP UI, including the support of add, edit, review setting and moderation status, associate to line items and bulk tools. This set of APIs were redesigned and newly developed to better support integration customer needs.

The [new creative management APIs](guides/dsp/creative-management) enable self-serve advertisers and their partners to programmatically manage their Amazon DSP AdCreatives, AdCreative-AdGroup associations and moderations. These new APIs replace the [previous creative management APIs](dsp-campaigns), with the following improvements:

* **Unified API structure:** The API model provides a single, unified structure (the adCreative) that can host multiple variations (the adExperiences). API clients will have access to the new offering automatically without additional API-onboarding overhead.
* **Worldwide availability:** The new APIs are available in all countries supported by Amazon DSP.
* **Improved Scale:** The list endpoint can support up to 100 objects read at a time, and the createCreativeAssociation, patchCreativeAssociation and deleteCreativeAssociation can support batch size of 20.
* **Enhanced Features**: The new API supports PATCH operations (partial updates) allowing individual field without sending all the fields. The new AdCreative association API can update association object state (ACTIVE, INACTIVE).

#### Assets Based Creative on Alexa

Alexa is the first device ad placement to integrate with the standard Asset Based Creative (ABC) creative template used by desktop and mobile advertisers. Amazon DSP self-service advertisers using ABC creatives, directing to to any landing page, now have the opportunity to run their display ads on Alexa Home Screen (AHS). This will help drive brand awareness and consideration objectives and prove an opportunity to reach incremental customers on Echo Show devices.

Buyers can use the standard Assets Based Creatives (ABC) and be served on the Alexa Home Screen (AHS). Alexa is an Amazon inventory type that will be enabled on Display Media Type Line items. This gives buyers a frictionless experience of serving an ABC ad to an additional Amazon supply.

#### Amazon DSP launches Contextual keyword targeting GA in US, UK, CA, AU

Contextual keyword targeting enables advertisers to connect with audiences by targeting content based on relevant freeform keywords or phrases - without relying on identifiers. Powered by our latest AI models, Amazon DSP places ads on Amazon properties and third-party sites, ensuring ads appear on pages containing or closely related to targeted keywords.

This product is now available in the US, UK, CA, AU to Amazon DSP self-service and managed-service advertisers and is supported on standard display and Amazon mobile display line items.

Contextual keyword targeting overcomes the limitations of category or product retail-based approaches by offering advertisers the flexibility to target freeform keywords, enabling increased reach and performance. Up until now, the existing contextual targeting options available on Amazon DSP restricted advertisers to predefined Amazon retail Categories (Browse Nodes) or Products (ASINs), so advertisers could target ‘picnic baskets’ but not an event like ‘4th of July’ or ‘summer.’ The flexibility of contextual keyword targeting is a universal fit for all of our customers, enabling advertisers increased opportunities to achieve their goals on Amazon DSP.

For more information, see our API documentation:

* Universal targeting:
  * Create target: [/dsp/v1/targets](https://advertising.amazon.com/API/docs/en-us/dsp-universal-targeting#tag/Targets/operation/CreateDspTargetsV1)
  * List targets: [/dsp/v1/targets/list](https://advertising.amazon.com/API/docs/en-us/dsp-universal-targeting#tag/Targets/operation/ListDspTargetsV1)
  * Delete target: [/dsp/v1/targets/deleteByObject](https://advertising.amazon.com/API/docs/en-us/dsp-universal-targeting#tag/Targets/operation/DeleteDspTargetsV1)

#### Optimize short-flight campaigns with Amazon DSP recommendations

We’ve expanded our Amazon DSP recommendations to support campaigns running seven days or less. This update enables our machine learning models to provide more timely and relevant optimization suggestions for campaigns during peak shopping events like Prime Day or Black Friday.

Advertisers can now benefit from Amazon DSP recommendations to help their orders pace during peak shopping events, helping them to quickly identify and address issues. Our recommendations are actionable with a single click, helping advertisers focus their time on what matters.

For more information, see our [API documentation](dsp-guidance).

#### Access DSP recommendations via new Guidance API

Amazon DSP has launched a new Guidance API and Quick Action API, enabling advertisers to programmatically access pre-flight and in-flight recommendations for their campaigns. The Guidance API provides the same intelligent suggestions for maximum average CPM, line-item frequency caps, viewability targeting and other optimization opportunities that are available through the DSP console. Advertisers can now retrieve recommendations for new campaigns pre-flight and in-flight optimizations for live campaigns, allowing for seamless integration with their own systems and workflows. The Quick Action API allows advertisers to seamlessly action Amazon DSP’s recommendations.

The Guidance API empowers advertisers to automate their campaign optimization processes, reducing manual effort and improving efficiency. By providing programmatic access to Amazon's machine learning-driven recommendations, advertisers can quickly identify and act on opportunities to enhance campaign performance and delivery. This API integration enables more frequent and consistent optimization, especially beneficial for advertisers managing large-scale campaigns or using custom tools for campaign management.

**DSP Guidance API**: Advertisers can use this API to view recommendations for their campaigns. The four new endpoints will aggregate the guidance at various object levels (advertiser, order or line items). Recommendations can be fetched for many objects at once.

Listed bellow are the new endpoints included in this API:

* `POST /dsp/v1/guidance/lineitems/list`
* `POST /dsp/v1/guidance/orders/list`
* `POST /dsp/v1/guidance/advertisers/list`

For full technical details, please read our [API documentation](dsp-guidance).

**DSP Quick Actions API**: This API allows advertisers to adopt the recommended changes returned by the Guidance API. Sending a request to `POST /dsp/v1/quickactions/{actionId}/executions` using the actionId included in the guidance response will trigger the relevant change to be adopted.

For full technical details, please read our [API documentation](dsp-quick-actions).

#### Amazon Your Garage datasets in Amazon Marketing Cloud

[Amazon Your Garage](https://www.amazon.com/gp/help/customer/display.html?nodeId=GTTH6M3A9A3W8QUC) is a virtual vehicle repository which allows customers to add their vehicle's make and model and quickly locate parts and accessories that fit their saved vehicles. [Amazon Your Garage](guides/amazon-marketing-cloud/datasources/amazon_your_garage_paid) signals are now available for Amazon Marketing Cloud (AMC) and allows advertisers to tap into those user-to-vehicle association signals. When you enroll in Amazon Your Garage, you will have access to the `amazon_your_garage` dataset containing signals such as vehicle type, make, or engine type, which are updated daily with the most recent user-to-vehicle associations.

AMC Amazon Your Garage is a paid feature and available for enrollment within US and CA AMC account marketplaces. The AMC Amazon Your Garage dataset contains signals from US, CA, and MX.

To learn more, visit the [Amazon Your Garage Instructional Query](https://advertising.amazon.com/marketing-cloud/instructional-queries/03fca7c6bb3d80c2dbdcbd21f6e6714e856f4dda85a90d1662a2e1dd31b2668d) (link only available for AMC UI users).

#### Introducing the Lookalike audiences for promotional events playbook for AMC

This comprehensive playbook will help advertisers perform an in-depth analysis of previous tentpole events and guide creating targeted lookalike audiences. It addresses key questions about ASINs and campaign selection, audience targeting timing, and effective strategies for keywords, frequency, and ad formats. To learn more, visit [Lookalike audiences for promotional events playbook](guides/amazon-marketing-cloud/playbooks/promotional_events).

#### New APIs launched that allow billing documents download including invoices, credit memos, and more

We’ve introduced a new API to streamline the retrieval of individual billing documents, such as invoices, credit memos, debit memos, and prepayment receipts. This new functionality enhances our existing /billing API, allowing customers to programmatically download these documents in various media formats, improving the accessibility and management of billing records.

This enhancement is essential for customers managing complex billing operations. Previously, invoices and related documents could only be accessed in a JSON format, limiting flexibility for users requiring these documents in a downloadable format. Now, with this update, customers can download essential billing documents in PDF and other formats, making it easier to manage and store important financial records.

For more information, see our [API documentation](billing#tag/Billing-Documents).

#### Understand the scale Amazon inventory can provide by demographics and audiences using our Historic Reach Curve Advanced API

Amazon launched support for historic all-up ad supported reach curves in the US, UK, DE and CA. For the first time, Amazon has released the ability to understand Amazon reach by first party, third party, and advertiser audiences. Using the Historic Reach Curve API - Advanced, you can programmatically access ad-supported reach metrics across Amazon’s portfolio of ad products.

Integrators now have visibility into the historic household and individual reach Amazon achieved. This data can be used as-is or as fuel to power your internal planning tools.

For full technical details, please read [our API documentation](media-insights-hub).

### October 2024

#### Deprecation announcement: Version 1 and 2 snapshots APIs will shut off on December 15, 2024

All snapshots endpoints are now deprecated, with a target shutoff date of December 15, 2024. Please migrate to the [exports API](exports) as indicated on our [Deprecations page](release-notes/deprecations).

The following endpoints and all associated HTTP operations are included in this deprecation:

- /{recordType}/snapshot
- /snapshots/{snapshotId}
- /v2/sp/{recordType}/snapshot
- /v2/sp/snapshots/{snapshotId}
- /v2/sp/snapshots/{snapshotId}/download
- /v2/hsa/{recordType}/snapshot
- /v2/hsa/snapshots/{snapshotId}
- /v2/hsa/snapshots/{snapshotId}/download
- /sd/{recordType}/snapshot
- /sd/snapshots/{snapshotId}
- /sd/snapshots/{snapshotId}/download

For more details on integrating with exports, see our [Migration guide](reference/migration-guides/snapshots-exports).

#### Global campaigns for Sponsored Products

With global campaigns for Sponsored Products, advertisers can launch and manage a single Sponsored Products campaign targeting multiple countries. Advertisers can in a few clicks control global and country-specific settings in their language and currency of preference when creating campaigns. These campaigns can run in all countries where products are sold unless marketers choose to target the campaign to a single country or a set of countries. Additionally, marketers can now edit campaigns in bulk by adding or removing advertised products or applying suggested bids across countries. They can also see overall advertising performance across their advertiser account, targeting, and products, either globally or filtered by country.

With global campaigns, this impacts Sponsored Product sellers, vendors, and authors who manage campaigns in more than one country and help save time and effort. This new feature also affects advertisers who want to launch a new product globally and would like to expand winning products to new regions, as well as marketers who would like to launch campaigns where global control is preferred.

For full technical details on how this feature works with the Amazon Ads API, refer to [our API documentation](global-cm).

#### Amazon Business Bid Adjustment available in the Sponsored Products v3 API

The Amazon Business Bid Adjustment and Reporting for Sponsored Products is available to all advertisers using the SPv3 API across US, CA, MX, DE, UK, FR, IT, ES, IN, and JP. The Amazon Business Bid Adjustment is a singular adjustment that allows advertisers to increase their bids by up to 900% across all placements on Amazon Business. This Amazon Business Bid Adjustment can be applied to existing or new Sponsored Products campaigns, and can be set independently or in addition to placement bid adjustments for Top-of-Search (first page), Rest-of-Search (remaining search results), and Product pages. All Sponsored Products advertisers in Amazon Business marketplaces, irrespective of their use of the Amazon Business Bid Adjustment, will now also have access to reporting that shows Sponsored Products campaign performance on Amazon Business from 9/5/2024 onwards. The Amazon Business performance data can be used by advertisers to guide their use of the Amazon Business Bid Adjustment.

By increasing bids for Amazon Business placements, advertisers are more likely to have their ads surface on the Amazon Business store and correspondingly generate more impressions, clicks, and sales (better reach and engagement) from business customers. Relative to B2C customers, business customers are ~3x more likely to purchase an item after viewing its product page, have a ~50% lower return rate, and on average make orders containing ~80% more units. This results in advertisers, on average, seeing a 2-3x higher return on ad spend on Amazon Business relative to their overall campaign performance (based on Amazon internal data, 2024). The Amazon Business performance data can be used by all Sponsored Products advertisers in Amazon Business marketplaces, irrespective of their use of the Amazon Business Bid Adjustment, to understand the effectiveness of their Sponsored Products campaigns on Amazon Business.

SP v3 API: Advertisers can use the new enum value SITE\_AMAZON\_BUSINESS by providing it when configuring the dynamic bidding specifications of a given campaign. All existing SP endpoints which accept or return this object now accept this new enum with a value from 0 to 900.

Reporting v3 API: Advertisers can use the new filter named campaignSite with the value AmazonBusiness as modifier to the campaignPlacement groupBy parameter in the spCampaigns report. Other groupBy parameters apart from campaignPlacement are not supported in the request. The generated report will show AB-only performance for all of their campaigns irrespective of the use of the AB Bid Adjustment.

For more information, see the [technical specifications for the Sponsored Products API](sponsored-products/3-0/openapi/prod#tag/Campaigns) and the [documentation for the reporting API](guides/reporting/v3/report-types/campaign).

SP API endpoint examples:

- `POST /sp/campaigns` for creating a new campaign
- `PUT /sp/campaigns` when updating an existing campaign
- `POST /sp/campaigns/list` when querying upon existing campaigns which contain the enum value.

Reporting API endpoint examples:

- `GET /reporting/reports/{reportId}` Gets a generation status of a report by id
- `DELETE /reporting/reports/{reportId}` Deletes a report by id
- `POST /reporting/reports` Creates a report request

> [NOTE]<ol><li>Amazon Business performance data is available starting 9/5/2024 onwards only.</li><li>The Amazon Business Bid Adjustment and Reporting for Sponsored Products will be coming soon to Bulksheets.</li></ol>

#### Awareness: Changes to third-party data provider consent policies across Amazon Ads

Amazon Ads has enhanced its existing mechanisms for publishers, advertisers and vendors (referred to as third parties) to share their customers’ consented data with Amazon Ads by introducing Amazon Consent Signal. While we will continue to accept the Interactive Advertising Bureau’s (IAB) Transparency & Consent Framework (TCF) signal, Amazon Consent Signal provides a way for third parties that don't use TCF to transmit their users’ consent choices along with a country code to Amazon Ads. For Sending Personal Information and Amazon Ads Consent Signal requirements, please click [here](https://advertising.amazon.com/resources/ad-policy/consent-signal-requirements).

From 1st November 2024, third parties can to share the country code and consent with Amazon Ads. We will continue to honor existing mechanisms for consent and will publish an update by which all data that partners make available to Amazon Ads, must include the IAB European Transparency & Consent Framework (TCF) signal(s) or the Amazon Consent Signal.

##### _Country Code_

From 1st November 2024, you can share a country code when passing customers’ consented data to Amazon Ads. The country code is a 2-character string in the ISO 3166 format that indicates in which country the consent was given (for example, US or GB). If you upload data without specifying a country code, we will automatically reject the data load. The Country Code must indicate the country in which consent was granted by the customer.

##### _Consent String_

From 1st November 2024, you can share a consent string when passing EEA or UK customers’ consented data to Amazon Ads. If you already use TCF, this is sufficient and you don't need to make any changes. If you do not use TCF, you can use the Amazon Consent Signal, see below, to pass your customers’ consent. To be clear, please do not pass both TCF and Amazon Consent Signal. If you do pass both, we will take TCF as the consent signal and ignore Amazon Consent Signal. If you use a Consent Management Platforms (CMP) to manage your consent please ensure your CMP provides you with the necessary data to send a TCF consent string or the Amazon Consent Signal details. If you pass your data to Amazon Ads directly (i.e. not through a CMP) then we have several resources available to help you do this.

##### Amazon DSP's DP API

Country code is a required field today as part the audience metadata sent via [DP API](data-provider/openapi#tag/Metadata/paths/~1v2~1dp~1audiencemetadata~1/post). Additionally, from 1st November 2024, third parties can begin to share consent string with Amazon Ads. Consent string will be a new field (see attachment). Instructions to add consent string are published [here](data-provider/openapi#tag/Add-or-remove-records/paths/~1v2~1dp~1audience/patch).

If you are not sending audiences for countries that are part of EEA and UK then please keep sending country codes; no further action is needed. If you are sending audiences for countries that are part of EEA and UK then please continue reading about Consent string requirements.

Amazon will continue to honor existing mechanisms for consent and also advise customers to utilize the TCF or Amazon Consent Signal. For any questions related to the DP API updates you can contact us [here](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/user/login?destination=portal%2F2).

##### Data consent requirements for Events Manager (CAPI, AAT ) users (Advertisers and Partners)

We will update the following Help Center articles:

- https://advertising.amazon.com/help/GLZ54GXQW773A6MG
- https://advertising.amazon.com/help/G9Y77VYQ3VJZU5YV
- https://advertising.amazon.com/API/docs/en-us/guides/overview

**What actions should events manager advertisers and partners take?**
Events Manager users (AAT, CAPI) can share any one of the consent formats that is most relevant for them.

1. Our GTM Templates will have now have additional consent checkbox and fields. Please use the consent field that is most relevant for you. If you are an agency/ partner working on behalf of your advertiser, use the consent provided by your advertiser.
2. For regular AAT there is a command called 'setAmazonConsent' that takes all of the consent fields as a JSON object.
3. For CAPI and partner integrations there will be an field on the events called AmazonConsent that takes the JSON of the consent fields.

##### Amazon Marketing Cloud (AMC), and AWS-Clean Room (AWS-CR) collaborations

Starting November 6, 2024, AMC will accept Transparency-and-Consent-Framework (TCF), Global Privacy Platform (GPP), and Amazon Consent Signal (ACS) consent signals. These consent signals will be optional in UK/EEA personal data uploads until they become mandatory. AMC will respect provided consent, rejecting records if denied. During the transitional period there will be no impact to data uploaded without consent signals.

We recommend all advertisers and third-party (3P) data providers provide this information to indicate the jurisdiction whose laws apply to processing the user's data.

The above-mentioned data decoration standards for country codes and consent signals also applies to all advertisers and 3Ps using AMC — AWS Clean Room collaborations.

Please refer to [Sending Personal Information to Amazon Ads & Amazon Consent Signal requirements](https://advertising.amazon.com/resources/ad-policy/consent-signal-requirements?ref_=a20m_us_spcs_conre) to know more about the policy. For detailed documentation, please visit the [AMC documentation section within the Advanced Tool Center (ATC)](guides/amazon-marketing-cloud/amc_consent_management).

##### Ads Data Manager APIs

Advertisers uploading their data to Ads data manager can provide consent in one of three formats: TCF, GPP, or Amazon Consent Signal. We require all records containing user-level identifiers uploaded to Amazon Ads to contain at least one consent attribute, as well as country code.

For specific technical instructions, the details are found [here](ads-data-manager).

#### Expand your reach with premium Amazon supply on Amazon DSP online video line items

Beginning November 1, advertisers running new and existing online video ad campaigns in Amazon DSP will benefit from additional Amazon first-party supply across Echo Show, Fire TV, IMDb, and Twitch. Advertisers who have selected any Amazon or third-party inventory source for their online video ad campaign will automatically have all eligible Amazon inventory sources included.

This change comes following improvements to our machine learning models and the previous launch of video ad inventory across IMDb (WW) and Twitch (WW). We will add additional online video supply from Echo Show, IMDb and Twitch in the coming months. Additional expansions across Alexa video inventory is planned for the coming months.

This auto-selection will happen in a phased manner and eligibility will be governed by ad policy and creative requirements specific to each inventory source. Advertisers have the option to proactively opt-out of this automatic inventory expansion if they choose by reaching out to a Programmatic Solution Consultant or Ad Support via Contact us in Support center.

Prior to this update, advertisers had to manually select Amazon inventory sources for their online video ad campaigns. Enabling automatic inventory optimization takes out the guesswork, simplifies campaign management, and frees up valuable time and resources, all while giving advertisers the opportunity to connect with a wider audience. This change also provides the ability to maintain control over ad placements and benefit from intelligent optimization strategies that can help achieve campaign goals more effectively.

For full technical details, please read [our API documentation](dsp-ad-group-and-campaign#tag/Ad-Group).

#### Expand your reach with premium Amazon supply on Amazon DSP display line items

Beginning November 1, advertisers running new and existing display ad campaigns in Amazon DSP will benefit from additional Amazon first-party supply across Echo Show, Fresh stores, Fire Tablet, Fire TV, IMDb, Twitch, and Whole Foods Market and Fresh digital displays. Advertisers who have selected any Amazon inventory source for their display campaign will automatically have all eligible Amazon inventory sources included.

This change comes following improvements to our machine learning models and the previous launch of display ad inventory across Echo Show (US, UK, DE, CA, MX, IT, JP), Fire Tablet (US, UK, DE, JP), IMDb (WW), and Twitch (WW). We will add additional supply from Fire TV, IMDb, Twitch, Whole Foods Market, and Fresh stores in the coming months.Additional expansions across Fire TV display component-based creatives and Twitch custom format ads are planned for the coming months.

This auto-selection will happen in a phased manner and eligibility will be governed by ad policy and creative requirements specific to each inventory source. Advertisers have the option to opt-out of this automatic inventory expansion if they choose by reaching out to Programmatic Solution Consultant or Ad Support via Contact us in Support center.

Prior to this update, advertisers had to manually select Amazon inventory sources for their display ad campaigns. Enabling automatic inventory expansion takes out the guesswork, simplifies campaign management, and frees up valuable time and resources, all while giving advertisers the opportunity to connect with a wider audience. This change also provides the ability to maintain control over ad placements and benefit from intelligent optimization strategies that can help achieve campaign goals more effectively.

For full technical details, please read [our API documentation](dsp-ad-group-and-campaign#tag/Ad-Group).

#### Amazon DSP campaign management APIs will enable read-only support for Amazon guaranteed deals

Starting Nov 6, 2024, the [Amazon DSP campaign management APIs](reference/dsp/dsp-campaign-management-overview) will enable read-only support for campaigns targeting Amazon guaranteed deals.

Summary of changes:

[POST/dsp/v1/campaigns/list](dsp-ad-group-and-campaign#tag/Campaign/operation/DspListCampaignV1):

* New boolean field: `targetsAmazonDeal`
  * If true, this campaign has ad groups targeting Amazon guaranteed deals, and the campaign, ad group, and targets are all read-only.
* New fields: `startDateTime`, `endDateTime`
  * `startDateTime`: This will be the first date of the first flight for flighted campaigns. It will be the start date for unflighted campaigns targeting Amazon deals.
  * `endDateTime`: This will be the last date of the last flight for flighted campaigns. It will be the end date for unflighted campaigns targeting Amazon deals.

[POST/dsp/v1/adGroups/list](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspListAdGroupV1):

* New `inventoryType` enum values: `AUDIO_AMAZON_DEAL`, `STREAMING_TV_AMAZON_DEAL`
  * `AUDIO_AMAZON_DEAL`: Targets Amazon audio inventory.
  * `STREAMING_TV_AMAZON_DEAL`: Targets Amazon streaming TV inventory.

[POST/dsp/v1/targets/list](dsp-universal-targeting#tag/Targets/operation/ListDspTargetsV1):

* The `inventorySourceTarget` will list the specific Amazon guaranteed deal being targeted.
* After pulling the dealId from the targets endpoint, you can use [/dsp/inventory/deals/list](dsp-deals-3p#tag/Deal/operation/listDealsDspDeals) to pull deal metadata.

We will post an announcement on our release notes page once this feature is released.

#### Improve optimization rule effectiveness using brand based cost control metric recommendations

Sponsored Brands launched the Optimization Rules Recommendation API to provide suggested Cost-Per-Click (CPC) when using Optimization Rules for Drive Page Visits campaigns. The bid suggestions are personalized based on brands owned by the advertiser, allowing advertisers to input competitive bids to drive campaign objectives.

An effective optimization metric threshold should balance auction performance with cost-effectiveness, and can vary greatly depending on your brand content and targeting strategies. The Optimization Recommendations API was designed with this principle, empowering advertisers to achieve results while still converging to cost efficient bids.

Advertisers can retrieve these recommendations through the new `/sb/recommendation/optimization` API call, for both existing campaigns and during the campaign creation process. This endpoint takes your cost control metric type, along with up to ten landing page asins or URLs, and returns both the recommended value and minimum value for your campaign.  For full technical details, please read our [API documentation](sponsored-brands/3-0/openapi/prod#tag/Recommendations/operation/SBOptimizationRecommendation).

#### Help increase engagement with advertiser-defined audiences through sponsored ads bidding controls

Sponsored Products and Sponsored Brands are launching a new bidding control to enable advertisers to reach and engage audiences segments defined and created by them. With the latest offering, advertisers can compose lists of audiences, such as those who have not bought their product before or exposed to their video awareness campaigns and set custom bids for them in their campaigns to drive more discovery and sales.

The new bidding control enables advertisers to engage with audiences created in Amazon Marketing Cloud (AMC), a privacy-safe, cloud-based, clean room solution.  The audiences are then available to their sponsored ads account so they can selectively modify the bids for them when launching their campaigns.

The new control helps advertisers more efficiently engage with selected audiences segments. Using bid controls, they can modify their bids to capture increased engagement and conversions from selected audiences segments, such as re-engaging shoppers in their journey along the marketing funnel, while also reaching the relevant Amazon shoppers that are searching for their targeted keywords. Advertisers can monitor performance through a new field in the AMC `sponsored_ads_traffic` table to understand when Amazon applied the bid increase by inspecting the column `matched_behavior_segment_ids`.

Advertisers will use the [Targetable Entities API](targetable-entities#operation/ListTargetableEntities) to retrieve Audiences (Discovery) after they are created in AMC. In Sponsored Products, they will then use the Audience ID (Canonical) with the [Sponsored Products V3 Campaigns API](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredProductsCampaigns) to create, bid adjust the audience and manage their campaigns. In Sponsored Brands, similarly, advertisers will use [Sponsored Brands Campaign Management API](guides/sponsored-brands/overview) to create and manage their campaigns, as well as adjust their audience bids.

#### Build a holistic first-party data strategy with Ads data manager

Announced at unBoxed 2024, Ads data manager (ADM) is a new standalone offering that simplifies and streamlines the process of first-party data management across Amazon Ads ad tech. ADM is a privacy-safe, easy-to-use interface that lets advertisers and their partners onboard their data once and securely reuse it across our ad products ( e.g. ADSP or AMC) for measuring conversions, engaging relevant audiences, and optimizing campaigns for sustainable business growth- all while retaining visibility and control of their data.

Advertisers' first party data is crucial for measuring campaign success and driving better marketing outcomes.  As customers adapt to reduced dependence on third party cookies and mobile advertising IDs, they are increasingly reliant on their first party data as critical inputs into audiences, measurement, optimization strategies.  To enable advertisers to unlock and scale the value of their first party data, we are launching Ads data manager. Through ADM, advertisers can access a single solution within Amazon Ads to build a holistic first party data strategy. Some of the key product differentiators include:

* Simplified and intuitive CX for data management: We are making data onboarding  frictionless, Amazon Ads advertisers (and their partners) will access a  unified interface which natively receives data through Amazon Ads Console  UI, connected APIs and CDP integrations.
* Interoperable experience across all Amazon Ad Tech products: You can upload your data once and reuse it across  all 1p data features of ad products (ADSP, AMC) to activate it for various use cases, all through one simple interface.
* Fast track onboarding via integrated partners: You will be able to easily stream data wherever  you traditionally house it, this will be made possible through a connector  library that offers integrations with partners like CDPs, MMPs.
* Secured data environments: ADM is a secure data environment with user-level access controls, giving you the ability to create isolated data environments for different use cases — all with end-to-end encryption
* Seamless hand-off between brands and agency partners: ADM will allow for streamlined data connections across advertisers and agency partners.

The Ads data manager consolidates all of Amazon’s Advertiser Data APIs into a single API collection for ease of integration. The main API functions are:

* DataSet APIs - Manages Datasets.  Datasets are tables with a specific schema. You may use this API to delete a dataset.
* Audience APIs - Creates an dataset with the audience dataset type. This API serves as a shortcut to create an audience dataset directly.  Audiences are treated the same as general datasets, but have a pre-defined schema template used for audiences.
* Sharing Rules APIs - Sharing rules allow you to list current dataset shares with linked advertiser accounts. You may also create new sharing rules to make datasets available for use in advertiser accounts for a specific application.
* Status APIs - Provides reporting on the status of uploads, size of your datasets, and volume of data that has been made available for a given sharing rule.

For more information, read our [API documentation](ads-data-manager).

#### Sponsored Ads advertisers can now unlock the power of Amazon Marketing Cloud capabilities through partners

All Amazon DSP direct advertisers, as well as partners registered with the Amazon Ads Partner Network who serve clients with or without Amazon DSP, are now eligible to register for AMC. For advertisers who use only Sponsored Ads today and work with Amazon Ads partners, AMC is now an accessible clean room where their partners can leverage the rich, granular, and event-level signals to generate bespoke insights, advise optimization tactics, and take insights-based actions on their behalf.

#### Advertisers that use AMC can now create AMC audiences for their sponsored ads campaigns

Advertisers can leverage rule-based or lookalike audiences in Sponsored Display targeting, Sponsored Products bid boost and Sponsored Brands bid boost. This functionality allows advertisers to act on the insights they derive from AMC and reach shoppers at the right time and place in their shopping journey.

Start creating custom audiences in AMC using [Rule-based audiences](guides/amazon-marketing-cloud/audiences/rule-based-audiences) and [Rule-based lookalike audiences](guides/amazon-marketing-cloud/audiences/rule-based-lookalike) to activate in your sponsored ads accounts.

#### Launching Ads data manager on Amazon Ads console and Amazon Ads API

Ads data manager is a new standalone offering that simplifies and streamlines the process of first-party data management across Amazon Ads, enabling marketers to be in the driver’s seat for making more effective, data-driven decisions. Designed with ‘data stewards’ in mind, ADM is a privacy-safe, easy-to-use interface that lets advertisers and their partners onboard their data once and securely reuse it across our ad products for measuring conversions, engaging relevant audiences, and optimizing campaigns for sustainable business growth- all while retaining visibility and control of their data.
For more details, see [Ads data manager console](adm/2_ads-data-manager-console) and [Ads data manager API](guides/ads-data-manager/get-started).

#### Show your display ads to audiences who are most likely to consider and buy from your business

Dynamic segments are collections of audiences that change based on performance and relevance criteria to reach consumers who are most likely to engage with your ads. You can select the dynamic segments to launch your campaigns easily and effectively and drive performance at scale. The new “Audiences likely to be interested in your ad“ setting leverages Amazon’s machine learning algorithms to show your ads to audiences who are most likely to consider and buy from your business — using a variety of signals such as Amazon's first-party shopping signals, your own conversion signals, and insights from your business website.

Once you have identified the audiences you want to use, you can add targeting clauses at the ad group level using [POST /sd/targets](sponsored-display/3-0/openapi#tag/Targeting/operation/createTargetingClauses). For more information on setting up audience targeting, see [audience targeting campaigns](guides/sponsored-display/audience-targeting). For more details, refer to the [full technical API documentation](guides/sponsored-display/non-amazon-sellers/get-started).

#### AMC launches self-service instance management APIs

We are excited to launch self-service instance management APIs on the Amazon Ads API as Open Beta.
AMC launched the ability to create, modify, and manage AMC instances using self-serve APIs, eliminating the need to contact an Amazon Ads representative. This feature is available to all AMC account holders, including advertising partners and direct advertisers.

The new APIs offer swift instance creation with automated approvals, using which partners can start generating value within minutes of creating an instance with 1 advertiser ID. In case of instances created with multiple advertisers, the instances are activated after a manual validation within 2 days.
Authorized AMC admin users can initiate an instance creation by sending a `POST /amc/instances` request with instance details. Advertisers can be added to an instance by calling `POST /amc/instances/{instanceid}/advertisers/updates` with advertiser IDs (DSP CFIDs and/or Sponsored Ads Entity IDs).

Listed below are the new endpoints that will help help you with instance creation and management:

- `POST /amc/instances` | Creates a new AMC instance.
- `POST /amc/instances/{instanceId}/advertisers/updates` | Creates a new advertiser update for the requested AMC instance.
- `GET /amc/instances/{instanceId}/advertisers/updates/{updateId}` | Gets the requested advertiser update for the requested AMC instance.
- `GET /amc/instances/{instanceId}/advertisers/updates` | Lists advertiser updates for the requested AMC instance.
- `PUT /amc/instances/{instanceId} `| Updates the requested AMC instance.
- `DELETE /amc/instances/{instanceId}` | Deletes the requested AMC instance.

For more information, see the [AMC administration guide](guides/amazon-marketing-cloud/admin/1_amc_administration).

#### Fire TV Inline Display Banner now available for Amazon DSP self-service advertisers

Amazon has launched the ability for all Amazon DSP self-service advertisers to book Fire TV Inline Display Banner campaigns in UK, DE, FR, IT, ES, JP and IN. The Inline Display Banner sits above the fold within the Sponsored Row of the Fire TV UI on the home tab. As the user scrolls down their screen, they will pass-over this ad placement to get to content rows below the fold. When they do so, the ad unit will reveal a “Mini-Details” view, taking up about half of the users screen.

This ad unit also accepts an optional video asset (:10-:30s) with sound. Advertisers have the opportunity to set up Entertainment or digital ASIN, Prime Video Channel benefit ID, video and physical ASIN click-through destinations (only available in UK and DE). This provides an opportunity for advertisers to drive incremental reach, brand awareness and consideration.

Offering Fire TV ad placements to be booked self service via Amazon DSP opens up opportunity for advertisers looking to utilize self service buying models and removes minimum spend requirements that come with managed service campaigns. This allows advertisers flexibility to launch campaigns quickly with lower SLAs compared to managed service, as well as the opportunity to optimize campaigns in real-time. Fire TV ad placements provide advertisers an opportunity to drive incremental reach, brand awareness and consideration. On average, Fire TV ad campaigns observed 2x+ incremental reach with only a 3% overlap in audience when added to Prime Video ad campaigns (Source: Amazon Internal, Jan-Mar 2024, US).

For full technical details, please read [our API documentation](dsp-ad-group-and-campaign#tag/Ad-Group).

#### Access valuable Brand Stores details from the Amazon Ads API

We are introducing a new Brand Stores API, which advertisers can use to programmatically to access details of Brand Stores on Amazon. These details include a list of stores and their pages, URL and its status. This information can be combined with other Amazon Ads data and Stores Insights to get holistic views and performance for Brand Stores on Amazon and its pages.

Historically, to get Brand Store details, Store owners could only access this information through the advertising console one store and page at a time. With introduction of the Brand Store APIs, Store owners can access the same information programmatically, through the Amazon Ads API. This is especially helpful for getting Brand Store details across different countries (or locales), or when a Brand is managing multiple Brand Stores.

Advertisers can get details of their Brand Stores and its pages such as `name`, `id`, `status` and `url` for a given `brandEntityId`. API response is paginated with max number of documents returned as 30. For full technical details, please read [our API documentation](brand-home).

#### Deprecation announcement - Reminder: Sponsored Display version 2 reporting endpoints will shut off on October 31, 2024

Regions: All

Following the release of the new [version 3 reporting endpoints](release-notes/index#sponsored-display-reports-on-amazon-ads-api-reporting-version-3-beta), we are announcing the deprecation of version 2 Sponsored Display reporting with a planned shutoff date of October 31, 2024. This includes the [POST /sd/{recordType}/report](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport) endpoint. Until then, expect additional throttling levels on the current Sponsored Display version 2 endpoint as we begin the deprecation process.

Going forward, you can access all version 2 Sponsored Display report types using the [version 3 reporting endpoints](offline-report-prod-3p). For more details on how to upgrade to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3) and [Frequently asked questions](guides/reporting/v3/faq).

If you have feedback, use the Amazon Ads API [Support page](support/overview) to submit a ticket with the subject title “SD reports v2 deprecation” by October 15, 2024.

#### AMC bespoke solutions is now available for all users of AMC on AWS Clean Rooms

Users of AMC on AWS Clean Rooms can now leverage bespoke solutions from the Solutions tab of the AMC console. Log in to the AMC console to access your first solution - High-value audiences.

#### Amazon Marketing Cloud users now have access to a new table to analyze video metrics

A new table, `dsp_video_events_feed`, is now available for AMC users. The table provides video metrics for each of the video creative events triggered by the video player and associated with the impression event. For more details, see [Amazon DSP video events table](guides/amazon-marketing-cloud/datasources/dsp_video_events_feed).

#### Amazon DSP’s new campaign management endpoints are now available in open beta

The updated campaign management resources for Amazon DSP are now available worldwide in open beta. With this release, we are making the endpoints available to all customers with access to an Amazon DSP account. You no longer need to request permission to access the new endpoints via the Amazon Ads API.

The new campaign management resources enable you to create, read and update Amazon DSP campaigns, ad groups, targets, flights and product conversion tracking details. They replace the [legacy campaign management APIs](dsp-campaigns), which are scheduled for deprecation on March 31, 2025 per our [previous announcement](release-notes/index#deprecation-announcement-amazon-dsp-legacy-campaign-management-endpoints-will-shut-off-on-march-31-2025). In addition to being available worldwide and supporting programmatic guaranteed deals, the new APIs improve the experience developing at scale, for example, through support of PATCH operations and larger batch sizes.

If you are integrated with the legacy resources, we’ve put together a [migration guide](reference/migration-guides/adsp-campaign-management) to help you transition to the new resources. If you are new to managing Amazon DSP campaigns via API, refer to our [reference documentation](reference/dsp/dsp-campaign-management-overview) and [developer guide](guides/dsp/developer-guide).

For questions, visit the [Support page](support/overview) to submit a ticket with the subject title “ADSP CM API Migration.”

### September 2024

#### Display frequency cap benefits to advertisers with actionable insights for Amazon DSP

Frequency cap insights show advertisers the impressions and the budget reinvested to reach more shoppers due to frequency caps used at line, order, and frequency group levels. Advertisers can now see these numbers in a new insight card and can download the data. Advertisers can also use a new Amazon Ads API to query frequency cap insights data.

Frequency caps allow advertisers and agencies to save impressions by preventing ads from showing to the same customers repetitively. However, prior to this launch, Amazon DSP didn’t provide any way to quantify the benefits. To bridge this gap, we built Frequency Cap Insights to quantify impressions reinvested, budget reinvested, and incremental reach.

For more information, see our [reference documentation](dsp-freq-insight).

#### Create new deals instantly with Amazon Media on Amazon DSP

Amazon DSP has launched the ability to create new preferred deals with Amazon Media directly in inventory hub for self-service customers worldwide. With this feature, customers can create new deals with Amazon Twitch, Amazon Freevee and Amazon Prime Video based on their applicable rate cards. Customers can configure supply-side audience segmentation and submit their deal proposals for approval, and activate new deals instantly on existing line items. This feature is launching to all Amazon DSP self-service customers.

With this release, Amazon DSP customers gain the ability to propose new deals without the need to work with their Amazon Ads representatives. They can discover available deals sold by Amazon Media, understand the applicable rates, choose the deals they are interested in, and precisely configure the supply-side audience segmentation they’d like. The deals supported at launch are approved programmatically, resulting in customers being able to activate newly created deals within minutes.

For more information, please read our [reference documentation](dsp-deal-props).

#### Target programmatic guaranteed display deals via Google AdX on Amazon DSP campaigns

Advertisers globally now have the ability to target programmatic guaranteed display deals in their Amazon DSP campaigns. Similar to programmatic guaranteed video deals that are already supported with Google AdX, customers need to negotiate and finalize programmatic guaranteed display deals within Google’s Authorized Buyers UI. Once the deal is finalized, the deal will now be available for targeting within display line items.

Supporting programmatic guaranteed display deals via AdX expands on the programmatic guaranteed offering within Amazon DSP. Before this release, we only supported video for programmatic guaranteed. But this release provides another method of guaranteed buying to meet advertisers’ needs. For more information on the deals API, see our [getting started guide](guides/dsp/deals).

#### Two new reporting metrics - Add to List for Authors and Book Publishers and Qualified Borrows for Book Publishers

Two new metrics have been added to V3 reporting:

1. Add to List (For Book Vendors and Authors) : Number of users who add advertised book to list during ad exposure and within attribution window
2. Qualified Borrow (For Book Vendors) : Number of Kindle Unlimited users who have downloaded the book due to ad exposure, during the attribution window, and have read a certain percent of the book within the window.

For more information, refer to our [reporting documentation](guides/reporting/v3/columns).

#### Advertisers who use the Amazon Ads API can now run ads on their Global Store listings

Availability: US, UK, DE

With the launch of Sponsored Products on the Amazon Global store, advertisers can now drive more discoverability and improve sales internationally using the Amazon Ads API. Learn more about the [Global Store](https://sellercentral.amazon.com/help/hub/reference/202139180)

**API details**

Product selector [[/product/metadata](product-metadata)]

The product selector API fetches the listings required for campaign creation. With this launch, we made changes in this API to fetch the global store offers for Sponsored Products ads creation. We introduced an attribute called `isGlobalStoreSelection `which advertisers will set to `true` to fetch their global store listings.

In the response, we'll show a `globalStoreSetting` attribute that holds the details of the source country from where Amazon's Global Store sourced the offer. Advertisers can further use these listings and their corresponding source country information for the campaign creation.

Campaign creation [[/sp/productAds](sponsored-products/3-0/openapi/prod#tag/Product-ads/operation/CreateSponsoredProductsProductAds)]

The campaign creation API accepts the global store specific attribute `globalStoreSetting` where advertisers can provide the source catalog country of the offer being used for ads creation. You can check eligibility of the products for campaign creation using the `catalogSourceCountryCode` information provided using the `globalStoreSetting.`

Product eligibility [[/eligibility/product/list](eligibility-prod-3p)]

We've introduced a new `globalStoreSetting` attribute that takes the source country code of the Global Store ASIN which is required for the eligibility check. Eligibility check of the offer would be done in the source marketplace for the advertiser.

Bid recommendations [[/sp/targets/bid/recommendations](sponsored-products/3-0/openapi/prod#tag/Theme-based-bid-recommendations/operation/GetThemeBasedBidRecommendationForAdGroup_v1)]

The bid recommendation API for new Ad groups takes a new attribute called `productDetailsList` ,which takes the ASIN details along with its source country code. The bidding system uses the `globalStoreSetting` attribute and the `catalogSourceCountryCode` information for eligibility check of the ASINs, which is required for the bid recommendation.

Keyword recommendations [[/sp/targets/keywords/recommendations](sponsored-products/3-0/openapi/prod#tag/Keyword-Targets)]

The keywords recommendation API for ASINs takes a new attribute called `productDetailsList`, which takes the ASIN details along with its source country code. Keywords recommendation system uses the `globalStoreSetting` attribute and the `catalogSourceCountryCode` information  to get the bid suggestions for the keywords.

#### 3P API Access for existing ABVP users

The ABVP team is excited to announce the launch 3P API access for ABVP. Advertisers with ABVP access can can now use the API feature to access a URL to directly download ABVP reports instead of clicking through the ABVP User Interface.

With 3P API access for our advertisers, users can now access a URL to directly download ABVP reports instead of clicking through the ABVP User Interface. Users are able to configure their API to access either the latest ABVP ASIN grain report and standard report for their advertiser.

Using the two 3P endpoints below

* /insights/brandBenchmarks/advertisers/{advertiserId}/allReportMetadata
* /insights/brandBenchmarks/advertisers/{advertiserId}/reports/{reportType}/indexDates/{indexDate}

ABVP users will be able to obtain the latest report metadata information from the first endpoint and then use that report metadata information to get a presigned S3 URL to download the report using the second endpoint. For full technical details, please review [our API documentation](brand-benchmarks).

#### Amazon DSP Traffic events enhancements in Amazon Marketing Cloud (AMC)

AMC has updated the Amazon DSP traffic events across impression, interaction, and Amazon Attributed event pairs.
20 new dimensional columns were added to the following AMC data views: `dsp_impressions, dsp_views, dsp_clicks, dsp_impressions_by_*_segment, and amazon_attributed_events_by_*_time`.

#### Set up AMC on AWS Clean rooms using the developer guide

A developer guide for [AMC on AWS Clean Rooms](guides/amazon-marketing-cloud/acr/1_overview) is now available on the Advanced tools center for users to reference and set up AMC on AWS Clean Rooms.

#### AMC users can now create AMC audiences without needing to subscribe to Paid features

AMC users can use the [conversion_all](guides/amazon-marketing-cloud/datasources/conversions_all_paid) data source to create AMC Audiences without needing to subscribe to Paid features. Also, refer to the a complete [list data sources you can use to create AMC Audiences](guides/amazon-marketing-cloud/audiences/rule-based-audiences#available-data-for-rule-based-audiences-queries).

#### Access and take action on invoices across all countries from a single page

Regions: All

Sponsored ads and Amazon DSP advertisers can now preview, download, and pay for invoices in all countries from a single page, which also has a country-level summary of the total amount due. Advertisers can also view a country-level summary for billing setup status, which determines campaign activation, total amount due and overdue per country, and pay consolidated amounts at a country level. Third-party integrators can download invoice summaries and pay consolidated amounts at a country level.

Previously, advertisers had to navigate to a billing center page at a specific country level to view the total amount due, overdue, and pay the amount for that particular country. With this launch, advertisers can view a single page with a country-level summary and paid, unpaid invoice table having invoices from all countries they are advertising in. This saves 2-3 clicks each time an advertiser goes to any country-level billing center page. This launch will also be applicable to third-party integrators where they will have public APIs to search and download invoices both individually and in bulk.

More details can be found [in the technical documentation](billing#tag/Billing-Invoice-Summary(s\)).

#### Increased parallel query execution capabilities in Amazon Marketing Cloud

Amazon Marketing Cloud has increased the parallel query execution capabilities of its workflows. Now, an AMC account can run 200 API queries simultaneously, a 4X increase from the previous limit of 50.  Also, an AMC instance can execute 10 API and 10 UI queries in parallel, doubling the previous limit of 5 for each type of query. See [Query execution limit](guides/amazon-marketing-cloud/reporting/execute-workflow#query-execution-limit).

#### Deprecation announcement: Sponsored Display version 2 reporting endpoints will shut off on October 31, 2024

Regions: All

Following the release of the new [version 3 reporting endpoints](release-notes/index#sponsored-display-reports-on-amazon-ads-api-reporting-version-3-beta), we are announcing the deprecation of version 2 Sponsored Display reporting with a planned shutoff date of October 31, 2024. This includes the [POST /sd/{recordType}/report](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport) endpoint.

Going forward, you can access all version 2 Sponsored Display report types using the [version 3 reporting endpoints](offline-report-prod-3p). For more details on how to upgrade to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3) and [Frequently asked questions](guides/reporting/v3/faq).

If you have feedback, use the Amazon Ads API [Support page](support/overview) to submit a ticket with the subject title “SD reports v2 deprecation” by August 31, 2024.

#### Awareness: Sponsored ads may begin appearing in Rufus-related placements

Regions: US

To help customers discover more products in Amazon’s generative AI-powered shopping assistant, referred to as Rufus, your ads may appear in Rufus-related placements. Rufus may generate accompanying text based on the context of the conversation. Your campaign reports won’t include Rufus metrics. For more information, contact your account representative. See our dedicated page to [learn more about Rufus](https://www.amazon.com/Rufus).

#### Deprecation announcement: v2/portfolios will be deprecated on March 1, 2025

We are announcing deprecation of the v2/portfolios endpoints with a shutoff date of March 1, 2025. API callers are encouraged to use the `/portfolios` endpoints instead.

For more information, see the [technical specifications for /portfolios](reference/portfolios). For information on deprecations, review the [Deprecations page](release-notes/deprecations).

#### Amazon DSP launches similar audiences (beta) to reach new adjacent audiences in just one click

Regions: US, CA, MX, UK, DE, ES, IT, FR, NL, SE, TR, UAE, SA, AU, JP, IN, BR

Similar audiences (beta) utilizes the latest in AI to enhance your campaign reach to consumers exhibiting similar shopping, streaming, or browsing behaviors or interests as your selected audiences. This feature helps you serve ads to these highly relevant prospective customers more efficiently at scale, without relying on third-party cookies or ad identifiers. With this beta launch, advertisers can activate similar audiences on any line item that includes custom-built audiences based on Amazon shopping interactions.

For full technical details, please see our [API reference documentation](dsp-ad-group-and-campaign#tag/Ad-Group).

### August 2024

#### Sponsored Brands introduces landing page edits in the Amazon Ads API - now available worldwide

Sponsored Brands introducing a landing page edit for Product Collection, Store Spotlight and Brand Video Ads. Advertisers and Partners are now able to change their ads landing page without creating a new campaign or ad group. Now, each ad can have individual landing pages within same ad group. You can now update your ad group based campaigns landing pages without recreating your ad group or campaign. For editing a landing page, advertisers and partners can use [/sb/ads/creatives/productCollectionExtended](sponsored-brands/3-0/openapi/prod#/Ad%20creatives/CreateExtendedProductCollectionCreative) for Product Collection ads, [/sb/ads/creatives/storeSpotlight](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateProductCollectionCreative) for Store Spotlight ads, and [/sb/ads/creatives/brandVideo](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateBrandVideoCreative) for Brand Video ads.

With this launch, we've expanded to UK, DE, FR, IT, ES, NL, AE, IN, SA, SE, PL, TR, EG, SG, ZA so more advertisers can access these features using the Amazon Ads API. For full technical details, please see our API reference documentation.

#### Recent enhancements to the Amazon Ads API integration dashboard

We have recently introduced several enhancements to the Amazon Ads API integration dashboard. We have:

- improved dashboard update frequency from daily to hourly. You are now able to see today's data.
- enabled URL linking with associated dashboard filters. You can now create a link to share a filtered view with colleagues.
- enhanced the filtering functionality in two ways:
  - Filters now carry over across dashboard tabs and do not need to be rebuilt.
  - You can now see a smaller subset of filter values when you apply filters on other dimensions. For example, in the Resources tab, when you set the Product filter to "Sponsored Products" you should only see resource values for Sponsored Products.
- introduced our feedback module on the “Your apps” and “Dashboard” pages so you can help us understand how to improve these features.
- reorganized the help content on the dashboard pages to make it easier to find information about individual tabs and the dashboard overall.

To learn more, see [Integration dashboard](guides/get-started/api-integration-dashboard).

#### Amazon DSP performance datasets available in Amazon Marketing Stream

Regions: All

[Amazon Marketing Stream](guides/amazon-marketing-stream/overview) now includes hourly performance metrics for Amazon DSP campaigns. This dataset will provide hourly aggregated performance metrics in near real-time, which can be used to enable timely campaign adjustments, intraday optimizations, and near real-time reporting solutions. This dataset can be used in conjunction with the Amazon DSP Campaign dataset to get up-to-date campaign information.

Amazon Marketing Stream is available via the Amazon Ads API. When you are ready to get started, visit our [Amazon Marketing Stream onboarding guide](guides/amazon-marketing-stream/onboarding/firehose/overview), which will walk through the process of getting API access, integrating with AWS, and subscribing to campaign datasets using the Amazon Marketing Stream API. If you are already integrated with Amazon Marketing Stream, see the new [Amazon DSP dataset documentation](guides/amazon-marketing-stream/datasets/adsp-performance).

#### Amazon DSP advertisers can now delete product audiences

Advertisers can now delete product audiences that are not part of an active line item. This feature will be available via the Amazon Ads API, and also in the Amazon DSP console.

Before, advertisers could not delete product audiences after they were created. Being able to delete unused product audiences along with audiences that are no longer targeted will reduce clutter and provide a better experience for advertisers.

For full technical details, please see our [API reference documentation](audiences#tag/Ads/operation/DspAudienceEdit).

#### Deprecation announcement: v3 content type for Sponsored Products theme-based bid recommendations will be deprecated on May 15, 2025

We are announcing deprecation of the `application/vnd.spthemebasedbidrecommendation.v3+json` content type for Sponsored Products theme-based bid recommendations with a shutoff date of May 15, 2025. API callers are encouraged to use `application/vnd.spthemebasedbidrecommendation.v4+json` instead.

For more information on version 4, see the [technical specifications for theme-based bid recommendations](sponsored-products/3-0/openapi/prod#tag/Theme-based-bid-recommendations). For information on deprecations, review the [Deprecations page](release-notes/deprecations).

#### AMC admins can now programmatically invite users to their accounts

With the launch of [user management APIs for Amazon Ads](#user-management-apis-are-launching-for-amazon-ads), AMC admin users can now programmatically invite admin and viewer users to their account. For details, see [Account management](guides/amazon-marketing-cloud/admin/account-management).

#### AMC users can now view the status of their created audiences using Amazon DSP APIs

The status of audiences created using AMC's rule-based audience APIs can be viewed using Amazon DSP's [list audience](audiences#tag/Discovery/operation/listAudiences) endpoint. For details on how to view the status of a specific audience Id, see [View audiences](guides/amazon-marketing-cloud/audiences/rule-based-audiences#rule-based-audiences-sql-queries-and-tables).

#### Introducing the Optimal frequency analysis playbook for Amazon Marketing Cloud

This Optimal frequency analysis playbook dives deeper into the concept of ad frequency to and guides users with tactics to get clear optimization insights and direct actions within Amazon Ads to optimize frequency analysis.

An example of the optimization is to define the right value threshold for identifying audiences that are considered under-served from a frequency standpoint and activate those. To learn more, visit [Optimal frequency analysis playbook](guides/amazon-marketing-cloud/playbooks/optimal-frequency-analysis).

#### Notice on datasets created using Advertiser data upload, version 1.0

Note that data that was uploaded using Advertiser data upload, version 1.0 (ADU 1.0) cannot be queried. To continue querying datasets that you had uploaded in ADU 1.0,  delete the previously uploaded data, and re-upload using Advertiser Data Upload 2.0 (ADU 2.0). See [Advertiser data upload FAQs](guides/amazon-marketing-cloud/amc-migration-hub/migration-FAQs#advertiser-data-upload-api). Reach out to amc-support@amazon.com for any further assistance.

#### Creative assets launches new batch APIs for asset registration

Regions: All

Creative assets is introducing two new batch API endpoints to help you manage your creative assets more efficiently. The new endpoints include [POST /assets/batchRegister](creative-asset-library#tag/Creative-assets/operation/assetsBatchRegister), which enables you to add custom tags and other metadata to assets to aid in asset management and usage for multiple assets in a batch API call, and [GET /assets/register/batch/{requestId}](creative-asset-library#tag/Creative-assets/operation/getAssetsBatchRegister), which provides registration details and current status of the individual assets requested in a batch API call.

These endpoints are available for all sponsored ads campaign types as well as for Amazon DSP, via the Amazon Ads API.

#### Sponsored TV launches brand store support in the US

Regions: US

We’ve enabled brand store support for Sponsored TV campaigns in the US. Now, vendors and registered brand owners can specify a page within their brand store to link viewers to with their Sponsored TV ads. When these ads are eligible for interactive ad formats, audiences can scan QR codes, click the ad, or even interact with the ad via their remote to discover more about your brand. When viewers interact with these ads with their remote, they’ll receive an email with a link to an advertiser’s specified brand store. As always, Sponsored TV interactive ad formats are turnkey, with no action required as long as they meet our eligibility criteria.

Previously, advertisers could only link to ASIN detail pages with their Sponsored TV ads, so if an ASIN went out of stock or assets featured multiple ASINs, advertisers either had to manually create new ads or choose one of many ASINs to link viewers to.  Now, advertisers can use more of their assets— like those those that feature multiple ASINs or size-intensive categories (e.g., shoes) for their Sponsored TV campaigns.

For full technical details, [review the API documentation](sponsored-tv-open-beta/#tag/Ads/operation/CreateSponsoredTvAds).

Learn more about [Sponsored TV creative specs and interactive ad formats](https://advertising.amazon.com/resources/ad-specs/sponsoredtv)

#### User management APIs are launching for Amazon Ads

Amazon Ads API customers can now perform user management-related read operations entirely through API integration. Specifically, they list users as well as their roles and permissions for any Amazon Ads account across all countries and ad products.

Previously, user management functionality was only available through the advertising console. This launch marks the second milestone in a series of authorization API launches which make user management functionality available to Amazon Ads API customers. In April 2024, Amazon Ads launched [user invitation APIs](user-invitations). With this launch, customers can list users, as well as their roles and permissions associated with any Amazon Ads account via public API, without the need to visit the advertising console. Customers can use these APIs for advertising accounts across all ad products, including sponsored ads, Amazon DSP, and Amazon Marketing Cloud (AMC). Customers can also use these APIs to list users, roles, and permissions for manager accounts. Finally, customers can pass a Global Account identifier with a country code for the sponsored ads account to invoke these APIs. By integrating with these APIs, third parties can build user management workflows on their own advertising platforms.

New APIs include:

`POST /users/list`: List users associated with a given Amazon Ads account. Each user object returned will contain the user ID and the masked email

`POST /userPermissions/list`: List the permissions that a given user has on a given Amazon Ads account

`POST /userRoles/list`: List the direct role that a given user has on a given Amazon Ads account

For more information, [review the API specification](user-permissions).

#### Sponsored Display has enhanced campaigns using tactic ‘T00030’ for endemic advertisers

Regions: All

Sponsored Display campaigns using tactic ‘T00030’ can now access all targeting capabilities available to endemic advertisers, including contextual targeting. Before this launch, Sponsored Display campaigns with tactic ‘T00030’ could only be used alongside audience strategies and entertainment targeting. To ensure backwards capability, there are no changes to tactic ‘T00020’ and it can continue to used for contextual targeting. The ‘T00020’ tactic will be supported for the foreseeable future, however we recommend using the ‘T00030’ tactic for new campaign creation to unlock all Sponsored Display targeting options.

By allowing greater Sponsored Display campaign flexibility with tactic ‘T00030’, advertisers can focus on achieving their marketing goals, without campaign level targeting constraints. Advertisers can use contextual targeting to help generate detail page traffic and then lean into audience strategies to reengage audiences to help secure missed sales opportunities or further cultivate brand loyalty. Within audience strategies, we added additional audiences for entertainment targeting (Games, Music & TV, Music & Radio) which can help advertisers reach potential customers during their entertainment journey - whether they're live streaming, watching videos, or researching games and movies on various sites beyond the Amazon store.

For full technical details, please see our [Sponsored Display targeting API](sponsored-display/3-0/openapi#tag/Targeting/operation/createTargetingClauses), including  `ContentTargetingPredicate`, `TargetingPredicate`, and `TargetingPredicateNested` that can now be used for Sponsored Display campaigns using ‘T00030’ for endemic advertisers. For more information on entertainment targeting, see our updated [developer guide](https://advertising.amazon.com/API/docs/en-us/guides/sponsored-display/entertainment-targeting).

#### Measure audience with Streaming TV incremental household reach

Advertisers on Amazon DSP now have access to incremental and exclusive reach (vs. Linear TV) reporting for their Amazon streaming TV campaigns. The two metrics that are part of this suite are Incremental Reach Rate and Exclusive Reach Rate. The Incremental Reach Rate quantifies the number of unique households reached by the STV campaign who were not exposed to Linear TV ads for the same product, expressed as a percentage of Linear TV household reach. The Exclusive Reach Rate quantifies the number of unique households reached by the STV campaign that were not exposed to Linear TV ads for the same product, expressed as a percentage of the STV campaign's total household reach.

The combination of these two metrics will enable an advertiser to determine the impact of the STV campaign's reach in comparison to their Linear TV campaign and also assess the return on investment (ROI) on the STV investments specifically with the Exclusive Reach Rate. STV Incremental Reach complements the Incremental Reach Forecast already available in the STV channel planner, so that advertisers can compare their past performance with potential reach.

To learn more, see the [exclusiveReachRate](guides/reporting/v3/columns#exclusiveReachRate) and [incrementalReachRate](guides/reporting/v3/columns#incrementalReachRate) column details.

#### Deprecation announcement: Sponsored Products bid recommendations for keyword, product, and auto targeting endpoint will shut-off on May 15, 2025

We are announcing the deprecation of [/v2/sp/targets/bidRecommendations](sponsored-products/2-0/openapi#tag/Bid-recommendations/operation/getBidRecommendations) effective August 15, 2024. The [theme-based bid recommendations](sponsored-products/3-0/openapi/prod#tag/Theme-based-bid-recommendations/operation/GetThemeBasedBidRecommendationForAdGroup_v1) endpoint is the replacement for `/v2/sp/targets/bidRecommendations`. The v2 endpoints will shut off on May 15, 2025.

For more information, review the [Deprecations page](release-notes/deprecations).

#### Understand the scale Amazon inventory can provide by demographics and audiences using our Reach Forecasting API

Regions: US, AE, AU, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, BR, SE, TR

For the first time, Amazon has provided the ability to integrate Amazon forecasts across all first-party and third-party supply. Using the Reach Forecasting API, you can programmatically generate reach forecasts showing the available reach based on your brand and campaign parameters.

To learn more, view the [technical specifications for the Reach Forecasting API](guides/media-planning/reach-forecasting/overview).

#### Sponsored Display support in advertiser test accounts

Advertisers will now be able to create Sponsored Display campaigns in a test account. Test accounts are beneficial because they allow integrators to make API requests to test and learn about the functionality in a safe manner, without serving ads or incurring costs. These campaigns will not be served to shoppers and will not incur any advertising charges. For more information on using test accounts for Sponsored Display campaigns, see our [Using test accounts for Sponsored Display documentation](guides/account-management/test-accounts/use-test-accounts#testing-sponsored-display) in test accounts.

#### Deprecation announcement: Reminder that Sponsored Display version 2 reporting endpoints will shut off on October 31, 2024

Following the release of the new [version 3 reporting endpoints](release-notes/index#sponsored-display-reports-on-amazon-ads-api-reporting-version-3-beta), we are announcing the deprecation of version 2 Sponsored Display reporting with a planned shutoff date of October 31, 2024. This includes the [POST /sd/{recordType}/report](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport) endpoint.

Going forward, you can access all version 2 Sponsored Display report types using the [version 3 reporting endpoints](offline-report-prod-3p). For more details on how to upgrade to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3) and [Frequently asked questions](guides/reporting/v3/faq).

If you have feedback, use the Amazon Ads API [Support page](https://advertising.amazon.com/API/docs/en-us/support/overview) to submit a ticket with the subject title “SD reports v2 deprecation” by August 31, 2024.

### July 2024

#### Inherited Settings are now available for Display inventory and viewability

You are now able to create, retrieve, and delete Inherited Settings in Manager and Advertiser accounts in the Amazon DSP UI and API.

Inherited Settings enable Managers and Advertisers to set settings that are used as presets when creating a new line item. Our first release includes Display inventory and viewability. We will communicate additional media types and settings when they become available.

For information, see our [API documentation](dsp-inherited-settings).

#### Amazon Marketing Cloud can now leverage the “collaboration” feature in AWS Clean Rooms

A [collaboration](https://docs.aws.amazon.com/clean-rooms/latest/userguide/create-collaboration.html) is a secure logical boundary in [AWS Clean Rooms](https://docs.aws.amazon.com/clean-rooms/latest/userguide/what-is.html) in which one member (such as an advertiser) can perform SQL queries on configured tables from other members (such as an advertiser partner or other publisher). AMC and advertiser collaborations in AWS Clean Rooms enable AMC customers to use AWS Clean Rooms to associate their first-party (event and identity) data with AMC’s data, to collaborate with Amazon Ads unique signals including matching audience identifiers enabled by [AWS Entity Resolution](https://docs.aws.amazon.com/entityresolution/latest/userguide/what-is-service.html), and generate differentiated insights to activate more relevant campaigns with Amazon Ads, all without having to share or move your data from your AWS cloud environment.

See the complete list of [AMC - AWS Clean Rooms reference](amc-administration).

#### Introducing the Off-Amazon conversions playbook for Amazon Marketing Cloud

This new playbook provides an overview of off-Amazon signals, how to load signals into AMC, and 12 non-endemic use-cases along with suggested actions. Advertisers can use this playbook to create valuable analysis of and take action with off-Amazon data. To learn more, visit [Off-Amazon conversions playbook](guides/amazon-marketing-cloud/playbooks/off-amazon-conversions).

#### Subscribe & Save Metrics & Benchmarks available in Brand Metrics Ads Console, and Ads API

Advertisers can now measure their retail and ads brand-category Subscribe & Save (SNS) performance with two new metrics in Brand Metrics.

1. Subscribe & Save customers: Total individual Subscribe & Save customers in the selected category over the selected timeframe.
2. % Subscribe & Save new-to-brand customers: The percentage of Subscribe & Save customers that are new-to-brand in the selected category over the selected timeframe.

For both SNS metrics, we provide Category Median and Category Top benchmarks:

- Category Median: The performance of the 50th percentile of brands in the selected category.
- Category Top: The average performance of the 95th–99th percentile of brands in the selected category.

Advertisers, agencies, and integrators can now understand their brand’s Subscribe & Save performance within a retail category, and quantify how many shoppers who purchased in a selected week or month are SNS customers and whether or not they have subscribed for the first time with their brand in the TTM (trailing twelve months e.g. new-to-brand). Advertisers can also benchmark their ad attributed and retail performance amongst category peers. With these benchmarks brands not running SNS can get insight into the missed SNS opportunities in category, and for brands running SNS they can gauge their performance in category against peers.

SNS data is also available through the Brand Metrics Public API is the [/insights/brandMetrics/report/{reportId}](brand-metrics-openapi#tag/Report/operation/getBrandMetricsReport) endpoint. For full technical details, please read [Brand Metrics API Documentation](brand-metrics-openapi).

#### Sponsored TV impressions forecasting

We have launched a forecasting service for Sponsored TV (ST), allowing Sponsored TV advertisers to view impressions for their campaigns. The Amazon Ads ST forecasting provides forecasts based on the Sponsored TV campaign and ad group settings. Currently, it supports weekly impressions forecasts and will expand to include other forecast metrics in the future. Forecasts are based on machine-learning models that are built on the observable performance of historical campaigns with similar settings. To learn more, view the [technical specifications for the forecasting API](sponsored-tv-open-beta#tag/Forecasts/operation/SponsoredTvForecasts).

#### Introducing Sponsored TV in Canada, Mexico, Brazil, and United Kingdom

Regions: CA, MX, BR, UK

Sponsored TV is now available in Canada, Brazil, Mexico, and United Kingdom to enable advertisers access to a self-service streaming TV advertising solution internationally.

Please see our [developer guide](guides/sponsored-tv/overview) to get started on Sponsored TV.

#### Portfolios API version 3

The Portfolios version 3 API brings parity between API integrators and the Amazon Ads console, and is the same API used by the console for portfolio functionality. Use this API to group your campaigns and set budgets for a given portfolio and group of campaigns. To learn more, view the [technical specifications for the Portfolios API](reference/portfolios).

#### Modeled conversions now reported for Sponsored Display campaigns

Regions: All

To offer a more complete view of campaign results, Amazon Ads uses modeling to account for conversions attributable to your campaign, but that are not directly measured due to the phasing out of third-party cookies and changing regulations. In these cases, Amazon Ads has insights about the ad click or view and the conversion that occurred, but cannot establish a direct link between the two events. In that scenario, Amazon Ads predicts the link between the ad interaction and the conversion event. Modeled conversions are reported in the same columns as directly attributed conversions, such as on the Purchase or Detail Page View column. In some cases, if we cannot produce meaningful estimates at the level of your reporting breakdown—for example at the targeting clause level—we will report conversions on an Unallocated row.

Amazon Ads models conversions so that advertisers can continue to measure the full impact of their advertising spend across all audiences, including previously un-addressable audiences, due to industry changes in third-party cookies and regulations. This helps advertisers continue to set and optimize bids and budgets effectively.

Advertisers using Amazon Ads API may see conversions reported on rows with “Unallocated,” or -20, set as the dimension value for reports grouped by targeting, as an example. These rows represent conversions attributable to that ad line or order that cannot be allocated at the level at which the report is returned. For full technical details, please read our [reporting documentation](https://advertising.amazon.com/API/docs/en-us/guides/reporting/v3/overview). To learn more about modeled conversions, please visit the [Support Center](https://advertising.amazon.com/help/G4LNN5YWHP6SM9TJ)

#### API and bulksheets support for South Africa

Regions: ZA

We have added support for South Africa (ZA) to the Amazon Ads API and bulksheets. For more details, see the [API endpoints by region](reference/api-overview#api-endpoints), [Bid constraints by marketplace](concepts/limits#bid-constraints-by-marketplace), [Budget constraints by marketplace](concepts/limits#budget-constraints-by-marketplace), as well as the `countryCode`, `currencyCode`, and `timezone` parameters in the [profiles reference](reference/2/profiles#/Profiles/listProfiles). For bulksheets, you can also reference [Bid constraints by marketplace](concepts/limits#bid-constraints-by-marketplace) and [Budget constraints by marketplace](concepts/limits#budget-constraints-by-marketplace).

#### API onboarding support is moving from Jira to Zendesk

The API onboarding support team will transition from Jira to Zendesk as our primary platform for managing customer support tickets starting July 8, 2024.

This change aims to provide a more direct and recognizable channel for assistance. If you experience issues onboarding to the API, you can reach our support team directly through the following email address: `ads-api-onboarding@amazon.com`

Please add **ads-api-support@amazon.com**, **ads-api-onboarding@amazon.com**, and **support@amazon-ads-api.zendesk.com** to your email's safe sender list to ensure seamless communication.

For more information about support for the Amazon Ads API, see ["Need support?"](https://amazon-ads-api.zendesk.com/hc/en-us/articles/25110088401435-Need-Support-Find-your-Answers-Here) in our Zendesk platform.

### June 2024

#### Managing failed audiences in Amazon Marketing Cloud

You are now able to delete and edit audiences with “Failed” status via the AMC UI and API. This allows you to only keep the custom audiences of use to you and reduce redundant audiences. For more information, refer to [Manage failed audiences](guides/amazon-marketing-cloud/audiences/rule-based-audiences#manage-failed-audiences).

#### Awareness: Updates to Sponsored Brands version 3 campaigns

Legacy Sponsored Brands version 3 campaigns created before October 2022 recently went missing in the advertising console after being rejected during creative moderation.  These `servingStatusDetails` for these campaigns returns `REJECTED_DETAIL` in [POST /sb/v4/campaigns/list](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns). These campaigns will show as `ARCHIVED`  in [POST /sb/v4/campaigns/list](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns) and the advertising console by **August 15th, 2024**.

To prevent similar issues with version 3 campaigns, we strongly recommend recreating all version 3 Sponsored Brands campaigns as new version 4 campaigns. You can use Sponsored Brands version 4 APIs or clone existing campaigns in the advertising console. To learn more about key benefits of using new ad group based campaigns, see our [Benefits guide](guides/sponsored-brands/v4-features).

#### Awareness: Advantage of using Sponsored Brands version 4 campaigns

In October 2022, Sponsored Brands introduced version 4 campaigns, featuring multiple ad groups and ads. Prior to this launch, advertisers lacked the ability to create a Sponsored Brands advertising campaign aligned with their marketing strategy. The introduction of ad groups enhances flexibility, particular for managing campaigns across ad products, as both Sponsored Products and Sponsored Display supports similar campaigns structure.

These campaigns will allow you to leverage new features such as:

1. Increasing brand impression share for top-of-search placement with BRAND IMPRESSION SHARE goal-based campaigns.
2. Expanding product collection ads into slideshow ads by adding two to five lifestyle images.
3. Assigning individual landing page URLs to each ad and modifying existing landing page ads URLs.
4. Testing different creative and landing page combinations within ad groups.
5. Creating campaigns with vertical video optimized for mobile shoppers.
6. Showcasing videos with a brand store featuring three products or no products.
7. Accessing a wide range of new metrics including addToCartViews, newToBrandDetailPageViewViews, and newToBrandDetailPageViewClicks.

To learn more, view our [benefits guide](guides/sponsored-brands/v4-features).

#### Prevention of duplicate requests for V3 reporting APIs

We recently implemented a feature to prevent duplicate requests for [version 3 reporting APIs](guides/reporting/v3/overview). If you make a `createReport` request before a duplicative report request is complete, you will now receive an API response status code of 425, indicating that it’s too soon to make the new request. If this happens, you should wait for your existing request to complete before creating another identical `createReport` request. Learn more about [v3 reporting](guides/reporting/v3/overview) and [v3 report statuses](guides/reporting/v3/get-started#checking-report-status)

If your use case requires multiple identical reporting requests, we encourage you to explore alternative reporting options such as [Amazon Marketing Stream](guides/amazon-marketing-stream/overview), which pushes data to partner or advertiser-owned AWS destinations in near real-time, without needing to call the API repeatedly.

For more help or technical assistance, reach out to the Solutions Architect or Partner Manager assigned to you. If you don’t have an assigned contact, you can request additional support through the [API service desk](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2).

#### Sponsored Brands optimization rules (beta) API is now available for drive page visits goal

Regions: US, AE, AU, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, BE, BR, EG, PL, SA, SE, SG, TR, EG, SA

We launched a new API optimization rules (beta), to simplify the campaign management process for advertisers using Sponsored Brands’ drive page visits goal. This launch allows advertisers to set performance-based parameters that remove the guess work from selecting an exact bid for each target. This helps advertisers of any size and with any budget achieve their goals with the help of Amazon’s first-party data and machine learning capabilities.

For goal-based campaigns set-up to drive page visits, advertisers can apply a cost-per-click threshold. We will then utilize machine learning models to define the bids while adjusting base bids up and down with the goal of increasing clicks, all while adhering to the cost-based rules. While Amazon Ads cannot guarantee performance, if the campaign is not meeting the cost target over a 21-day period (excluding special days), we recommend adjusting cost target or budget to allow our systems meet the cost target and achieve more clicks.

Optimization rules are now available for Sponsored Brands through the Amazon Ads API. For full technical details, see our see our [optimization rules API](sponsored-brands/3-0/openapi/prod#tag/Optimization-rules) and [developer guide](guides/sponsored-brands/campaigns/managing-optimization-rules).

#### Sponsored Display for all businesses can now be optimized for lead generation in beta

Sponsored Display has launched lead generation ads into beta for US-based, self-service advertisers who do not sell on Amazon. Advertisers who sell products or services outside of the Amazon store can now grow their business by collecting customer leads instantly with lead generation ads. With this launch, advertisers can measure and optimize for leads to drive real business outcomes.

The customer’s purchase journey often involves extensive research across multiple options and/or trials. Advertisers seek incremental value from customer journey milestones including lead generation from digital ads. Previously, advertisers could only drive Sponsored Display ad traffic to their own websites to generate leads, but were unable to quantify impact on lead generations with Amazon Ads.  Advertisers can now optimize for lead generation outcomes. Using lead generation ads, advertisers can use self-service creative building tools to populate privacy-compliant lead generation forms. They can review lead metrics and access customer lead information stored in a privacy-safe database (Amazon Leads Manager).

Lead forms are now available for Sponsored Display through the Amazon Ads API. For full technical details, see our [lead forms API](sponsored-display/3-0/lead-forms) and our [developer guide](guides/sponsored-display/non-amazon-sellers/lead-forms).

#### Introducing the AMC Python collection for onboarding and getting started

This new resource, a Jupyter Notebook containing a collection of Python scripts, will help you streamline onboarding the Amazon Ads API and exploring AMC APIs. This includes endpoints for administration and workflow management with example request body and automations that make it easy to manage authorization and explore AMC APIs. You can find the collection files in the [GitHub repository for Amazon Marketing Cloud](https://github.com/amzn/ads-advanced-tools-docs)

To use the collection, download the Jupyter Notebook from GitHub, and follow the instructions listed to onboard and get started with AMC.

#### The NCS CPG Insights Stream data source has been added to AMC Paid Features

Availability: Only within the US AMC account marketplace

AMC dataset name: ncs\_cpg\_insights\_stream

Lookback: 12.5 month, 19 day delay, and refreshed weekly (on Fridays)

NCS CPG Insights Stream provides modeled offline transaction signals for all US households. Transaction signals (dollars and units) are aggregated at the weekly household level for specified product categories. Transaction signals are acquired from a diverse and balanced set of brick-and-mortar stores in the US covering over 3 million UPCs. The subscription allows AMC customers to measure the impact of your advertising on offline sales, understand cross-channel purchasing journeys, and discern audiences with different purchasing patterns online and offline.

#### AMC Flexible Shopping Insights data source updates

The conversions\_all data source has been updated to remove Amazon Ad Server (previously Sizmek Ad Server) traffic events from the ad-exposed, exposure\_type column logic. Additionally, there was an update to the conversions\_all data source to negate and resolve duplicative conversions\_ids for the `repeatSnSOrder` conversion event sub-type records.

#### AMC UI documentation can now be accessed through the Support Center

The content in the Support Center contains web-based topics to help you learn how to use and navigate the AMC UI. To access AMC UI documentation in the advertising console Support Center, click the **?** icon in the AMC console header, at the top-right of the page and select **Learn by Product > Amazon Marketing Cloud**. Note that you must have access to AMC UI to access AMC content in Support Center.

#### Keyword translations for Sponsored Products in bulksheets

Availability: Egypt, Saudi Arabia, and United Arab Emirates

When you create Sponsored Products campaigns using bulksheets, you can now add keywords in your preferred or native language. To use this new feature, you will input keywords in your preferred language in a new column labeled “Native language keyword,” and input your preferred language code in a new column labeled “Native language locale.” Leave the “Keyword text” field blank. When you upload your bulksheets file, your “native language” keyword will be automatically translated into English or the default language of your locale.

Before this launch, advertisers in some locales had to enter keywords in English, even if their preferred or native language was not English. With keyword translations, advertisers have the option to enter keywords in their native language instead.

[Learn more about automatic keyword translations](bulksheets/2-0/bulksheets-keyword-translations-guide)

#### Notice on new metrics for existing Amazon Marketing Stream datasets

Soon, [Amazon Marketing Stream](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-stream/overview) will start adding new metrics to existing datasets. Some of these metrics may be in closed beta, so it’s possible that you won’t have access to the metric values initially and will instead see zero values for the data. We recommend that you ensure your Stream implementation accommodates for new metrics to avoid any breaking changes whenever metrics are added.

We will publish a new release note, along with more guidance about the new metrics, when they are generally available to all customers.

#### The Partner Opportunities API now includes Amazon Ads account team recommendations

With this launch, partners gain visibility into what Amazon account managers suggest for their advertisers. Partners can then determine how to use these recommendations to help their advertisers achieve marketing objectives, including the ability to directly activate them through the new /apply API functionality. Bringing these recommendations into Partner Opportunities gives partners a single distribution channel to download actionable and relevant recommendations for the advertisers each agency or tech partner supports.

Recommendations and insights help advertisers focus more efficiently and spend time on work that is more likely to drive positive results, such as increasing sales or improving return on ad spend (ROAS). Partners have expressed interest in understanding the recommendations from Amazon Ads account teams so they can add context and help activate these recommendations for advertisers. Now, partners can leverage Partner Opportunities API to access these recommendations for each advertiser they manage, giving partners more visibility and flexibility to help advertisers work toward their goals.

This is available to all registered Amazon Ads partners using the Ads API. Partners must be registered in the [Amazon Ads Partner Network](https://advertising.amazon.com/partners/network) and setup for API access. For more information on API access for partners, see the [onboarding guide](https://advertising.amazon.com/API/docs/en-us/guides/onboarding/apply-for-access#to-apply-for-api-access-as-a-partner).

Within the existing [Partner Opportunities API](https://advertising.amazon.com/API/docs/en-us/guides/recommendations/partner-opportunities/overview), partners will see new opportunities for each of the new recommendations. These recommendations will be categorized using the `AMAZON_ACCOUNT_TEAM_RECOMMENDATIONS` Objective Type. Recommendations using this `AMAZON_ACCOUNT_TEAM_RECOMMENDATIONS` objective type can be used in the new `/apply` API endpoint to directly activate the recommendations for each specific advertiser. See all [Partner Opportunities API documentation](https://advertising.amazon.com/API/docs/en-us/partner-opportunities) for more information.

### May 2024

#### [Awareness; Coming Later] Better Reach Amazon Business shoppers with Sponsored Product campaigns

An Amazon Business (AB) bid adjustment for Sponsored Products is in development and will be available in Amazon Business Marketplaces - US, CA, MX, DE, IT, FR, ES, UK, IN, JP. In Q4 2024, advertisers using the Amazon Ads API or bulksheets will be able to apply a custom bid adjustment for placements on AB to their existing or new Sponsored Products campaigns. The new bid adjustment for AB can be set independently or in addition to placement bid adjustments for top-of-search (first page), rest-of-search and product-pages. With this new single AB bid adjustment, advertisers can boost their bids by up to 900% across AB placements. Regardless of their use of the AB bid adjustment, advertisers will have access to reporting that shows campaign performance on AB. Those advertisers who use the AB bid adjustment can gauge its impact on AB campaign performance through this reporting. See our [reference documentation](sponsored-products/3-0/openapi/prod) for more information.

#### Leverage new inventory with programmatic guaranteed deals for Video in France, Germany, Italy, Netherlands, Spain, Sweden, United Kingdom and India

Advertisers in France, Germany, Italy, Netherlands, Spain, Sweden,  United Kingdom and India now have the ability to work with publishers to [create programmatic guaranteed deals](dsp-create-update-deals), and use those deals on video line items. This capability enables advertisers to leverage premium video inventory from publishers through Amazon Publisher Direct (APD), Google, FreeWheel, and Magnite Streaming. When a line item is set to use a programmatic guaranteed deal, the interface is streamlined and simplified to reflect that the publisher will be managing the inventory.

#### Sponsored Display optimization rules (beta) API is now available for inflight campaigns

Regions: US, AE, AU, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, BE, BR, EG, PL, SA, SE, SG, TR, EG, SA

We've extended the Sponsored Display optimization rules (beta) API to inflight campaigns enabling advertisers to quickly turn any campaign into optimization rule based campaign while also allowing the feature to be turned on or off. This API helps to simplify the campaign management process for advertisers using Sponsored Display. This launch allows advertisers to set performance-based parameters that remove the guess work from selecting an exact bid for each target.

For campaigns optimized for reach, clicks, or conversions, advertisers can apply a cost-based rule like cost per thousand ad views (vCPM), cost per click (CPC) or cost per order (CPO). We will then utilize machine learning models to define the bids while adjusting base bids up and down with the goal of increasing outcomes, all while adhering to the cost-based rules.  Additionally, we may add targets while your campaign is running to try to stay at or below a cost per order value you have specified. While Amazon Ads cannot guarantee performance, if the campaign is not meeting the cost target over a 21-day period (excluding special days), we recommend adjusting cost target or budget to allow our systems to reach the right audiences.

Optimization rules are now available for Sponsored Display through the Amazon Ads API. For full technical details, see our [reference documentation](sponsored-display/3-0/openapi).

#### Amazon’s Developer Support Engineering team is moving from Jira to Zendesk

The Developer Support Engineering team will transition from Jira to Zendesk as our primary platform for managing customer support tickets starting May 13, 2024.

This change aims to provide a more direct and recognizable channel for assistance, streamlining the initial step of ticket creation. Moving forward, you can reach our support team directly through the new email address: `ads-api-support@amazon.com`

To ensure seamless communication, please add **ads-api-support@amazon.com** to your email's safe sender list. This action will help prevent our communications from being inadvertently marked as spam, allowing us to provide timely and effective support through Zendesk.

As with Jira, the API support team will offer support via the Help Center. Once you've established a login, the Help Center can be accessed at [amazon-ads-api.zendesk.com](https://amazon-ads-api.zendesk.com). For more information about support for Amazon Ads advanced tools, see our [Support overview](support/overview).

#### Get greater transparency for your Freevee campaigns with Content Genre and Content Rating reporting

For streaming TV ad-impressions delivered on Freevee, we have expanded our traffic reporting to add [content transparency reports in downloadable reports](guides/reporting/v3/report-types/audio-and-video). Advertisers can now see which Content Genre and Content Rating their ad impressions were delivered on. Advertisers can already use Content Genre, and Content Rating during planning and campaign settings. These reports provide advertisers a closed loop feedback with their campaign settings and media plans.

As advertisers transition their budgets from linear TV to digital media like streaming TV, content signals provide them greater visibility into their investments. Advertisers can now assess if their ads were contextually relevant and use the reporting data to plan their future media investments and adjust their campaign settings.

#### Awareness: Sponsored Display version 2 reporting endpoints will shut off on October 31, 2024

Following the release of the new [version 3 reporting endpoints](release-notes/index#sponsored-display-reports-on-amazon-ads-api-reporting-version-3-beta), we are announcing the deprecation of version 2 Sponsored Display reporting with a planned shutoff date of October 31, 2024. This includes the [POST /sd/{recordType}/report](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport) endpoint.

Going forward, you can access all version 2 Sponsored Display report types using the [version 3 reporting endpoints](offline-report-prod-3p). For more details on how to upgrade to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3) and [Frequently asked questions](guides/reporting/v3/faq).

If you have feedback, use the Amazon Ads API [Support page](support/overviewview) to submit a ticket with the subject title “SD reports v2 deprecation” by August 31, 2024.

#### Sponsored Brands forecasts now provide support for clicks

Sponsored Brands campaign performance forecasting provides metric forecasts to advertisers during the campaign creation process.

Currently we have impression metrics for detail page view campaigns which gives the estimated weekly and monthly impression based on targeting, bid, and budget settings. Now we are adding support for click forecasting. To use click forecasting, you will need to add `creativeAsins` in the ad group setting to help us predict the clicks for campaign. Then the response will include forecasted clicks.

For more information, see the [API reference](sponsored-brands/3-0/openapi/prod#tag/Forecasts).

#### Amazon Data Firehose is available as a destination for Amazon Marketing Stream

Amazon Marketing Stream is expanding its data delivery options by introducing Amazon Data Firehose as a new destination. Amazon Data Firehose is a fully managed service for delivering real-time streaming data to destinations such as Amazon Simple Storage Service (Amazon S3), Redshift, OpenSearch Service, OpenSearch Serverless, Splunk, and any custom HTTP endpoint or HTTP endpoints owned by supported third-party service providers. With Amazon Data Firehose, customers can subscribe to Amazon Marketing Stream datasets and have them delivered to a location of their choice—including the options mentioned here. Without Firehose, Stream datasets can only be delivered to Amazon SQS, so Firehose provides more flexibility for customers who prefer a different data output.

By integrating Amazon Data Firehose as a delivery option, customers can streamline their data ingestion process and minimize operational costs. As a fully managed service, it automatically scales to accommodate data volume, and eliminates the need for ongoing administration. Additionally, Firehose can batch, compress, and encrypt data before delivery—reducing storage consumption at the destination and enhancing security.

To learn more, view the [Getting started guide](guides/amazon-marketing-stream/onboarding/firehose/get-started) .

### April 2024

#### Coming soon: Automatic keyword translations for Sponsored Products in bulksheets

Regions: Egypt & Saudi Arabia

Soon, customers in Egypt and Saudi Arabia will be able to add keywords in their preferred or native language. The keywords will then be automatically translated into English when the file is uploaded. When this new feature launches, you will see two new columns labeled **Native language keyword** and **Native language locale**, where you can input keywords in your native language along with the language code. When you upload the bulksheets file, the translated keywords will automatically populate the current “Keyword text” field. You can see the translated keywords if you download a file later.

We’ll provide more details and documentation for using this feature once it launches.

#### Keyword Groups beta release

Regions: US

Keyword Groups is a new targeting control for Amazon Ads Sponsored Products keyword-based campaigns that enables advertisers to reach relevant audiences through a collection of keywords.

Once a Keyword Group specification is created, the performance of Keyword Groups will be available in the search terms report. Keyword Groups improves campaign performance by dynamically updating the keywords within a group through the campaign lifecycle and eliminates the need for advertisers to constantly curate new keywords. Additionally, Keyword Groups can be used alongside keywords within the same ad group.

Keyword Groups can be accessed through a new Keyword Groups recommendations API included in this release. Furthermore, existing operations for bid suggestions and viewing/updating have been updated to accommodate Keyword Groups.

APIs:

* POST /sp/targeting/recommendations/keywordGroups: Use this API operation to get recommended Keyword Groups for a list of ASINs. Please use Keyword Groups recommended by this API for your ASINs to benefit from the feature only.
* POST /sp/targets: This is an existing operation that has been extended to support creation of Keyword Groups.
* POST /sp/targets/list: This existing operation will return all Keyword Groups for an ad group
* POST /sp/targets/bid/recommendations : This existing API endpoint now gives bid recommendations for Keyword Groups with the following options. See [theme based bid recommendations API documentation](sponsored-products/3-0/openapi/prod#tag/Theme-based-bid-recommendations/operation/GetThemeBasedBidRecommendationForAdGroup_v1) for more information.

See our [documentation](guides/sponsored-products/keyword-group-targeting) for more information.

#### Deprecation announcement: Reminder that AMC instance-level APIs will shut off on August 1, 2024

As we recently announced [AMC APIs being available on Amazon Ads](release-notes/index#amazon-marketing-cloud-apis-are-now-part-of-the-amazon-ads-api), we recommend users currently on the AMC instance-level APIs migrate to the upgraded APIs as soon as possible to take advantage of the enhanced security, usability, and performance of new APIs.

> [WARNING] We will stop supporting the instance-level APIs from **June 01, 2024** and retire those on **August 01, 2024**.

After the instance-level APIs retire, calling any of the endpoints will return a 404 error. Migrating to the new APIs at the earliest can help prevent disruptions to your existing business operations. Visit the [AMC migration hub](guides/amazon-marketing-cloud/amc-migration-hub/migration) to learn more.

In addition, **new AMC instances created after March 04, 2024 will not be compatible with the instance-level APIs**. You need to build to Amazon Ads APIs before this date to make sure your API workflows can be applied to your new instances.

#### Preview: Sponsored Brands will be introducing two new campaign goals, “Acquire New Customers” and “Drive Ad Views”

These new capabilities are in preview only. We will update our release notes and API documentation once this functionality is made available. Advertisers will soon have access to two new goals, “Acquire New Customers" and “Drive Ad Views”, each with a unique set of campaign controls that help achieve that goal. The “Acquire New Customers" goal can be measured using the metric [newToBrandPurchases](guides/reporting/v3/columns#newToBrandPurchases), helping advertisers drive sales from customers who have not purchased a brand product within a 1-year lookback. The “Drive Ad Views" goal can be measured using the metric [viewableImpressions](guides/reporting/v3/columns#viewableimpressions), helping advertisers reach and engage audiences across shopping and entertainment contexts.

Along with the new goals mentioned, advertisers will soon have the option to input specific cost controls to help achieve those goals within a given cost threshold. As we introduce these changes, we will also update and improve our insights and recommendations to help further maximize campaign goals.

For full technical details, see our updated [Sponsored Brands API reference documentation](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredBrandsCampaigns). Under the create campaign resource, we have expanded the number of goal options, including `ACQUIRE_NEW_CUSTOMERS` and `AD_VIEWS`.

#### Deprecation announcement: Final reminder for Sponsored Brands version 3 campaign management, draft, and media APIs shut off

The shutoff date for Sponsored Brands version 3 campaign management APIs will be May 30th, 2024. As a reminder, Sponsored Brands version 3 draft and media APIs were deprecated on January 31, 2024. Please migrate to the new version 4 campaign management endpoints as documented on our [deprecations page](release-notes/deprecations) and in our [migration guide](reference/migration-guides/sb-v3-v4). As a part of this deprecation, Sponsored Brands draft endpoints have reached their end of life and will not be replaced with new endpoints.

The following endpoints and all associated HTTP operations are included in this deprecation:

* /sb/campaigns/
* /sb/adGroups/
* /sb/adGroups/{adGroupId}
* /sb/campaigns/{campaignId}
* /sb/drafts/campaigns
* /sb/drafts/campaigns/{draftCampaignId}
* /sb/drafts/campaigns/submit
* /media/upload

If you would like to share feedback, please use the Amazon Ads API [Service desk](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2) to open a ticket with subject title "SB v3 Campaign CRUD Deprecation" by April 30th, 2024.

#### User invitation APIs are launching for Amazon Ads

Regions: All

Amazon Ads API customers can now invite users to advertising accounts entirely through API integration. They can perform all invitation-related operations throughout the invitation lifecycle, including creating, redeeming, revoking, listing, and resending invitations.

Previously, all user management functionality was only available through the advertising console. This launch marks the first milestone in a series of authorization API launches, which will allow customers to execute invitation and user management-related actions entirely via the Amazon Ads API, without the need to visit the advertising console. Customers can now use the API to create and manage user invitations for sponsored ads accounts, including vendors, authors, merch-on-demand, and FireTV. This new user invitation functionality is also available for manager accounts and Amazon DSP. By integrating with these APIs, third-party partners can also build user invitation workflows on their own advertising platforms.

New APIs:

* POST `/user-invitations`: Create a batch of invitations. Given an advertising account identifier, a user’s email address, and a role or set of permissions, invite one or many (up to 50 in batch) users to the advertising account. Invitations expire after 2 weeks.
* PUT `/user-invitations`: Update a batch of invitations. Given a list of one or many (up to 50) invitation identifiers, update these invitations to revoke them, or resend them to refresh the expiration window.
* PUT `/user-invitations/redeem`: Redeem an invitation. Given an invitation identifier, redeem the invitation, and be added to the account upon redemption. This operation requires that the terms agreement be shown to the user and accepted before redemption.
* POST `/user-invitations/list`: List the pending invitations for a given account.
* GET `/user-invitations/{invitationId}`: Get details about a given invitation, such as role, permissions, email, expiration and account identifier. This API will also return a list of termsTypes, which indicates the type of terms (MARKETING\_CLOUD, ADVERTISING, PARTNER\_NETWORK) that must be accepted before redemption, using the POST `/termsTokens` API.

See [our reference documentation](user-invitations) for more information.

In addition, we have updated the existing POST `/termsTokens` API.

* POST `/termsTokens`: Create a terms token for the customer to accept Amazon Ads terms. With this update, you can now specify the type of terms to be accepted (MARKETING\_CLOUD, ADVERTISING, PARTNER\_NETWORK) depending on the type of account the user is invited to. For more information, see the [API specification](account-management#tag/Terms-Token/operation/CreateTermsToken).

#### Ad Library API is now available

Region: EU

The Ad Library API is now available. The Ad Library API contains data about ads and affiliate marketing content displayed on the Amazon EU store (Germany, France, Italy, Spain, Sweden, Poland, Belgium, Netherlands) and is available globally. Examples of data include advertiser or creator name, impressions by country, and the name of the product, service, or brand featured.

To get started, view the [Ad Library overview](guides/ad-library/overview).

#### Registration is now open for the Amazon Ads Developer Summit 2024

This summit is designed to educate integrators who are at the beginning and intermediate stages of their journey with Amazon Ads APIs, Amazon Marketing Stream, and Amazon Marketing Cloud (AMC). You'll learn about best practices and hear ideas for capabilities you can build. We'll deep dive into Amazon APIs and reporting tools and share advanced use cases. Hear from Amazon product leaders about topics that include:

- Enhancing reporting and optimization capabilities
- Building full-funnel solutions for a variety of advertising customers
- Media planning and programmatic capabilities
- And much more!

There will also be opportunities to get answers to your technical questions and provide product feedback. The summit will be held on May 16-17, 2024 in NYC.

[Learn more and register](https://events.bizzabo.com/584663/home).

### March 2024

#### Sponsored Display optimization rules (beta) API has expanded to new regions

Regions: BE, BR, EG, PL, SA, SE, SG, TR, EG, KSA

We've expanded Sponsored Display optimization rules (beta) API to new regions. This API helps to simplify the campaign management process for advertisers using Sponsored Display. This launch allows advertisers to set performance-based parameters that remove the guess work from selecting an exact bid for each target. This helps advertisers of any size and with any budget achieve their goals with the help of Amazon’s first-party data and machine learning capabilities. Before this launch, advertisers needed to translate their desired goals into efficient bids and targets across different cost types. In reality, this turns out to be the biggest issue driving lower scale and campaign effectiveness. By translating goals into bids using machine learning with optimization rules (beta), Amazon can help advertisers achieve better results through continuous optimization. In the future this will be extended to targeting optimizations to further maximize goals.

For campaigns optimized for reach or click, advertisers can apply a cost-based rule like cost per thousand ad views (vCPM) and cost per click (CPC). We will then utilize machine learning models to define the bids while adjusting base bids up and down with the goal of increasing outcomes, all while adhering to the cost-based rules. While Amazon Ads cannot guarantee performance, if the campaign is not meeting the cost target over a 21-day period (excluding special days), we recommend adding additional targets to allow our systems to reach the right audiences.

Optimization rules are now available for Sponsored Display through the Amazon Ads API which was previously only available in AE, AU, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK and US regions. For full technical details, see our [reference documentation](sponsored-display/3-0/openapi#tag/Optimization-Rules-(beta\)).

#### Leverage new inventory with programmatic guaranteed deals for Video in Japan, Saudi Arabia, Turkey, and United Arab Emirates

Advertisers in Japan, Saudi Arabia, Turkey, and United Arab Emirates now have the ability to work with publishers to create programmatic guaranteed deals, and use those deals on video line items. This capability enables advertisers to leverage premium, video inventory from publishers through Amazon Publisher Direct (APD), Google, FreeWheel, and Magnite Streaming. When a line item is set to use a programmatic guaranteed deal, the interface is streamlined and simplified to reflect that the publisher will be managing the inventory.

Programmatic guaranteed deals enable direct-buys between advertisers and publishers, while still tapping programmatic model benefits. By using programmatic guaranteed deals, advertisers can access additional video inventory, help reduce manual campaign activation and management processes, and work with publishers on contextual targeting. This can help advertisers save time, become more efficient, meet their delivery goals, and help customers discover new brands.

See [our reference documentation](dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals) for more information.

#### New Amazon Sponsored TV documentation

We’ve released new overview and tutorial content for Sponsored TV. This content helps you create a Sponsored TV campaign and understand its structure and how to create and preview video creatives.

Find all new Sponsored TV content under [Sponsored TV > Overview](guides/sponsored-tv/overview).

Also see our updated [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman), where we’ve added new Sponsored TV request templates.

#### Subscribe and Save retail events are now available in the AMC conversions_all paid dataset

This includes publication of a new column, `sns_subscription_id`, within the [conversions_all](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-cloud/datasources/conversions_all_paid) schema. The sns\_subscription\_id is the identifier for the Subscribe and Save (SnS) enrollment and can be used as a join\_key for subscription and fulfillment events within the SnS series. Specific to the **conversions_all** dataset the SnS events are associated with the conversion event subtypes listed below, and are eligible for data decoration within the `sns_subscription_id` column:

* `snsSubscription`
* `firstSnSOrder`
* `repeatSnSOrder`

For all other conversion event datasets, the SnS events are associated with the conversion event subtypes `snsSubscription` and `order`, and will not include reference to the `sns_subscription_id` data attribute.

The above applies to all of the equivalent audience tables. SnS retail events are resolved and joined to advertising data feeds with the `user_id_type = adUserId`. SnS retail events have an initial publication (backfill) date within AMC of **June 2023**; there are no SnS retail events within AMC prior to this date.

For more information, refer to the [Instructional Query (IQ): Introduction to Subscribe and Save (SnS) repeat purchases (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/v2/instructional-queries/bf4b57a8aecd12c19f5928b73f709fa0fb5f3ea5414dc127cdb68f13f11c1e32).

#### Amazon Prime Video supply sources available in AMC

These premium video ad placements are represented via the enumerated DSP supply sources listed below:

* Sports on Prime (including TNF and NBA): `supply_source = ‘AMAZON LIVE STREAM’` (supply\_source\_id: 610)
* Prime Video (on-demand content): `supply_source = ‘Prime Video ads’` (supply\_source\_id: 623)

Both of these DSP supply sources are eligible to produce events within the DSP traffic tables, as well as the joined traffic and conversion event feeds below:

* dsp\_impressions
* dsp\_views
* dsp\_clicks
* dsp\_impressions\_by\_matched\_segments
* dsp\_impressions\_by\_user\_segments
* amazon\_attributed\_events\_by\_conversion\_time
* amazon\_attributed\_events\_by\_traffic\_time

#### Sponsored TV (ST) signals are now available in AMC

AMC customers now have access to Sponsored TV (ST) signals if the corresponding Sponsored Ads entities are mapped to their AMC instances. The signals can be identified by applying the filter `ad_product_type = 'sponsored_television' `in the following tables:

* amazon\_attributed\_events\_by\_traffic_time
* amazon\_attributed\_events\_by\_conversion\_time
* sponsored\_ads\_traffic

#### Persona builder API now generally available for Amazon DSP advertisers

Regions: US, CA, MX, BR, UK, DE, FR, IN, ES, IT, AE, SA, NL, SE, TR, JP, AU

In January, we [announced the launch](release-notes/index#use-persona-builder-beta-to-create-and-get-insights-on-your-personas) of the persona builder API in beta. This feature is now generally available for Amazon DSP users.

The persona builder API takes an audience expression, representing a persona, as input. The API returns aggregated insights for that persona. An advertiser can call the API to save an audience expression to Amazon DSP as a “combined audience.”

To get started, view the [API reference](persona-builder).

#### Amazon DSP reports added to version 3 reporting

We are excited to launch Amazon DSP reports on the latest version (version 3) of the Amazon Ads API as Open Beta. This release provides agencies, brands, and service providers with Amazon DSP reporting in addition to the existing Sponsored Ads reports. The new reports adhere to a common request and response schema we introduced with V3 reports, and provide customers with the same benefits currently available for Sponsored Ads reports: the ability to select date ranges, standardized metric names, and better reports availability over version 2.

This release launches the following reports on V3 reporting for Amazon DSP: Campaign, Inventory, Geography, Technology, Audiences, Off-Amazon, Product, and Conversion Source, in addition to the already migrated Campaigns and Inventory reports. With this release, customers will find better consistency between Amazon DSP reports and those for other ad products.

The new Amazon DSP reports come with standardized metric names allowing consistent naming across all ad product reports. This will enable customers to easily compare metrics across ad product reports. Another major change with this release is using a 14-day attribution window for all applicable Amazon DSP metrics. We dropped deprecated metrics and attribution suffixes in metric names from the reports, making it easier to read and analyze reporting information.

For full technical details, please see our [updated documentation](guides/reporting/v3/overview) in the advanced tools center.

#### Deprecation announcement: Version 1 and 2 snapshots APIs will shut off on October 15, 2024

All snapshots endpoints are now deprecated, with a target shutoff date of October 15, 2024. Please migrate to the [exports API](exports) as indicated on our [Deprecations page](release-notes/deprecations).

The following endpoints and all associated HTTP operations are included in this deprecation:

- /{recordType}/snapshot
- /snapshots/{snapshotId}
- /v2/sp/{recordType}/snapshot
- /v2/sp/snapshots/{snapshotId}
- /v2/sp/snapshots/{snapshotId}/download
- /v2/hsa/{recordType}/snapshot
- /v2/hsa/snapshots/{snapshotId}
- /v2/hsa/snapshots/{snapshotId}/download
- /sd/{recordType}/snapshot
- /sd/snapshots/{snapshotId}
- /sd/snapshots/{snapshotId}/download

For more details on integrating with exports, see our [Migration guide](reference/migration-guides/snapshots-exports).

### February 2024

#### Deprecation announcement: Amazon DSP Pixel APIs will be shut off on August 1, 2024

Regions: All

In 2023, we deprecated Pixel functionality within Amazon DSP in a phased manner to provide enough time for advertisers to migrate over to our durable alternative, [Amazon Advertising tag](https://advertising.amazon.com/help/GLZ54GXQW773A6MG).

As we approach Chrome third-party cookie deprecation in the second half of 2024, we plan to shut off legacy Pixel APIs in Amazon DSP by August 1, 2024. We recommend all advertisers currently using Pixels to migrate to Amazon Ad Tag at the earliest. View the [deprecation details](release-notes/deprecations#amazon-ads-api-deprecations) for more information.

#### Updates to the API compatibility and versioning policy

We have updated the Amazon Ads API compatibility and versioning policy to clarify our approach to enumerated fields and values. For details, see [Compatiblity and versioning](reference/concepts/compatibility-versioning-policy).

#### Traffic events API now available in EU marketplaces

Region: EU

We have launched the new traffic events API. This is a data clean room-based service that helps advertisers access event-level data (such as clicks, views and impressions) for eligible EU campaigns. This data can be queried using standard SQL syntax within the body of the traffic events API call. The traffic events API will return results if the advertiser account ID you include in the API call is associated with any eligible campaigns. For full technical details, please visit the [traffic events API overview](guides/traffic-events/overview).

#### Sponsored Products budget recommendations and missed opportunities via Amazon Marketing Stream GA

Regions: US, CA, MX, UK, DE, ES, IT, FR, BE, PL, NL, SE, TR, AE, SA, AU, JP, IN, SG, BR

Sponsored Products budget recommendations and missed opportunities insights are now available in [Amazon Marketing Stream](https://advertising.amazon.com/solutions/products/amazon-marketing-stream?ref_=a20m_us_wn_ams_030723_p_ams). Until today, Sponsored Products budget recommendations and missed opportunities insights were only available to advertisers via the Amazon Ads API. We are excited to now support this insight via Amazon Marketing Stream. This provides an unmodified view of budget recommendations and missed opportunities, which include the additional clicks, impressions and sales from the past 7 days that the campaign could have received had it stayed in budget for the hours it was out of budget.

Sponsored Products budget recommendations and missed opportunities via Amazon Marketing Stream addresses two use cases: 1) delivery of recommendations in near real-time when a campaign goes out of budget, and 2) delivery of recommendations for all campaigns owned by a subscribed advertiser with no additional API calls beyond the initial subscription.

Amazon Marketing Stream is available via the Amazon Ads API. When you are ready to get started, visit our [Amazon Marketing Stream onboarding guide](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-stream/overview) that will walk through the process of getting API access, integrating with AWS, and subscribing to campaign datasets using the Amazon Marketing Stream API.

#### Improve your Brand Store’s performance with quality metrics and recommendations

Regions: All

Brand Store quality metrics and recommendations are now available with the Amazon Ads API. Now you can access your Store’s average dwell time, how you compared to peer groups on Amazon, your Store quality rating, and a list of recommended actions you can take to improve your Store’s quality and performance. A high rating means that you’ve taken actions that Amazon believes will increase the duration that shoppers spend on your Store (dwell time). We use your Store rating to recommend specific actions that may help improve your Store performance and dwell time.

Learn more about [Brand Store insight metrics](guides/brand-store/insight-metrics).

#### Deprecation announcement: Sponsored Brands legacy version of product collection ads APIs will be shut off on November 30, 2024

As we recently announced new version of [Sponsored Brands product collection ads APIs on Amazon ads](release-notes/index#sponsored-brands-product-collection-introduces-slide-show-creatives-on-the-amazon-ads-api), we recommend advertisers and partners to start migrating to the [/sb/v4/ads/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsExtendedProductCollectionAds) and [/sb/ads/creatives/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateExtendedProductCollectionCreative) APIs as soon as possible to take advantage of creating the product collection slide show for product collection ads, and edit landing pages of product collection ads.

> [NOTE] The legacy `/sb/v4/ads/productCollection` and `/sb/ads/creatives/productCollection` will not enforce mandatory custom image restriction.

The following endpoints and all associated HTTP operations are included in this deprecation:

* [/sb/v4/ads/productCollection](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsProductCollectionAds)
* [/sb/ads/creatives/productCollection](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateProductCollectionCreative)

If you would like to share feedback, please use the [Amazon Ads API Service Desk](support/overview#amazon-ads-api) to open a ticket with subject title "Product collection Ads API Deprecation" by April 30, 2024.

#### Awareness: Sponsored Brands product collection ads requirement changes

Sponsored Brands is making changes to requirements for the product collection ad format. Beginning January 31, 2024, new Sponsored Brands campaigns using the product collection ad format created using the API or Amazon advertising console will require a custom image that meets policy. We will begin enforcing this requirement for all product collection ads on May 31st, 2024. We recommend migrating to the new [/sb/v4/ads/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsExtendedProductCollectionAds) endpoint which requires a custom image as part of ad creation. The previous [/sb/v4/ads/productCollection](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsProductCollectionAds) and [/sb/ads/creatives/productCollection](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateProductCollectionCreative) will be deprecated, with a target shutoff date of November 30, 2024. The /sb/v4/ads/productCollectionExtended also allows advertisers and partners to create a slide show for product collection ads using collection of custom images and edit their ad landing pages.

In addition, existing product collection ads that don’t contain a custom image will stop serving after May 31, 2024. To remediate, we highly recommend that you either use the Amazon ad console to update your product collection campaigns with custom images or use the [/sb/ads/creatives/productCollection](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateProductCollectionCreative) or [/sb/ads/creatives/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateExtendedProductCollectionCreative) endpoints to update your creatives.

> [NOTE] The creative edit APIs does not support updating creative for legacy (version 3) campaigns. To add a custom image to a legacy product collection campaign, please use the advertising console. You can also create a new campaign using the API.

Learn more about product collection and production collection extended ads in the [FAQs](guides/sponsored-brands/ads/product-collection#frequently-asked-questions). If you would like to share feedback, please use the [Amazon Ads API Service Desk](support/overview#amazon-ads-api) to open a ticket with subject title "ASIN only Deprecation" by March 30, 2024.

#### Sponsored Brands introduces landing page edits on the Amazon Ads API

Regions: US, CA, MX, BR, JP, SG, AU

Sponsored Brands introducing a landing page edit for Product Collection, Store Spotlight and Brand Video Ads. Advertisers and Partners are now able to change their ads landing page without recreating a new campaign or ad group. Now, each ad can have individual landing pages within same ad group. You can now update your ad group based campaigns landing pages without recreating new ad group or campaign. For editing a landing page, advertisers and partners can use `/sb/ads/creatives/productCollectionExtended` for [Product Collection ads](guides/sponsored-brands/ads/product-collection#change-to-a-new-landing-page-from-existing-simple-landing-page), `/sb/ads/creatives/storeSpotlight` for [Store Spotlight ads](guides/sponsored-brands/ads/store-spotlight#change-to-a-new-store-spotlight-landing-page-from-existing-store-spotlight-landing-page), and `/sb/ads/creatives/brandVideo` for [Brand Video ads](guides/sponsored-brands/ads/video#change-to-a-new-store-landing-page-from-existing-brand-video-ad).

For full technical details, please see the updated documentation on [/sb/ads/creatives/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateExtendedProductCollectionCreative),  [/sb/ads/creatives/storeSpotlight](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateStoreSpotlightCreative) and [/sb/ads/creatives/brandVideo](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateBrandVideoCreative).

#### Sponsored Brands Product Collection introduces slide show creatives on the Amazon Ads API

Regions: All

Sponsored Brands introducing a new creative Slideshow ads, that will appear at the top of search results page. Slideshow ads are an extension of the existing lifestyle creative, with a carousel of 2-5 lifestyle images that will auto-rotate. Shopper can engage with the slideshow by pausing it at any point in time or switching between different images by making forward or backward swipes.

Advertisers can create the new slideshow using `POST /sb/v4/ads/productCollectionExtended` where advertiser can provide collection on lifestyle images representing the brand. You can also edit your existing Product Collection creatives using `POST /sb/ads/creatives/productCollectionExtended` where advertiser can add collection of lifestyle images to convert those ads into slideshow. For more details, see [Slideshow ads creative](https://advertising.amazon.com/resources/whats-new/slideshow-ads-creative-for-sponsored-brands/?ref_=a20m_us_search_title).

For full technical details, please read [our documentation](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateExtendedProductCollectionCreative).

#### Support for ads in the exports API

Regions: All

We have added support for ads to the exports API. Previously, the exports API was only available for campaigns, ad groups, and targets.

An ad represents the smallest unit of an advertising campaign, including what a customer sees and how they interact with the ad. As part of this release, we have aligned on a common model for all ad types across sponsored ads campaigns. To learn more about the common ad model, see [Ads](reference/common-models/ads).

With this release, exports now include all functionality provided in snapshots. For more details, view the [Migration guide](reference/migration-guides/snapshots-exports).

To get started with target exports, see:

* [Exports tutorial](guides/exports/get-started)
* [Exports API reference](exports)

#### Sponsored Brands video introduces vertical video creatives on the advertising console and Amazon Ads API

Regions: All

Advertisers can now launch vertical video campaigns with a Store as the landing page. Vertical video is available under “Drive page visit” goal, enabling advertisers to highlight a collection of one, two, or three advertised products. Under the “Grow brand impression share” goal, advertisers can launch a video campaign without, or with one, or two, or three advertised products. Shoppers clicking on an advertised product will visit the product’s detail page; shoppers clicking on any other ad element will visit the brands Store on Amazon. Shoppers will be able to discover the brands they love today - and learn about the brands they will crave tomorrow - with vertical video creatives. Shoppers can explore brands and visit the Amazon Store to learn more about a brand’s ethos, or explore advertised products by clicking on any of the displayed products. Advertisers will be able to re-use their existing vertical videos from social media, and find new creative ad formats to share their brand story.

For full technical details, please see the updated documentation on [Sponsored Brands video](guides/sponsored-brands/ads/video).

#### Amazon DSP Public API Update to Include New Creative-Line Item Statuses

Regions: US

We have launched three new fields available in the public API for “line item creative associations,” which provide the creative line item association status. These fields are `state`, `associationModerationStatus`, and `associationModerationReasons`, and they can be used to determine which line items the creative is approved or not approved to run on based on policies.

As part of the line item moderation capability, these fields help you determine if a creative is approved to run based on moderation policies.

For full technical details, please read [our documentation](dsp-campaigns#tag/LineItemCreativeAssociation).

### January 2024

#### Deprecation announcement: AMC instance-level APIs will shut off on August 1, 2024

As we recently announced [AMC APIs being available on Amazon Ads](release-notes/index#amazon-marketing-cloud-apis-are-now-part-of-the-amazon-ads-api), we recommend users currently on the AMC instance-level APIs migrate to the upgraded APIs as soon as possible to take advantage of the enhanced security, usability, and performance of new APIs.

> [WARNING] We will stop supporting the instance-level APIs from June 01, 2024 and retire those on August 01, 2024.

After the instance-level APIs retire, calling any of the endpoints will return a 404 error. Migrating to the new APIs at the earliest can help prevent disruptions to your existing business operations. Visit the [AMC migration hub](guides/amazon-marketing-cloud/amc-migration-hub/migration) to learn more.

In addition, **new AMC instances created after March 04, 2024 will not be compatible with the instance-level APIs**. You need to build to Amazon Ads APIs before this date to make sure your API workflows can be applied to your new instances.

#### Advertiser Data Upload APIs are now generally available on Amazon Ads

Advertiser Data Upload APIs (ADU 2.0) allows users of AMC to bring their own first-party datasets into AMC for custom and advanced analysis use-cases, such as audience overlap reports, custom attribution, frequency analysis, comparison of off-Amazon behaviors to Amazon’s advertising events, or analysis by product/ASIN. For more details, refer to [Advertiser data upload](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview).

#### Troubleshoot your Sponsored Products campaigns for ad-serving issues with the campaign diagnostics API

Advertisers and partners can now troubleshoot their Sponsored Products campaigns for any product eligibility, ad policy, or featured offer competitiveness issues by calling the campaign diagnostics API. This API allows partners to programmatically fetch an aggregated list of issues across supported issue types (product eligibility, ad policy, featured offer competitiveness) for a single Sponsored Products campaign or list of Sponsored Products campaigns through a single API call.

Previously, API users had to call separate APIs for product eligibility and ad policy troubleshooting, each taking different entity type (SKU, AdID respectively), then consolidate the results to get an overview of issues impacting their Sponsored Products campaigns. The campaign diagnostics API removes the discovery and cognitive load associated with troubleshooting a Sponsored Products campaign by presenting an aggregated view along with an impact assessment of the issue across the campaign.

For more details, see the [technical specifications for the campaign diagnostics API](campaign-diagnostics).

#### Billing APIs now support credit card and direct debit payment methods

We’ve expanded the Amazon Ads payment registration APIs to support credit card and direct debit. Third-party clients can now integrate with the Amazon Ads billing APIs to register credit card and direct debit payment methods, as well as view metadata related to such registrations.

For more details, see the [technical specifications for the billing APIs](billing).

#### Announcing the Amazon Ads API integration dashboard

Regions: All

Amazon Ads is introducing a new, personalized experience for developers using the Amazon Ads API. With this release, developers can now log in to the advanced tools center to view key metrics and trends related to their Amazon Ads API integration.

The logged-in experience displays information about your Login with Amazon application and includes an interactive dashboard showcasing API activity. The integration dashboard can help you monitor your integration health, explore trends, and identify inefficiencies in your call patterns. Available breakdowns include calls by response code, product, marketplace, API resource, and hourly call volume.

Access the new experience directly from the Amazon Ads advanced tools center by signing in using your developer.amazon.com account credentials. Once logged in, you can view the Login with Amazon applications associated with your Amazon Developer account. Select an application that has access to the Amazon Ads API to view the integration dashboard.

To learn more, see [Integration dashboard](guides/get-started/api-integration-dashboard).

#### Search term reports for Sponsored Brands in bulksheets

We [recently announced](release-notes/index#search-term-reports-for-sponsored-products-in-bulksheets) that Sponsored Products search term report data was available in bulksheets. Now, we’ve expanded to support Sponsored Brands search term reports. This report will be in a new tab and will include much of the same information you would find in the advertising console report, including the customer search terms that were used to discover your ads—-but in bulksheets, you will also have the entity IDs available, so you can quickly make adjustments if needed.

You can include a search term report in bulksheets if you go to the bulk operations main page, create a custom spreadsheet, and check the box labeled Sponsored Brands search term report in Step 1.

Learn more about [search term reports in bulksheets](bulksheets/2-0/bulksheets-search-term-report).

#### Use persona builder (beta) to create and get insights on your personas

Regions: US, CA, MX, BR, UK, DE, FR, IN, ES, IT, AE, SA, NL, SE, TR, JP, AU

You can now better understand your audiences and create your own custom brand personas with persona builder. Using Amazon insights, you can discover and validate audience shopping and streaming behaviors. Then, seamlessly save and activate those same audiences through your Amazon Ads campaigns.

Persona builder enables you to use Amazon’s first-party insights to learn more about your audiences, such as their interests and relevant life events. You can create your own custom brand personas by combining existing audiences into a single persona. For example, if you are releasing a premium headphone with fitness branding, you can create a persona by combining the Amazon “interested in technology,” “in market for headphones,” and “in market for fitness clothing” segments. You can reach your personas on Amazon and beyond by saving them to Amazon DSP.

Persona builder API takes an audience expression, representing a persona, as input. The API returns aggregated insights for that persona. An advertiser can call the API to save an audience expression to Amazon DSP as a “combined audience.”

The following are the key API endpoints:

- **POST /insights/bandedSize:** Get the size range for the input audience;
- **POST /insights/demographics:** Get aggregated demographic insights for the input audience;
- **POST /insights/topOverlappingAudiences:** Get top audiences overlapping with the input audience;
- **POST /insights/topCategoriesPurchased:** Get top retail categories purchased by the input audience;
- **POST /insights/primeVideo:** Get insights on top content streamed on Prime Video by the input audience;
- **POST dsp/audiences/combinedAudiences:** Create a combined audience based on the input audience expression.

#### Ads account registration and account details endpoints are now available globally through the Amazon Ads API in open beta

Technology partners now have globalized registration and account APIs, available in open beta, to offer advertisers an integrated Amazon Ads onboarding experience from within other platforms. They can also get all accounts of a user with a single, global API call. Advertisers that are eligible for multiple countries now get one advertising account that spans all countries through a new globalized account ID. Each country does have its own ID specific to each advertiser, which you’ll need to make individual requests for most other APIs.

Previously, ads account registration was only available through Amazon's UI. Once an account was created, advertisers could authorize them to technology partners in the context of a single country, where partner APIs needed to call different endpoints in different regions to compile the countries. Now, they can register a single account globally and get all countries and profile IDs with a single request.

Available APIs:

- **Accept Terms and Conditions:** All registrations must be accompanied by acceptance of the Amazon Ads Agreement. Anyone registering must be presented with the terms and acknowledge agreement. To register, partners must call to get a terms token and activate it by agreeing to terms. Partners can call to check the activation status of the token.
- **POST /adsAccounts:** Register an account. Given a Seller Central account ID, Vendor Central ID, or business details, tech partners can request an ads account. An account ID will immediately be created and returned synchronously, while the individual countries are created asynchronously.
- **GET /adsAccounts/{advertisingAccountId}:** Get details for an account. For registration status, Amazon will return a status of created, pending, partially created or disabled. Additional globalized metadata is available, such as the profile ID and entity ID for each country within the ads account.
- **POST /adsAccounts/list:** List details for all ads accounts to which the authorized user account has access.

For more information, see the user guides for [creating accounts](guides/account-management/accounts/create-accounts) and [retrieving accounts](guides/account-management/accounts/retrieve-accounts) or view the [API specification](account-management).

#### Amazon DSP Events Manager signals are now available in AMC

AMC now has access to off-Amazon signals from Amazon DSP Events Manager including Amazon Ad Tag (AAT), Conversions API (CAPI), and Mobile Measurement Partner (MMP) integrations.

With this launch, signals from 10/20/2023 are available.

As part of this launch, two new dimensions (conversion\_event\_source, and conversion\_event\_name), and three new metrics (off\_amazon\_product\_sales, off\_amazon\_conversion\_value, and combined\_sales) have been added to the following tables:

* `amazon_attributed_events_by_traffic_time`
* `amazon_attributed_events_by_conversion_time`
* `conversions`
* `conversions_all`
* `conversions_with_relevance`
* All equivalent audience tables

> [NOTE] The newly added dimensions and metrics will reflect in the tables of your sandbox instances in a future release.

To know more about these signals, and how to use them in your analysis, please refer to the new [Instructional Query (IQ): Introduction to Events Manager](https://advertising.amazon.com/marketing-cloud/instructional-queries/f28930f6d139cc028439497753c1eb1e6cbef52e49a108df56072c1f6b36d404).

#### Advertisers using Sponsored Display can now drive traffic to their Store pages

Regions: US, CA, MX

Advertisers using Sponsored Display can now drive traffic to their Store pages. By launching this new capability, Amazon Ads enables advertisers to use their Sponsored Display images or video ads to redirect shoppers to their [Store](https://advertising.amazon.com/solutions/products/stores?ref_=a20m_us_hnav_p_st) landing page.

By enabling all creatives within Sponsored Display to link to Stores, advertisers can reach audiences along their shopping and entertainment journey while helping to increase traffic to their Store pages. Sponsored Display allows advertisers to reach shoppers across their entire shopping and entertainment journey, in the Amazon store or third-party destinations. By focusing on driving specific goals using either audiences or contextual targeting, advertisers can quickly set-up powerful display campaigns that link directly to their Store pages.

This new feature drives brand growth by introducing shoppers to a brand's broader product catalog and increasing direct engagement through the Store environment using more Amazon Ads products. Linking back to Store pages allows brands to tell their story in their own voice, without any distractions from other brands or offers.

For full technical details, including the updated `landingPageURL` for `STORE` now available within Sponsored Display [productAds](sponsored-display/3-0/openapi#tag/Product-Ads/operation/createProductAds).

#### Language translations are now available for Sponsored Display custom creatives

Regions: US, CA, DE, ES, BE, NL, SE, EG, AE, SA, JP, IN

In any given marketplace, customers have the option to shop in various languages. For example, on [amazon.com](https://www.amazon.com/) in the US, English is the default language, but we also have a full Spanish language experience for customers who select Spanish as their language of preference. We’ve released a new translation feature that automatically translates Sponsored Display custom creatives to all supported languages within a marketplace. This expands visibility of Sponsored Display, while also providing shoppers with a more seamless experience, enabling them to engage with Sponsored Display ads in the language they’ve selected.

Previously, Sponsored Display custom creatives ads were only shown in the default language of each marketplace. This feature launch allows for all custom creative ads to be automatically translated, for free, with no extra input required from advertisers, making the ads available for secondary language shoppers to see and engage with.

For full technical details related to Sponsored Display creatives, please see the [API reference](sponsored-display/3-0/openapi#tag/Creatives).

#### Sponsored Brands forecasting for impressions is now available through the Amazon Ads API

Regions: US, CA, MX, UK, DE, ES, IT, FR, BE, PL, NL, SE, EG, TR, AE, SA, AU, JP, IN, SG, BR

Sponsored Brands has launched a new API that provides forecasts of impressions based on your campaign settings. Impressions forecasts are an estimate of the number of times your ad(s) will be displayed. These forecasts are available for campaigns with a single ad group that have selected the “Drive page visits” goal-based campaign control. Forecasts are based on machine-learning models that are built on the observable performance of historical campaigns with similar settings. They are not a guarantee of actual impressions.

For full technical details, please see the [API reference](sponsored-brands/3-0/openapi#tag/Forecasts).

#### Announcing the availability of Sponsored Brands V4 APIs for KDP and traditional authors

Regions: US, UK, DE, FR, IT, ES

We are excited to announce that Sponsored Brands version 4 campaign management API now supports author advertisers. Advertisers, agencies, and tool providers can leverage these APIs to create and manage ad group-based campaigns for authors, which are similar to the advertising console. When setting up a new Sponsored Brands campaign, authors will need to first claim the book, KDP or traditionally published, in their [Author Central (A2C) account](https://www.amazon.com/ap/signin?openid.pape.max_auth_age=3600&openid.return_to=https%3A%2F%2Fauthor.amazon.com%2Fhelp%2FG9RVCRGG7Q7TVLAU&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.assoc_handle=amzn_author_mobile_us&openid.mode=checkid_setup&language=en_US&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0). The existing Sponsored Brands [Ad eligibility](https://advertising.amazon.com/resources/ad-policy/book-ads#:~:text=Sponsored%20Brands%20requires%20authors%20and,Products%20or%20Sponsored%20Display%20ads) still apply.

For full technical details, please see the [API reference](sponsored-brands/3-0/openapi/prod).

### December 2023

#### Sponsored Brands video introduces video-only creatives through the Amazon Ads API

Regions: US, CA, MX, UK, DE, FR, IT, ES, NL, BE, SE, PL, TR, AE, SA, EG, IN, SG, AU, JP, BR

Advertisers can now launch video-only creatives that render exclusively on Top of Search and drive audiences entirely to the Store. Prior to this launch, advertisers could only promote one product, or a collection of products, with their Store video ads. This new creative, available under Grow Brand Impression Share outcome optimization, allows advertisers to share their branded story without tying a video campaign to a product, or a collection of products. Advertisers also have the option to edit their existing live video campaigns without the need to re-create a new campaign - under Grow Brand Impression Share - and eliminate any of the one, two, or three advertised products.

For full technical details, please see the updated [API reference](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsBrandVideoAds).

#### Support for targets in the exports API

Regions: All

We have added support for targets to the exports API. Previously, the exports API was only available for campaigns and ad groups.

Targets allow advertisers to influence when ads will or will not appear, as well as specify how much how much to bid for an ad when these conditions are met. As part of this release, we have aligned on a common model for all targeting types across sponsored ads campaign types, including keywords, product targeting, auto targeting, theme targeting, and all negative targeting types. To learn more about the common targeting model, see [Targets](reference/common-models/targets).

To get started with target exports, see:

* [Exports tutorial](guides/exports/get-started)
* [Exports API reference](exports)

#### Campaign and ad group exports API now generally available

Regions: All

Previously, we launched the exports API in preview with limits on TPS and job queue capacity. We have now lifted all restrictions on the exports API and are designating the API as generally available.

The exports API simplifies ingestion of Amazon Ads data by providing a common model across different ad products, creates parity between asynchronous export APIs and synchronous LIST APIs (including extended data fields), and improves performance over the previous snapshot APIs.

To get started with the exports API, see:

* [Snapshots to exports migration guide](reference/migration-guides/snapshots-exports)
* [Exports tutorial](guides/exports/get-started)
* [Exports API reference](exports)
* [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/blob/main/postman/Amazon_Ads_API.postman_collection.json)

#### Amazon Marketing Cloud now supports statistical analysis on event-level data

AMC launched support for the following most commonly used statistical functions in SQL. These functions can be used in AMC queries, to perform statistical analysis on event-level data.

- `STDDEV_POP`: Returns the standard deviation of a set of numeric values, when the values represent the entire population.
- `STDDEV_SAMP`: Returns the standard deviation of a set of numeric values, when the values are for a sample set of data and not the entire population.
- `STDDEV`: An alias for STDDEV_SAMP. Returns the standard deviation of a set of numeric values, when the values are for a sample set of data and not the entire population.
- `VAR_POP`: Returns the variance of a set of numeric values, when the values represent the entire population.
- `VAR_SAMP`: Returns the variance of a set of numeric values, when the values are for a sample set of data and not the entire population.
- `VARIANCE`: An alias for VAR_SAMP. Returns the variance of a set of numeric values, when the values are for a sample set of data and not the entire population.
- `SKEWNESS`: Returns the estimate of the skewness of a set of numeric values. Skewness is a measure of the asymmetry of the distribution. A positively skewed distribution has a thicker upper tail than lower tail, while a negatively skewed distribution has a thicker lower tail than upper tail. A normal distribution has a skewness of zero.
- `PERCENTILE`: Returns the value at a given percentile of a distribution of values.
- `APPROX_PERCENTILE`: Returns the approximate, discrete percentile value.
- `MEDIAN`: An alias for PERCENTILE(<value>, 0.5). Calculates the median value for the range of values.

See [AMC SQL reference](guides/amazon-marketing-cloud/amc-sql/overview) for a list of SQL functions AMC supports.

#### Amazon Marketing Cloud playbooks are now available to all AMC users

[AMC Playbooks](guides/amazon-marketing-cloud/playbooks/playbooks_overview) are intended to accelerate specific marketing use cases leveraging AMC data.

#### Amazon Marketing Cloud data sources adds new campaign\_id\_string fields

A new field, **campaign\_id\_string**, was added to the following AMC data sources:

* sponsored\_ads\_traffic
* amazon\_attributed\_events\_by\_conversion_time
* amazon\_attributed\_events\_by\_traffic\_time
* conversions\_with\_relevance
* dsp\_impressions
* dsp\_views
* dsp\_clicks
* dsp\_impressions\_by\_user\_segments
* dsp\_impressions\_by\_matched\_segments

> [NOTE] The **campaign\_id\_string** field was also added to the corresponding "\_for\_audiences" data sources based on the list from above.

This column is the campaign identifier presented as a string datatype. For DSP campaign events, this value will populate with the same value as the campaign\_id column and represented the DSP order id. For Sponsored Ads events, this value will populate with the alphanumeric campaign id string that is associated with the Sponsored Ads campaign object settings. For data sets that join traffic and conversion events, this column will be populated for both Sponsored Ads and DSP traffic events, whereas the legacy campaign\_id column will only be populated for DSP events. When conducting SQL joins across data sets or implementing SQL filtering within WHERE statements, please ensure the proper data type has been selected, campaign\_id(long data type) versus campaign\_id\_string(string data type).

Additionally, **winning\_bid\_cost** field was updated to reflect a "soft deprecated" status. The column will still function within AMC SQL queries and populated with LONG data type values, however, it is not recommended for use within customer analysis and insights generation. This value is in the process of being deprecated by upstream data providers from whom AMC sources data attributes for DSP impression events. The winning\_bid\_cost column is published into the following data sources, and their corresponding “\_for\_audiences” data sources:

* amazon\_attributed\_events\_by\_conversion\_time
* amazon\_attributed\_events\_by\_traffic\_time
* dsp\_impressions
* dsp\_clicks
* dsp\_impressions\_by\_user\_segments
* dsp\_impressions\_by\_matched\_segments

[Learn more about Amazon Marketing Cloud data sources](guides/amazon-marketing-cloud/datasources/overview)

#### Amazon Ads billing launches public APIs for payment profiles and updates to the existing payment agreement APIs

Regions: US, CA, MX, UK, DE, ES, IT, FR, BE, PL, NL, SE, EG, TR, AE, SA, AU, JP, SG, BR

Amazon Ads has launched new APIs to create payment profiles for managing billing operations. A payment profile represents the instructions on how to collect payments for invoices, and can be used in one or more payment agreements. Advertisers can also mark a payment profile as the default profile for their payment operations, and specify which accounts are eligible to share a payment profile.

With this launch, third-party clients now have the option of integrating with the Amazon Ads billing APIs to create independent and reusable payment profiles. For global advertising accounts encompassing multiple advertiser accounts, advertisers now have the option of creating a single payment profile to be shared across all their advertising accounts. A global advertising account can also mark a payment profile as their default payment profile, and all advertising accounts that get created under the global advertising account will automatically use the same payment profile, without needing to setup the payment information.

For full technical details, please see the billing [overview](guides/account-management/billing) and [API reference](billing).

#### Sponsored ads billing registration is now available for third-party integrators through the Amazon Ads API

Third-party integrators can now complete their billing registration for sponsored ads accounts using the Amazon Ads API. Advertisers can add their billing information, apply it to one or more countries where they are advertising, and set it as a default on the account for future countries where they register.

Previously, advertisers had to manage each country’s billing information separately and the registration could only be completed in the advertising console. With the billing registration APIs, advertisers can complete the registration programmatically and/or through integrator platforms, so they can get started faster.

Six APIs for billing profile management are currently in open beta:

* [POST /billingProfiles](billing-profiles#tag/Billing-Profiles/operation/CreateBillingProfiles): Creates one or more billing profiles under a sponsored ads global account. A billing profile can exist even if it’s not associated with any country.Note: To ensure accuracy of the information shown on the store, we request that you review the ‘Advertiser Name’ and ‘Payer Name’ in our ad systems and update them if required.
* [POST /billingProfileUsages](billing-profiles#tag/Billing-Profiles/operation/ApplyBillingProfile): Applies billing profile(s) to one or more countries. Applying a billing profile on a country means corresponding billing information (address & tax) will be used for invoicing, tax computations and other billing workflows.
* [PUT /billingProfiles](billing-profiles#tag/Billing-Profiles/operation/UpdateBillingProfiles): Updates one or more billing profiles under a sponsored ads global account. When you update a billing profile, the new details will be effective across all applied countries.
* [POST /billingProfiles/list](billing-profiles#tag/Billing-Profiles/operation/GetBillingProfiles): Fetches the paginated list of billing profiles under a sponsored ads global account.
* [POST /billingProfileUsages/list](billing-profiles#tag/Billing-Profiles/operation/GetBillingProfileUsages): Fetches the billing profile details for all countries under a sponsored ads global account.
* [GET /billingProfileAgreementContents/{billingProfileAgreementContentId}](billing-profiles#tag/Billing-Profiles/operation/GetBillingProfileAgreementContent): Fetches the content of agreements (example: tax agreement, non-canadian residency agreement, etc.). Users will need to read the content and provide the consent  as part of creating or updating a billing profile.

[See the full technical specifications for the billing profiles APIs.](billing-profiles)

#### Sponsored Brands launches version 4 (multi-ad groups) for bulksheets

Sponsored Brands multi-ad group campaigns are now available in bulksheets. With this latest version, you’ll be able to manage Sponsored Brands ad groups in bulksheets. You can also add custom bidding adjustments by placement, update creative assets in bulk, and create all Sponsored Brands ad formats–including Store spotlight ads. Previously in bulksheets, you could not create or manage ad groups for Sponsored Brands, add custom bidding adjustments, or update creative assets.

You can still use [the previous version of Sponsored Brands in bulksheets](bulksheets/2-0/get-started-with-sb-bulksheets), but you won’t see ad groups or any placement data in the spreadsheet. To manage these entities, you must use the latest version, which you can get by selecting the checkbox labeled **Sponsored Brands multi-ad group data<sup>NEW</sup>** when you download a bulksheets file.

[Learn more about Sponsored Brands multi-ad group campaigns in bulksheets](bulksheets/2-0/sb-multi-ad-groups-overview)

#### Sponsored Brands goal-based campaigns and theme targeting now available worldwide

Regions: All

Different brands prioritize different brand building goals depending on their marketing strategy and their brand building journey. Deciding the right campaign controls (budget, targets, bids, ad creative formats) for those goals can be time-consuming. With this launch, we've introduced two new goals: “Drive Page Visits” and “Grow Brand Impressions Share”; each with a unique set of campaign controls to achieve that goal.

To help drive these goals, theme targeting is a new method to control your campaign targeting. It uses advanced machine learning algorithms that take into account historical performance targets and brand interactions to create a large set of keywords. It then categorizes those keywords into relevant groups to use with your campaigns. Two targeting groups are available in this launch: “Keywords related to your landing pages.” and “Keywords related to your brand”. These targeting groups will be pre-selected to simplify the campaign creation process for advertisers.

With this launch, we've expanded to AU, MX, BR, BE, NL, PL, SE, TR, EG, KSA, UAE, SG and UK so more advertisers can access these features via the Amazon Ads API. For instructions on how to enable these features, refer to our developer guide on how to [create Sponsored Brands campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns) and our developer guide on how to create [theme based targets](guides/sponsored-brands/theme-based-targeting).

#### Amazon DSP Campaign Management API Adds New Optimization Fields

The [/dsp/orders](dsp-campaigns#tag/Order) endpoint has added new enum values and fields to support the new simplified optimization experience.

Summary of changes:

1) Added two new enum values for the goal field:

* `CONVERSIONS`
* `CONSIDERATION`

2) Added a new field: `targetKpi`

These changes can be found in version 2.5. See the changes [here](dsp-campaigns#tag/Order) in version `application/vnd.dsporders.v2.3+json`.

#### Awareness: Deactivation of non-age-gender demographic segments in Amazon DSP

We are deactivating all non-age-gender-demographics segments for Germany, France, Italy, Spain from the following Amazon DSP services: Audience Central UI, Audience Discovery API, Audience Picker UI, Overlapping Audiences, Audience Insights UI, Insights API, Data Decoration Service (DDS, including data output from the EDX interface). Deactivation means that these segments will not be available for any targeting or insights after December 31, 2023. However, any existing campaigns can continue using it until that date.

We will deprecate the impacted segments from targeting UIs on December 04, 2023. Any new campaigns that are created after this date will no longer be able to add those segments for targeting. While these segments will remain visible in the targeting UIs (as deactivated), these segments cannot be added to any campaigns for targeting.

The non-age-gender demographic segments in EU4 are currently sourced from Experian. Experian EU made a decision to stop offering these segments based on an internal assessment by Experian’s EU team and was driven by privacy compliance reasons. This decision wasn’t specific to Amazon.

Due to the removal of Experian demographic data provision in EU4 mentioned above (Germany, France, Spain and Italy), in November 2022, the demographic segments in the Standard Catalog are no longer being refreshed, and they would be removed by 12/31/2023.

For a full list of segments to be deactivated, you can reference [this table](release-notes/deprecations#deactivation-of-non-age-gender-demographic-segments-in-amazon-dsp).

### November 2023

#### Sponsored Display launches entertainment targeting

Region: US

With Sponsored Display, advertisers can discover, reach, and engage audiences in relevant contexts across their shopping and entertainment journey. With this launch, entertainment targeting will allow advertisers to reach audiences who are either actively engaged in those entertainment contexts or have shown interest in them. Advertisers can engage with specific genres or categories including movies/TV, music and games.

Entertainment targeting helps advertisers reach engaged audience during their entertainment journey, whether they're live streaming, watching videos, or researching games and movies on blog sites. This helps advertisers expand their reach beyond the Amazon store. Additionally, you can ensure your brand’s ads show up next to content category/genre that aligns with the advertisement. These new targets also allow advertisers to further connect with highly engaged Twitch audiences. We've found that viewers that watch Twitch’s streaming content spend, on average, more than 42 minutes per visit, the highest among all social media platforms.

For full technical details, please see our updated [developer guide](guides/sponsored-display/entertainment-targeting) and our [targeting API](sponsored-display/3-0/openapi#tag/Targeting/operation/createTargetingClauses), including our new `ContentTargetingPredicate` that can be used for either contextual (T00020) or audience (T00030) campaigns.

#### Gross and invalid traffic report now available through the Amazon Ads API

Regions: All

The Amazon Ads version 3 reporting API now supports the gross and invalid traffic report. For Sponsored Products, Sponsored Brands, and Sponsored Display campaigns, this report can be accessed programmatically.

To optimize the efficacy of their campaigns, large advertisers conduct analytics on metrics such as gross impressions, invalid impressions, invalid impression rate, gross clicks, invalid clicks, and invalid click rate. With this release, advertisers can now programmatically access the data for analytics, whereas previously the data had to be manually downloaded and then used. Amazon Ads' traffic quality systems assist in safeguarding advertiser budgets from invalid traffic, such as non-human, fraudulent, and other illegitimate traffic. These metrics provide greater visibility into invalid traffic that Amazon Ads filters and aid advertisers in optimizing their campaign performances.

For full technical details, please read the [Gross and invalid report definition](guides/reporting/v3/report-types/gross-and-invalid-traffic).

#### Hours of day now available for schedule-based budget rules

Regions: US, CA, UK, IN, JP

We’ve added hours of day for schedule-based budget rules for sponsored ads campaigns. Now you can set up budget rules that increase budget at specific times of day. This is in addition to specific days of the week and specific date ranges currently offered as schedules for budget rules.

You can now support campaign budget coverage at specific times of the day by setting up budget rules with hours of day granularity. You can also utilize hourly performance reporting through [Amazon Marketing Stream](guides/amazon-marketing-stream/overview) to find the times of day that your campaigns are performing best, and set rules for those time periods to help boost performance.

Budget rules help you plan budget changes in advance so you can spend less time making manual adjustments. Advertisers can use budget rules to reduce the likelihood of campaigns running out of budget, especially during peak days, times, or holiday events. You can set up budget rules with hours of day schedules using the Amazon Ads console or the Amazon Ads API.

For full technical details, please see the [user guide](guides/rules/budget-rules/schedule-based) and [API reference](sponsored-products/3-0/openapi/prod#tag/BudgetRules/operation/CreateBudgetRulesForSPCampaigns).

#### Translations of advanced tools center content now available in Chinese and Japanese

Regions: All

Developers can now read translations of advanced tools center content in Chinese and Japanese. The current translations reflect the status of advanced tools center documents as of July 2023.

Visit the [translations](guides/translations) page for more information, or go directly to the [Chinese translation](https://d3a0d0y2hgofx6.cloudfront.net/zh-cn/index.html) or [Japanese translation](https://d3a0d0y2hgofx6.cloudfront.net/ja-jp/index.html).

#### Schedule based bid rules now available for Sponsored Products

Regions: US, MX, CA, UK, DE, ES, FR, IT, IN

We’re launching schedule bid rules for Sponsored Products campaigns, so advertisers can set up bid rules that increase bids at specific times of the day, days of the week, and date ranges. With the launch of schedule bid rules, advertisers can automate their bid changes without missing sales opportunities. Advertisers can also utilize new hourly performance reporting to find the times of day that your campaigns are performing best, and set rules for those time periods to help boost performance.

To learn more, see our [schedule based guide](guides/sponsored-products/scheduled-bid-rules).

#### Sponsored Display allows optimization rules to be included in the forecasting API

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, US

Sponsored Display has improved the accuracy of the [forecasting API](sponsored-display/3-0/openapi#tag/Forecasts) by allowing [optimization rules](sponsored-display/3-0/openapi#tag/Optimization-Rules-(beta)) to be included as an input.

Forecasting helps advertisers understand the impact of their choices during campaign creation. This includes a minimum and maximum range of available impressions, viewable impressions, and clicks that could be available based on the selected advertised products, optimization type, targeting, bid, and optimization rule.

For full technical details, please see our updated [forecasting API](sponsored-display/3-0/openapi#tag/Forecasts).

#### Sponsored Display updates to targeting recommendation API

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SE, UK, US

Sponsored Display now includes `themes` in the response of the [targeting recommendations API](sponsored-display/3-0/openapi#tag/Targeting-Recommendations).

Theme-based recommendations offer multiple options to target relevant products without the need for additional research from advertisers. Target products under each theme are curated specific to the use case, with high-quality suggestions based on Amazon's first-party signals. Each theme will have up to 50 individually recommended products, with flexibility to choose products individually or select all products within a theme.

For full technical details, please see our updated [targeting recommendations API](sponsored-display/3-0/openapi#tag/Targeting-Recommendations) documentation.

#### Sponsored Brands version 2 reporting now supports click and view-based attribution

Sponsored Brands reporting version 2 reports now support view and click attributed data for conversion metrics (that is, conversions, sales, units sold, new-to-brand, and detail-page-view metrics) for Sponsored Brands campaigns with a `costType` of `VCPM`. The conversion metrics will have the right data depending on the attribution type for the campaign.  For campaigns with a `costType` of `CPC`, metrics will continue to report click attributed data.

Sponsored Brands recently launched campaigns to grow brand impression share campaigns that are transacted on VCPM basis.  It’s important to report both view and click attributed conversion metrics for these campaigns, so that advertisers can understand the full performance impact.

See [version 2 metrics](guides/reporting/v2/metrics) for updated metric definitions.

#### DSP audiences edit API now supports editing the name, ASIN list, and lookback period for product audiences

Regions: All

Advertisers can now edit the name, ASIN list, and lookback period for product audiences that are not part of an active campaign.

For technical specifications, please consult the [API reference](audiences#tag/Ads/operation/DspAudienceEdit).

#### Amazon DSP launches goal-based bidding

Regions: All

Amazon DSP is launching goal-based bidding, which helps advertisers to specify a performance target for their campaigns and allow Amazon DSP to automatically optimize their bids in real-time in order to maximize delivery at their performance target. With this release, advertisers can automatically optimize to both delivery and performance goals.

Goal-based bidding can be used to maximize delivery at a performance threshold that advertisers define. You can now specify a KPI target when creating a campaign—for instance, you can set a target CPA of $10 and choose “Prioritize KPI target” as the optimization strategy. This allows you to maximize your campaign goal, such as conversions, while ensuring that the campaign remains at or near your specified target CPA.

Goal-based bidding is part of the ‘optimization’ field of the order object in [POST /dsp/orders](dsp-campaigns#tag/Order/operation/CreateOrders). To get suggested KPI target range, you can use the [POST /dsp/campaigns/targetKpi/recommendations](target-kpi-recommendations#operation/getGsbTargetKpiRecommendation) endpoint.

### October 2023

#### Unified pre-moderation API

Regions: US, CA, MX, BR, UK, DE, FR, IN, ES, IT, AE, SA, NL, SE, PL, TR, EG, JP, AU, SG

The Unified Pre-moderation API now offers new capabilities to validate creatives against Technical specifications and Policy requirements. These validations are done ahead of Ad submission. The Unified Pre-moderation API is compatible with Sponsored Ads and DSP.

For technical specifications, please consult the [API reference](pre-moderation).

#### Sponsored ads consolidated invoice statement download is now global

Regions: All

Sponsored ads advertisers can now choose to download invoices from the last 90 days for one country or for all countries in their global sponsored ads account.
Two Amazon Ads APIs are currently in open beta where you can retrieve this information:

* [POST /billingStatements](billing-statements#tag/Billing-Statements/operation/CreateBillingStatement): This API lets you request for a billing statement to be generated, and returns a billing statement ID.
* [GET /billingStatements/{billingStatementRequestId}](billing-statements#tag/Billing-Statements/operation/GetBillingStatement): Call this API to check the generation status of your statement. When the statement is ready, the response includes the pre-signed URL where you can download the file. Download URLs are valid for three hours.

#### Amazon Marketing Cloud APIs are now part of the Amazon Ads API

Amazon Marketing Cloud is now available on the Amazon Ads API to automate workflows, establish integrations, and develop tools and apps at scale. Advertisers and partners can now access Amazon Marketing Cloud (AMC) APIs from the Amazon Ads API, along with APIs for other Amazon Ads solutions such as sponsored ads and Amazon DSP. Using standards from the rest of the Amazon Ads APIs, the updated AMC APIs follow the industry-standard OAuth 2.0 authorization framework and consistent endpoint URL paths. A [developer guide](guides/amazon-marketing-cloud/overview) is also provided to help users get started and use these APIs.
As documented in the [Advanced tools center](https://advertising.amazon.com/API/docs/en-US), the new AMC APIs currently support features including reporting, signal management, and audience management.

Previously, advertisers and partners have been using AMC APIs that at an AMC instance level. The new set of AMC APIs offer further enhanced security, usability, and performance, allowing developers to easily utilize, manage, and build with these APIs.
See what’s [changed](guides/amazon-marketing-cloud/migration) and [start your onboarding journey to the Amazon Ads API](guides/onboarding/overview).

#### Reach and inspire new audiences using Sponsored TV (beta)

Regions: US

Introducing Sponsored TV, a Streaming TV ad solution for brands of any size to connect with new customers in their living rooms—  even if they have never advertised on TV before. Pre-packaged with retail aware, shoppable ad formats, Sponsored TV allows marketers to buy Streaming TV with the same flexibility of Amazon Ads self-service ads — that means no minimum spend requirements or upfront commitments.  In just a few clicks, brands can help engage relevant viewers across 30+ streaming TV services like Twitch or Freevee. Now, any endemic advertiser can benefit from campaigns modeled by Amazon. With this launch, campaigns are optimized in real time, simplifying the complex process of buying programmatic TV ads.

Now, advertisers can easily get started with Streaming TV, with no minimum spend requirements, daily budgets, and evergreen campaigns. Amazon's first-party insights help brands brands reach audiences based on likely content interests such as music videos, video games or dramas as well as product categories on Amazon like headphones or running shoes. Finally, Sponsored TV ads are eligible for interactive ad formats to help you connect with viewers as you reach new audiences.

For more information on how to get started with Sponsored TV, please visit the developer guide on [how to get started with Sponsored TV](guides/sponsored-tv/overview).

#### Target Promotion API is now available for Sponsored Products

Regions: All

We’re launching target promotion APIs to allow advertisers to perform target promotion across all their campaigns at scale. Target promotion is a performance advertising strategy that identifies targets for further refinement to drive more efficient spend against high-performing targets and reduce spend on low-performing targets. This combines the common practice of “target harvesting,” which identifies targets to move from broader targeting (e.g., automatic targeting) to more focused targeting (e.g., manual targeting with broad match), with optimized bids and budget changes.

Before today, Sponsored Products advertisers needed to download a search term report and manually manipulate the report data to add new targets to existing or new campaigns. There was no automated way to add high-performing targets from an automated targeting campaign into a separate manual targeting campaign at scale. With the launch of target promotion APIs, advertisers can achieve more performant campaigns by promoting high-performing targets within auto-targeting campaigns to manual-targeting campaigns at scale.

For more information on how to enable this feature, refer to our developer guide on how to [create target promotion](guides/sponsored-products/target-promotion).

#### Sponsored Products budget recommendations and missed opportunities dataset (beta) available in Amazon Marketing Stream

Regions: All

Sponsored Products budget recommendations and missed opportunities insights are now available in Amazon Marketing Stream. This provides an unmodified view of budget recommendations and missed opportunities, which includes the additional clicks, impressions, and sales from the past 7 days that the campaign could have received had it stayed in budget for the hours it was out of budget.

Sponsored Products budget recommendations and missed opportunities via Amazon Marketing Stream addresses two use cases: delivery of recommendations in near real time when a campaign goes out of budget, and delivery of recommendations for all campaigns owned by a subscribed advertiser with no additional API calls beyond the initial subscription.

Amazon Marketing Stream is available via the Amazon Ads API. When you are ready to get started, visit our Amazon Marketing Stream [onboarding guide](guides/amazon-marketing-stream/onboarding) that will walk through the process of getting API access, integrating with AWS, and subscribing to campaign datasets using the Amazon Marketing Stream API. If you are already integrated with Amazon Marketing Stream, see the new [Dataset documentation](guides/amazon-marketing-stream/datasets/budget-recs-missed-opportunities).

#### Add-to-cart and new-to-brand detail page view metrics are now available for Sponsored Brands and Sponsored Display in version 3 reporting

Regions: All

You can now request new metrics related to add-to-cart actions and new-to-brand details page views. For both Sponsored Brands and Sponsored Display, new metrics include: [addToCart](guides/reporting/v3/columns#addtocart), [addToCartRate](guides/reporting/v3/columns#addtocartrate), [eCPAddToCart](guides/reporting/v3/columns#ecpaddtocart), [newToBrandDetailPageViews](guides/reporting/v3/columns#newtobranddetailpageviews), [newToBrandDetailPageViewRate](guides/reporting/v3/columns#newtobranddetailpageviewrate), and [newToBrandECPDetailPageViews](guides/reporting/v3/columns#newtobrandecpdetailpageview) for both Sponsored Brands and Sponsored Display reports through the version 3 reporting API.

Sponsored Display also supports additional metrics: [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [brandedSearchRate](guides/reporting/v3/columns#brandedSearchRate), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), and [newToBrandDetailPageViewClicks](guides/reporting/v3/columns#).

For full definitions and report availability for each metric, see [Columns](guides/reporting/v3/columns).

You can call the [version 3 reporting API](offline-report-prod-3p) to access these metrics.

#### Sponsored Display reports on Amazon Ads API reporting version 3 (beta)

Regions: All

We are excited to launch Sponsored Display reports on the latest version (version 3) of the Amazon Ads API in beta. The new reports adhere to a common request and response schema we introduced with version 3 reporting, as well as the ability to select date ranges, standardized metric names, and better reports availability over version 2.

This release launches the following reports on version 3 reporting for Sponsored Display: campaigns, ad groups, targeting, advertised product, and purchased product. The matched target report is merged into other reports to reduce duplication of metrics across multiple reports. With this release, customers will find better consistency between Sponsored Display reports and those for other ad products.

The new Sponsored Display reports come with standardized metric names allowing consistent naming across all ad product reports. This will enable customers to easily compare metrics across ad product reports. Another major change with this release is using a 14-day attribution window for all applicable Sponsored Display metrics. We dropped deprecated metrics and attribution suffixes in metric names from the reports, making it easier to read and analyze reporting information.

For full technical details, please see the [migration guide](reference/migration-guides/reporting-v2-v3) and [report types overview](guides/reporting/v3/report-types/overview).

#### Preview: Sponsored Brands reports on Amazon Ads API reporting version 3

Regions: All

> [NOTE] Sponsored Brands reports are currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the release notes.

We are excited to launch Sponsored Brands reports on the latest version (version 3) of the Amazon Ads API in preview. The new reports adhere to a common request and response schema we introduced with version 3 reporting, as well as the ability to select date ranges, standardized metric names, and better reports availability over version 2.

This release launches the following reports types for Sponsored Brands in version 3: campaign, placement, ad group, targeting, search term, and ad, in addition to the existing purchased product report. With this release, customers will find better consistency between Sponsored Brands reports and those for other ad products.

The new Sponsored Brands reports come with standardized metric names allowing consistent naming across all ad product reports. This will enable customers to easily compare metrics across ad product reports. Another major change with this release is using a 14-day attribution window for all applicable Sponsored Brands metrics. We dropped deprecated metrics and attribution suffixes in metric names from the reports, making it easier to read and analyze reporting information.

For full technical details, please see the [migration guide](reference/migration-guides/reporting-v2-v3) and [report types overview](guides/reporting/v3/report-types/overview). For access, please [submit a Jira ticket](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2).

#### Deprecation announcement: Legacy Bid Optimization Strategy and Adjustment by Shopper Segment will deprecated on March 31st, 2024

The legacy bid optimization strategy and adjustment by shopper segment feature will be deprecated on March 31, 2024. If the parameters `bidAdjustmentsByShopperSegment` and `bidOptimizationStrategy` are included in the bidding object of the [POST /sb/v4/campaigns](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredBrandsCampaigns) request after March 31, 2024, the values will be treated as `null` values.
If you have any questions, visit the [Amazon Ads API Support](support/overview) page and create a ticket with the subject title: "Sponsored Brand New To Brand optimization."

#### Sponsored Brands expands goal-based campaigns and theme targeting to additional marketplaces

Regions: CA, DE, ES, FR, IN, IT, JP, UK, US

Different brands prioritize different brand building goals depending on their marketing strategy and their brand building journey. Deciding the right campaign controls (budget, targets, bids, ad creative formats) for those goals can be time-consuming. With this launch, we've introduced two new goals: “Drive Page Visits” and “Grow Brand Impressions Share”; each with a unique set of campaign controls to achieve that goal.

To help drive these goals, theme targeting is a new method to control your campaign targeting. It uses advanced machine learning algorithms that take into account historical performance targets and brand interactions to create a large set of keywords. It then categorizes those keywords into relevant groups to use with your campaigns. Two targeting groups are available in this launch:  “Keywords related to your landing pages.” and “Keywords related to your brand”. These targeting groups will be pre-selected to simplify the campaign creation process for advertisers.

This feature was previously only available in the US, but with this launch, we've expanded to CA, DE, ES, FR, IN, IT, JP, and UK so more advertisers can access this feature via the Amazon Ads API. For instructions on how to enable these features, refer to our developer guide on how to [create Sponsored Brands campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns) and our developer guide on how to create [theme based targets](guides/sponsored-brands/theme-based-targeting).

#### French advertisers can now measure brand marketing impact using Amazon Brand Lift

Regions: FR

French advertisers can now measure upper and mid-funnel Amazon Ads campaigns with [Amazon Brand Lift](https://advertising.amazon.com/en-us/resources/whats-new/amazon-dsp-brand-lift/), joining advertisers in the United States, United Kingdom, Canada, and Germany, where Amazon Brand Lift was already available.

Amazon Brand Lift is an insightful and privacy-safe way for advertisers to quantify the impact of upper and mid-funnel campaigns. With participation of the sizable, representative, and engaged [Amazon Shopper Panel](https://panel.amazon.ca/) community, Amazon Brand Lift helps provide objective and concrete measurement results. The reporting provides insights, such as how much a campaign has impacted the percentage of respondents who report being aware of a brand or interested in purchasing from a brand.

To create, manage, and retrieve results for Amazon Brand Lift studies, use the following endpoints:

* Create study: POST/dsp/measurement/studies/brandLift
* Update study details: PUT/measurement/studies/brandLift
* Get results: GET/measurement/studies/brandLift/{studyId}/result

For more details, view the [API reference](dsp-measurement/#/Measurement).

#### Brand Posts are now available through the Amazon Ads API

Regions: US

Advertisers in US can now create Posts via the Amazon Ads API. All sellers and vendors now have access to a Brand profile to create Posts. To have a Brand profile, you must be registered in Amazon Brand Registry. By combining the [creative assets](creative-asset-library) API and [Posts](posts) APIs, you can automate Post creation and the publishing process.

[Learn more about the Amazon Brand Registry or contact the team](https://www.amazon-brand-registry.com/us/contact_us2).

Previously, advertisers needed to have a Store before they could create a Posts profile, and could not reuse the existing image assets available in the creative asset library when creating Posts. With this launch, some new capabilities include:

1. Stores dependency removed: Advertisers no longer need to have a Store to create a Post profile, making the process much more streamlined with the introduction of Brand profiles.
2. Asset reuse: Advertisers can reuse an existing image asset available in the creative asset library to create Posts.

For more information, see our guides for [Managing](guides/posts/managing) and [Reporting](guides/posts/reporting) on Posts, as well as the [API reference](posts).

#### Sponsored Display forecasting for clicks and viewable impressions is now available

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, US

Sponsored Display has improved the [forecasting API](sponsored-display/3-0/openapi#tag/Forecasts) by adding clicks and viewable impressions.

Forecasting helps advertisers understand the impact of their choices during campaign creation with a minimum and maximum range of available impressions, viewable impressions, and clicks that coudld be available for a set of selected advertised products, optimization type, targeting, and bid values. This helps advertisers create campaigns that will deliver results, understanding scale by experimenting with the campaign setup, and forecasting the output of an investment before they get started.

Forecasting will be available for [Sponsored Display](sponsored-display/3-0/openapi#tag/Forecasts) through the Amazon Ads API. For full technical details, please see our updated [forecasting API](sponsored-display/3-0/openapi#tag/Forecasts) documentation.

#### Awareness: Sponsored Brands may increase or decrease bids to help achieve goals

Regions: All

To help advertisers achieve specific goals for their Sponsored Brands campaigns, bids can be automatically adjusted up or down. If we find an opportunity where your ad is more likely to lead to page visits or conversions for campaigns with “Drive Page Visits” goal with a `costType` of CPC, we may increase your bid for that auction to help achieve that goal. Similarly, if we find an opportunity that looks less likely to lead to your campaign goal, we may lower your bid for that auction. For example, Amazon can adjust bids if we see higher expected results that can “Drive Page Visits”.

For more information related to bidding, please see our updated [help page](https://advertising.amazon.com/help/GPDRGYSKVB7H4BUR).

#### Amazon Ads billing launches public APIs for payment registration, eligibility, and execution

Regions: US, CA, MX, UK, DE, ES, IT, FR, BE, PL, SE, AU, JP, BR

We have launched new APIs for basic billing operations, so advertisers can check which payment methods are eligible to register to their advertising account. They can then update the payment method their advertiser account is using. Lastly, we also added support for executing payments on open invoices using either the advertiser’s current payment method or a one-time payment method.

With this launch, third-party clients can integrate with the Amazon Ads billing APIs and trigger basic operations that are part of the billing lifecycle. This feature is already available via the advertising console, so this launch expands access via the public API.

There are four new endpoints included in this release:

* [POST /billing/paymentMethods/list](billing#tag/Get-Payment-Methods)
* [POST /billing/paymentAgreements](billing#tag/Create-Payment-Agreement)
* [POST /billing/paymentAgreements/list](billing#tag/Get-Payment-Agreements)
* [POST /billing/invoices/pay](billing#tag/Pay-Invoices)

For full details, see the [API reference](billing) and [overview](guides/account-management/billing).

### September 2023

#### Sponsored Display has enhanced the forecasting API by adding improved responses

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, US

Sponsored Display has added forecast status to the API to better understand how targeting can affect delivery and scale.

Forecasting helps advertisers understand the impact of their choices during campaign creation with a minimum and maximum range of available impressions inventory for a set of selected advertised products, optimization type, targeting, and bid values. This helps advertisers create campaigns that will deliver results, understanding scale by experimenting with the campaign setup, and forecasting the output of an investment before they get started.

The new `forecastStatus` is now available through the Sponsored Display including `IMPRESSION_TARGETING_TOO_NARROW` and `IMPRESSION_TARGETING_TOO_BROAD`. For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#/Forecasts).

#### Preview: Campaign and ad group exports APIs

Regions: All

Note: We’re announcing these APIs in preview so that you can make test calls and start your integration. During preview, these endpoints have a smaller TPS and job queue capacity.  Once these APIs are ready for scaled adoption and support their full TPS and job queue capacity, we’ll post a new release note.

Amazon Ads API has launched new export APIs as an update to the existing snapshots functionality. The new APIs currently support asynchronous campaign and ad group exports for Sponsored Products, Sponsored Brands, and Sponsored Display.

These APIs simplify ingestion of Amazon Ads data by providing a common model across different ad products, create parity between asynchronous export APIs and synchronous LIST APIs (including extended data fields), and improve performance over the previous snapshot APIs.

To get started with the export API, see:

* [Migration guide](reference/migration-guides/snapshots-exports) (snapshots to exports)
* [Exports tutorial](guides/exports/get-started)
* [Exports API reference](exports)
* [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/blob/main/postman/Amazon_Ads_API.postman_collection.json)

In the near future, we will expand exports to add support for targets and ads.

#### Amazon Ads common models documentation

With the launch of [export APIs](exports), we’ve added a new section to the advanced tools center where we document common Amazon Ads models. The goal of these models is to simplify understanding of and reduce differences across ad products. Currently this documentation covers campaigns and ad groups, including the fields and enum values in the common models, usage across ad products, and mapping to the latest versions of each ad product’s campaign management APIs.

In the future, we’ll be adding more entities to this documentation, starting with targets and ads, and using these common models in future API releases.

For more details see:

- [Campaign model](reference/common-models/campaigns)
- [Ad group model](reference/common-models/ad-groups)
- [Enums](reference/common-models/enums)

If you have any comments or questions on these models, please leave a comment in [Github](https://github.com/amzn/ads-advanced-tools-docs/discussions/139).

#### Improve Sponsored Products campaign performance on rest-of-search placements (beta)

Regions: US, CA, MX, UK, DE, ES, IT, FR, BE, PL, NL, EG, TR, AE, SA, AU, JP, IN, SG, BR

Sponsored Products Rest Of Search Placement Bid Adjustment is now available in beta. Advertisers using the Amazon Ads API or bulksheets can now apply bid adjustment for rest-of-search placements in their Sponsored Products campaigns. The new placement bid adjustment control for rest of search works same as the adjustments available for top-of-search (first page) and product-page placements. Through the new control, advertisers can adjust their bid for rest of search by up to 900%, same as the other two placements.

The new placement bid adjustment helps advertisers to balance their campaign performance across the three different placements (top of search, rest of search, product pages). The control is available for all targeting types (auto and manual) and bidding strategies (fixed and dynamic bidding). Advertisers already have access to campaign performance split by the three placements in campaign manager and Sponsored Products placement report. Using the available reporting, advertisers can measure the impact of using the new control in Sponsored Products campaigns.

For the API, you can view the relevant API contract changes by looking at `dynamicBidding.placementBidding.placement` in [POST /sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredProductsCampaigns). Use the new enum `PLACEMENT_REST_OF_SEARCH` to add an adjustment percentage.

For bulksheets, you'll apply this new placement in the "bid adjustment" entity row when you create or update campaigns. Learn more about [creating bid adjustments in bulksheets](bulksheets/2-0/create-sp-campaign#step-2-define-the-bidding-adjustment-entity-optional)

#### Integral Ad Science (IAS) Context Control Targeting and Avoidance on third-party supply

Regions: All

Amazon DSP now supports Integral Ad Science (IAS) Context Control products for brand suitability (avoidance) and contextual targeting. This feature is available worldwide to DSP self-service advertisers, and supported on standard display, video, and AAP - mobile app line items running on third-party supply sources. Advertisers will pay a CPM fee from their campaign budgets for the use of these products.

Per IAS - “Context Control products use patented cognitive semantic technology that relies on natural language processing (NLP) to dynamically comprehend the nuances of context using sentiment and emotion analysis across 50 languages. This provides customers precise, page-level content classification at scale.”

For brand suitability, advertisers will be able to choose avoidance segments that are suitable for protecting their brands from exposure to specific content, or negative sentiments they do not want their brands to be associated with. They can do this without avoiding all instances of a certain topic and thus implement brand suitability standards that align closely with their brand’s values.

For contextual targeting, advertisers can target contextually relevant content for their brand or campaign. This also allows them to increase ad relevance in third-party supply environments where ad identifiers are not available. For more information, see our [updated documentation](dsp-campaigns#tag/Order/operation/CreateOrders).

#### Sponsored Display improves bid recommendations by supporting campaigns with video creatives

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SA, SE, TR, UK and US

Sponsored Display is launching bid recommendations for video creatives. Advertisers can now customize creatives and receive bid recommendations when using any custom creative, including headline and logo, image, or video. Previously, bid recommendations were only customized for campaigns with static images. With this launch, advertisers receive relevant bid recommendations to ensure their video campaigns have improved inputs to help them reach their audiences.

For full technical details, see our updated [bid recommendations](sponsored-display/3-0/openapi#tag/Bid-Recommendations)resource. This resource will help find relevant bid suggestions when using either video or image creatives.

#### Sponsored Display optimization rules (beta) API is now available

Regions: AE, AU, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK and US

We launched a new API, [optimization rules (beta)](sponsored-display/3-0/openapi#tag/Optimization-Rules-(beta\)), to simplify the campaign management process for advertisers using Sponsored Display. This launch allows advertisers to set performance-based parameters that remove the guess work from selecting an exact bid for each target. This helps advertisers of any size and with any budget achieve their goals with the help of Amazon’s first-party data and machine learning capabilities.

Before this launch, advertisers needed to translate their desired goals into efficient bids and targets across different cost types. In reality, this turns out to be the biggest issue driving lower scale and campaign effectiveness. By translating goals into bids using machine learning with optimization rules (beta), Amazon can help advertisers achieve better results through continuous optimization. In the future this will be extended to targeting optimizations to further maximize goals.

For campaigns optimized for reach or click, advertisers can apply a cost-based rule like cost per thousand ad views (vCPM) and cost per click (CPC). We will then utilize machine learning models to define the bids while adjusting base bids up and down with the goal of increasing outcomes, all while adhering to the cost-based rules. While Amazon Ads cannot guarantee performance, if the campaign is not meeting the cost target over a 21-day period (excluding special days), we recommend adding additional targets to allow our systems to reach the right audiences.

Optimization rules are now available for [Sponsored Display](sponsored-display/3-0/openapi) through the Amazon Ads API. For full technical details, see our [updated documentation](sponsored-display/3-0/openapi#tag/Optimization-Rules-(beta\)).

#### Bulksheets downloads are now available in all languages

Regions: All

You can now download a bulksheet in your chosen language if that language is selected in your advertiser profile. For example, if Japanese is the language selected in your profile, you will see Japanese text in bulksheets downloads. When you upload the file, you can use any supported language, regardless of the language selected in your profile. Note that the “Operation” field will be blank in the downloaded file. If you need guidance on what values to enter into this field for languages other than English, refer to [this language guide for bulksheets translations](bulksheets/2-0/bulksheets-language-guide).

#### Amazon DSP campaign signals now available on Amazon Marketing Stream

Regions: All

Amazon Marketing Stream is expanding to include new Amazon DSP campaign datasets. This includes information on Amazon DSP campaigns, flights, ad groups, and ad group targets.

These signals enable Amazon DSP advertisers and partners to be notified in near real time via Amazon Marketing Stream when Amazon DSP campaign data changes (for example, when campaigns pause, budgets change, or bids update) and what the modification was across thousands of advertising campaigns. Amazon Marketing Stream can help improve information integration patterns and operational efficiency by reducing the number of API calls that are required for intraday campaign optimization. Advertisers and partners can use this dataset to react to events in near real time, such as campaign status changes, rather than run multiple intraday snapshots. Amazon Marketing Stream helps simplify the extract, transform, and load operations compared to snapshots, by showing you only information that has changed, when it occurs.

The Amazon DSP campaign data is split into four datasets: campaigns, flights, ad groups, and ad group targets for each AWS region. To learn more and access this feature, review the Amazon Marketing Stream [Onboarding guide](guides/amazon-marketing-stream/onboarding), and subscribe to the new [Amazon DSP campaign datasets](guides/amazon-marketing-stream/datasets/dsp-campaign-management).

#### Original video download is now available on Sponsored Brands media API

Regions: All

Sponsored Brands API now allows advertisers to download their original video media using the [GET /media/describe](sponsored-brands/3-0/openapi#tag/Media/operation/describeMedia). Advertisers can use `originalMediaUrl` to retrieve their original video media in .MP4 format. Previously, advertisers could only download a preview of the original video using `publishedMediaUrl`.

Going forward, please use the [asset library](creative-asset-library) for all creative management as the [POST /media/upload](sponsored-brands/3-0/openapi#tag/Media/operation/createUploadResource) will be deprecated on 1/31/2024. For further instruction, refer to the API specifications [here](sponsored-brands/3-0/openapi#tag/Media/operation/describeMedia).

### August 2023

#### New goal KPIs added to Amazon DSP API

For the kpiName field in the Goal Configurations Discovery API and Orders API, we've added three new enum values and changed two to match the ADSP UI. We've also changed `TOTAL_RETURN_AD_SPEND` to `TOTAL_RETURN_ON_AD_SPEND` in the Goal Configurations Discovery API goalKpi field. In addition, we've added definitions for each enum value.

Here are the details:

Goal Configurations Discovery API goalKpi field:

- Added descriptions
- Changed from `COST_PER_ACQUISITION` to `COST_PER_ACTION`
- Removed `COST_PER_DOWNLOAD`
- Added `COST_PER_FIRST_APP_OPEN`
- Added `COST_PER_INSTALL`
- Added `TOTAL_COST_PER_SUBSCRIPTION`
- Changed from `TOTAL_RETURN_AD_SPEND` to `TOTAL_RETURN_ON_AD_SPEND`

Orders API goalKPI field:

- Added descriptions
- Changed from `COST_PER_ACQUISITION` to `COST_PER_ACTION`
- Removed `COST_PER_DOWNLOAD`
- Added `COST_PER_FIRST_APP_OPEN`
- Added `COST_PER_INSTALL`
- Added `TOTAL_COST_PER_SUBSCRIPTION`

See the changes [here](dsp-campaigns/#tag/Discovery/operation/getGoalConfigurations).

#### Deprecation announcement: Planned shutoff date of May 28, 2024 for version 1 profiles

[Profiles version 1 endpoints](reference/1/profiles) are now deprecated, with a target shutoff date of May 28, 2024. Please migrate to the [latest version of the profiles endpoint](reference/2/profiles) as indicated on our [Deprecations page](release-notes/deprecations).

The following endpoints and all associated HTTP operations are included in this deprecation:

* /v1/profiles
* /v1/profiles/{profileId}

If you would like to share feedback, please use the Amazon Ads API [Service Desk](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2) to open a ticket with subject title "Profiles v1 Deprecation" by December 15, 2023.

#### Sponsored Brands launches goal-based campaigns

Regions: US

Different brands prioritize different brand building goals depending on their marketing strategy and their brand building journey. Deciding the right campaign controls (budget, targets, bids, ad creative formats) for those goals can be time-consuming and prone to errors. With this release, we’re introducing Sponsored Brands goal-based campaigns, providing simplified campaign creation and management workflow for the desired goals to better achieve your brand marketing objectives. In this release, we’ll introduce two new goals: “Drive Page Visits” and “Grow Brand Impressions Share”; each with a unique set of campaign controls to achieve that goal. This helps advertisers of any size grow their brands and deliver business objectives with simplified campaign controls and guidance powered by Amazon’s 1P data and machine learning capabilities.

Advertisers can access this feature via the Amazon Ads API. For instructions on how to enable this feature, refer to step one of this [guide to getting started with campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns#goal-based-campaigns).

#### Sponsored Brands launches theme-based targeting

Regions: US

Sponsored Brands theme targeting provides a new method to control your campaign targeting. It uses advanced machine learning algorithms that take into account historical performance targets and brand interactions to create a large set of keywords. It then categorizes those keywords into relevant groups to use with your campaigns. Two targeting groups are available in this launch: “Keywords related to your brand” and “Keywords related to your landing pages.” These targeting groups will be pre-selected to simplify the campaign creation process for advertisers.

Advertisers can access this feature via the Amazon Ads API. For instructions on how to enable this feature, refer to this [developer guide](guides/sponsored-brands/theme-based-targeting).

#### Sponsored Brands performance metrics are now available in beta through Amazon Marketing Stream

Sponsored Brands datasets are now available globally in Amazon Marketing Stream. We are excited to expand the Sponsored Brands datasets in Amazon Marketing Stream to all countries currently supported by the Amazon Ads API. These datasets are currently available in beta. Amazon Marketing Stream delivers Amazon Ads campaign metrics and information to advertisers' or integrators' AWS accounts via a push-based model in near real time. This data will not be available for Sponsored Brands version 3 (legacy) campaigns.

Amazon Marketing Stream is already available globally for Sponsored Products and Sponsored Display campaigns and sponsored ads budget usage via the Amazon Ads API for agencies, tool providers, and direct advertisers, including vendors and sellers worldwide. When you’re ready to get started, visit the Amazon Marketing Stream [onboarding guide](guides/amazon-marketing-stream/onboarding).

For more information on the new Sponsored Brands datasets, see:

- [Sponsored Brands traffic](guides/amazon-marketing-stream/datasets/sb-traffic)
- [Sponsored Brands conversion](guides/amazon-marketing-stream/datasets/sb-conversion)
- [Sponsored Brands clickstream](guides/amazon-marketing-stream/datasets/sb-clickstream)
- [Sponsored Brands rich media](guides/amazon-marketing-stream/datasets/sb-rich-media)

#### Awareness: Campaigns created using Sponsored Brands version 3 are now stored as multi-ad group campaigns across all regions

Regions: All

As of August 10th, all campaigns created using the version 3 Sponsored Brands [POST /sb/campaigns](sponsored-brands/3-0/openapi/prod#tag/Campaigns) endpoint are stored as multi-ad group (version 4) campaigns. With this change, existing integrations should continue to work as intended. Advertisers, agencies, and tool providers can leverage the Sponsored Brands version 4 endpoints or the Amazon advertising console to create additional ad groups and ads.

Previously, these changes were [limited to the US marketplace](release-notes/index#awareness-campaigns-created-using-sponsored-brands-version-3-are-now-stored-as-multi-ad-group-campaigns) but we have completed the backend migration across all regions. For full technical details, see the [version 4 migration guide](reference/migration-guides/sb-v3-v4#june-2023-update).

#### Preview: Sponsored Products rest-of-search bid adjustment control

Advertisers will soon be able to apply bid adjustment controls for rest-of-search placements in their Sponsored Products campaigns. The new placement bid adjustment control for rest-of-search placements works in the same way as the adjustments available for top-of-search (first page) and product-page placements. Through the new control, advertisers will be able to adjust their bid for rest of search by up to 900% (same as the other two placements). This will be available for bulksheets as well as the API.

You can view the relevant API contract changes by looking at `dynamicBidding.placementBidding.placement` in [POST /sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredProductsCampaigns). Once available, you will be able to use the new enum `PLACEMENT_REST_OF_SEARCH` and add an adjustment percentage. This enum is currently in a read-only state, and we will update the release notes when you can start using this feature.

#### Automatic video localization now available for Sponsored Brands video creatives

Regions: DE, ES, IT, FR

Amazon Ads has made it easier for advertisers to create policy-compliant Sponsored Brands video campaigns in marketplaces where they are unfamiliar with the local language. Now, if advertisers add a video to their Sponsored Brands video ad that does not match with the language requirements of the country, they can opt to have their video auto-localized into the country’s local language. Localization takes up to 72 hours and the campaign goes live automatically once the video is localized. This localization service is offered to advertisers at no extra cost and comes quality assured by professional linguists.  At launch, the service supports English source videos into German, French, Italian, and Spanish.

Every month, thousands of campaigns are rejected due to advertisers submitting an English video for non-English speaking regions. This new feature helps advertisers overcome this friction related to building creatives in non-english languages, and enables more campaigns to be created.

The new `consentToTranslate` field is supported in the following endpoints:

* /sb/v4/ads/video
* /sb/v4/ads/brandVideo
* /sb/v4/ads/list
* /sb/ads/creatives/video
* /sb/ads/creatives/brandVideo
* /sb/ads/creatives/list

For full technical details, please see our updated documentation for [/sb/v4/ads/brandVideo](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsVideoAds) as an example.

#### Amazon Marketing Stream is now generally available for near real-time insights

Regions: All

We are excited to announce that Amazon Marketing Stream, which was first launched for North America as beta on June 21, 2022, is now generally available. Amazon Marketing Stream delivers Amazon Ads campaign metrics and information to advertisers’ or integrators’ AWS accounts via a push-based model in near real time. You can subscribe to Amazon Marketing Stream datasets using your API token and AWS account. Once you are subscribed, Amazon Marketing Stream delivers hourly, near real-time performance metrics and signals. Stream supports different categories of data: hourly performance reporting metrics, near real-time campaign management notifications, and push-based recommendations. Stream is available in all locations where the Amazon Ads APIs are supported.

In the past, access to scaled campaign reporting was limited to pull-based Amazon Ads API calls that provided daily performance, and on-demand access to other campaign information. API users who desired more real-time, intra-day campaign insights have manually pulled this information by calling Amazon Ads API multiple times a day, sometimes resulting in increased throttling. In addition, the current approach puts the burden on developers to identify changes between new and old data. Such API usage patterns highlighted the need to build a product that better addresses our customers’ evolving use cases.

Amazon Marketing Stream is available via the Amazon Ads API. When you’re ready to get started, visit our Amazon Marketing Stream [onboarding guide](guides/amazon-marketing-stream/onboarding) that will walk through the process of getting API access, integrating with AWS, and subscribing to campaign datasets using the Amazon Marketing Stream API.

### July 2023

#### Deprecation announcement: Legacy bulksheets will be deprecated on September 28, 2023

The legacy version of bulksheets will be deprecated on September 28, 2023, and will no longer be available for campaign management. Ahead of this date, we encourage you to [migrate to the new version](bulksheets/2-0/migration-guide) if you haven’t started already.

To help you through this transition, we’ve published a new set of bulksheets documentation, getting started guides, video tutorials, detailed instructions for creating and managing all sponsored ads campaigns, and other resources. Please check out these links to learn more:

* [Bulksheets overview](bulksheets/2-0/overview-about-bulksheets)
* [Migration guide](bulksheets/2-0/migration-guide)
* [Video tutorials](bulksheets/2-0/bulksheets-videos)
* [FAQs](bulksheets/2-0/faqs)

#### Deprecation announcement: Sponsored Brand versions 3 campaign management, draft, & media APIs will shut off on January 31, 2024

Sponsored Brands versions 3 campaign management, draft, and media APIs are being deprecated, with a target shutoff date of January 31, 2024. Please migrate to the new versions of campaign management endpoints as documented on our [Deprecations page](release-notes/deprecations) and in our [Migration guide](reference/migration-guides/sb-v3-v4). As a part of this deprecation, Sponsored Brands draft endpoints have reached their end of life and will **not** be replaced with new endpoints.

**Note**: This deprecation does **not** include Sponsored Brands [snapshots](reference/sponsored-brands/2/snapshots) or [reporting](sponsored-brands/3-0/openapi#tag/Reports) APIs.

The following endpoints and all associated HTTP operations are included in this deprecation:

* /sb/campaigns/
* /sb/adGroups/
* /sb/adGroups/{adGroupId}
* /sb/campaigns/{campaignId}
* /sb/drafts/campaigns
* /sb/drafts/campaigns/{draftCampaignId}
* /sb/drafts/campaigns/submit
* /media/upload

If you would like to share feedback, please use the Amazon Ads API [Service desk](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2) to open a ticket with subject title "SB v3 Campaign CRUD Deprecation" by August 15th, 2023.

#### Businesses that don’t sell in the Amazon store can now grow their business using Sponsored Display (beta)

Regions: US

We have extended Sponsored Display capabilities to advertisers who don’t sell in the Amazon store. Previously, Sponsored Display was only available for brands that sell products on Amazon. With an easy-to-use interface and no minimum spend requirements, advertisers can create compelling display ads in just a few clicks to help grow their business.

Advertisers who don’t sell on Amazon can now connect with relevant audiences for their business using Sponsored Display. With this launch, advertisers can:

* **Reach relevant audiences, in relevant locations, on and off the Amazon store:** Activate Amazon Ads’ exclusive audiences built from shopping and streaming signals. Choose postal code, city, designated market area (DMA), and/or state locations. Reach audiences on Amazon.com, Twitch, IMDb, and thousands of websites and apps beyond the Amazon store.
* **Drive visits to your site with visually-appealing creative formats:** Boost visibility and drive visits to your website with engaging display ads. All advertisers need is an image, headline, and logo to get started. We’ll do the heavy lifting and build ads optimized for a variety of placements, sizes, and devices to increase reach.
* **Easy and simple campaign set-up:** Create compelling display ads in just a few clicks, with a few basic inputs across bidding, targeting, creatives, and landing page destination.
* **Advertise with no minimum budget requirements:** With no minimum budget requirements, Sponsored Display unlocks the power of Amazon Ads for any self-service advertiser.

For full technical details, please see our new developer guide on [Sponsored Display for all advertisers](guides/sponsored-display/non-amazon-sellers/get-started). You can also explore all the calls needed to build an off-Amazon Sponsored Display campaign in our [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman).

#### Campaign and ad group IDs now available in Sponsored Brands purchased products API reports

Regions: All

Campaign ID and ad group ID are now available when you request a Sponsored Brands purchased products report. You can find these IDs in two new columns in the report. These IDs will not be available for Sponsored Brands version 3 (legacy) campaigns.

These IDs are important because they are unique identifiers that do not change over time, whereas the entity names can change. Without the IDs, advertisers would need to locate the IDs outside of the purchased products report, which can waste time and increase manual effort.

To use this feature, request the additional base metrics (`campaignId` and `adGroupId`) as part of the Sponsored Brands [purchased product report](guides/reporting/v3/report-types#sponsored-brands) generated via the [version 3 reporting API](offline-report-prod-3p).

#### Sponsored Brands missed opportunities and budget recommendations are now available for advertisers globally

Regions: CA, MX, UK, DE, ES, IT, FR, BE, PL, NL, SE, EG, TR, AE, SA, AU, JP, IN, SG, BR

Sponsored Brands advertisers in CA, MX, UK, DE, ES, IT, FR, BE, PL, NL, SE, EG, TR, AE, SA, AU, JP, IN, SG, BR can now access the missed opportunities and budget recommendations API (previously only available in the US), which provides budget recommendations for campaigns that have gone out-of-budget and estimates of missed business impressions, clicks, and sales.

Advertisers can leverage this service to understand the impact of out-of-budget campaigns and reduce the likelihood of going out-of-budget, which will help campaigns reach their full potential.

For full technical details, see the [API reference](sponsored-brands/3-0/openapi/prod#tag/Budget-Recommendations/operation/GetBudgetRecommendations).

#### Sponsored Brands brand video ads now allow a collection of products

Regions: All (except BE)

We have enhanced Sponsored Brands brand video ads so that advertisers can highlight a collection of products to direct shoppers to their brand store. Prior to this launch, advertisers could only promote one product with their brand video ads. Now, advertisers can highlight a collection of products (up to three products) when creating a new campaign.

Stores are more discoverable to shoppers, as they are the primary method for advertisers to showcase brands and products in a multi-page, immersive shopping experience on Amazon. Stores allow brands to curate content that inspires, educates, and helps shoppers discover the brand’s entire product selection, taking shoppers from inspiration to action.

For full technical details, see the updated documentation on [Sponsored Brands brand video ads](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsBrandVideoAds). This feature is only available on the Sponsored Brands version 4 Amazon Ads APIs. If you have not yet migrated from Sponsored Brands version 3 to Sponsored Brands version 4, please review our [migration guide](reference/migration-guides/sb-v3-v4).

#### Improved OpenAPI specification rendering

Regions: All

We have updated our rendering UI for OpenAPI specifications to provide an easier browsing experience for developers. The new layout allows easier navigation across the different methods and API endpoints. With this change, we have added a consistent layout across the schemas, fixed deep indentation when expanding and collapsing schemas, added the ability to view schemas and examples at the same time, and enabled easy copying of samples and parameters. API reference pages are also now mobile friendly and accessible.

To explore the new layout, check out any of our [API reference documents](reference/openapi-download). If you have any feedback, please leave a comment in the related [GitHub discussion](https://github.com/amzn/ads-advanced-tools-docs/discussions/75#discussioncomment-6489608).

#### Omnichannel Metrics fee launches for US Consumer Packaged Goods advertisers

Regions: US

As of June 26, 2023, Omnichannel Metrics (OCM) charges a usage fee of “4% of media spend”.

To calculate the OCM fee, 4% will be applied to the "Supply cost" in Amazon DSP and only applied to impressions where OCM is enabled. For example, if an order is booked to run from 8/1/23 to 8/30/23, and OCM is enabled on 8/15/23, you will only be charged for impressions that run from 8/15/23 to 8/30/23. Advertisers using OCM will be billed on a monthly basis following standard invoicing timelines.

[amazonOmnichannelMetricsFee](guides/reporting/dsp/metrics#amazonOmnichannelMetricsFee) can be found while setting up OCM in the Amazon DSP Studies page. Once OCM is enabled, the fee can be accessed via Amazon DSP Campaign manager, Report center, and Amazon Ads API for integrators.

OCM enables you to measure the impact of your ad tactics on holistic shopping activities across retail outlets, on-Amazon and off-Amazon) while campaigns are mid-flight. Through our automated budget optimization feature, you can reallocate budgets to better performing line-items before the campaign ends, to help improve the overall campaign ROAS. OCM launched for general availbility in October 2022 for all US CPG advertisers.

[amazonOmnichannelMetricsFee](guides/reporting/dsp/metrics#amazonOmnichannelMetricsFee) can be accessed via Amazon DSP Campaign manager, Report center, and Amazon Ads API for integrators.

#### Chinese and Japanese translations now available for bulksheets downloads

Bulksheets downloads are now available in Chinese and Japanese languages. If you have one of these selected as the language in your advertiser profile, you can download a bulksheets file in the selected language. All headers, error messages, and other text will be translated. Some text, such as campaign and ad group names, will remain unchanged. For bulksheets uploads, you can use Chinese, Japanese, or English, regardless of the language selected in your profile. For bulksheets uploads, you can use Chinese or Japanese if these are selected in your profile, or you can upload in English, regardless of the language selected in your profile.

### June 2023

#### Amazon DSP launches deals creation and editing API feature

Regions: US, CA, MX, UK, DE, ES, IT, FR, AU, UAE, JP, BR, NL, TR, SE, IN, SA

Amazon DSP is launching several new endpoints available through the Amazon Ads API to enable advertisers to manage their Amazon DSP programmatic deals:

* `POST /dsp/inventory/deals` - Creates a third party deal object
* `PUT /dsp/inventory/deals/{dealId}` - Updates a third party deal object
* `GET /dsp/inventory/deals/{dealId}/advertiserPermissions/advertiserList` - Gets list of advertisers assigned to the deal and its advertiser permissions strategy type
* `PUT /dsp/inventory/deals/{dealId}/advertiserPermissions/advertiserList` - Updates advertiser permissions by adding/removing advertisers to/from the advertiser permission list
* `POST /dsp/inventory/deals/{dealId}/advertiserPermissions` - Assigns permissions strategy to deal object
* `GET /dsp/inventory/deals/{dealId}/advertiserPermissions` - Gets the advertiser permissions strategy type for a given deal
* `DELETE /dsp/inventory/deals/{dealId}/advertiserPermissions` - Deletes both the permission strategy and associated advertisers from the deal advertiser permissions

Though these features were previously available in the Amazon DSP console, advertisers can now access them programmatically through the Amazon Ads API. This helps bring the Amazon Ads API experience in line with the advertising console, and gives advertisers greater flexibility when managing Amazon DSP programmatic deals.

With these additional Amazon Ads API endpoints, advertisers can create and update their Amazon DSP programmatic deal libraries, and build their own business applications leveraging these objects. They can now push their own deals to their Amazon DSP deal library, and assign specific advertiser permissions to each deal.

This release supports all standard deal types, except Amazon Audience Guaranteed deals, which users will need to continue creating/updating via their Amazon DSP console. For full technical details, please see our [deal create and update API documentation](dsp-create-update-deals) and [deal advertiser permissions API documentation](dsp-deals-permissions).

#### Campaign signals now available on Amazon Marketing Stream

Regions: All

Amazon Marketing Stream (beta) expands to include new sponsored ads campaign datasets. This includes information on Sponsored Products, Sponsored Brands, and Sponsored Display campaigns, ads, ad groups, and targeting.

These signals enable Amazon Ads advertisers and partners to be notified in near-real-time via Amazon Marketing Stream when sponsored ads campaign data changes (e.g., when campaigns pause, budgets change, or bids update) and what the modification was across thousands of advertising campaigns. Amazon Marketing Stream can help improve information integration patterns and operational efficiency by reducing the number of API calls that are typically required for intra-day campaign optimization. Advertisers and partners can use this dataset to react in near real-time to events that happen, such as campaign status changes, rather than run multiple intraday snapshots. Amazon Marketing Stream helps simplify the extract, transform, and load operations compared to snapshots, by showing you only information that has changed, when it occurs.

The new campaign datasets are available to advertisers and partners using [Amazon Marketing Stream](guides/amazon-marketing-stream/overview) (beta). This is available worldwide, with campaign datasets available by [AWS region](guides/amazon-marketing-stream/overview#geographic-availability).

The sponsored ads campaign data is split into four datasets: campaigns, ad groups, ads, and targets for each [AWS region](guides/amazon-marketing-stream/overview#geographic-availability). You can access it following the instructions for [Amazon Marketing Stream Onboarding Guide](guides/amazon-marketing-stream/onboarding), and [subscribe to the new campaign datasets](guides/amazon-marketing-stream/managing-subscriptions).

#### Bulksheets column header name and value updates for readability

We've updated the names of some column headers and values to make them more readable. For example, in the Portfolios sheet, the `Portfolio Id` heading is now `Portfolio ID` and in the `Budget Policy` column, the `noCap` value is now `No Cap` and the `dateRange` value is now `Date Range`.

You'll see these new heading and values in any new sheets that you request. Note that previous and new column header values will continue to function as before in bulk upload, and there is no need to update old sheets to the new headings and values.

We're currently updating the documentation and we'll continue to document the previous headers and values and add the new headers and values alongside to make these updates clear.

#### Rapid retail analytics

Regions: AE, AU, BE, BR, CA, DE, EG, ES, FR, IT, JP, IN, MX, NL, PL, SA, SE, SG, TR, UK, US

Rapid retail analytics, available through the [Selling Partner API](https://developer.amazonservices.com/), allows advertisers to unlock hourly retail metrics. Analytics are available minutes after the close of an hour. This provides access to near real-time retail signals like ordered units and glance views to help advertisers measure performance and adjust their marketing and advertising activities.

Partners and advertisers can leverage this timely information to add a never-before-available layer of measurement and insights to improve ASIN-level analytics, automate campaign changes, and inform cross retail-ads optimizations. Tool providers can add advertiser value by using rapid retail analytics to automatically increase budget allocated to an ASIN when hourly sales are below an expected range. They can also build their own specific strategies, such as ingesting advertiser forecasts.

Rapid retail analytics is exclusively available through the Selling Partner API, with full details [here](https://developer-docs.amazon.com/sp-api/docs/sp-api-release-notes#june-21-2023). If you are not yet using the Selling Partner API, see the [getting started guide](https://developer-docs.amazon.com/sp-api/docs/registering-as-a-developer).

#### Updated: Sponsored ads daily budgeting policy and options

Regions: All

[Previously we announced](release-notes/index#new-sponsored-ads-daily-budgeting-policy-and-options) updates to our daily budgeting policy that allow you to spend up to 100% more than your average daily budget. This update enables you to benefit from high-traffic days by using budgets from previous days when your spend is below your daily budget. The daily budget amount is averaged over the course of a month.

We have now updated the default rollover value for all accounts to be 100%. This change only impacts accounts that have not yet selected a default, either through the API or the advertising console. If you have already chosen either 25% or 100%, you won't experience any change in your settings. You can set your default to 25% using the [POST /accountBudgets/featureFlags](account-budgets#/Advertiser/updateAccountBudgetFeatureFlags) endpoint.

For more information, view the [User guide](guides/account-management/average-daily-budget).

#### Awareness: Campaigns created using Sponsored Brands version 3 are now stored as multi-ad group campaigns

Regions: US

As of May 22, any campaigns created using the version 3 Sponsored Brands POST /sb/campaigns endpoint are stored as multi-ad group (version 4) campaigns. With this change, existing integrations should continue to work as intended. Advertisers, agencies, and tool providers can leverage the Sponsored Brands version 4 endpoints or the Amazon advertising console to create additional ad groups and ads. This change is currently only in place for US advertisers. We will expand this change to advertisers in other regions in the coming months.

See the [version 4 migration guide](reference/migration-guides/sb-v3-v4#june-2023-update) for additional details.

#### Vendor test accounts now available for Belgium marketplace

Regions: BE

Vendor test accounts are now available for Belgium through the Amazon Ads API. For an updated list of supported marketplaces for both vendor and author test accounts, see [Creating test accounts](test-accounts/create-test-accounts) in the advanced tools center.

#### A new look for the advanced tools center

We've released a new user interface for the advanced tools center, featuring an updated look and feel as well as a new navigation. The navigation is now responsive, and readable on mobile devices.

Documentation is split into categories based on use case:

* [Developer guides](guides/overview)—featuring tutorials, product overviews, and code samples for developers
* [API reference](reference/api-overview)—containing OpenAPI specifications and conceptual content about REST APIs
* [No-code tools](bulksheets/no-code-overview)—including bulk operations documentation

You can also find a dedicated section for [Support](support/overview) and [Release notes](release-notes/index) for all advanced tools.

If you have any feedback on the new design, we’d love to hear from you in [GitHub](https://github.com/amzn/ads-advanced-tools-docs/discussions/75#discussioncomment-6186446).

#### Top-of-search impression share available through the Amazon Ads API

Regions: US, CS, MX, UK, DE, ES, IT, FR, BE, PL, NL, SE, EG, TR, AE, AU, JP, IN, SG, BR, SA

We added a new top-of-search impression share metric for Sponsored Products and Sponsored Brands to reporting in the Amazon Ads API. This metrics helps brands measure the effectiveness of their campaigns and keywords at driving brand awareness at scale.

Integrators can now access the top-of-search impression share metric daily through the API. You can apply the metric to your systems to power notifications, automations, and other functionality. This helps support meeting top-of-search impression share goals on critical campaigns and keywords. These metrics can help brands accelerate brand strategy analysis and optimization as well as customer acquisition.

The new top-of-search impression share metric is available at the campaign and keyword level through the sponsored ads reporting APIs. For Sponsored Products, the metric is available in sponsored ads [version 3 reporting](guides/reporting/v3/report-types). For Sponsored Brands, the metric is available through the sponsored ads [version 2 reporting](guides/reporting/v2/report-types). For more information on the metric and its report availability, see `topOfSearchImpressionShare` in [version 3](guides/reporting/v3/columns#topOfSearchImpressionShare) and [version 2](guides/reporting/v2/metrics#topOfSearchImpressionShare).

### May 2023

#### Amazon DSP launches expanded off-Amazon conversion import and management options

Regions: All

Advertisers tracking conversions occurring on their owned website, app, or offline property (such as an in-store environment) can now leverage two new options for conversion information delivery. Amazon Advertising tag can be used for tracking user actions taken on an advertiser-owned website. In addition to website conversions, Conversions API can accept events occurring in an advertiser-owned app, or offline property, such as an in-store environment through a server-to-server connection with the advertiser’s server or an integrated third party server. In addition to conversion signal delivery, the Conversions API allows advertisers to create and manage their conversion definitions, as well as access a user information deletion feature to assist advertisers with privacy-compliance for newly opted out users.

Both sources support the use of durable identifiers, such as consented hashed email and are offered as durable alternatives to Simple Pixel (to be deprecated by the end of 2023). Conversions API is a great option for customers who host their event information themselves or in a third party server and prefer not to implement a third party tag on their website. Conversions API newly introduces the ability to import off-Amazon offline transactions, i.e. an end-user making a purchase in a store environment.

Conversions API and corresponding features will be available to managed service and self-serve Amazon DSP advertisers world-wide. Tracked conversions can be viewed and managed in the Amazon DSP console’s Conversions page. To use the conversions for campaign measurement, conversions can be associated with orders using a new ‘Off-Amazon conversions’ widget when creating orders in the Amazon DSP console. Conversions signals will also be made available as seed information for audience building in the Amazon DSP console within Audiences.

To start importing conversions, customers will use the Amazon DSP API to create a new object called a conversion definition. Conversion definitions enable advertisers to associate additional information about the event, such as fallback value or processing preferences. When importing events, each event will be mapped to a conversion definition using a conversion definition ID. For full technical details, please read [the API reference](dsp-conversion-builder).

#### Announcing the tactical recommendations API (beta) for sponsored ads

Regions: All

Partners and advertisers can now use the tactical recommendations API to get suggestions for their Sponsored Products, Sponsored Brands, and Sponsored Display campaigns. Recommendations are made by Amazon Ads machine learning algorithms or account management teams, and each recommendation includes the campaign setting changes needed to execute the recommendation. Example recommendation types include campaign optimizations like recommended bids, budgets, or targets, as well as suggested settings for creating new campaigns.

The tactical recommendations API contains three new endpoints:

* [POST /recommendations/list](recommendations#/Recommendations/ListRecommendations) to retrieve recommendations
* [POST /recommendations/update](recommendations#/Recommendations/UpdateRecommendation) to update recommendations
* [POST /recommendations/apply](recommendations#/Recommendations/ApplyRecommendations) to automatically apply recommendations to your account

For more information, view the [API reference](recommendations), [Overview](guides/recommendations/tactical-recommendations/overview), and [Getting started guide](guides/recommendations/tactical-recommendations/get-started).

#### New fields in Sponsored Brands Keyword Recommendation response

Regions: US, CA, UK, DE, ES, IT, FR, PL, NL, SE, UA, SA, AU, JP, IN, SG, BR, MX

New metrics are now available for the Sponsored Brands keyword recommendation through the Amazon Ads API. You can now receive search term impression share and search term impression rank for each recommended keyword. Search term impression share and rank are only available if your account has generated impressions for search terms matching the recommended keywords.

These metrics help with making informed keyword selections by identifying potential keywords to target based on your impression share and impression rank of search terms. Search term impression share shows how your account-wide impression share for each search term compares to other advertisers, and the overall percentage of ad impressions you receive compared to other advertisers over last 7 days. For example, if you have a Sponsored Brands impression share of 20% for a search term, it means that you won 20% of all Sponsored Brands ad impressions for that search term. Similarly, search term rank helps you understand the level of activity on terms that you're targeting. For example, if you have a search term impression rank of 3 on a search term, it means that you received the third most Sponsored Brands ad impressions for the same search term over the last 7 days.

For full technical details, please see the [API reference](sponsored-brands/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getKeywordRecommendations).

#### Brand Metrics expands to include India, Australia, and Mexico marketplaces.

Marketplaces: US, CA, UK, IT, FR, DE, ES, JP, IN, AU, MX

Brand Metrics is a new measurement solution that quantifies opportunities for your brands at each stage of the customer journey on Amazon, and helps brands understand the value of different shopping engagements that impact stages of that journey. With Brand Metrics you can now access awareness and consideration indices that compare brand performance to peers using models predictive of consideration and sales. Brand Metrics is built at scale to measure all shopping engagements with your brand on Amazon, not just ad-attributed engagements and quantifies the number of shoppers in the awareness, consideration, and purchase stages of the shopping journey with your brand. Brand Metrics also provides Return on Engagement, so you can measure the average historical sales following a consideration event or purchase.

Brand Metrics helps you...

* **Understand** your brand performance. Brand Metrics measures the number of actual on-Amazon shopper engagements.
* **Measure** the impact of your upper and mid funnel tactics to see how they contribute to shoppers moving through the purchase journey.
* **Evaluate** engagement metrics to understand the value of your branded intent, and how brand purchasers go on to generate additional sales in the 12 months following a purchase.
* **Monitor** your performance relative to your category and peers at each stage of the purchase journey and over time.
* **Optimize** your marketing and advertising efforts on Amazon to engage more shoppers and build your brand.

This release features two new endpoints - [/insights/brandMetrics/report](brand-metrics-openapi/#tag/Report/operation/generateBrandMetricsReport) and [/insights/brandMetrics/{reportId}](brand-metrics-openapi/#tag/Report) to generate and retrieve Brand Metrics report in CSV or JSON format.  With these endpoints the developers can now create customizable reports by passing in a date range, a brand name, or by passing in specific metrics from a list of Brand metrics.

For full technical details please see our [overview](guides/reporting/brand-metrics/overview) and [Brand Metrics reference documentation](brand-metrics-openapi) in the Amazon Ads API.

#### Awareness: Sponsored Display will be introducing dynamic client-level throttling

On 05/22/2023, Sponsored Display will introduce dynamic client-level throttling based on the most common partner traffic patterns. These limits are determined on a per-region basis with multiple limit tiers that correspond with our existing service capacity. With these changes, Sponsored Display’s campaign management client level throttling will be more consistent with Sponsored Products V3 API’s.

Please note that different time periods of higher utilization may occur each day. Keep this in mind if you encounter a higher occurrence of 429 HTTP response codes. For more information, see our updated [rate-limiting](reference/concepts/rate-limiting) documentation. Rate limits in the Amazon Ads API are dynamic and based on system load. If you experience frequent rate limits, you can decrease this by following our [best practices](reference/concepts/rate-limiting#avoiding-rate-limits-for-reports) to avoid rate limiting.

If you have any questions, visit the [Amazon Ads API Support page](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/create/5) and create a ticket with the subject title: "Sponsored Display dynamic client-level throttling."

#### Sponsored Display audiences now available for advertisers in Belgium

Regions: BE

[Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display) has extended audiences targeting for registered sellers and vendors to Belgium, enabling access to views and purchases remarketing. These targeting strategies enable advertisers to reach new prospective customers through both awareness and consideration campaigns using custom lookback windows.

With the launch of Sponsored Display audience targeting in Belgium, registered sellers and vendors can customize the audiences they want to reach based on customer shopping signals and granular product refinements. For example, an advertiser can engage with audiences who searched, viewed, or purchased a collection of products on Amazon and reach them again on and off Amazon. Advertisers using Sponsored Display can also customize their creatives choosing any combination of video, headline, logo, or lifestyle image to help audiences discover their brands and products.

For full technical details, see our Sponsored Display [targeting documentation](sponsored-display/3-0/openapi#tag/Targeting).

#### Keeping users informed about the latest service status information with Amazon Ads Status (beta)

We are excited to launch [Amazon Ads Status (beta)](https://status.ads.amazon.com/), a new offering that provides all advertisers with the latest status information on Amazon Ads services. Advertisers will be able to see the current status of services like campaign creation, campaign management, ad delivery, and reporting on sponsored ads and Amazon DSP. Should there be a service interruption, advertisers can see more details for the event, including the description and status updates for that service.

In the past, there has not been a dedicated location where advertisers or API integrators could look to get the current status of Amazon Ads services. During any service interruption, advertisers could only get that information after they log in to the advertising console or Amazon DSP. With the introduction of [Amazon Ads Status (beta)](https://status.ads.amazon.com/), users can now view the current status of Amazon Ads services and be updated with the latest information during any potential service disruptions, without the need to log in to the advertising console or Amazon DSP.

The ad status information can be accessed programmatically using [https://status.ads.amazon.com/status.json](https://status.ads.amazon.com/status.json), which returns Amazon Ads Status in JSON format. The JSON schema is also available using [https://status.ads.amazon.com/status.schema.json](https://status.ads.amazon.com/status.schema.json).

Questions or comments on this feature? We'd love to hear your feedback in [GitHub](https://github.com/amzn/ads-advanced-tools-docs/discussions/85).

#### Get new insights about your Store from ASIN engagement metrics

Regions: US, CA, MX, UK, DE, ES, EG, PL, IT, FR, AU, AE, JP, BR, NL, TR, IN, BE, SG, SE, SA

We are introducing new ASIN engagement metrics for select brand Stores\* using the Amazon Ads API. With these metrics, eligible brands can understand how shoppers are engaging, and to what degree, with products listed on their brand Store. Metrics include ASIN renders, views, in-stock views, in-stock view rate, average in-stock view price, clicks, click-through rate, add-to-cart rate, purchases, units sold, conversion ratio, and average sales price.

Store engagement metrics allow brands to better understand how shoppers interact with ASINs featured on their Store. Brands can use this signal over time to help increase shopper engagement, improve their store merchandising, keep high performing products in stock, and help boost overall Store performance.

The ASIN engagement API returns key ASIN engagement insights for a given `brandEntityId`. For full technical details, see the [API reference](stores/open-api#/Stores%20Analytics/getAsinEngagementForStore).

\*Not all brand Stores will have access to ASIN engagement metrics at launch; we’re working on a phased rollout plan to additional Stores in 2023.

#### More control over auto-targeting campaigns in bulksheets

When you create Sponsored Products campaigns with automatic targeting, you can now create the four product targeting entities and apply a custom bid to each targeting expression (close-match, loose-match, complements, and substitutes). You can also set the targeting entities to be paused if you don’t want to apply a bid to a specific target. If you want Amazon to automatically create these targeting expression entities, you can choose not to create them, and one targeting entity will be created for each expression type with the ad group default bid applied. Learn more about creating [Sponsored Products auto-targeting campaigns](bulksheets/2-0/create-sp-campaign).

### April 2023

#### Search term reports for Sponsored Products in bulksheets

Search term report data is now available for Sponsored Products in bulksheets. This report will be in a new tab and will include much of the same information you would find in the advertising console report, including the customer search terms that were used to discover your ads—-but in bulksheets, you will also have the entity IDs available, so you can quickly make adjustments if needed.

You can include a search term report in bulksheets if you go to the bulk operations main page, create a custom spreadsheet, and check the box labeled **Sponsored Products search term report** in Step 1.

Learn more about [search term reports in bulksheets](bulksheets/2-0/bulksheets-search-term-report)

#### Awareness: Sponsored Display campaigns with no delivery could be automatically paused

Starting September 30, 2023, any [Sponsored Display campaigns](https://advertising.amazon.com/solutions/products/sponsored-display) that have not delivered any viewable impressions, clicks, or conversions over a 6-month look-back period could be automatically paused. This will allow advertisers and partners to focus on the campaigns that are driving outcomes.

To help campaigns deliver results, Sponsored Display has features to create high-performance campaigns based on the outcomes they’ve selected. Advertisers and partners can create campaigns that are optimized for reach (to increase awareness), page-visits (to increase consideration), or conversions (to increase sales).

We suggest adopting the following tools to help Sponsored Display campaigns deliver viewable impressions, clicks, or conversions (depending on how the campaigns are optimized):

* [Bid recommendations API](sponsored-display/3-0/openapi#tag/Bid-Recommendations)
* [Targeting recommendations API](sponsored-display/3-0/openapi#tag/Targeting)
* [Budget usage API](sponsored-display/3-0/openapi/prod#tag/Budget-Usage)
* [Campaign diagnostic recommendations through Amazon Marketing Stream](guides/amazon-marketing-stream/data-guide#sponsored-ads-campaign-recommendations-sponsored-ads-campaign-diagnostics-recommendations)

If you have a question, you can go to the [Amazon Ads API Support page](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/create/5) and create a ticket with the subject title: "Sponsored Display pausing campaigns with no delivery."

#### A new search experience in the advanced tools center

We’ve introduced a new search UI to improve discoverability in the advanced tools center. The new design makes it easier to sort based on the type of content you’re looking for: Guides & tutorials, API reference documentation, or release notes. You can also see your past searches.

We’ve also improved our search algorithm to cover more content and enabled fuzzy match, so even if your search term doesn’t match exactly, you should see results.

Give it a try and let us know if you have any feedback in [GitHub](https://github.com/amzn/ads-advanced-tools-docs/discussions/75#discussioncomment-5723315).

#### Introducing a new homepage for the advanced tools center

We’ve improved the design of the advanced tools center homepage to make it easier to discover resources and technical documentation.

From the new homepage you can:

* Easily access video tutorials, courses, user guides, and events.
* Quickly navigate to popular content and see what’s new.
* Compare our different advanced tools offerings from a single page.

Check out the [new homepage](index) now, and if you have any feedback, we’d love to hear it in [GitHub](https://github.com/amzn/ads-advanced-tools-docs/discussions/75).

#### New metrics available for Sponsored Brands performance-based budget rules

Regions: All

In [July 2022](release-notes/archive/ads-api#announcing-the-availability-of-performance-based-budget-rules-api-for-sponsored-brands), we expanded the budget rules API to support performance-based budget rules for Sponsored Brands campaigns using return on ad spend (ROAS). We have now added two new metrics for Sponsored Brands performance-based budget rules: top-of-search impression share (`metricName` is `IS`) and percent of sales new-to-brand (`metricName` is `NTB`).

Budget rules help you plan budget changes in advance so you can spend less time making manual adjustments. You can set rules based on specific dates or performance goals. Advertisers can use budget rules to reduce the likelihood of campaigns running out of budget, especially during peak days or holiday events.

For more information, see:

* [Sponsored Brands budget rules API reference](sponsored-display/3-0/openapi/prod#tag/BudgetRules)
* [Sponsored Brands budget rules overview](guides/rules/budget-rules/overview)
* [Sponsored Brands performance-based budget rules](guides/rules/budget-rules/performance-based)

#### Sponsored Display launches recommendations for Amazon audiences in Amazon Ads API

Sponsored Display has launched targeting recommendations for Amazon audiences. Amazon audiences is a catalog of pre-built, easy-to-use audience segments that help advertisers discover and reach new customers.

Advertisers can now find suggestions on Amazon audiences to help discover new customers based on their brand’s core customers. For example, if an advertiser sells camping accessories, they can select the recommended audience “Outdoor Enthusiasts.” These recommendations are built by Amazon Ads and informed by a variety of first-party shopping signals. Self-service advertisers can now directly connect with audiences relevant to their promoted products by selecting the recommendations. These recommendations are now available for Sponsored Display audience targeting campaigns within the Amazon Ads API.

Advertisers can now find relevant suggestions on audiences across four segments: in-market, lifestyle, interests, and life events that are informed by Amazon’s first-party shopping and streaming signals.

For full technical details, please see our [Sponsored Display documentation](sponsored-display/3-0/openapi#tag/Targeting-Recommendations), including the updated targeting recommendation documentation.

**Note**: If you are already specifying a version in the `Accept` request-header field, you must update to `application/vnd.sdtargetingrecommendations.v3.3+json` to access the new features.

#### New Amazon Sponsored Brands and creative asset library documentation

We’ve released new overview and tutorial content for Sponsored Brands and the creative asset library API. This content helps you create a Sponsored Brands campaign and understand its structure, the different ad formats, and how to upload and register creatives.

Find all new Sponsored Brands content under [Sponsored Brands > Overview](guides/sponsored-brands/overview). Find all new creative asset library API content under [Common resources > Creative Asset Library beta](guides/creative-asset/asset-library-overview).

Also see our updated [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman), where we’ve added new Sponsored Brands and creative asset library request templates.

### March 2023

#### Budget usage percentage now generally available for real-time pulls in Amazon Ads API

Regions: All

With the new budget usage feature now available to API users without access restrictions, users worldwide can pull their real-time budget usage percentage for Sponsored Products, Sponsored Brands, and Sponsored Display using the Amazon Ads API. You can pull budget usage information for both campaigns and portfolios.

Your budget usage percentage helps you understand how fast your campaigns and portfolios are spending daily budgets, and can give you the information needed to make quick budget decisions.

For more information, see the budget usage resource in the [Sponsored Products reference](sponsored-products/3-0/openapi/prod#tag/Budget-Usage), [Sponsored Brands reference](sponsored-brands/3-0/openapi/prod#tag/Budget-Usage), [Sponsored Display reference](sponsored-display/3-0/openapi/prod#tag/Budget-Usage), or [Portfolios reference](reference/portfolios#/Budget%20Usage). You can also test the budget usage endpoints using our [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman).

#### Sponsored Display launches video creative capabilities for advertisers in Belgium, Egypt, Poland, Saudi Arabia, Singapore, and Turkey

Regions: BE, EG, SA, SG, PL, TR

[Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display) has expanded video creative capabilities for advertisers in Belgium, Egypt, Poland, Saudi Arabia, Singapore, and Turkey. The new video format empowers advertisers to showcase their products and brand through immersive storytelling (e.g., tutorials, demos, unboxing, and testimonials). Advertisers using Sponsored Display audiences or contextual targeting can build awareness and consideration of their products and brand on and off Amazon using a video ad format.

Advertisers can now choose to customize creatives using a headline and logo, image, or video through Sponsored Display’s self-service display solution that helps grow their business with Amazon across more marketplaces. The new video creative capability supports videos up to 45 seconds so advertisers can better tell their stories to customers, helping them understand the product and brand with more engaging content.

Video creatives can help advertisers tell better stories while serving ads on and off Amazon in their own visual language, helping shoppers get excited about the brand and product experience. If insights on creative are available that helps drive traffic or engagement, advertisers can use that creative for their Amazon Ads campaigns. Ads will link directly to the advertiser’s product detail pages, so audiences can learn more about their product and consider purchasing.

For full technical details, please see our updated [Sponsored Display documentation](sponsored-display/3-0/openapi#tag/Ad-groups/operation/createAdGroups), including contextual targeting support for `creativeType` of `VIDEO`.

#### New Amazon DSP reporting documentation

We’ve released new overview and tutorial content for the DSP reporting API.

This content helps you:

- [Request your first report](guides/reporting/dsp/get-started).
- Understand DSP reporting concepts such as [dimensions](guides/reporting/dsp/dimensions) and [reporting according to your account type](guides/reporting/dsp/reporting-by-account-type).
- Get answers to [frequently asked DSP reporting questions](guides/reporting/dsp/faq).
- Reference a consolidated lists of DSP [report types](guides/reporting/dsp/report-types) and [metrics](guides/reporting/dsp/metrics).

Find all new reporting content under [Reporting > Amazon DSP](guides/get-started/first-call).

See also our updated [Postman collection](https://github.com/amzn/ads-advanced-tools-docs/tree/main/postman). We've included new DSP report request templates for each DSP report type.

#### New sponsored ads daily budgeting policy and options

Regions: All

We have updated our daily budgeting policy for Sponsored Products, Sponsored Brands, and Sponsored Display, allowing you to spend up to 100% more than the average daily budget on any given day. This update enables you to benefit from high-traffic days by using budgets from previous days when your spend is below your daily budget. The daily budget amount is averaged over the course of a month.

This feature is available for vendors, sellers, and authors, and the policy change is effective worldwide for sponsored ads.

You can also choose to set the additional spend from leftover amounts to 25% or 100% for all your campaigns using the average daily budget settings API. For more information, view the [user guide](guides/account-management/average-daily-budget) and [API reference](account-budgets).

#### Active budget rules on a campaign now increase your daily budget independently of other rules

As [announced at the end of January](release-notes/index#awareness-budget-rules-are-becoming-additive), additive budget rules are now live for users of the Amazon Ads API and advertising console. Now, if multiple budget rules meet their conditions, all rules are applied and increase daily budget independently.

This change will impact all advertisers worldwide (vendors and sellers) who use budget rules for their Sponsored Products, Sponsored Brands, and Sponsored Display campaigns through the Amazon Ads API or advertising console.

You can learn more about additive budget rules in the user guide associated to your product ([Sponsored Products](guides/rules/budget-rules/campaigns-api), [Sponsored Brands](sponsored-brands/budget-rules/campaigns-api), [Sponsored Display](guides/rules/budget-rules/campaigns-api)).

#### Access valuable brand Stores insights from the Amazon Ads API

Regions: US, UK, DE, FR, JP, CA, IT, ES, IN, AU, NL, AE, SA, TR, BR, MX, SG, EG, BE, SE, PL

We are introducing a new brand Stores insights API you can use to programmatically to access Stores insights. These insights include performance metrics like number of store visitors, sales, orders, and page views. This information can be combined with other ads data for you to get holistic view of your Store’s performance.

Historically, Store owners could only access insights through the Amazon advertising console one store at a time. With introduction of the Store insights API, Store owners can access the same information through the Amazon Ads API. This is especially helpful for getting Store insights across different countries (or locales).

The insights API returns key Store insights such as views, orders, units purchased, and store visits for a given `brandEntityId`. Advertisers are also able to filter the results based on date range, Store page, and source of traffic such as ads, organic traffic, and custom tags. The API response is paginated a max number of documents returned of 1500.

For full technical details, see the [API reference](stores/open-api#/Stores%20Analytics).

#### Improve campaign impression delivery using the Sponsored Brands campaign insights API

Regions: US, CA, MX, UK, DE, ES, PL, IT, FR, AU, AE, JP, BR, NL, TR, SE, IN, SA, SG

Sponsored Brands campaign insights provides advertisers with new metrics and insights to inform Sponsored Brands campaign creation. This feature:

* Provides search term impression share and search term impression rank for keywords.
* Informs an advertiser if selected keywords have low search traffic.
* Informs an advertiser if bids have a low probability of winning an auction.

Search term impression share indicates the percentage share of all ad-attributed impressions an advertiser received on that keyword in the last seven days and search term impression rank indicates an advertiser’s ranking among all advertisers for a keyword by ad impressions in a locale or country. Search term metrics are only available for keywords the advertiser has previously targeted with Sponsored Brands ad impressions.

Campaign insights enable advertisers to make informed keyword selections and productive bids during the campaign creation process. The search term metrics can help advertisers identify potential keywords to target. The keyword and bid insights can help advertisers understand if they should make adjustments to their keyword selections or bid amounts. These new metrics and insights can improve an advertiser’s ability to win and deliver impressions.

Vendors and sellers can use the new [POST /sb/campaigns/insights](sponsored-brands/3-0/openapi/prod#tag/Insights/operation/SBInsightsCampaignInsights) endpoint to send a request. Currently, only keyword targets are supported.

#### Sponsored Ads campaign recommendations in Amazon Marketing Stream worldwide

Amazon Marketing Stream (beta) has added a new dataset, surfacing sponsored ads campaign recommendations to partners worldwide. With this release, partners can now receive targeted recommendations that they can use to manage campaigns more efficiently. These recommendations not only highlight opportunities to improve the performance of existing ad campaigns or create new ad campaigns, but also surface the suggested campaign settings to take action on those opportunities. These recommendations have been available to advertisers in the advertising console and we are now expanding access through Amazon Marketing Stream worldwide.

The sponsored ads campaign recommendations come in the form of pre-packaged campaign setting changes suggested by Amazon Ads machine learning algorithms, based on a diagnosis of the advertiser’s catalogue and campaigns (e.g., campaigns that have opportunity to improve ROAS compared to similar campaigns by optimizing bids, or unadvertised ASINs that can drive incremental sales if added to an ongoing campaign). Each recommendation will have an insight on how the opportunity was identified and details on the inputs for the recommendation. The campaign setting changes are delivered as campaign optimizations including recommended bids, budgets, or targets, as well as suggested settings for creating a new campaign  After subscribing to this dataset on Amazon Marketing Stream, partners receive these recommendations daily. Partners can easily update or apply the recommendations in bulk by calling the [tactical recommendations API](recommendations) and passing the recommendation ID. Partners can also use the sponsored ads campaign management APIs to make the suggested campaign changes.

The current recommendations available in the dataset include:

* Sponsored Products budget recommendations to identify high performing campaigns (ROAS of greater than three) that might run out of budget in next seven days (utilized 80% of their budget).
* Sponsored Products budget rules for special events to identify campaigns which might go out of budget during upcoming high-traffic days in the next eight weeks and apply a budget increase for those specific days.
* Sponsored Brands and Sponsored Display bid optimization to identify low-performing campaigns and suggest bid optimization to improve ROAS.

We will enabling more recommendations in the coming months.

For more details:

- View [dataset schema and IAM policies](guides/amazon-marketing-stream/data-guide#sponsored-ads-campaign-recommendations-sponsored-ads-campaign-diagnostics-recommendations)
- Create SQS queues for the new dataset using our [CloudFormation template](https://github.com/amzn/ads-advanced-tools-docs/tree/main/amazon_marketing_stream)
- View [Tactical recommendations API user guide](guides/recommendations/tactical-recommendations/stream-user-guide) for Stream

#### Worldwide support for Sponsored Display in Amazon Marketing Stream

Sponsored Display datasets are now available globally in Amazon Marketing Stream (beta). We are excited to expand the Sponsored Display datasets (sd-traffic and sd-conversion) in Amazon Marketing Stream (beta) to all countries currently supported by the Amazon Ads API. Amazon Marketing Stream delivers Amazon Ads campaign metrics and information to advertisers' or integrators' AWS accounts via a push-based model in near real time.

To access the Sponsored Display datasets in the EU or FE regions, you should use the following AWS source ARNs as part of your IAM policy:

sd-traffic dataset:

- EU: `arn:aws:sns:eu-west-1:947153514089:*`
- FE: `arn:aws:sns:us-west-2:310605068565:*`

sd-conversion dataset:

- EU: `arn:aws:sns:eu-west-1:664093967423:*`
- FE: `arn:aws:sns:us-west-2:818973306977:*`

For more details:

- View the [Data guide](guides/amazon-marketing-stream/data-guide) for full IAM policies for each dataset.
- Create SQS queues for Sponsored Display datasets in the new regions using our updated [CloudFormation template](guides/amazon-marketing-stream/cloud-formation).

#### Amazon Marketing Stream reference implementation

We have released a new reference implementation that developers can use to set up and manage an Amazon Marketing Stream integration. This python-based reference application uses the AWS CDK to provision the suggested AWS infrastructure needed to confirm dataset subscriptions and start receiving advertiser data. The package also contains a CLI to create, update, and read Stream dataset subscriptions for any given advertiser.

For more information, find the reference implementation in [GitHub](https://github.com/amzn/amazon-marketing-stream-examples) or view our [Reference implementation overview](guides/amazon-marketing-stream/reference-implementation).

### February 2023

#### Amazon Marketing Stream (beta) launch in India

We are excited to expand Amazon Marketing Stream (beta) to India. This expansion follows our [global launch in October 2022](release-notes/index#amazon-marketing-stream-beta-global-launch-except-in).

**Note**: Advertisers in India are considered part of the EU region and should be subscribed to queues in AWS region eu-west-1.

#### Sponsored Brands version 4 campaign management APIs are now generally available

Regions: AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, SA, SG, UK, US

Today, we are moving the Sponsored Brands version 4 campaign management API functionality out of beta and into general availability status. Advertisers, agencies, and tool providers can leverage these APIs to create and manage their Amazon Sponsored Brands campaigns worldwide. You can generate create and manage ad group-based campaigns as well as legacy campaigns similar to the advertising console. To encourage adoption, the version 4 APIs follow the existing API pattern for Sponsored Products or Sponsored Display for campaign management. This functionality is available to advertisers, tool providers, and agencies that have self-service access to Sponsored Brands.

We [previously released](release-notes/archive/ads-api#sponsored-brands-ad-groups-are-now-available-in-open-beta-through-amazon-ads-api) these endpoints in beta. Today, we have added two new endpoints to [update](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/UpdateSponsoredBrandsCampaigns) and [delete](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/DeleteSponsoredBrandsCampaigns) campaigns. Note that the endpoint structure has changed. For example, POST /sb/beta/campaigns is now POST /sb/v4/campaigns.

**The beta endpoints will be deprecated on August 31, 2023**. You will need to start using the general availability endpoints by this date. For more details, see [Deprecations](release-notes/deprecations).

For more information, see:

- [Migration guide](reference/migration-guides/sb-v3-v4)
- [Creating version 4 campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns)
- [API reference](sponsored-brands/3-0/openapi/prod#tag/Campaigns)

#### Awareness: Budget rules history endpoints to be deprecated on August 31, 2023

The following endpoints will be deprecated on August 31, 2023:

- GET /sp/campaigns/{campaignId}/budgetRules/budgetHistory
- GET /sb/campaigns/{campaignId}/budgetRules/budgetHistory
- GET /sd/campaigns/{campaignId}/budgetRules/budgetHistory

For more information, see [Deprecations](release-notes/deprecations).

#### Amazon DSP reporting API adds advertiser limits to entity-level reports

Regions: US, CA, MX, DE, ES, FR, IT, ES, NL, UK, AU, IN, JP, UAE, SA

To improve the performance and stability of the Amazon DSP reporting API, we have set a limit for entity-level report requests to one hundred advertisers per report request. For entities with more than one hundred advertisers, you now need to request reports of up to one hundred advertiser IDs at a time. For entities with less than one hundred advertisers, there is no change. Additionally, entities that are invalid or do not have any advertisers will now receive an error message. These changes apply to all users of the reporting API across all available regions.

For more details, see the [DSP reporting reference](dsp-reports-beta-3p/#/Reports).

#### Version 3 reporting API bug fix

We are updating the version 3 reporting API to fix a bug in the purchased product report for Sponsored Brands and Sponsored Products.

This bug caused currency values to be reported in cents or equivalent denominations instead of dollars and equivalent denominations. This has now been fixed in the purchased product report for both Sponsored Products and Sponsored Brands. Now all currencies will be reported in dollar or equivalent denominations based on the currency of the profile that a given campaign is associated with. For more information, please see the version 3 [purchased product report](guides/reporting/v3/report-types#purchased-product-reports).

#### Sponsored Display launches reach and frequency metrics

Regions: US, CA, ES, FR, IT, DE, UK, AE, JP, IN, AU, NL, BR, MX, SE, SG, PL, TR, EG, SA, BE

Sponsored Display is launching reach and frequency metrics for vendors and registered sellers across all regions in the Amazon Ads API.

With the release of the [cumulativeReach](guides/reporting/v2/metrics#cumulativereach) metric, advertisers can better understand the total number of unique users that an ad was exposed to. With the launch of the [averageImpressionFrequency](guides/reporting/v2/metrics#avgimpressionsfrequency) metric, advertisers can better understand the average number of times those users saw an ad and how that drives users down the funnel.

Metric definitions:

- `cumulativeReach` = Total number of unique users exposed to an ad from either a campaign, ad group, or product ad over the lifetime of the campaign or the past six months, whichever is shorter. This metric is updated daily.
- `avgImpressionsFrequency` = Average number of times unique users were exposed to an ad over the lifetime of the campaign.

When requesting reports for campaign, ad group and product ad, you can add the new metrics of `avgImpressionsFrequency` and `cumulativeReach` for any Sponsored Display campaign (CPC or vCPM) in the Amazon Ads API. For full technical details, please see our updated Sponsored Display [reporting documentation](guides/reporting/v2/report-types).

#### API specifications are now indexed in advanced tools center search

Technical specifications for all endpoints in the Amazon Ads API are now indexed in the search interface in the advanced tools center. Developers will see results for search terms that match paths or descriptions for all operations in the API. To search technical specifications, use the search bar in the Amazon Ads advanced tools center.

#### Amazon DSP offers new Brand Safety segments

Amazon DSP introduces new Brand Safety segments which enable greater control over how advertisers serve ads on inventory associated with these topics:

- ‘Death & Injuries - High Risk’
- ‘Death & Injuries - High & Medium Risk’
- ‘Death & Injuries - High, Medium & Low Risk’
- ‘Extreme & Graphic‘

The brand safety functionality gives you more control on where your ads serve to protect your brand and block ads from delivering on content that doesn't align with your brand. For more details, refer to our [API documentation](dsp-campaigns/#/LineItem/getLineItem).

#### Belgium support

Regions: BE

We have added support for Belgium (BE) to the Amazon Ads API. For more details, see the [API endpoints by region](reference/api-overview#api-endpoints), [Bid constraints by marketplace](reference/concepts/limits#bid-constraints-by-marketplace), [Budget constraints by marketplace](reference/concepts/limits#budget-constraints-by-marketplace), as well as the `countryCode`, `currencyCode`, and `timezone` parameters in the [profiles reference](reference/2/profiles#/Profiles/listProfiles).

#### Sponsored Display expands ASIN report to vendors and adds view-based attribution in the Amazon Ads API

Regions: US, CA, ES, FR, IT, DE, UK, AE, JP, IN, AU, NL, BR, MX, SE, SG, PL, TR, EG, SA, BE

Sponsored Display expands ASIN report to vendors while adding view-based attribution in the Amazon Ads API. Previously this report was only available for vendors. Also known as the purchased product report, pulling this data provides details on non-advertised products that were purchased by shoppers after they interacted with a Sponsored Display ads. Along with expanding to vendors, we’ve also added metrics to Sponsored Display view attributed metrics for those campaigns with vCPM (cost per 1 thousand viewable impressions) billing and attribution.

This report helps advertisers identify new products within their portfolio that they may consider advertising, identify new cross-selling product targeting opportunities, and uncover insights into shoppers buying behavior. Use the new view based metrics for both sellers and vendors when pulling data from `/sd/asins/report` with campaigns using the `costType` vCPM in the Amazon Ads API. These new metrics include:

- [viewAttributedUnitsOrdered1dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered1dOtherSKU)
- [viewAttributedUnitsOrdered7dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered7dOtherSKU)
- [viewAttributedUnitsOrdered14dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14dOtherSKU)
- [viewAttributedUnitsOrdered30dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered30dOtherSKU)
- [viewAttributedSales1dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales1dOtherSKU)
- [viewAttributedSales7dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales7dOtherSKU)
- [viewAttributedSales14dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales14dOtherSKU)
- [viewAttributedSales30dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales30dOtherSKU)
- [viewAttributedConversions1dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions1dOtherSKU)
- [viewAttributedConversions7dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions7dOtherSKU)
- [viewAttributedConversions14dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions14dOtherSKU)
- [viewAttributedConversions30dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions30dOtherSKU)

For full technical details, please see our updated [reporting documentation](guides/reporting/v2/report-types#asin-reports).

### January 2023

#### Amazon DSP launches deals discovery API feature

Regions: US, UK, DE, JP, FR, IT, IE, IL, ES, CA, CH, MA, LU, FI, NO, DK, MX, KW, AU, AE, AT, BE, BR, BH, NL, IN, SA, SE, TR, NZ

Amazon DSP is launching 4 new endpoints available through the Amazon Ads API to enable advertisers to manage their Amazon DSP programmatic deals:

* [POST /dsp/inventory/deals/list](dsp-deals-3p#/Deal/listDealsDspDeals) - Retrieves a list of programmatic deals, optionally matching filter inputs.
* [POST /dsp/inventory/deals/performance/list](dsp-deals-3p#/Deal%20Performance/listDealsPerformanceDspDeals) - Retrieves performance metrics for each specified deal.
* [GET /dsp/inventory/deals/{dealId}/lineItems](dsp-deals-3p#/Deal/getDealLineItemsDspDeals) - Retrieves list of line items targeting a given single deal.
* [POST /dsp/inventory/deals/exchanges/list](dsp-deals-3p#/Deal%20Exchanges/listDealsExchangesDspDeals) - Retrieves list of supply sources for deals.

Though these features were previously available in the Amazon DSP console, advertisers can now access them programmatically through the Amazon Ads API. This helps bring the Amazon Ads API experience in line with the advertising console, and gives advertisers greater flexibility when managing Amazon DSP programmatic deals.

With these additional Amazon Ads API endpoints, advertisers can retrieve their Amazon DSP programmatic deal libraries at scale, and build their own business applications leveraging these objects. They can now synchronize their Amazon DSP deal library with their internal repositories, and retrieve deal metadata, performance metrics, and line items targeting each deal.

This release supports all standard deal types, with some exceptions for Amazon Audience Guaranteed deals: while these deals can be retrieved, Amazon Ads API users may not yet retrieve performance data of these deals. For full technical details, please see our [API documentation](dsp-deals-3p/).

#### Missed opportunities and budget recommendations API for Sponsored Brands

Regions: United States (US)

Sponsored Brands is releasing the new missed opportunities and budget recommendations API which will provide recommendations for campaigns that have gone out of budget. This API will provide predicted missed sales, clicks, and impressions during these out of budget periods, while also recommending a new budget to prevent them in the future.

Roughly ten percent of all Sponsored Brands campaigns go out of budget daily, losing out on numerous interactions with customers. The missed opportunity and budget recommendations API contextualizes the business impact of these periods in the form of measurable key performance indicators. By adopting the suggested budget, advertisers can prevent these cases in the future and ensure their campaigns are reaching their full potential.

This feature is available for vendors and sellers through the Amazon Ads API. Advertisers can use the new [POST /sb/campaigns/budgetRecommendations](sponsored-brands/3-0/openapi/prod#tag/Budget-Recommendations) endpoint that accepts a list of campaigns and returns suggested budget, percent time in budget, and missed opportunities in clicks, impressions, and sales.

#### Awareness: Budget rules are becoming additive

Regions: All

By the end of February 2023, we will make budget rules to be additive, meaning that more than one rule can be active at a time. Currently, only a single rule can be active, but with this release, if multiple budget rules meet their conditions, all rules will be applied and increase daily budget independently.

We’re making this change so that budget rules are more customizable, allowing advertisers to build strategies for budget increases based on multiple triggers instead of having to consider which budget rule will be active at a given point in time.

This update will impact all advertisers worldwide (vendors and sellers) who use budget rules for their Sponsored Products, Sponsored Brands, or Sponsored Display campaigns through the Amazon Ads API or advertising console. We will post a separate release note to notify advertisers once this change is live.

If you have a question, you can go to the [Amazon Ads API Support page](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/create/5) and create a ticket with the subject title *Budget Rules Additive Behavior*.

#### Omnichannel Metrics integrates with automated budget optimization (beta)

Regions: United States (US)

Omnichannel Metrics (OCM) now offers advertisers the capability to automate budget optimization for the total impact of your campaign, inclusive of both online and offline sales, on and off Amazon. Advertisers opted-in for budget optimization will have their budget automatically shift between line items based on omnichannel performance, thereby helping to optimize the overall outcome of your campaign.

The new OCM automated budget optimization feature alleviates the current manual process of allocating budget across multiple line items to optimize budget and maximize Combined ROAS.

This feature is designed to help our advertisers and agencies to 1) increase efficiency by reducing wasted impressions on lower-performing ad tactics, 2) enable agile optimization allowing for line items with specific budget requirements to be opted out of budget optimization, while other line items in the same campaign could continue to be optimized leveraging this budget feature, and 3) empower advertisers with the control to opt out, pause or re-enroll line items within the budget optimization at any point in the campaign.

This feature is available for CPG and grocery advertisers with an active Omnichannel Sales Study, including both managed service and self-service DSP accounts.

To access this feature, use the Amazon DSP create order API with the optimization goal `PURCHASES_ON_OFF_AMAZON` and goal KPI of `COMBINED_RETURN_ON_AD_SPEND`. The budget and bid optimization will be automatically selected. For full technical details, please see our [updated documentation](dsp-campaigns/#/Order/CreateOrders).

#### Sponsored Display bid recommendations are expanding for Amazon audiences and purchases remarketing

Regions: AE, AU, CA, DE, ES, FR, IN, IT, JP, MX, NL, UK, US

Amazon Ads is launching bid recommendations for Amazon audiences (i.e., In-market, Life events, Lifestyle and Interests) and purchase remarketing. Bid recommendations will be provided for all Sponsored Display audience targeting campaigns, including those selected to “Optimize for viewable impressions”, “Optimize for page visits”, or “Optimize for conversions”.  These recommendations will be available for Sponsored Display campaigns within the Amazon Ads API.

This helps advertisers receive bid recommendations based on the bid optimization they select. When the bid optimization is "Optimize for viewable impressions", the bid suggestions will be for 1000 viewable impressions rather than per click, which helps advertisers maximize impression share for the selected audiences. When bid optimization is "Optimize for page visits", bid suggestions will help advertisers get impressions on requests that are likely to get clicks. When bid optimization is "Optimize for conversions", bid suggestions will help advertisers get impressions on requests likely to convert.

For full technical details, please see our [Sponsored Display documentation](sponsored-display/3-0/openapi#tag/Bid-Recommendations), including the updated bid recommendations documentation.

#### Input guidance for Sponsored Products in bulk operations

Input guidance is now available for Sponsored Products campaigns in bulksheets. This new feature includes dropdown options for some fields, including **Product**, **Entity**, **Operation**, and more. After you select values for those first three fields, cells in that row will automatically highlight the fields that can be filled in—both required and optional ones. You’ll also see guidance if you click on cell headers. For example, if you click into a “Daily Budget” cell header, you will see tips on budget limits, along with guidance on entering values such as decimal places.

You can access this new feature if you go to the bulk operations main page and check the box labeled **Guidance for Sponsored Products inputs [BETA]**. Then, download a bulksheets template and navigate to the Sponsored Products tab to see the details described above. [Learn more about the new input guidance feature for bulksheets](bulksheets/2-0/input-guidance)
