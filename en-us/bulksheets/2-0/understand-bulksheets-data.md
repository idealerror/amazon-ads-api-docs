---
title: Understand bulksheets campaign data 
description: A deeper dive to help you understand the available data that you can include when you create and download a custom bulksheets file.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
keywords:
    - bulksheets data
    - bulksheets  
---
# Understand bulksheets campaign data

When you create and download a custom spreadsheet for bulk operations, you will use a checklist to decide which data to include. This article will guide you on how to navigate this process. 

| [IMAGE] |   |
| ---- | ---- |
| Depending on what you choose to include, your downloaded bulksheet can have one tab for each campaign type: Sponsored Brands, Sponsored Display, and Sponsored Products. You can see entity IDs that you'll use to update campaigns in bulk, performance metrics for the date range you select, and other data. <br><br> If you include brand assets data, the sheet will also have a "Brand Assets Data" tab, which shows the rich media from your assets library. This data is important if you want to manage Sponsored Brands campaigns using bulksheets, so be sure to check this box if you are managing this campaign type.<br><br> The section below explains more details about customizig bulksheets campaign data. | ![Use the checklist to customize the data in your bulksheet](/_images/bulksheets/2-0-images/bulksheets-data-selection.png "Use the checklist to customize the data in your bulksheet") |


## Decide which campaign data you want to see

Use the checkboxes to customize the data that will appear in the sheet. 

* **Terminated campaigns**: Excluded by default. Check this box if you want to see data for campaigns that are no longer active.
* **Campaign items with zero impressions**: Excluded by default. **TIP:** For keyword optimization, it might be helpful to check this box to include items with zero impressions. This can help you see which keywords you might want to update or negate. 
* **Placement data for campaigns**: Included by default. This shows where your ads appeared on the page. 
* **Brand assets data**: Excluded by default. If you are managing Sponsored Brands campaigns, it might be helpful to include this data so you can easily see your asset IDs.  
* **Sponsored Products, Sponsored Brands, and Sponsored Display**: These are all included by default, but you can exclude any campaign types if you only want to see certain ones. Excluding any of these might speed up the dowlnload time. 
* **Guidance for Sponsored Products**: Included by default. This guidance will give you tips and help when creating Sponsored Products campaigns. [Learn more about input guidance for Sponsored Products](bulksheets/2-0/input-guidance)

**NOTE**: Brand assets must be in your creative assets library before they can be uploaded using bulksheets. This means you will need to use the advertising console to create a Sponsored Brands campaign, and then perform a bulksheets download that includes brand assets data to obtain the IDs. 

## Brand assets data: Additional information

Brand assets data is a collection of the creative assets you’ve used to create Sponsored Brands campaigns. When you create a new Sponsored Brands campaign with bulksheets, you will need certain IDs in order to populate your campaign with the correct logos, landing pages, and other data. You can find these IDs in the “brand assets data” tab in your downloaded bulksheets. 

* **Column A: Asset Type**: Options include backgroundVideo, brandLogo, customImage, otherImage, productImage, video. You can filter this column to isolate certain asset types. This is useful when creating Sponsored Brand campaigns, as you will need IDs such as brand logo or video ID. 

| [IMAGE] |   |
| ---- | ---- |
| **TIP**: Use a filter on Column A (“Asset Type”) to quickly find asset types → <br> <br> For example, if you need the “Brand Logo Asset ID,”  you can apply a filter Column A to show only the "brandLogo" fields.| ![Sponsored Brands asset type column](/_images/bulksheets/2-0-images/sb-asset-type-column.png)|

* **Column B: Brand Entity ID**: *Applies to sellers only*. Locate in Column B of the “Brand Assets Data” tab. Format is typically alpha-numeric, resembling `ENTITY00BR549`.
* **Column C: Asset ID**: Check Column A (“Asset Type”) to see which asset type the ID represents.
* **Column D: Asset Name**: The name you provided when you created/uploaded the asset.
* **Column E: Asset URL**: The link to the asset hosted on Amazon servers. 

**ADDITIONAL TIPS**

* You can **freeze** the top header row to make your bulksheets data easier to reference, especially if the sheet contains many rows that you need to scroll through. In MS Excel, under the “View” tab, click **Freeze top row**. In Google Sheets, under the “View” tab, select **Freeze > 1 row**.
* Add **filters** to all columns to make sorting and grouping easier. In Excel or Sheets, select the entire sheet by clicking the upper-left corner of the sheet. Then click the “filter” icon in Sheets or **Sort & Filter** in Excel. 
