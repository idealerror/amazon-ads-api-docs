---
title: Bulksheets legacy general information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

>[WARNING] This content is for the legacy version of bulksheets, which is now deprecated. Effective immediately, the legacy template will no longer work. Visit the [new bulksheets overview](bulksheets/2-0/overview-about-bulksheets) to learn more about getting started with the new version, so you can take advantage of the latest features.

# General Information

Bulk operations allows advertisers to create, manage, and optimize multiple campaigns at scale, saving time and minimizing manual effort. Bulksheets is a spreadsheet-based tool that enables bulk operations sponsored ads campaigns. Advertisers can download their sponsored ads metrics in a bulksheet, make edits, and upload.

## Why use bulk operations?

* Save time with the ability to simultaneously add or remove hundreds of keywords and product attribute targets and adjust bids and budgets at scale
* Deliver results by downloading performance metrics against respective campaigns and optimizing them at scale
* Reduce latency with the ability to work offline by editing your bulksheet locally

## Getting started with bulk operations

Determining whether bulk operations is the right option for you is dependent on the amount of campaigns you manage. If you are creating a campaign for the first time or modifying a small number of campaigns, campaign manager in the advertising console is a good option. However, to manage scaled changes across a large number of campaigns, ad groups, ads, keywords, and product attribute targets, bulksheets is a more efficient option, helping you save time and effort.

To access the bulk operations tool for sellers, navigate to "Advertising" on the top navigation menu in Seller Central. You'll be directed to the advertising console, where you can select "Bulk operations" in the side navigation menu. For vendors, select "Bulk operations" in the side navigation menu within the advertising console.
![Bulk Operations](/_images/bulksheets/general-info/1.png)

## Managing Campaigns

Each Sponsored Products campaign is a collection of nested ad entities such as a Campaign, Ad Groups, Keywords, Ads etc. To create an entity at any level, creating its parent entity correctly is a prerequisite. Lack of this knowledge is a source of common confusion, often resulting in errors when trying to create and manage campaigns via Bulksheets. Hence, it is important to understand the relationship between different entity types for an ad product before creating them using Bulksheets. For example, see [Sponsored Products Entity Hierarchy](bulksheets/sp/sp-entities/sp-entity-hierarchy). If a Bulksheets user trying to create new Ad Group and Ads for a Sponsored Products campaign describes attributes for the Ad Group incorrectly, the Ads associated with that Ad Group will automatically fail.

You can create or manage campaigns by downloading a bulksheet template or a bulksheet with sponsored ads, i.e. a spreadsheet file (.xlsx or .xls) containing sponsored ads information. 

### Creating a new bulksheet 

Click "Download a bulk operations template" to download your bulksheet template. Open the downloaded .xlsx file and edit the rows in the tab titled "Sample create new <Sponsored Ad Type> campaign" to create a new campaign.

![Download Template](/_images/bulksheets/general-info/2.png)


![Template](/_images/bulksheets/general-info/3.png)

<br/>
<br/>

**Pro tip:** For ease of use, delete all tabs in the bulksheet except the one that corresponds to the sponsored ads campaign type you would like to create. For example, if you want to create a Sponsored Products campaign, you can delete all tabs in the template except Sample create new SP campaign. Use this template to create new rows that correspond to the Record Type, as shown in the template above.

### Using an existing bulksheet

You can also download existing campaigns into a bulksheet. Doing so will enable you to either update your existing campaigns or create new campaigns. To download your existing campaigns, visit the Create & download a custom spreadsheet section in the Bulk operations tab. 
![Download Bulksheet](/_images/bulksheets/general-info/4.png)

<br/>
<br/>

**Pro tip:** If you are interested in editing campaigns that are currently live, exclude Terminated Campaigns. Excluding terminated campaigns also expedites the bulksheet download. If you are downloading a bulksheet to only manage Sponsored Products ads, exclude Brand Asset Data as Sponsored Products do not need it.

You can use the following filter settings for the downloaded campaign data in a bulksheet:

* __Date range__ : Only entities with non-zero impressions in that date range will be downloaded to the bulksheet file. 
* __Exclude (checkboxes)__
  * __Terminated campaigns__: By default, terminated campaigns are excluded, i.e. this checkbox is selected. If the checkbox is checked, all terminated campaigns will be excluded from the downloaded bulksheet. This includes campaigns that have ended, have been stopped or deleted, or have been rejected (in the case of Sponsored Brands). 
  * __Campaign items with zero impressions__: By default, campaign items with zero impressions are excluded, i.e. this checkbox is selected. This means that campaigns with zero impressions will not be included in the downloaded bulksheet unless a user unchecks the box. As a result, the bulksheet will only include campaigns with one or more impressions with the exception of entities that will never have impressions (such as Sponsored Brands drafts, which are always included).
  * __Placement data for campaigns__: By default, placement data for campaigns is included, i.e. this checkbox is not selected. As a result, the downloaded report will include placement data. This means Sponsored Products and Sponsored Brands campaigns will have placement type attributes such as All, Top of shopping results, Product Detail Page, and Other Placements will be downloaded in the bulksheet along with the respective performance metrics.
  * __Brand assets data__: By default, brand asset information is excluded, i.e. this checkbox is selected. If un-checked, a Brand Assets Data tab will appear in the downloaded bulksheet. This is a read-only directory featuring all rich media that has been uploaded to the asset library, including the brand logo, images, and videos. 



### Uploading a bulksheet

Once your bulksheet is ready, navigate back to "Bulk operations" in the advertising console. Click "Choose file" and select the appropriate bulksheet from the location where you saved it. Supported file types are .xlsx and .xls. Click "Upload to process changes" to upload your bulksheet.

![Upload Bulksheet](/_images/bulksheets/general-info/5.png)

<br/>
<br/>

The amount of time required to upload varies based on the number of rows your bulksheet includes. Once uploaded, the appropriate status will appear in the "Status" column, letting you know whether the information uploaded successfully or not. Review any errors or warnings before you try re-uploading the bulksheet. To access an error report, click "Download Report" in the "Report" column. 


![Uploads](/_images/bulksheets/general-info/6.png)

<br/>
<br/>

__Note__: Once the upload process is complete, the system may take a few additional minutes to reflect the changes from your bulksheet in the Campaigns tab in Ads Console side navigation menu. You may need to periodically refresh the Bulk operations tab, as it does not self-refresh. 

<br/>
<br/>

**Pro tip:** To reduce processing time, ensure the bulksheet you are submitting contains only the rows you wish to update. In your spreadsheet, you can delete rows that do not contain updates and save your changes before uploading the file

### Measuring campaign performance with bulksheets

You can also use bulksheets to measure campaign performance at scale. The downloaded bulksheet includes various performance metrics including impressions, clicks, spend, orders, total units, sales, and advertising cost of sales (ACOS).

<br/>
<br/>

![Bulksheet](/_images/bulksheets/general-info/7.png)

<br/>
<br/>

**Few tips to consider when using bulksheets to measure campaign performance**

* Use a spreadsheet for analysis and insights: Use spreadsheet functions to analyze data quickly and identify campaigns that need optimization. 
* Download bulksheets periodically for comprehensive metrics: Download bulksheets regularly to view metrics and learn how your campaigns are performing over the long term. Only campaigns that have had impressions in the last 60 days can be downloaded. As a result, it is important to download bulksheets routinely to get insight into long-term trends, like year-over-year performance or performance between different holiday seasons over the years.

<br/>
<br/>