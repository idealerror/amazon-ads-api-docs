---
title: Developer blog
description: Read the latest updates, best practices, and more in the developer blog for the Amazon Ads advanced tools. 
type: guide
interface: api
---

# Developer's Blog

### Introducing ease of use best practices to the Amazon Ads API 

We recently released product targeting functionality for Sponsored Brands campaigns.  With this release, new endpoints have been introduced that implement a set of best practices. These best practices contribute to the ease of use of the Amazon Ads API. 

These best practices include: 

1. The use of pagination tokens to ensure users don’t retrieve data sets that have changed between calls. 
2. Separating success and error responses, so there's now an explicit list of errors. This makes it easy to create a new request to retry all error objects.  
3. GET operations that were previously filtered with query parameters are now POST operations with request parameters. This enables developers to create more complex filters without running into URL character limits.
4. Replacing PUT and POST archive operations with explicit DELETE operations to avoid risk of accidentally deleting an object. 

Developers will see more of these best practices implemented in new API functionality going forward. We're implementing these best practices based on customer feedback, and we're excited to hear developer opinions. You can give us feedback by opening a service desk ticket with the subject line: “Feedback on best practices”.

### Introducing Sponsored Display

Today, we launched Sponsored Display beta, a new self-service advertising solution that helps advertisers grow their business by reaching relevant audiences both on and off Amazon.   

Why Sponsored Display?

* **Reach the right  audiences**: Audiences are automatically created based on Amazon shopping activities to help advertisers reach customers beyond those actively shopping on Amazon.
* **Maximize impact with minimal effort**: In just a few  clicks, advertisers can promote their entire product catalog with creatives that are auto-generated and optimized for conversion.
* **Support business objectives**: From product awareness to consideration and conversion, Sponsored Display helps measure and meet advertiser goals.
 
The Amazon Ads API now supports Sponsored Display campaigns that reach shoppers off Amazon who visited a product detail page or detail pages of similar products but didn't make a purchase. We've designed the Sponsored Display API functionality for easy adoption. Sponsored Display API functionality uses the same security scope as both Sponsored Products and Sponsored Brands, so the same authentication token can be used to access Sponsored Display functionality. There are no additional onboarding steps -- simply call the Sponsored Display API endpoint. We've also designed API functionality for Sponsored Display to be consistent with Sponsored Products and Sponsored Brands, so developers already working with either of these can apply the same practices to Sponsored Display. New users of the Advertising API can learn more about getting started [here](https://advertising.amazon.com/API/docs/v3/guides/account_setup). As part of this launch, we will also retire legacy functionality that allowed users to update and retrieve Sponsored Display campaign information through Sponsored Products resources. We expect to remove this functionality by Q4 2019. Users that are currently using these resources and want to continue managing or reporting on Sponsored Display campaigns should adopt the newly released functionality.
 
With this launch, advertisers are able to access Product Display Ads audiences and product targeting features within Sponsored Display. All Product Display Ads campaigns are now part of Sponsored Display, with no additional action required. This change will be coming to the API and additional marketplaces later this year.
 
