---
title: Overview - how to update campaigns with bulksheets 
description: A general overview on how to make updates to campaigns in bulksheets. Applies to all sponsored ads campaign types.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
keywords:
    - update campaigns
    - bulksheets  
---

# Overview - Update campaigns with bulksheets

This article provides an overview to help you get started with using bulksheets for campaign management and optimization. We’ll cover how you can use a downloaded spreadsheet as the basis for updating a campaign, along with a list of the fields that cannot be changed or updated. 

## Create & download a custom spreadsheet

Before you edit a bulksheets file, you should first [download a file from the “Bulk operations” page](bulksheets/2-0/understand-bulksheets-data). This file will contain past campaign data, including entity IDs and other information depending on which options you select in the download process. You can use this downloaded file as a template to start with when you update a campaign, if you don’t want to start from a blank template. Note that the downloaded file will contain additional column headers including read-only and performance metrics—these fields will be ignored in the upload, so you can leave them in the sheet when you upload your file instead of spending time deleting the data. 

>[NOTE] The examples below use Sponsored Products campaigns, but the general guidelines and linked resources apply to all campaign types. <br><br> Also note that bulksheets entity IDs are different than the IDs in the advertising console, so be sure you're getting the IDs from a bulksheets download, and not from the console.

#### Sponsored Products example 1a: Downloaded file unchanged

The example below shows a downloaded bulksheets template that contains past campaign data, exactly as it might look after you create and download a custom spreadsheet—to save horizontal space, the metrics columns are not included: 

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id (Read only)    |Keyword Id (Read only)    |Product Targeting Id (Read only)    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Campaign	|	|6555226389	|	|	|	|	|	|SP Auto Holidays 2022	|	|20220105	|20220719	|AUTO	|enabled	|110	|	|	|	|	|	|	|Dynamic bids - down only	|	|
|Sponsored Products	|Bidding Adjustment	|	|6555226389	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|Dynamic bids - down only	|placementProductPage	|
|Sponsored Products	|Ad Group	|	|6555226389	|8796676911	|	|	|	|	|	|Baseball toys 	|	|	|	|enabled	|	|	|	|2.75	|	|	|	|	|	|
|Sponsored Products	|Product Ad	|	|6555226389	|8796676911	|	|729402352	|	|	|	|	|	|	|	|enabled	|	|	|B07NDMCL1D	|	|	|	|	|	|	|
|Sponsored Products	|Product Ad	|	|6555226389	|8796676911	|	|207480567	|	|	|	|	|	|	|	|enabled	|	|	|B0864QL8CG	|	|	|	|	|	|	|

For this example, let’s assume you only want to update the **campaign name**. Using the downloaded spreadsheet shown above, you can update the relevant fields only, and leave the other data unchanged to make this update. 

#### Sponsored Products example 1b: Downloaded file with updated campaign name

To update a campaign name, the mandatory required fields are **Product**, **Entity**, **Operation**, **Campaign ID**, and **Campaign Name**. Since some of those fields are already populated in the file, you only need to change values in the **Operation** field and **Campaign Name** field for that row. See the additional notes below the example for more information. 

Here’s how the updated sheet would look—the updated fields are shown in **bold**:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id (Read only)    |Keyword Id (Read only)    |Product Targeting Id (Read only)    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Campaign	|**Update**	|6555226389	|	|	|	|	|	|**New SP Campaign Name**	|	|20220105	|20220719	|AUTO	|enabled	|110	|	|	|	|	|	|	|Dynamic bids - down only	|	|
|Sponsored Products	|Bidding Adjustment	|	|6555226389	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|Dynamic bids - down only	|placementProductPage	|
|Sponsored Products	|Ad Group	|	|6555226389	|8796676911	|	|	|	|	|	|Baseball toys	|	|	|	|enabled	|	|	|	|2.75	|	|	|	|	|	|
|Sponsored Products	|Product Ad	|	|6555226389	|8796676911	|	|729402352	|	|	|	|	|	|	|	|enabled	|	|	|B07NDMCL1D	|	|	|	|	|	|	|
|Sponsored Products	|Product Ad	|	|6555226389	|8796676911	|	|207480567	|	|	|	|	|	|	|	|enabled	|	|	|B0864QL8CG	|	|	|	|	|	|	|

<br>
####Additional notes

* If the “Operation” field is blank, the data in that spreadsheet row will be ignored completely when you upload the file. This means that you can leave data in the spreadsheet even if you don’t want to update it, as long as you leave the “Operation” field blank. If your file contains many rows, this might be more efficient than deleting the rows that you don’t want to update. 
* For the remaining fields that contain values, you could delete the values and leave the fields blank since they are not required for updating a campaign name, but you might find it more convenient to leave them unchanged since it will not affect the upload. 
* **Start Date** and **Targeting Type** fields cannot be modified after the campaign is created, so you must either leave them exactly as they are, or delete them altogether. If you change these values, you will get an error when you upload the file. Learn more about [which fields you can and cannot change when updating campaigns](bulksheets/2-0/editable-non-editable-bulksheets-fields) 
* **End Date** can be left unchanged, or it can be updated if you enter new date using the format yyyyMMdd. If you leave this field blank, it will remove the end date from the campaign, and the campaign will run indefinitely.

## More information about updating campaigns 

For a more thorough walkthrough of updating specific campaign types, check out the articles linked below. 

* [Update Sponsored Products campaigns](bulksheets/2-0/update-sp-campaigns) 
* [Update Sponsored Brands campaigns](bulksheets/2-0/update-sb-campaigns) 
* [Update Sponsored Display campaigns](bulksheets/2-0/update-sd-campaigns) 
