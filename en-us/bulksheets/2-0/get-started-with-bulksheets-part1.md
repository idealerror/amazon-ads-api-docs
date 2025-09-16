---
title: Getting started with bulksheets - Part 1
description: General guide for getting started with bulksheets. Applies to all sponsored ads campaign types and is intended as an entry point to campaign-specific getting started guides.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
    - Onboarding
keywords:
    - get started 
    - bulksheets
---

# Get started with bulksheets: Part 1

[← Bulksheets general overview](bulksheets/2-0/overview-about-bulksheets)

This article gives a general overview of getting started with bulksheets for sponsored ads campaigns, including how to find the “Bulk operations” menu, how to create and download your first bulksheets file, and how to interpret the first three foundational columns of the spreadsheet. Then, you can select the appropriate product-specific guide based on the campaign type you want to create.

To get started:

|[TILES]For Vendors	|For Sellers	|
|---	|---	|
|1. Sign in to your Amazon Ads account	|1. Sign in to your Amazon Ads account	|
|2. Navigate to the advertising console	|2. Navigate to Seller Central	|
|3. From the left side menu panel, click **Sponsored ads > Bulk operations**	|3. Under "All campaigns," click **Bulk operations**	|

From the "Bulk operations" page, you can create, download, edit, and upload your spreadsheet to create or edit campaigns. You have two options for getting a bulksheets file: either create a custom spreadsheet, or download a blank template:

|[LINES]Create & download a custom spreadsheet	|Download a bulk operations template	|
|---	|---	|
|If you’ve already created some campaigns (in the ads console or using API), you can create a bulksheets file that contains your past campaign data. If you’re new to bulksheets, this might make it easier to see how the campaign data corresponds to spreadsheet entities and column headers.	|This option will produce a bulksheets template that is completely blank other than the column headers. You will have one tab for each campaign type: Sponsored Products, Sponsored Brands, and Sponsored Display. You can delete all tabs except the one that relates to the campaign type you want to create.	|

>[TIP]The articles that show how to create new campaigns show the blank template format. The ones that show how to update existing campaigns show the downloaded custom format.

The first step is to download a blank template, or create and download your custom spreadsheet.

![Bulksheets download options](/_images/bulksheets/2-0-images/GettingStartedPart1_BulkOpsMain.png "Bulksheets download options") 

The process outlined below walks through creating and downloading a custom spreadsheet. Then, you can get started with editing and uploading a bulksheets file for Sponsored Brands, Sponsored Display, and Sponsored Products campaigns by referencing the product-specific guides linked at the bottom of this article.

## How to create and download a custom spreadsheet

1. From the “Bulk operations” page, use the dropdown to select a **date range**. You can include data from a maximum of 60 days. 
2. Use the checkboxes to customize the data you want to include:

    2a. **Terminated campaigns**: Excluded by default. Check this box if you want to see data for campaigns that are no longer active.
    
    2b. **Campaign items with zero impressions**: Excluded by default. **TIP:** For keyword optimization, it might be helpful to check this box to include items with zero impressions. This can help you see which keywords you might want to update or negate. 
    
    2c. **Placement data for campaigns**: Included by default. This shows where your ads appeared on the page. 
    
    2d. **Brand assets data**: This applies to Sponsored Brands campaigns only. When you download a custom sheet, this field is checked by default. But if you are creating a Sponsored Brands campaign, you should check this box to include this data because you will need some of the asset IDs to create the Sponsored Brands campaign. **NOTE**: Brand assets must be in your creative assets library before they can be uploaded using bulksheets. This means you will need to use the advertising console to create a Sponsored Brands campaign, and then perform a bulksheets download that includes brand assets data to obtain the IDs. 

    2e. **Campaign types**: You can include the sponsored ads campaign types you want to see in the file. 
    
    2f. **Guidance for Sponsored Products**: If you're managing Sponsored Products campaigns, you can [include Excel input guidance](bulksheets/2-0/input-guidance). This feature provides hints and tips to help you understand what to enter in each field of the spreadsheet, and which fields are required for each campaign entity. 

    2g. **Sponsored Products search term data**: You can include search term report data for Sponsored Products, which will show the customer search terms that were used to find your ads. This sheet also includes the related campaign entity IDs, so you can make updates more efficiently in bulksheets. 

