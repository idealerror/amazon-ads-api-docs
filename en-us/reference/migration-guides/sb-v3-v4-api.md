---
title: Migrating to version 4 Sponsored Brands campaigns using the legacy campaigns migration API
description: A migration guide for moving to Sponsored Brands version 4 campaign resources via the legacy campaigns migration API
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - Migration guide
    - Version 4
    - Multi-ad group campaigns
    - legacy campaigns
---

# Legacy Campaigns Partner Migration Guide 

## I. Introduction

Sponsored Brands (SB) advertisers can now upgrade their legacy (Version 3) campaigns to the new ad group-based structure (Version 4). This migration provides key benefits such as enhanced campaign organization through structured ad groups, access to new theme targeting options, shopper cohort bidding, cost controls, landing page edits, etc. The upgrade unifies campaign management for all Sponsored Brands advertising, streamlining the advertiser experience. 

To facilitate this transition, we've developed migration APIs that enable advertisers and partners to independently convert their campaigns to the new structure.

>[TIP]View the **[technical specifications for the legacy campaigns migration API](sponsored-brands/3-0/openapi/prod#tag/V3-Campaign-Migration)**.

## II. What are the key benefits?

Below are the key benefits of using new ad group campaigns. See [FAQ#26](#q26-what-features-are-not-available-for-legacyv3-campaigns) for full list.

1. **Consolidate and simplify campaign structure in a way that works for you** by creating ad groups that align with your marketing strategies and goals. 
2. **New reporting:**  The V4 campaigns supports new metrics such as [brandedSearchesViews](guides/reporting/v3/columns#brandedSearchesViews), [brandedSearchRate](guides/reporting/v3/columns#brandedSearchRate), [eCPBrandSearch](guides/reporting/v3/columns#eCPBrandSearch), [addToCartViews](guides/reporting/v3/columns#addToCartViews), [newToBrandDetailPageViewViews](guides/reporting/v3/columns#newToBrandDetailPageViewViews), and [newToBrandDetailPageViewClicks](guides/reporting/v3/columns).
3. **Goal-based campaigns:** The V4 campaigns allow you create goal-based campaigns to help different brands prioritize different brand building goals depending on their marketing strategy and their brand building journey. You can create campaigns with new goals: “Drive Page Visits”, “Grow Brand Impressions Share”, “Acquire new customers”**\*** and “Ad Views”**\***; each with a unique set of campaign controls to achieve that goal.
4. **Theme-based targeting:** To help drive advertiser goals, theme targeting is a new method to control your campaign targeting. It uses advanced machine learning algorithms that take into account historical performance targets and brand interactions to create a large set of keywords. It then categorizes those keywords into relevant groups to use with your campaigns. Two targeting groups are available in this launch: “Keywords related to your landing pages.” and “Keywords related to your brand”. These targeting groups will be pre-selected to simplify the campaign creation process for advertisers.
5. **New ad formats:**
    - **Vertical Videos:** Advertisers can now launch vertical video campaigns with a Store as the landing page. Vertical video is available under “Drive page visit” goal, enabling advertisers to highlight a collection of one, two, or three advertised products. Under the “Grow brand impression share” goal, advertisers can launch a video campaign without, or with one, or two, or three advertised products. Shoppers clicking on an advertised product will visit the product’s detail page; shoppers clicking on any other ad element will visit the brands Store on Amazon. 
    - **Slideshow:** Advertisers can create creative [Slideshow ads](https://advertising.amazon.com/resources/whats-new/slideshow-ads-creative-for-sponsored-brands/?ref_=a20m_us_search_title) that will appear at the top of search results page. Slideshow ads are an extension of the existing lifestyle creative, with a carousel of 2-5 lifestyle images that will auto-rotate. Shoppers can engage with the slideshow by pausing it at any point in time or switching between different images by making forward or backward swipes.
6. **Accessibility and Editability:** Supports creative edits for all ad formats and [edit landing page URL](release-notes/index#sponsored-brands-introduces-landing-page-edits-on-the-amazon-ads-api). Each ad can have individual landing pages within same ad group. You can now update your ad group based campaigns landing page URLs without recreating new ad group or campaign.
7. **Monitor performance of ad group campaigns with reports**. You can easily retrieve reports based on keywords, campaign placement, search terms, attributed purchases, and more. Plus, you’ll no longer need to run separate reports for different campaign types and manually calculate the performance of your ad spend for Sponsored Brands.
8. **Make the most of testing based on your goals**, whether that’s building brand awareness, driving consideration, or increasing sales. You can test targeting against different ad formats or landing pages and measure the performance. Plus, you can test different versions of the same ad formats within the same ad group to get a better understanding of shopper behavior.
9. **Use consistent strategies across Amazon Ads sponsored ads products** to run your campaigns more effectively. The ad groups feature is also available for both [Sponsored Products](https://advertising.amazon.com/solutions/products/sponsored-products/) and [Sponsored Display](https://advertising.amazon.com/solutions/products/sponsored-display/).

## III. Limitations 

* The Migration API works asynchronously. When the API is called, it doesn't instantly migrate the campaigns. Instead, the campaigns are migrated in the background. Status APIs must be called to check the current migration status.
* An advertiser can run only **one ongoing migration job** at a time. Submitting a list of Campaign IDs for migration while another job is in progress will cause the new job to fail.
* We do not recommend using shadow campaigns for migration unless you need to create and retrieve new Campaign IDs to update your system before other entities are created. 
* Migrated campaigns may require re-moderation according to the latest ad policies. Consequently, they may take up to 72 hours to complete the moderation process.
* While ad spends for migrated campaigns (from legacy systems) will be backfilled, discrepancies may occur due to the distributed system's nature. The system might receive impression events after the legacy (Version 3) campaign has been migrated and archived.
* Legacy (Version 3) campaigns will archive immediately after the new campaign is created and all associated entities (ads, creatives, keywords) are verified to match the V3 campaign.

## IV. Migration Sequence

#### 1. Standard migration without using inactive staged campaigns

![Standard migration without using inactive staged campaigns](/_images/sponsored-brands/sb_v3_v4_migration_sequence.png)

#### 2. Migration sequence using inactive staged campaigns

![Migration sequence using inactive staged campaigns](/_images/sponsored-brands/sb_v3_v4_migration_sequence_inactive.png)

## V. Sample Requests

### POST /sb/v4/legacyCampaigns/migrationJob

The `enableThemeTargeting` property adds theme-based targeting based to your new version 4 campaigns. Default is `true`; to disable, set to `false`.

#### Request

```bash
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/legacyCampaigns/migrationJob' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxxx' \
--header 'Accept: application/vnd.SponsoredBrands.SponsoredBrandsMigrationApi.v4+json' \
--data-raw '{
    "migrationJob": [{
        "campaignIds": ["63665260422798"],
        "isStagedMigration": false,
        "newCampaignState": "ENABLED",
        "enableThemeTargeting": true,
        "brandEntityId": ""
    }]
}'
```

#### Response

```json
{  
    "jobId": "1"
}
```

### POST /sb/v4/legacyCampaigns/migrationJob/results

#### Request

```bash
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/legacyCampaigns/migrationJob/results' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxxx' \
--header 'Accept: application/vnd.SponsoredBrands.SponsoredBrandsMigrationApi.v4+json' \
--data-raw '{
    "migrationJob": {
        "jobId": "1"
    }
}'
```

#### Response

```json
{
  "jobId": "1",
  "migrationJobStatus": "COMPLETE",
  "campaigns": [
    {
      "legacyCampaignId": "63665260422798",
      "newCampaignId": "35407526816553",
      "migrationStatus": "STAGING_COMPLETE"
    }
  ],
   "nextToken": "1" 
}
```

### POST /sb/v4/legacyCampaigns/migrationJob/status

#### Request

```bash
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/legacyCampaigns/migrationJob/status' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxxx' \
--header 'Accept: application/vnd.SponsoredBrands.SponsoredBrandsMigrationApi.v4+json' \
--data-raw '{
    "migrationJob": {
    "jobId": "1"
    }
 }'
```

#### Response

```json
{  
    "jobId": "1",  
    "migrationJobStatus": "`IN_PROGRESS`"
}
```

## VI. Important notes

* Begin with **zero sales/impressions campaigns** in **XX numbers**, and wait for the migration job to complete. Since the advertiser can only run one migration job at a time, it is recommended to **wait for the ongoing migration job to finish before starting another**.
* The **migrated campaign's start date** will be updated to the **date the migration begins**, while the campaign name will remain the same with a **"migrated" suffix** appended.
* Once a legacy (Version 3)  campaign enters the **migration phase**, avoid making **any updates or deletions** to related entities such as **ad groups, ads, keywords, or targets**. Modifying these entities during migration may disrupt the process and result in an **incomplete migration**.
* Ensure that the **brandEntityId** is included in the **startMigrationJob request for seller accounts**. If not provided, the migration job will attempt to fetch the existing **brandEntityId** associated with the campaign. If multiple or no brandEntityId is found, the **campaign migration will fail**.
* The **legacy** **campaign ID** can be retrieved from the **tags of the migrated version 4 campaign**. It is stored under the **"legacyCampaignId"** key in the migrated campaign's tags.
* Ensure that the **legacy campaign has a custom image** added under **creative**. If no custom image is present, the **campaign migration will fail**.
* The **supported landing pages** for **V2 campaigns** include **Simple Landing Page, Stores Page, Detail Page, and Custom Landing Page**. The campaign migration will fail for non-supported custom landing pages.
* The **migration job duration** can vary from **several minutes to several hours**, depending on the **overall traffic received by the Migration APIs**.

## VII. Frequently Asked Questions

#### Q1. Are there any recommended pre-requisite steps that should be considered before starting the migration?

Advertiser can use phased migration approach, focusing on systematically moving low-impact campaigns and advertisers with fewer campaigns. By starting with a small, carefully selected subset of campaigns, teams can identify potential issues, validate performance metrics, and ensure a smooth transition. The migration strategy prioritizes gradual implementation, allowing for real-time issue detection and mitigation, which protects advertiser interests and maintains system integrity. This methodical approach enables advertisers to confidently move their campaigns while minimizing potential disruptions, ultimately creating a more robust and reliable advertising ecosystem that supports incremental transformation and risk management.

#### Q2. What type of campaigns are allowed for migration?

This APIs only supports Sponsored Brands legacy campaign with the campaign state “enabled” or “paused” is eligible for migration. The campaign with **“archived” status are not eligible** for the migration.

#### Q3. What's are deadlines to complete the migration from version 3 campaigns to version 4 campaigns?

**Migration Deadline Requirements**

Advertisers must complete campaign migrations in two phases:

 1. Phase#1: Zero Impression Campaigns

* Deadline: July 30, 2025
* Migrate all campaigns with no impressions

2. Phase#2:  Impression and Sales Generating Campaigns

* Deadline: September 30
* Migrate campaigns with active impressions and sales

**Automatic Migration Contingency:** If advertisers do not complete voluntary migrations by September 30, Amazon will automatically migrate all Version 3 (V3) campaigns to Version 4 (V4) campaigns by December 31.
0 zero impression/sales - Q2
impressions/sales generated campaigns by - Q3, 2024

#### Q4. Why to use theme targets for campaigns with zero impressions? 

With Amazon's Theme Targeting, advertisers can now effortlessly expand their campaign reach using advanced machine learning algorithms. Customers benefit from intelligent keyword discovery that automatically identifies relevant search terms based on landing pages and brand interactions. By pre-selecting targeting groups, advertisers can simplify campaign creation, reduce manual keyword research, and increase the likelihood of reaching potential customers more effectively.

#### Q5. Will the version 3 campaign performance reporting metrics will be ported to new versions 4 campaigns?

With Amazon's Migration API, advertisers can now manage campaign transitions with precision and data integrity. The migration process ensures a clean transfer of campaign settings and structures while preventing unintended performance data carryover. Advertisers gain control over their data continuity by leveraging shadow campaign migration, enabling them to manually link and track performance metrics across version transitions. This approach empowers advertisers to maintain comprehensive performance insights while supporting a systematic, controlled migration that prioritizes data accuracy and system flexibility.

#### Q6. How will Amazon Ad Console show newly migrated version 4 vs legacy campaign?

With Amazon's Migration Console, advertisers can now easily track and manage campaign transitions with unprecedented clarity. The Ad Console automatically distinguishes migrated campaigns by appending a "-migrated" postfix to new Version 4 campaigns, while legacy campaigns are clearly marked as "Archived". This transparent approach ensures advertisers can seamlessly monitor their campaign status, with legacy campaigns automatically archiving only after successful Version 4 campaign creation. If migration encounters issues, the original campaign remains original status, preventing potential disruptions to ongoing advertising efforts.

#### Q7. Does all my campaign data and entities (adgroup and ad) entities accessible after staged campaign is created? 

After creating a staged campaign, only the campaignId will be accessible. However, this campaignId can’t be used to retrieve the campaign data. Once, you’re ready to enable staged campaign you should invoke [POST /sb/v4/legacyCampaigns/migrationJob](#post-sbv4legacycampaignsmigrationjob) API with isStagedMigration as false to enable the staged campaign.  The campaign data and all it’s child entities can only be accessed after campaign migration is successfully completed. 

#### Q8. Can I request to migrate campaigns that are under pending moderation status?

No, campaigns under pending moderation will not be allowed to migrate. You can request the migration once moderation is approved/rejected. Any legacy campaign with the state "enabled" or "paused" and for which moderation is not pending is eligible for migration.

#### Q9. How should I handle campaigns that received `migrationJobStatus=FAILED`?

You should retry once, Amazon will be alarmed in such case and will look at potential options to resolve such cases. Sometimes it can be a simple retryable network glitch and other times it may be due to the fact that old campaign not being compatible with new campaign. Either ways migration job will not archive legacy campaign unless new campaign is created with all the properties of legacy campaigns. We will expose another field called error codes along with error reason so that you can retry/not retry based on the response.

#### Q10. Is there any limit on how many times I can retry migration if I received `migrationJobStatus=FAILED`?

Currently for non retryable errors you should not retry migration but rather reach out to Amazon API support team for guidance. For retry-able errors you can retry upto 3 times per day.

#### Q11. What batch size do you recommend for migrating campaigns?

For the very first instance we’d recommend 10 campaigns and closely track the status along with details of new campaign to understand end to end impact. Once, small scale testing is done you can go upto 1000-1500 per job(number will get finalized once we go through stress testing).

#### Q12. What is the max limit for campaign ids in `/sb/v4/legacyCampaigns/migrationJob?`

The max limit 1000 campaigns. *number will get finalized once we go through stress testing.

#### Q13. Should I manually archive the legacy campaigns once version 4 campaigns are created in stage?

It's important not to manually archive legacy campaigns once new campaigns are created in the staging phase. This is because staged campaigns aren't live campaigns. Staged campaigns aren't active, so they won't receive any clicks or impressions. If you archive the campaign manually then campaign won't be converted through migration APIs. You'll have to create fresh campaign through existing campaign using similar properties

#### Q14. How will moderation of this new campaigns will happen?

New campaigns maybe moderated as per latest policies, currently there’s an open item in progress to try and auto approve/reject based on legacy campaign’s migration status

#### Q15. How should we handle the campaigns that uses product collection but does not have custom image?

It will fail with non retryable error. 

#### Q16. How to verify if all child entities are created as expected?

Post launch of new campaign, you can use V4 campaign management APIs for campaign, adgroup, ad and fetch campaign, adgroup, ad for legacy and migrated campaign to migrate all entities. You can also verify keywords by invoking /sb/keywords and /sb/negativeKeywords by using legacy and migrated campaignIds.

#### Q17. Can I migrate without using migration API to have better control? 

You can create a new campaign using current campaign management APIs or through Ad console based on current properties of a legacy campaign. However they will go through moderation again and re-moderated. Key advantage with this approach is you can archive/disable legacy campaign based on your control and also provides flexibility to use some of the new features at the time of campaign creation itself.

#### Q18. What if I want to use goal-based or theme targeting features in migrated campaign?

We would recommend creating a campaign with new properties through v4 campaign management APIs if you need to go beyond cloning of the campaign. Migration API will only clone the campaign properties without adding any new controls to avoid unwanted discrepancies. All newly cloned campaigns will default to “Drive Page Visits” as goal with  CPC as a costType.

#### Q19. Can I migrate Archived campaigns?

No, this is not allowed. Only enabled and paused campaigns without any pending moderation are in scope of migration.

#### Q20. What if I’m unable to enable a staged campaign?

Make sure no updates have been applied to campaign since staging and reach out Amazon support for further deep dive because its possible that you can miss last updates during transition state.

#### Q21. How long do we have after staging for staged campaign is enabled?

Ideally we expect this to be as near as possible to avoid the possibility of campaign properties being updated or re-moderated. 

#### Q22. How long does it take the migration job to complete and campaigns to get migrated ?

The migration job duration can vary from several minutes to several hours, depending on the overall traffic received by the Migration APIs.

#### Q23. How can I determine the reason for my campaign migration failure?

If your campaign migration fails, you can identify the error reason by checking the *status reason* of the campaign in the response from the `POST /sb/v4/legacyCampaigns/migrationJob/results` API. The failure is most likely due to a validation error, which means the advertiser needs to update the campaign to make it compatible for migration.

#### Q24. My campaign migration failed without any status reason?

While it is unlikely that this will happen, if your campaign migration fails without a status reason, please reach out to API support for assistance at the following link: [API Support](support/overview).

#### Q25. Can I get my legacy campaignId using the new migrated CampaignId ?

We store the legacy campaignId in the tags of newly migrated campaign. You can find the legacy campaignId under those tags.

#### Q26. What features are not available for legacy(V3) campaigns?

|No	|Feature Group	|Feature	|Legacy Campaigns	|Adgroup Campaigns	|
|---	|---	|---	|---	|---	|
|1	|Goal	|BRAND\_IMPRESSION\_SHARE	|Not Available	|Available	|
|---	|---	|---	|---	|---	|
|2	|	|PAGE\_VISIT	|Not Available	|Available	|
|5	|	|Smart Default	|Not Available	|Available	|
|6	|CostType	|CPC	|Available	|Available	|
|7	|	|VCPM	|Not Available	|Available	|
|8	|	|Capability to create Multi Adgroup and Ad	|Not Available	|Available	|
|	|Bidding	|AMC Shopper Cohort Bidding	|Not Available	|Available	|
|	|	|New to Brand Cohort Bidding	|Not Available	|Upcoming\*\*	|
|	|	|Top of Search Placement Bidding	|Not Available	|Upcoming\*\*	|
|9	|Targeting	|Keyword	|Available	|Available	|
|10	|	|Product Targeting	|Available	|Available	|
|11	|	|Theme Targeting	|Not Available	|Available	|
|12	|**Ad Format**	|Brand Video	|Not Available	|Available	|
|13	|	|Vertical Video	|Not Available	|Available	|
|14	|	|Slideshow with custom images	|Not Available	|Available	|
|15	|**Creatives**	|Creative Edits	|Basic available for Product Collection only	|Available for all ad creatives (Product Collection, Store Spotlight, Video, Brand Video, Multi-ASIN Video, Vertical Video, Slideshows)	|
|16	|	|Landing Page Edits	|Not Available	|Available	|
|17	|**Reporting**	|**New Metrics:** addToCart, addToCartRate, eCPAddToCart, newToBrandDetailPageViews, newToBrandDetailPageViewRate, newToBrandECPDetailPageViews	|Not Available	|Available	|
|18	|	|Support of Adgroup ID and Ad ID in SB Purchased Product Reporting	|Not Available	|Available	|
|19	|	|V3 Reporting	|Will be avaible Q3, 2024	|Available	|
|20	|	|Future new metrics support	|Not Available	|Available	|
|21	|**Amazon Marketing Stream (AMS)**	|Amazon Marketing Stream (AMS)	|Not Available	|Available	|
|22	|Authors	|Product Collection and Video	|Not Available	|Available	|
|23	|	|Creative Edits	|Not Available	|Available	|
|24	|	|Kindle Edition Normalized Page (KNEP) - Reads and Royalties metric	|Not Available	|Available	|

\*\***_upcoming features in 2025_**