To learn more about API functionality for Sponsored Display, see the [reference documentation](https://advertising.amazon.com/API/docs/v3/reference/SponsoredDisplay/campaigns). To learn more about Sponsored Display, visit the [Sponsored Display page](https://advertising.amazon.com/products/sponsored-display).

> **Note**: Sponsored Display is available only in the US to professional sellers and vendors, and agencies with clients who sell on Amazon.


## New Sponsored Brands improvements

Recently we announced three new improvements to the broad match feature, which helps advertisers reach more shoppers and control advertising costs. In July, we are making these releases available in the API:

  * Sponsored Brands broad match now includes variations such as plurals, synonyms, or other words related to the keyword. For example, an ad targeting "shoes" using broad match may show on a shopper query for "sneakers". This helps advertisers reach even more shoppers who may be interested in their products. This functionality is now available in the API. See the relevant documentation [here](https://advertising.amazon.com/API/docs/v2/reference/keywords/ad_group_biddable_keywords)
  * To help advertisers refine their broad match campaign performance, Sponsored Brands now offers broad match modifiers. Broad match modifiers help ensure certain keywords are always included in the search terms advertisers are bidding on. Advertisers can use broad match modifiers by adding â€œ+â€ before the word. This functionality is now available in the API. See relevant documentation [here](https://advertising.amazon.com/API/docs/v2/reference/keywords/ad_group_biddable_keywords)
  * Sponsored Brands now offers negative keyword targeting. Negative keywords prevent ads from displaying when shoppersâ€™ search queries match negative keywords. Now negative keywords can be used with broad, phrase, and exact match types to prevent ads from being displayed for search terms that aren't meeting business objectives. This release will be available in the API later this month, but to help you prepare, we are making the documentation available early. See the relevant documentation [here](https://advertising.amazon.com/API/docs/v3/reference/sponsored_brands/sb_campaigns) and [here](https://advertising.amazon.com/API/docs/v2/reference/profiles). General release of the functionality will be announced in the release notes section.

## Introducing Sponsored Brands campaign creation

Today we are launching the ability to create Sponsored Brands campaigns via the Amazon Ads API in open beta.  
This release introduces version 3 of the API, which includes features that will improve the advertiser and developer 
experience.  This release will also bring the Amazon Ads API close to parity with the advertising console for 
sponsored ads.

We believe this release will make it easier for advertisers to use Sponsored Brands. Advertisers that use the Amazon 
Advertising API can now create Sponsored Brands campaigns at scale in the solutions they use to manage their Amazon 
business or marketing activities. Advertisers can also make their processes more efficient and create more Sponsored 
Brands campaigns faster. Finally, advertisers can achieve better performance by implementing comprehensive creative 
testing.

This release will also improve the developer experience. Developers that adopt Sponsored Brands campaign creation 
functionality and, through it the new version of the API, will be better positioned to adopt upcoming sponsored ads and 
API releases. For sponsored ads releases, in 2019, we are prioritizing releasing new Sponsored Brands functionality 
based on customer feedback. For API releases, new API functionality will only be released in the new API version. 
Furthermore, the new version of the API will introduce improvements that make it easier for developers to adopt 
upcoming releases including meeting openAPI specifications. 

This functionality is available to all developers that have access to the Amazon Ads API. To learn more visit 
the [blog post](https://advertising.amazon.com/API/docs/blog), 
[relevant documentation](https://advertising.amazon.com/API/docs/v3/reference/sponsored_brands/sb_campaigns) section, 
or the [FAQ](https://advertising.amazon.com/API/docs/faq) section in the developer portal. Also, make sure to see this 
section on Sponsored Brands [moderation](https://advertising.amazon.com/API/docs/v3/guides/developer_notes#Sponsored-Brands-moderation) 
and campaign life-cycle [status](https://advertising.amazon.com/API/docs/v3/guides/developer_notes#Sponsored-Brands-Campaign-Status).

## A note about Advertising terminology and name changes
### October, 2018
We [recently announced](https://advertising.amazon.com/blog/amazon-advertising-simplified?ref=_int-email) that we are now simply, Amazon Ads. With this, we retired the names Amazon Media Group (AMG), Amazon Marketing Services (AMS), and Amazon Ads Platform (AAP), and announced a simplified product portfolio. This change impacts API users because of syntax built into the structure of the API. The documentation for v2 will continue to reference the older product names in the technical reference sections. A key is provided below to assist in transition to the new terminology.

| New terminology| Retired terms |
| --- | --- |
| Amazon Ads |-	Amazon Marketing Services (AMS)<span class="break"> </span>-	Amazon Media Group (AMG)<span class="break"> </span>-	Amazon Ads Platform (AAP)	 |
| Sponsored Brands (SB) | Headline Search Ads (HSA) |
|Amazon DSP | Amazon Ads Platform (AAP) |

### Introducing the Advertising API v2.0!
Today weâ€™re introducing a new version of the Advertising API with v2.0.

**Why did we advance the API version?**
We did this in order to introduce some new features for [Sponsored Brands (SB)](https://advertising.amazon.com/products/sponsored-ads) (formerly known as Headline Search Ads) and functionality to improve the API performance by reducing rate-limiting (or throttling). Last year we added SB reporting to the API. Today we are adding the ability to manage operations and to retrieve snapshots for existing SB campaigns. At this time you cannot create new campaigns or product ads through the API. New SB campaigns can only be created in the UI. Once created, you can update or archive SB campaigns. You can also now request a snapshot showing every level of your SB campaigns in one report. This is a much more efficient operation than running a command to list campaigns, then listing keywords, and then listing product ads, etc. Snapshots give it to you all in one.

We also mentioned some improvements to reduce throttling. We have observed that many brands split their advertising business across multiple agencies or e-commerce specialists. With the changes introduced in API v2 we can now identify individual operators and place safeguards to prevent their activity from negatively impacting any other customers. The change that makes this possible is the new requirement to include the client_id in the header of all API calls to the v2 endpoints.

**What are the most impacting changes?**
One thing you will notice right away is that your API calls will now return data for both seller and vendor profiles. This will allow you to manage operations or retrieve performance data for both advertiser segments with a single API call.

Using the new API features of v2 means calling the new v2 endpoints. Look closely at the reference documentation for campaigns, keywords, reports, and snapshots to see which operations require use of the new endpoints.

**Note:** The new endpoints still refer to HSA for Headline Search Ads in certain places. 

Stay tuned to the Developer's Blog for news about upcoming features and releases.
Thanks for using the Amazon Ads API!


Ross Diaz - Technical Content Manager

Amazon Ads API Development


### Welcome to the Amazon Ads API Developers Portal!
#### March 26, 2018
When we built this portal, we had one vision in mind: To make using the
Advertising API an enjoyable experience for you. We want to save you
time, make your operations more efficient, and increase your knowledge
and comfort level with the API.

This documentation space was designed with developers in mind and is generated from the feedback we have collected over the last few years from our advertisers who use the API to integrate their applications. Here are some of the most requested features we are now able to provide you with:

-   Web-published HTML documentation. This means no more downloading and
    storing PDFs to your local device and having to check back
    constantly for updates. The content on the Developer Portal will
    always be the most updated and accurate documentation available.

-   Code snippets that are easy to copy to your clip board.

-   Full list of valid character constraints for keywords and campaigns.

We want to add new features to the portal to improve your
experience. As we release new features and capabilities for the
Advertising API, you will find everything you need to understand and
start using those features right here in the Developers Portal. We place
a priority on delivering the features and information that you want.

Now itâ€™s your turn to speak up and tell us how weâ€™re doing! Is something
missing? Do you have a great idea for how to make the portal better? Is there a particular feature we introduced that you love? Is
there a pain point in working with the API that we havenâ€™t addressed
yet? Let us know!

If your access to the API is already approved, you can give us feedback by opening a service desk ticket with the subject line: â€œDevelopers Portal
feedbackâ€ and tell us what improvements you would like to see on the
portal to make it more useful to you. We are listening to what you have
to say and working to provide you with the information you need to maximize the API for your business.

On behalf of all of us here at the Advertising API development team we
want to sincerely thank you for using the API to manage your advertising
operations. We are truly excited to be able to deliver to you an
improved documentation experience providing the essential information
you need for integrating with the Advertising API.

All the best,

Ross Diaz - Technical Content Manager
Amazon Ads API Development