---
title: Sponsored Brands version 4 features and benefits
description: An overview of the features and benefits of Sponsored Brands version 4 
type: guide
interface: api
tags: 
    - Sponsored Brands
keywords: 
    - Features
---

# Sponsored Brands version 4 features and benefits

On October 2022, [Sponsored Brands introduced](https://advertising.amazon.com/resources/whats-new/sponsored-brands-ad-groups/?ref_=a20m_us_search_title) hierarchical version 4 campaigns, featuring multiple ad groups and ads. Prior to this launch, advertisers lacked the ability to create a Sponsored Brands advertising campaign aligned with their marketing strategy. The introduction of ad groups enhances flexibility, particular for managing campaigns across programs, as both [Sponsored Products](https://advertising.amazon.com/solutions/products/sponsored-products?ref_=a20m_us_wn_sb_102422_p_sp) and [Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display?ref_=a20m_us_wn_sb_102422_p_sd) already support ad groups. 

## Key benefits

1. **[Consolidate and simplify campaign structure](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-brands-ad-groups/?ref_=a20m_us_wn_gw) in a way that works for you** by creating ad groups that align with your marketing strategies and goals. 
2. **Goal based campaigns:** The Sponsored Brands version 4 campaigns allow you [create goal based campaigns](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-brands-goal-based-campaigns/?ref_=a20m_us_wn_gw) to help different brands prioritize different brand building goals depending on their marketing strategy and their brand building journey. You can create campaigns with new goals: “drive page visits” and “grow brand impression share”, each with a unique set of campaign controls to achieve that goal.
3. **Theme targeting:** To help drive advertiser goals, [theme targeting](https://advertising.amazon.com/resources/whats-new/sponsored-brands-introduces-theme-targeting/?ref_=a20m_us_search_title) is a new method to control your campaign targeting. It uses machine learning algorithms to account for historical performance targets and brand interactions to create a large set of keywords. It then categorizes those keywords into relevant groups to use with your campaigns. Two targeting groups are currently available: “Keywords related to your landing pages” and “Keywords related to your brand.” These targeting groups will be pre-selected to simplify the campaign creation process for advertisers.
4. **Vertical videos:** Advertisers can now launch [vertical video campaigns](https://advertising.amazon.com/resources/whats-new/sponsored-brands-video-introduces-vertical-video-creatives/?ref_=a20m_us_search_title) with a Store as the landing page. Vertical video is available under “Drive page visit” goal, enabling advertisers to highlight a collection of one, two, or three advertised products. Under the “Grow brand impression share” goal, advertisers can launch a video campaign without, or with one, or two, or three advertised products. Shoppers clicking on an advertised product will visit the product’s detail page; shoppers clicking on any other ad element will visit the brand’s Store on Amazon. 
5. **Slideshow:** Advertisers can create creative [Slideshow ads](https://advertising.amazon.com/resources/whats-new/slideshow-ads-creative-for-sponsored-brands/?ref_=a20m_us_search_title), that will appear at the top of search results page. Slideshow ads are an extension of the existing lifestyle creative, with a carousel of 2-5 lifestyle images that will auto-rotate. Shopper can engage with the slideshow by pausing it at any point in time or switching between different images by making forward or backward swipes.
6. **New reporting:**  Accessing a wide range of new metrics, including [addToCartViews](https://advertising.amazon.com/en-us/resources/whats-new/detail-page-view-metrics-are-available-globally/?ref_=a20m_us_wn_gw), [newToBrandDetailPageViewViews](https://advertising.amazon.com/en-us/resources/whats-new/detail-page-view-metrics-are-available-globally/?ref_=a20m_us_wn_gw), and [newToBrandDetailPageViewClicks](https://advertising.amazon.com/en-us/resources/whats-new/detail-page-view-metrics-are-available-globally/?ref_=a20m_us_wn_gw) for Sponsored Brands version 4 campaigns. 
7. **Accessibility and editability:** Supports creative edits for [all ad formats](https://advertising.amazon.com/resources/whats-new/creative-editing-available-sponsored-brands-video/?ref_=a20m_us_search_title) and [edit landing page URL](https://advertising.amazon.com/API/docs/en-us/release-notes/index#sponsored-brands-introduces-landing-page-edits-on-the-amazon-ads-api). You can assign individual landing page URLs to each ad within same ad group. You can update your ad group based campaigns landing page URLs without recreating new ad group or campaign.
8. **Monitor performance of ad group campaigns with reports**. You can easily retrieve reports at campaign, keywords, campaign placement, search terms, purchase product, and more using singular [reporting version 3 APIs](https://advertising.amazon.com/API/docs/en-us/release-notes/index#preview-sponsored-brands-reports-on-amazon-ads-api-reporting-version-3). You’ll no longer need to run separate reporting for two Sponsored Brand version 3  and Sponsored Brand version 4 campaigns.
9. **Make the most of testing based on your goals**, whether that’s building brand awareness, driving consideration, or increasing sales. You can test targeting against different ad formats or landing pages and measure the performance. Plus, you can test different versions of the same ad formats within the same ad group to get a better understanding of shopper behavior.
10. **Use consistent strategies across sponsored ads** to run your campaigns more effectively. The ad groups feature is also available for both [Sponsored Products](https://advertising.amazon.com/solutions/products/sponsored-products/) and [Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display/).

## **Handling version 3 campaigns**

The Sponsored Brand version 3 campaigns are in “Lights On” mode and have not received new advertiser features or new measurement metrics since October 2022. We highly recommend migrating these campaigns to version 4 campaigns using [Sponsored Brands version 4 APIs](sponsored-brands/3-0/openapi/prod)or [cloning them using the ad console](https://advertising.amazon.com/help/GPFGH67KMNLQ5TKU). By migrating, you can enhance your campaigns and take advantage of these new capabilities. 

## Feature availability

|Feature group	|Feature	|Version 3	|Version 4	|
|---	|---	|---	|---	|
|Goal	|BRAND\_IMPRESSION\_SHARE	|Not Available	|Available	|
|	|PAGE_VISIT	|Not Available	|Available	|
|	|Smart default	|Not Available	|Available	|
|Cost type	|CPC	|Available	|Available	|
|	|VCPM	|Not Available	|Available	|
|Campaigns|Capability to create multiple ad groups and ads	|Not Available	|Available	|
|Targeting	|Keyword	|Available	|Available	|
|	|Product 	|Available	|Available	|
|	|Theme 	|Not Available	|Available	|
|Ad format	|Product collection	|Available	|Available	|
|	|Store spotlight 	|Available	|Available	|
|	|Brand video	|Not Available	|Available	|
|	|Vertical video	|Not Available	|Available	|
|	|Slideshow with custom images	|Not Available	|Available	|
|Creatives	|Creative edits	|Product collection	|Available for all ad creatives (product collection, store spotlight, video, brand video, multi-ASIN video, vertical video, slideshows)	|
|	|Landing Page Edits	|Not Available	|Available	|
|Reporting	|New metrics: addToCart, addToCartRate, eCPAddToCart, newToBrandDetailPageViews, newToBrandDetailPageViewRate, newToBrandECPDetailPageViews	|Not Available	|Available	|
|	|Support of ad group ID and ad ID in SB purchased product report	|Not Available	|Available	|
|	|Version 3 reporting	|Not Available	|Available	|
|Amazon Marketing Stream	|Amazon Marketing Stream	|Not Available	|Available	|
|Authors	|Product Collection and Video	|Not Available	|Available	|
|	|Creative Edits	|Not Available	|Available	|
|	|Kindle Edition Normalized Page (KNEP) - Reads and Royalties metric	|Not Available	|Available	|

Learn more about [migration to version 4 APIs](reference/migration-guides/sb-v3-v4.)