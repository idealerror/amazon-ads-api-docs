---
title: Bulksheets 2.0 release notes 
description: A repository of release notes for bulksheets 2.0, starting in December 2021 when the new 2.0 version was launched. 
type: release-note
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
keywords:
    - release notes
    - bulksheets  
---

# Bulksheets release notes

This document contains release notes related to the bulk operations before 2023. For more recent announcements, see [Release notes](release-notes/index).

## November 2022

### View multiple ad groups for Sponsored Brands campaigns in bulksheets downloads
When you create Sponsored Brands campaigns in the advertising console, you can create multiple ad groups within the campaign. Now, you can view targets for all of these ad groups if you download a bulksheets custom spreadsheet and include Sponsored Brands in the file. This feature lets you view keyword and product targeting for all ad groups in the downloaded file. Note that you cannot *create* multiple Sponsored Brands ad groups using bulksheets—you must still use the console to create. 


## October 2022

### Expanded product targeting is available for Sponsored Products in bulksheets

You can now use **expanded product targeting** for Sponsored Products manual targeting campaigns in bulksheets. With this feature, you can expand targeting to items that are closely related to the product you want to target. This can help you discover additional products related to the ones you're already targeting. 

To use expanded product targeting in bulksheets, define the target in the row where you are [creating the "product targeting" entity in your bulksheet file](bulksheets/2-0/create-sp-campaign#step-4b-create-the-product-targeting-entity). Set the "product targeting expression" value to be **asin-expanded="12345"**, using the value of the ASIN you want to target. 

## August 2022

### Sponsored Display product targeting has evolved to contextual targeting

We have evolved Sponsored Display’s product targeting into contextual targeting, allowing advertisers to reach more of their audiences at more points in their shopping and entertainment journey. The contextual targeting strategy will now allow advertisers to reach audiences both in the Amazon store and outside the Amazon store when customers view content related to targeting strategies. If you [use bulksheets to create and manage Sponsored Display campaigns](bulksheets/2-0/create-sd-campaign), the “product targeting” entity will now be “contextual targeting.” 

Learn more about [Sponsored Display’s new contextual targeting](https://advertising.amazon.com/resources/whats-new/sponsored-display-contextual-targeting), including more details about the launch and why it’s important to advertisers. 


## June 2022

### ASIN eligibility is now shown in bulksheets

When you download a Sponsored Products bulksheets file, you can now see ASIN eligibility statuses, along with reasons for ineligibility. This information will be available in two new informational-only columns: **Eligibility Status** and **Reason for Ineligibility**. Sellers and vendors can find these new columns next to the SKU field in the bulksheets file.

Without this information, it can be difficult to understand why your ad might be getting fewer clicks than expected. For example, a product might become ineligible after you create the ad, or a product might have gone out of stock, resulting in missed sales and wasted time on ASIN troubleshooting. Having ASIN eligibility statuses in bulksheets can help you make more informed decisions when optimizing campaigns, and provides even greater parity with the API when using bulksheets.

## May 2022

### Some informational-only fields have been moved closer to the front of bulksheets files 

Some of the name-based informational-only fields have been rearranged in bulksheets. Now when you download a bulksheets file, you can see fields such as **campaign name** and **ad group name** without needing to scroll as far. Previously, these fields were placed near the end of a bulksheets file, and this made it difficult to associate the names with their related fields such as keyword, budget, and start date. 

### Portfolio Name field is now available in bulksheets downloads 

You can now see portfolio names in bulksheets downloads. Previously, only the portfolio ID was shown. This update will make it easier to see if a campaign is associated with a particular portfolio. 

### Ad format is shown in downloaded Sponsored Brands files even if filters are applied 

In Sponsored Brands bulksheets downloads, you can now see the **ad format** when filters are applied. Previously, when you filtered by keywords, the ad format was not visible. With this change, you can filter by keywords and see if a keyword is for a video campaign or a non-video campaign. When you apply filters, the ad format will now be visible in a new “Ad format (informational only)” field. 

## April 2022

### Multiple ad groups are now supported for Sponsored Display

You can now create multiple ad groups in a single Sponsored Display campaign using bulksheets. For campaigns that are already running, you can also add more ad groups to existing campaigns. Previously, Sponsored Display only supported one ad group per campaign. Learn more about [managing Sponsored Display campaigns using bulksheets](bulksheets/2-0/get-started-with-sd-bulksheets).

### ASIN codes are now included in bulksheets downloads for sellers

Sellers now can see the ASIN code associated with your SKU input in bulksheets for Sponsored Products and Sponsored Display. The ASIN code will appear in the "ASIN (informational only)" column when you request a bulksheets download. Previously, the ASIN column was empty for sellers, and there wasn’t a way to know which ASINs corresponded to SKUs in bulksheets. 

## March 2022

### Updates to the bulk operations page

We are launching some cosmetic updates for the [bulk operations homepage](https://advertising.amazon.com/bulksheet/HomePage). This is the page where you can create and download a bulksheets file to manage campaigns in bulk. The changes include updates such as button styles. The functionality will not be affected by these updates. 

### View click-through rates in bulksheets 

Previously, click-through rates had to be calculated manually in bulksheets. Now, you can see see click-through rates when you download a bulksheets file. This new column can be found between the "Spend" and "Clicks" columns in the Sponsored Products, Sponsored Brands, and Sponsored Display tabs. 

## February 2022 

### Sponsored Display now supports similar-product targeting
When managing Sponsored Display campaigns using bulksheets, you can now target based on similar products. Using the "Targeting Expression" field, enter **similar-product** if you want your ads to be shown to shoppers who viewed the detail pages of products that are similar to your advertised products.

Learn more about [managing Sponsored Display campaigns using bulksheets](bulksheets/2-0/create-sd-campaign)

## January 2022

### New bulksheets download filter for campaign type
Effective January 21, 2022, you now can filter to include or exclude Sponsored Products, Sponsored Brands, and Sponsored Display during the bulk download process. For advertisers or agencies managing large numbers of campaigns, this new filter can help expedite the download process by limiting the file to certain campaign types. 

### Portfolios are supported in bulksheets for all campaign types
Portfolios are available in bulksheets for Sponsored Products, Sponsored Brands, and Sponsored Display. You can now create and update portfolios, including portfolio budgets. Portflolios in bulksheets can not only help you organize campaigns and budgets, but are also a way to update bid optimizations in bulk. 

## December 2021

### New version of bulksheets is now available
The new bulksheets gives you access to more features that are already available to advertisers who use the API, so you can have the benefits of API, with the functionality of a spreadsheet interface. With the newest version, you can 

* update campaign and ad group names
* add multiple ad groups, keywords, and targets in bulk
* see campaign metrics such as impressions, clicks, ACoS, CPC, ROAS, and more
* manage Sponsored Display campaigns. 

Learn more about [the new bulksheets](bulksheets/2-0/overview-about-bulksheets) 


