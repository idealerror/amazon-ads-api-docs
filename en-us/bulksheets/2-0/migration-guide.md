---
title: Bulksheets migration guide 
description: A guide to help you migrate from the legacy bulksheets to the new 2.0 version.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
    - Onboarding
keywords:
    - migration guide
    - bulksheets  
---

# Migration guide

>[WARNING] Legacy bulksheets is now deprecated. Effective immediately, the legacy template will not work, and you must use the new version to continue managing sponsored ads campaigns using bulksheets.

This guide is intended to help you migrate from the legacy bulksheets to the newest version. It will help you understand how the old template relates to the new template with a side-by-side comparison, along with other tips for success. 

The new bulksheets gives you access to more features that are already available to advertisers who use the API, so you can have the benefits of API functionality with the simplicity of a spreadsheet interface. To access bulksheets:

|[TILES]For Vendors    |For Sellers    |
|---    |---    |
|1. Sign in to your Amazon Ads account    |1. Sign in to your Amazon Ads account    |
|2. Navigate to the advertising console    |2. Navigate to Seller Central    |
|3. From the left side menu panel, click *Sponsored ads* *>* *Bulk operations*    |3. Under "All campaigns," click *Bulk operations*    |

You can download a new bulksheets file either by creating a custom spreadsheet or by downloading a blank template. If you aren’t sure how to get a bulksheets file, check out [Get started with bulksheets: Part 1](bulksheets/2-0/get-started-with-bulksheets-part1). Having a new bulksheets file on hand might make it easier to reference the changes outlined in the following sections. 

The sections below describe what you can expect with the new version of bulksheets for Sponsored Products, Sponsored Brands, and Sponsored Display campaigns. 

