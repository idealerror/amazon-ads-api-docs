---
title: Getting started with bulksheets for Sponsored Brands 
description: A guide to help you manage Sponsored Brands campaigns using bulksheets.
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands
    - Campaign management
keywords:
    - get started
    - bulksheets  
---

# Get started with bulksheets 2.0 for Sponsored Brands

>[WARNING] Be sure you’ve reviewed [part one of this guide](bulksheets/2-0/get-started-with-bulksheets-part1), which walks through Step 1 in the “Bulk operations” process shown below. You should already have your downloaded template ready before proceeding with step 2, editing your file. 

This article will walk through the steps of getting started with bulksheets for Sponsored Brands. You’ll find guidance and tips for entering campaign data into your spreadsheet so you can upload the sheet to create campaigns using bulksheets. 

![Bulk operations main screen](/_images/bulksheets/2-0-images/all-get-started-main-bulk-ops-pg.png "Bulk operations main screen")

### Prepare to edit your bulksheets file

Before you start entering data into your file, here are some guidelines and tips to help you understand the functionality of the sheet. 

**Allowable values for the first three columns**

Many of the bulksheets values will vary depending on your specific situation. For example, “Budget” amount and “Keyword text” values might vary greatly depending on the individual user or campaign. But some columns have strict limitations on what you can enter. In particular, for the first three foundational columns for Sponsored Brands, here are the allowed values:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Brands	|Campaign	|Create	|
|	|Keyword	|Update	|
|	|Negative Keyword	|Archive	|
|	|Product targeting	|*Submit*+	|
|	|Negative product targeting	|	|
|	|*Draft campaign*+	|	|
|	|*Draft keyword*+	|	|
|	|*Draft negative keyword*+	|	|
|	|*Draft product targeting*+	|	|
|	|*Draft negative product targeting*+	|	|

+The **Submit** operation is for the “Draft campaign” entity only. <br>**Draft entities** will not be covered in this guide. 

**ADDITIONAL TIPS:**

* You will always need a new row for each entity, even if you are creating multiples of the same entity. For instance, to create 10 different keywords, you would need 10 different keyword rows under the “Entity” column. 
* Be sure to always include the “Product” (or campaign type) in Column A for every row that contains an entity you want to create. 
* Every **entity** (in Column B) will be in its own separate row. Then, you will define attributes for each entity in the other cells that span that specific row. 
* The “Operation” column must contain a value in any row where you want updates to occur. Otherwise, the row will be ignored. If you’re creating a new campaign, then you would type **Create** in each row of the “Operation” column for each entity in that campaign. 

**EXAMPLE 1**: To create a Sponsored Brands campaign with five keywords, you would set your first three columns as:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Brands	|Campaign	|Create	|
|Sponsored Brands	|Keyword	|Create	|
|Sponsored Brands	|Keyword	|Create	|
|Sponsored Brands	|Keyword	|Create	|
|Sponsored Brands	|Keyword	|Create	|
|Sponsored Brands	|Keyword	|Create	|

**EXAMPLE 2**: To update three Sponsored Brands keywords, set the first three columns as:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Brands	|Keyword	|Update	|
|Sponsored Brands	|Keyword	|Update	|
|Sponsored Brands	|Keyword	|Update	|

**EXAMPLE 3**: To archive three Sponsored Brands keywords, set the first three columns as:

|[grid orange]Col A	|Col B	|Col C	|
|---	|---	|---	|
|**Product**	|**Entity**	|**Operation**	|
|Sponsored Brands	|Keyword	|Archive	|
|Sponsored Brands	|Keyword	|Archive	|
|Sponsored Brands	|Keyword	|Archive	|

The next article walks through the complete [Sponsored Brands campaign creation process](bulksheets/2-0/create-sb-campaign), including guidelines on Sponsored Brands entities and how they factor into the steps to create a bulksheets file.

