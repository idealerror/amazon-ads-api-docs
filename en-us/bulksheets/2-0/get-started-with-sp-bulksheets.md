---
title: Getting started with bulksheets for Sponsored Products 
description: A guide to help you manage Sponsored Products campaigns using bulksheets.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - get started
    - bulksheets  
---

# Get started with bulksheets for Sponsored Products

>[WARNING] Be sure you’ve reviewed [part one of this guide](bulksheets/2-0/get-started-with-bulksheets-part1), which walks through Step 1 in the “Bulk operations” process shown below. You should already have your downloaded template ready before proceeding with step 2, editing your file. 

This article will walk through the steps of getting started with bulksheets for Sponsored Products. You’ll find guidance and tips for entering campaign data into your downloaded spreadsheet so you can upload the sheet to create campaigns using bulksheets.

![Bulk operations main screen](/_images/bulksheets/2-0-images/all-get-started-main-bulk-ops-pg.png "Bulk operations main screen")


### Prepare to edit your bulksheets file

Before you start entering data into your file, here are some guidelines and tips to help you understand the functionality of the sheet. 

**Allowable values for the first three columns**

Many of the bulksheets values will vary depending on your specific situation. For example, “Bid” amount and “ASIN” values might vary greatly depending on the individual user or campaign. But some columns have strict limitations on what you can enter. In particular, for the first three foundational columns for Sponsored Products, here are the allowed values:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Products	|Campaign	|Create	|
|	|Ad group	|Update	|
|	|Product ad	|Archive	|
|	|Keyword	|	|
|	|Negative keyword	|	|
|	|Bidding adjustment	|	|
|	|Campaign negative keyword	|	|
|	|Product targeting	|	|
|	|Negative product targeting	|	|

**TIPS:**

* You can include [input guidance](bulksheets/2-0/input-guidance) in your downloaded file for Sponsored Products for hints about what you should enter in each field.
* You will **always need a new row for each entity**, even if you are creating multiples of the same entity. For instance, to create 10 different keywords, you would need 10 different keyword rows.
* With Sponsored Products, you can create **multiple ad groups in a single campaign**.
* Be sure to always include the “Product” (or campaign type) in Column A for every row that contains an entity you want to create. 
* Every **entity** (in Column B) will be in its own separate row. Then, you will define attributes for each entity in the other cells that span that specific row. 
* The “Operation” column (Column C) must contain a value in any row where you want updates to occur. Otherwise, the row will be ignored. If you’re creating a new campaign, then you would type **Create** in each row of the “Operation” column for each entity in that campaign. 

**EXAMPLE 1**: To create a Sponsored Products campaign with one ad group, one product ad, and five keywords, you would set your first three columns as:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Products	|Campaign	|Create	|
|Sponsored Products	|Ad group	|Create	|
|Sponsored Products	|Product ad	|Create	|
|Sponsored Products	|Keyword	|Create	|
|Sponsored Products	|Keyword	|Create	|
|Sponsored Products	|Keyword	|Create	|
|Sponsored Products	|Keyword	|Create	|
|Sponsored Products	|Keyword	|Create	|

If you wanted to add multiple ad groups to a campaign, you would add more rows setting the “Entity” and “Operation” columns accordingly.

**EXAMPLE 2**: With bulksheets, you can create campaigns that use both keyword and product targeting by creating a new ad group for each targeting type (you cannot have both keyword and product targeting in a single ad group). In the example below, you’re creating one campaign with *two ad groups*. The first ad group contains *three keywords*. The second ad group contains *two product targeting* entities. Using this example, you could add as many ad groups as you want in a single campaign using bulksheets:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Products	|Campaign	|Create	|
|Sponsored Products	|Ad group	|Create	|
|Sponsored Products	|Product ad	|Create	|
|Sponsored Products	|Keyword	|Create	|
|Sponsored Products	|Keyword	|Create	|
|Sponsored Products	|Keyword	|Create	|
|Sponsored Products	|Ad group	|Create	|
|Sponsored Products	|Product ad	|Create	|
|Sponsored Products	|Product targeting	|Create	|
|Sponsored Products	|Product targeting	|Create	|

If you’re ready to complete the campaign creation process, check out [How to create Sponsored Products campaigns in bulksheets](bulksheets/2-0/create-sp-campaign).