3. Click **Create spreadsheet for download**.

#### What to expect after you download your spreadsheet 

We recommend taking a few minutes to review the column headers and related data present in the sheet to get a sense of what the values and entities might look like. If you included **Guidance for Sponsored Products** in the download, you can click into the headers for tips, and some fields will have dropdown menus. 

You’ll have a “Portfolios” tab where you can see budget data and other information for any campaigns that you’ve grouped into a portfolio. Learn more about [using portfolios with bulksheets](bulksheets/2-0/bulksheets-portfolios).

If you’re creating a Sponsored Brands campaign and you opted to include **brand assets data**, your file will include a “Brand assets data” tab that contains entity IDs, landing page URLs, and other brand-related data. Learn more about how to [understand bulksheets campaign data](bulksheets/2-0/understand-bulksheets-data).

After you review the data, you can delete everything in the sheet except the column headers to create a brand new campaign. When updating existing campaigns, you can keep some of the campaign data in the sheet and edit only the fields you want to change--but this guide will focus on **creating a new campaign** using the new bulksheets blank template. We recommend reviewing these steps to get familiar with the new functionality before you start [updating existing campaigns](bulksheets/2-0/campaign-update-overview). 

#### Product, Entity, and Operation fields 

>[TIP]See a [side-by-side comparison of the old and new template](bulksheets/2-0/migration-guide#side-by-side-comparisons) in the migration guide

<br>
With the newest version of bulksheets, the first three column headers will always be **Product**, **Entity**, and **Operation**. Most of the spreadsheet column headers will vary depending on which type of campaign you’re managing, but the first three headers will be identical regardless of campaign type. And while the data you enter will vary based on “product” (or campaign type), here are some examples of what you might enter in these first three columns:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|_Enter the campaign type:_|_Enter the entity type. A few examples include:_|_Enter the action you want the spreadsheet to perform for that row. Typically the options are:_
|Sponsored Products	|Campaign	|Create	|
|Sponsored Brands	|Ad group	|Update	|
|Sponsored Display	|Keyword	|Archive|


>[TIP] If you included **input guidance for Sponsored Products**, these first three columns will all be dropdown menus with a list of possible values. You can also see additional tips and hints if you click into the column headers. 

#### Parent and child entity relationships

You can think of the “Entity” column as a hierarchy, with the campaign being the top-level “parent” entity. Under any campaign, you will have “child” entities such as ad groups. And under any ad group, you will have one or more product ads (or “Products” in the UI, in the form of an ASIN or SKU). For each ad, you’ll have keywords, and so on. The product-specific guides below include entity diagrams for reference. 

You will always need a new row for each entity, even if you are creating multiples of the same entity. For instance, to create 10 different keywords, you would need 10 different keyword rows under the “Entity” column. We’ll go deeper on this in the product-specific guides. 

These first three columns are the foundation of any advertising campaign: Similar to campaign creation in the advertising console UI, the first step is to choose your campaign type before defining the additional campaign attributes. 

#### Product-specific guides

To continue getting started with the new bulksheets and learn more about product-specific spreadsheet details, select the campaign type you’re using:

* [Sponsored Products: Get started with bulksheets](bulksheets/2-0/get-started-with-sp-bulksheets)
* [Sponsored Brands: Get started with bulksheets](bulksheets/2-0/get-started-with-sb-bulksheets)
* [Sponsored Display: Get started with bulksheets](bulksheets/2-0/get-started-with-sd-bulksheets)

