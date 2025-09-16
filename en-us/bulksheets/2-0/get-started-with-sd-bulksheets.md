---
title: Getting started with bulksheets for Sponsored Display 
description: A guide to help you manage Sponsored Display campaigns using bulksheets.
type: guide
interface: bulk-operations
tags:
    - Sponsored Display
    - Campaign management
keywords:
    - get started
    - bulksheets  
---

# Get started with bulksheets for Sponsored Display

>[WARNING] Be sure you’ve reviewed [part one of this guide](bulksheets/2-0/get-started-with-bulksheets-part1), which walks through Step 1 in the “Bulk operations” process shown below. You should already have your downloaded template ready before proceeding with step 2, editing your file.

This article will walk through the steps of getting started with bulksheets for Sponsored Display. You’ll find guidance and tips for entering campaign data into your downloaded spreadsheet so you can upload the sheet to create campaigns using bulksheets. 

![Bulk operations main screen](/_images/bulksheets/2-0-images/all-get-started-main-bulk-ops-pg.png "Bulk operations main screen")

### Prepare to edit your bulksheets file

Before you start entering data into your file, here are some guidelines and tips to help you understand the functionality of the sheet. 

**Allowable values for the first three columns**

Many of the bulksheets values will vary depending on your specific situation. For example, “Bid” amount and “ASIN” values might vary greatly depending on the individual user or campaign. But some columns have strict limitations on what you can enter. In particular, for the first three foundational columns for Sponsored Display, here are the allowed values:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Display	|Campaign	|Create	|
|	|Ad group	|Update	|
|	|Product ad	|Archive	|
|	|Audience targeting	|	|
|	|Contextual targeting	|	|
|	|Negative product targeting	|	|

**TIPS:**

* Sponsored Display campaigns require a targeting tactic: either **Audience targeting** or **Product targeting**. To target audiences *on and off Amazon*, select **Audience targeting**.
* To target the intended product or audience, you’ll need the related IDs for elements such as product category or audience lifestyle. To get these IDs, you can use past campaign data by exporting data into a bulksheet from campaigns where you assigned those targeting expressions. The **Resolved Product Targeting Expression** column will show the values as they appear in the UI. You can then map this value to the corresponding ID in the **Product Targeting Expression** column.
* Be sure to always include the “Product” (campaign type like Sponsored Display, Sponsored Products, or Sponsored Brands) in Column A for every row that contains an entity you want to create. 
* Every **entity** (in Column B) will be in its own separate row. Then, you will define attributes for each entity in the other cells that span that specific row. 
* The “Operation” column must contain a value in any row where you want updates to occur. Otherwise, the row will be ignored. If you’re creating a new campaign, then you would type **Create** in each row of the “Operation” column for each entity in that campaign. To update or archive, set the “Operation” column to **Update** or **Archive** accordingly. 

**EXAMPLE 1**: To create a Sponsored Display campaign with one ad group, one product ad, and audience targeting, your first three columns would look like this:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Display	|Campaign	|Create	|
|Sponsored Display	|Ad group	|Create	|
|Sponsored Display	|Product ad	|Create	|
|Sponsored Display	|Audience targeting	|Create	|

**EXAMPLE 2**: If you want to have multiple product ads for the campaign, use a new row for each product ad:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Display	|Campaign	|Create	|
|Sponsored Display	|Ad group	|Create	|
|Sponsored Display	|Product ad	|Create	|
|Sponsored Display	|Product ad	|Create	|
|Sponsored Display	|Product ad	|Create	|
|Sponsored Display	|Product ad	|Create	|
|Sponsored Display	|Audience targeting	|Create	|

For each entity row, you’ll define more details about each entity in the remaining columns. The next article walks through the complete [Sponsored Display campaign creation process](bulksheets/2-0/create-sd-campaign), including guidelines on Sponsored Display entities and how they factor into the steps to create a bulksheets file. 