Jump to: [What’s new](bulksheets/2-0/migration-guide#Changes-in-the-new-bulksheets) | [Tips and guidelines](bulksheets/2-0/migration-guide#Tips-and-guidelines) | [Known issues and workarounds](bulksheets/2-0/migration-guide#known-issues-and-workarounds) | [Side-by-side comparisons](bulksheets/2-0/migration-guide#side-by-side-comparisons)

## Changes in the new bulksheets

A high-level overview of important changes in bulksheets

|[grid orange] New in bulksheets    |Important details    |
|---    |---    |
|Product field <br>(Column A)    |This is where you'll set the campaign type: Sponsored Products, Sponsored Brands, or Sponsored Display    |
|Entity field (Column B)    |This new field was "record type" in legacy bulksheets. It defines which part of the campaign you are creating or updating. A few examples include **campaign**, **ad group**, **keyword**.     |
|Operation field (Column C)    |In the new "Operation" column, you can indicate the action that should occur in a given row. Options include **create**, **update**, or **archive**. With Sponsored Brands bulksheets, there is also a *submit* option when you create draft campaigns, which creates the campaign but prevents it from launching while in draft mode. <br>For any row where you want to perform an action, the "Operation" field is required. This new feature can save time when uploading bulksheets file by ignoring data that you don't want to update.    |
|Update campaign and ad group names    |You can now [update the names of campaigns](bulksheets/2-0/campaign-update-overview) and ad groups. This lets you reuse spreadsheets and update these names more efficiently in bulk.     |
|Support for Sponsored Display    |You can now use bulksheets to manage your Sponsored Display campaigns. Learn more in [Get started with Sponsored Display](bulksheets/2-0/get-started-with-sd-bulksheets).     |
|Run campaigns indefinitely    |With the new bulksheets, you can allow campaigns to continue running with no set end date by leaving the "End date" field blank   |
|Support for Sponsored Brands product targeting and all ad formats    |Sponsored Brands now supports product targeting and additional ad formats in bulksheets. Learn more in [Get started with Sponsored Brands](bulksheets/2-0/get-started-with-sb-bulksheets).    |
|New metrics available   |When you create and download a bulksheets file, you can now see CPC, ROAS, and Conversation Rate to facilitate bid optimization.    |
|Date format    |In the new bulksheets, the date format must be entered as YYYYMMDD, so February 25, 2022 would be entered as **20220225**. July 12, 2023 would be **20230712**.     |



## Tips and guidelines

|[grid orange]Tips for using 2.0    |Important details    |
|---    |---    |
|Brand assets data for Sponsored Brands    |When you download a bulksheets file from the "Bulk operations" page, you will see a checkbox that will exclude brand assets data. If you want to see brand assets for your Sponsored Brands campaigns, you should uncheck this box.     |
|Updating keywords    |If you want to change the keywords associated with a campaign, you can do this in a single bulksheets upload: For the keywords you want to remove, set the "Operation" field to **archive** and include the Keyword IDs. In that same sheet, you can add new keywords using **create** in the "Operation" field and entering new ones in the "Keyword Text" field.      |
|Add multiple ad groups in Sponsored Products campaigns    |You can add multiple ad groups under a single Sponsored Products campaign to make it easier to group your ads by category, season, or any other attribute. This is not possible in the advertising console.     |
|Read-only fields    |Fields marked as read-only aren't important when you are creating a new campaign, and they cannot be edited. But these fields can be helpful when you review downloaded campaign data. For example, there is a read-only "Campaign ID" field. If you wanted to view all keywords associated with that campaign, you could filter by Campaign ID to see only the keywords used in that campaign.      |


## Known issues and workarounds

|[grid orange]Known issues    |Workarounds    |
|---    |---    |
|Campaign negative keywords can't be paused     | For any campaign negative keywords that you want to delete or no longer use, you should set the "Operation" to be **Archive**. Note that archiving a campaign negative keyword will also delete the keyword from all records.
|Commas and symbols are not supported in numbers    |When writing numbers, do not use commas or symbols such as % or $. For example, one-thousand five hundred should be written as 1500. Fifty percent would be written as 50.
|Keyword limitation for Sponsored Brands    |For Sponsored Brands only: When creating campaigns, 100 keywords is the maximum allowed limit. If you enter more than 100 keywords, you will see an error after you upload the bulksheets file. When updating campaigns, you can add as many keywords as you want.<br> <br>A workaround for the keyword limitation is to submit two bulksheets uploads: First, create the campaign using just one or a few keywords. Then, after the file uploads, submit an additional upload for that same campaign using the *update* operation. In the updated file, you can add as many keywords as you want to the campaign.     |
|Sponsored Brands performance data by placement is not available     |Use the legacy version of Bulksheets to see placement-specific performance data for Sponsored Brands.     |


## Side-by-side comparisons

The table below shows a comparison of the column headers in the legacy version of bulksheets and the new version so you can see how the legacy column data maps to the new version for Sponsored Products and Sponsored Brands. We didn’t include a comparison for Sponsored Display because that campaign type wasn’t supported in legacy bulksheets.  


### Sponsored Products

|[grid orange]Legacy column header    |Equivalent header in new bulksheets    |Tips    |
|---    |---    |---    |
|Record ID    |This field no longer exists in the new bulksheets. Instead, each entity now has its own ID column: Campaign ID, Portfolio ID, Product ad ID,  Ad group ID, and so on.    |When you create a new entity, Amazon will generate its ID. Then, you can find the ID when you download a bulksheets file with campaign data.    |
|Record Type    |Entity    |The entity you're creating or updating. For example, *campaign*, *ad group*, *keyword*, and so on.    |
|Campaign ID    |Campaign Id    |When creating a new campaign, enter a text-based name. When updating a campaign, enter the Amazon-generated ID.    |
|Campaign    |Campaign Name    |Enter a text-based campaign name.     |
|Campaign Daily Budget    |Daily Budget    |Do not use commas when formatting numbers. Use periods only.     |
|Portfolio ID    |Portfolio Id    |To associate a new campaign with an existing portfolio, enter the ID in this field when you define the campaign entity. You can't create new portfolios with bulksheets at this time.      |
|Campaign Start Date    |Start date    | Format is YYYYMMDD. Be sure the start date occurs in the future, or you will get an error. This sometimes happens when you set the date to the current time, but you don't upload the bulksheets file until later.     |
|Campaign End Date    |End date    | Optional if you want the campaign to run indefinitely. If used, the format is YYYYMMDD.    |
|Campaign Targeting Type    |Targeting Type    |For Sponsored Products campaigns, this is either *manual* or *auto*.     |
|Ad Group    |Ad Group Name    |    |
|Max Bid    |This field is split into separate columns now: *Ad Group Default Bid* and *Bid* (for keywords and targeting).     |If the *Bid* column is left blank for a keyword or targeting entity, the *Ad Group Default Bid* will be applied. <br> <br> For example, if you want all keywords within an ad group to have the same bid, you can leave the *Bid* column blank for those keyword rows and have the *Ad Group Default Bid* apply to the child entities.     |
|Keyword or Product Targeting    |This field is split into separate columns now: *Keyword Text* and *Product Targeting Expression*.     |    |
|Product Targeting ID    |Product Targeting Id    |You can get this ID, and all entity IDs, in a downloaded bulksheets file with past campaign data.    |
|Match Type    |Match Type    |This field is only used when creating a *keyword* entity.    |
|SKU/ASIN    |In the new bulksheets, you will see separate columns for SKU and ASIN.     |You should enter data into one of these fields and leave the other blank, depending on whether you're a vendor (use ASIN) or a seller (use SKU).     |
|Campaign Status    |State    |In new bulksheets, you will use the same *State* column for each entity you create to set the status. For *Campaign Status* in the new bulksheets, use the *State* field in the row where you are creating the *Campaign* entity.     |
|Ad Group Status    |State    |Similar to the above: In the row where you are creating an *Ad Group*, use the *State* field to define status.     |
|Status    |State    |Enter *enabled* if you want the entity to be live after you upload the file. Other options include *paused* and *archived*.    |
|Bidding strategy    |Bidding Strategy    | Set at the campaign level. Can be *dynamic bids - down only*, *dynamic bids - up and down*, or *fixed bid*   |
|Placement Type    |Placement    |    |
|Increase bids by placement    |Percentage    |Do not use symbols such as `%`.<br> <br>For example, `50%` would be entered as `50`.    |


### Sponsored Brands

|[grid orange]Legacy column header    |Equivalent header in new version    |Tips    |
|---    |---    |---    |
|Record ID    |This is replaced by the individual entity IDs (Campaign ID, Keyword ID, and so on).    |When you create a new entity, Amazon will generate its ID. Then, you can find the ID when you download a bulksheets file with campaign data.    |
|Record Type    |Entity    |The entity you're creating or updating. For example, *campaign*, *ad group*, *keyword*, and so on.    |
|Campaign ID    |This is split up into 2 columns; the "Campaign Id" column is for submitted SB campaigns the "Draft Campaign Id" column is for draft campaigns.    |When creating a new campaign, enter a text-based name. When updating a campaign, enter the Amazon-generated ID.    |
|Campaign    |Campaign Name    |Enter a text-based campaign name.     |
|Campaign Type    |This column doesn't exist in the new bulksheets.     |This information is conveyed in the entity column. Submitted campaigns should be specified as "Campaign", whereas draft campaigns should be specified as "Draft Campaign".   |
|Ad Format    |Ad Format    |Ad format options in bulksheets include "Product Collection" and "Video". Bulksheets doesn't support "Store Spotlight" currently.    |
|Budget    |Budget    |Do not include any symbols such as percent, dollar sign, or commas. Use numbers and periods only.    |
|Portfolio ID    |Portfolio Id    |Used only in the campaign entity. You can enter an ID if you want to associate the campaign with a portfolio. This field can only be used if you already have a portfolio in campaign manager--you cannot create new portfolios using bulksheets.    |
|Campaign Start Date    |Start date    |Format is YYYYMMDD. Be sure the start date occurs in the future, or you will get an error. This sometimes happens when you set the date to the current time, but you don't upload the bulksheets file until later.    |
|Campaign End Date    |End date    | Optional if you want the campaign to run indefinitely. If used, format is YYYYMMDD.    |
|Budget Type    |Budget Type    |    |
|Landing Page Url    |Landing Page Url    |When you create a SB campaign with Amazon Store or Custom URL landing page type, this defines the landing page URL. Do not use this field to create a new landing page. Use the Landing Page ASINs field instead.    |
|Landing Page ASINs    |Landing Page ASINs    |When you create a campaign with a new landing page, this field defines which products are shown on the page.     |
|Brand Name    |Brand Name    |    |
|Brand Entity ID    |Brand Entity Id    |For sellers only. This field was optional in legacy bulksheets, but is now required in the new version. Each asset (video or picture) has an associated Brand Entity ID, which you should copy from the Brand Assets Data Excel sheet when creating new Sponsored Brands campaigns.    |
|Brand Logo Asset ID    |Brand Logo Asset Id    |If you need this ID, be sure to include brand assets data when you download a bulksheets file.    |
|Headline    |Creative Headline    |    |
|Creative ASINs    |Creative ASINs    |    |
|Media ID    |Video Media Ids    |    |
|Automated Bidding    |Bid Optimization    |    |
|Bid Multiplier    |Bid Multiplier    |    |
|Ad Group    |This column doesn't exist in the new bulksheets.    |This field was removed entirely, because Ad Groups are automatically created and managed by the Sponsored Brands service.    |
|Max Bid    |Bid    |The meaning is the same: This is the CPC amount you are willing to pay when a shopper clicks your ad. As with all number formatting in the new bulksheets, do not use symbols or commas.     |
|Keyword    |Keyword Text    |Enter one keyword or phrase per row. To enter more keywords, add a new entity for each keyword.    |
|Match Type    |Match Type    |    |
|Campaign Status    |State    |The "State" column now defines the status for each entity.    |
|Ad Group Status    |This column doesn't exist in the new bulksheets.    |This field was removed entirely, because Ad Groups are automatically created and managed by the Sponsored Brands service.    |
|Status    |State    |Enter *enabled* if you want the entity to be live after you upload the file. Other options include *paused* and *archived*.    |
|Placement Type    |This column doesn't exist in the new bulksheets.    |Placement-level performance data will be supported in the future.    |
