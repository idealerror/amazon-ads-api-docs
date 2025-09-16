---
title: Getting started with bulksheets for Sponsored Products across retailers (Retail Ad Service) 
description: A guide to help you manage Sponsored Products across retailers (Retail Ad Service).
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - get started
    - bulksheets  
    - Retail Ad Service
---

# Bulksheets guide - Sponsored Products across retailers  

* * *

## How to manage Sponsored Products campaigns across retailers in bulksheets

This section describes the process of creating and updating sponsored products campaigns across retailers using bulksheets. 

If you want to learn more about bulksheets in general, review the [bulksheets overview article](bulksheets/2-0/overview-about-bulksheets).
 
Bulksheets is a spreadsheet-based tool that allows sponsored ads advertisers to create, update, and optimize multiple campaigns in batches, reducing time and manual effort. For example, you can update the names of campaigns or ad groups, or change bids in large batches via spreadsheet, instead of doing this one-by-one. 

This guide will provide detailed instructions for how to proceed. But at a very high level, using bulksheets involves three main steps: 1. Creating a spreadsheet file to download | 2. Editing the spreadsheet | 3. Uploading the file to make the changes

Steps 1 and 3 are completed from the [bulk operations homepage](bulksheet/HomePage) while Step 2 is done offline via spreadsheet: 
![Bulk operations main screen ras-1](/_images/bulksheets/2-0-images/bulk-operations-ras-1.png "Bulk operations ras-1 screen") You can edit the downloaded file offline, and come back when you’re ready to upload. 
* * *

### Access the bulk operations homepage

| [IMAGE | ]
| --- | --- |
| <p>The first step is to visit the bulk operations home page from campaign manager.</p><p>From the left side navigation, hover on the “Sponsored ads” tab and click **Bulk operations** → </p><p>This will open the bulk operations homepage where you can download a file.</p>  | ![Bulk operations main screen ras-2](/_images/bulksheets/2-0-images/bulk-operations-ras-2.png "bulk-operations-ras-2.png") |

### Create & download a custom spreadsheet

| [IMAGE | ]
| --- | --- |
| <p>In “Step 1. Create & download a custom spreadsheet,” select a date range for performance metrics (clicks, impressions, ROAS, etc.) → </p><p>Next, select the data you want to include in the download. At a minimum, you must include **Sponsored Products data**—this will populate an RAS tab in your spreadsheet, which will include the Sponsored Products across retailers campaigns. You can also include **Sponsored Products search term data** to get insights on the words that shoppers enter when they search on Amazon. Learn more about [bulksheets search term reports](bulksheets/2-0/bulksheets-search-term-report).</p><p>You can also include **Campaign items with zero impressions** to see campaigns that are generating no valuable traffic—or to see campaigns you recently created that might not have gotten impressions yet. **Placement data** gives insights on where your ads were shown.</p><p>When you’re ready, click **Create spreadsheet for download →** </p> | ![Bulk operations main screen ras-3](/_images/bulksheets/2-0-images/bulk-operations-ras-3.png "bulk-operations-ras-3") |

Scroll down to **Bulk spreadsheet history** to download the file. Note that it may take several minutes for the “Download” link to appear, and you may need to refresh the page:

![Bulk operations main screen ras-4](/_images/bulksheets/2-0-images/bulk-operations-ras-4.png "bulk-operations-ras-4")


### What to expect when you open the file

Your spreadsheet will have multiple tabs, but should include tabs labeled **Portfolios**, **Sponsored Products Campaigns**, **SP Search Term Report**, and **RAS Campaigns** if you selected the options shown in the checklist above. 

Open the **RAS Campaigns** sheet. If you already have active campaigns in your account, you’ll see details about each campaign entity (e.g. Campaign, Ad group, Product ad, etc.) in each row of the spreadsheet. Otherwise, the sheet might contain no data initially. 

**A few tips**:

* Apply filters to all columns to make the sheet easier to review and navigate
* Notice that Column A, the “Product” is always **RAS** when managing Sponsored Products across retailers campaigns
* For Column B, RAS “Entity” options include **Campaign**, **Ad group**, **Product ad**, and **Target**
* The “Operation” field (Col C) is always blank when you download a file. You’ll enter values into this field whenever you want to make changes in that row to either **Create**, **Update**, or **Archive**. This is an important field in bulksheets, so review this [getting started guide to learn more about the “Operation” field](bulksheets/2-0/get-started-with-bulksheets-part1#product-entity-and-operation-fields).
* Notice that most IDs are populated for their relevant fields—e.g., the “Target” entities contain Target IDs. For Target and Product ad IDs, leave these blank when you create the campaign initially. When you create the campaign, the IDs will be generated and you can see them when you download the campaign later.
* For Campaign ID and Ad group IDs ONLY, you will add temporary IDs when you create them initially, and unique numeric IDs will be generated when you create the campaign.  More details on this are in the sections below.

### Deep-dive into the bulksheets file 

The table below shows all column headers, *transposed* for easier reading in this format, along with guidelines on what to enter and other notes/tips. The values are not case sensitive. 

|Column header	|Guidelines on what to enter & other tips	|
|---	|---	|
|Product	|Required in every row. Enter RAS 	|
|Entity	|Required in every row. This defines the entity you want to manage in each row. Options for RAS include Campaign, Ad group, Product ad, and Target. 	|
|Operation	|Required in every row to define the action you want to take in that row. Options are Create, Update, or Archive. In a downloaded bulksheet, you can leave this field blank for any row where you do not want to make any changes, and our system will ignore that row. 	|
|Campaign ID	|Required in every row, to associate the child entities with the parent-level campaign, and the IDs must match exactly. When creating new campaigns, you must enter a temporary text-based ID, and a unique numeric one will generate when you create the campaign. For new campaigns, we recommend using the same value that you assign for the Campaign name. 	|
|Ad Group ID	|Required in every row other than the Campaign row. This associates child entities with the parent-level Ad group. Similar to the Campaign ID above, you must enter a temporary text-based ID when creating new Ad groups, and a unique one will be generated upon creation. For new Ad groups, we recommend using the same value that you assign for the Ad group name. 	|
|Target ID	|Leave blank when creating new Targets. To make updates later, you'll need the unique ID, which will already be populated in a downloaded custom spreadsheet. 	|
|Product Ad ID	|Similar to the above: Leave blank when creating new ads. You'll need the ID to make updates later, and you can find that ID in a downloaded file. 	|
|Portfolio ID	|Required in the Campaign row--but only if the campaign is in a portfolio. Otherwise, leave blank. 	|
|Retailer	|Leave blank	|
|Retailer ID	|Required in the Campaign and Ad group rows. 	|
|Retailer Offer ID	|This is sometimes referred to as SKU - if you do not know the SKU, you can download the product catalog in the campaign creation workflow ([https://advertising.amazon.com/cb/create/rmssp](https://advertising.amazon.com/cb/create/rmssp?retailerId=86254810291451&entityId=ENTITY1IQQ5C2UU54DV)) Note: You need an Amazon Ads Console account to access this link. If you do not have an account, you can set up a vendor account to access RAS.	|
|Name	|Required in the Campaign and Ad group rows. Enter a unique name. We recommend using this same name for the Campaign ID and Ad group ID when creating new campaigns. 	|
|State	|Required in all rows. Options are ENABLED or PAUSED. You'll typically enter ENABLED. 	|
|Start Date	|Required in the Campaign row. Format must be YYYYmmDD. For example, July 4, 2025 would be written as 20250704	|
|End Date	|Optional. Enter a value if you want to set a specific end date, and use the YYYYmmDD format	|
|Targeting Type	|Required in the "Campaign" entity row to designate it as a manual targeting or auto targeting campaign. Options are AUTO or MANUAL. If set to AUTO, the "Target type" can be either Keyword or Auto. If set to MANUAL, the "Target type" must be Keyword. 	|
|Target Type	|Required in the "Target" entity row if the campaign targeting type is AUTO. Options include AUTO or KEYWORD. Note that if set to AUTO, the "Negative" field must be FALSE because these targets cannot be negative.  	|
|Target Level	|Required in the "Target" entity row if the campaign targeting type s MANUAL. Enter AD GROUP	|
|Budget	|Required in the "Campaign" entity row. Enter a numeric value.	|
|Budget Type	|Required in the "Campaign" entity row. Enter DAILY 	|
|SKU	|Leave blank unless otherwise indicated. 	|
|Ad Group Default Bid	|Required in the "Ad group" entity row. Enter a numeric value.	|
|Negative	|Required for the "Target" entity row. Options are TRUE or FALSE. This is a binary to indicate if your target is negative, e.g. a negative keyword target. Negative targets do not need bids, so leave the "Bid" field blank if this is set to TRUE. Also note that if "Target type" is AUTO, this value must be FALSE because auto targets cannot be negative	|
|Bid	|Required in the "Target" entity row if your "Targeting type" is "Keyword" and "Negative" (Col W) is set to FALSE. Enter a numeric value. This sets your keyword-level bid for keyword targets. 	|
|Currency Code	|	|
|Keyword Text	|Required in the "Target" entity row if your "Targeting type" (Col Q) is Keyword. If you want to add more keyword targets, you'll need a new row for each "Target" entity.	|
|Keyword Match Type	|Required in the "Target" row if your "Target type" is "Keyword." Enter BROAD, PHRASE, or EXACT 	|
|Auto Match Type	|Required in the "Target" entity row if your "Target type" is set to "Auto." Options include Keywords Close Match, Keywords Loose Match, and Product Substitutes. 	|
|Bidding Strategy	|Required in the "Campaign" entity row. Enter MANUAL 	|
|Bidding Adjustment Placement	|Optional in the "Campaign" entity row. Enter PLACEMENT TOP	|
|Bidding Adjustment Percentage	|Required in the "Campaign" entity row if you added a Bidding adustment placement. Enter a numeric value. 	|

### Create new campaigns 

**A few tips:**

* To create a new campaign from scratch, you can add new rows into the downloaded file to create the new entities. In Excel, highlight the number of rows you want to add, click **Insert** from the top nav, and select “Insert sheet rows.” As long as the “Operation” field remains blank for the existing campaigns in the sheet, those will remain unchanged when you upload the sheet to create the new entities.  
* For manual-targeting campaigns, the “Target type” (Column Q) must be Keyword, and the “Negative” field must be set to FALSE because manual keyword targets must be positive.
* For auto-targeting campaigns, the “Target type” can be either Keyword or Auto. Keyword targets can be either positive or negative, but Auto target type must be positive.

#### Example 1. Auto-targeting campaign with Ad group, Product ad, and Keyword target

To start filling out the sheet, you’ll start with the first blank row and start working through each field for each entity row—you will need one row for each entity, as shown in Figure 1 above. 

Necessary fields for each entity include:

* Campaign row: Product, Entity, Operation, Campaign ID, Name, State, Start date, Targeting type, Budget, Budget type, bidding strategy
* Ad group row: Product, Entity, Operation, Campaign ID, Ad group ID, Retailer ID, Name, State, Ad group default bid
* Product ad row: Product, Entity, Operation, Campaign ID, Ad group ID, Retailer ID, Retailer Offer ID, State
* Target row: Product, Entity, Operation, Campaign ID, Ad group ID, State, Target type, Target level, Negative, Bid, Keyword text, Keyword match type

Putting it all together, the rows for creating this new campaign might resemble:

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|Currency Code	|Keyword Text	|Keyword Match Type	|Auto Match Type	|Bidding Strategy	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|Create	|Campaign name 1	|	|	|	|	|	|	|	|Campaign name 1	|Enabled	|20240930	|	|Auto	|	|	|20000	|Daily	|	|	|	|	|	|	|	|	|Manual	|
|RAS	|Ad Group	|Create	|Campaign name 1	|Ad group name 1	|	|	|	|	|24483676143932	|	|Ad group name 1	|Enabled	|	|	|	|	|	|	|	|	|1.5	|	|	|	|	|	|	|	|
|RAS	|Product Ad	|Create	|Campaign name 1	|Ad group name 1	|	|	|	|	|24483676143932	|7890	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Target	|Create	|Campaign name 1	|Ad group name 1	|	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.5	|	|dog collar	|phrase	|	|	|

If you download this same campaign later, it might resemble the following—note that the temporary Campaign and Ad group IDs have changed to system-generated ones, and the blank “Product ad ID” and “Target ID” fields are now populated:

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|Currency Code	|Keyword Text	|Keyword Match Type	|Auto Match Type	|Bidding Strategy	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|	|294852839	|	|	|	|	|	|	|	|Campaign name 1	|Enabled	|20240930	|	|Auto	|	|	|20000	|Daily	|	|	|	|	|	|	|	|	|Manual	|
|RAS	|Ad Group	|	|294852839	|443678957	|	|	|	|	|224958227503444	|	|Ad group name 1	|Enabled	|	|	|	|	|	|	|	|	|1.5	|	|	|	|	|	|	|	|
|RAS	|Product Ad	|	|294852839	|443678957	|	|3329739	|	|	|224958227503444	|7890	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Target	|	|294852839	|443678957	|587584038	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.5	|	|dog collar	|phrase	|	|	|

#### Example 2. Manual-targeting campaign with Ad group, Product ad, two Keyword targets, and one Negative keyword target

Necessary fields for each entity include:

* Campaign row: Product, Entity, Operation, Campaign ID, Name, State, Start date, Targeting type, Budget, Budget type, bidding strategy
* Ad group row: Product, Entity, Operation, Campaign ID, Ad group ID, Retailer ID, Name, State, Ad group default bid
* Product ad row: Product, Entity, Operation, Campaign ID, Ad group ID, Retailer ID, Retailer Offer ID, State
* Target rows: Product, Entity, Operation, Campaign ID, Ad group ID, State, Target type, Target level, Negative, Bid (for non-negative keywords only), Keyword text, Keyword match type

Putting it all together, the rows for creating this new campaign might resemble:

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|Currency Code	|Keyword Text	|Keyword Match Type	|Auto Match Type	|Bidding Strategy	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|Campaign name 2	|Enabled	|20241201	|	|Manual	|	|	|15000	|Daily	|	|	|	|	|	|	|	|	|Manual	|
|RAS	|Ad Group	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|224958227503444	|	|Ad group name 2	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Product Ad	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|224958227503444	|1029384	|	|Enabled	|	|	|	|	|	|	|	|	|1.75	|	|	|	|	|	|	|	|
|RAS	|Target	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|1.25	|	|suit pant	|Phrase	|	|	|
|RAS	|Target	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|0.75	|	|black tie	|Broad	|	|	|
|RAS	|Target	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|TRUE	|	|	|floral tie	|Exact	|	|	|

#### Example 3. Auto-targeting campaign with Ad group, Product ad, and three Auto targets

Necessary fields for each entity include:

* Campaign row: Product, Entity, Operation, Campaign ID, Name, State, Start date, Targeting type, Budget, Budget type, bidding strategy
* Ad group row: Product, Entity, Operation, Campaign ID, Ad group ID, Retailer ID, Name, State, Ad group default bid
* Product ad row: Product, Entity, Operation, Campaign ID, Ad group ID, Retailer ID, Retailer Offer ID, State
* Target rows: Product, Entity, Operation, Campaign ID, Ad group ID, State, Target type, Target level, Negative, Auto match type (because the “Target type” is AUTO). Also note that the “Negative” value must be FALSE because Auto targets cannot be negative. 

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|Currency Code	|Keyword Text	|Keyword Match Type	|Auto Match Type	|Bidding Strategy	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|Campaign name 2	|Enabled	|20241201	|	|Auto	|	|	|15000	|Daily	|	|	|	|	|	|	|	|	|Manual	|
|RAS	|Ad Group	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|224958227503444	|	|Ad group name 2	|Enabled	|	|	|	|	|	|	|	|	|1.75	|	|	|	|	|	|	|	|
|RAS	|Product Ad	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|224958227503444	|1029384	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Target	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|	|Enabled	|	|	|	|Auto	|Ad Group	|	|	|	|	|FALSE	|	|	|	|	|Keywords Close Match	|	|
|RAS	|Target	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|	|Enabled	|	|	|	|Auto	|Ad Group	|	|	|	|	|FALSE	|	|	|	|	|Keywords Loose Match	|	|
|RAS	|Target	|Create	|Campaign name 2	|Ad group name 2	|	|	|	|	|	|	|	|Enabled	|	|	|	|Auto	|Ad Group	|	|	|	|	|FALSE	|	|	|	|	|Product Substitutes	|	|

### Update existing campaigns

After you create campaigns—regardless of whether it’s in the advertising console or bulksheets—you can always download the campaigns into bulksheets and make updates directly from the file. The process for updating is typically simple and requires far fewer inputs compared with the “Create” operation. Assuming you have downloaded a custom spreadsheet as described above, you only need to make *two manual inputs*: 1. Enter UPDATE in the “Operation” field, and 2. Enter the value for what you want to change in the relevant spreadsheet field. All other required fields (e.g. IDs) should already be populated in the downloaded file. 

The examples below include additional rows beyond the ones being updated because this is typically how you will interact with the downloaded sheet to make updates. You can leave the other data in the sheet, and our system will ignore any rows where the “Operation” field is left blank. This lets you make quick updates without needing to delete other data from the sheet. 

 [Learn more about updating campaigns in bulksheets](bulksheets/2-0/campaign-update-overview)

Some common “Update” actions are shown below. All examples assume you have downloaded a custom spreadsheet that contains your current campaign data, including the entity IDs. 

>[NOTE] In all of the examples below, the values shown in **bold** represent values where you’ll need to manually input or change a value. The non-bold values should already be present in your downloaded file. 

#### Example 1a. Update campaign name

Required fields: Product, Entity, **Operation**, Campaign ID, **Campaign name**, State

In the “Campaign” entity row, be sure all of the required inputs are filled in. Below is what the downloaded sheet might look like after you add the necessary values:. 

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|Update	|500371812775953	|	|	|	|	|	|	|	|New campaign name	|Enabled	|
|RAS	|Ad Group	|	|500371812775953	|410868284805583	|	|	|	|	|224958227503444	|	|Ad group name 1	|Enabled	|
|RAS	|Ad Group	|	|500371812775953	|376584553712841	|	|	|	|	|224958227503444	|	|Ad group name 2	|Enabled	|
|RAS	|Product Ad	|	|500371812775953	|376584553712841	|	|308096492277259	|	|	|224958227503444	|508320885088718	|	|Enabled	|

Note that the campaign row is the only one that needs updates. You can simply leave the other fields unchanged, and the system will ignore those rows since the “Operation” field has been left blank. This represents how advertisers typically interact with bulksheets to make quick/simple updates. 

#### Example 1b. Update campaign name and ad group name

You can make as many updates in a single file as necessary. Let’s say you want to update the ad group name along with the campaign name. Your sheet would resemble this: 


|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|Update	|500371812775953	|	|	|	|	|	|	|	|New campaign name	|Enabled	|
|RAS	|Ad Group	|Update	|500371812775953	|410868284805583	|	|	|	|	|224958227503444	|	|New ad group name	|Enabled	|
|RAS	|Ad Group	|	|500371812775953	|376584553712841	|	|	|	|	|224958227503444	|	|Ad group name 2	|Enabled	|
|RAS	|Product Ad	|	|500371812775953	|376584553712841	|	|308096492277259	|	|	|224958227503444	|508320885088718	|	|Enabled	|

#### Example 2. Update keyword bid

Required fields: Product, Entity, **Operation**, Campaign ID, Ad group ID, Target ID, State, **Bid**

In the relevant “Target” row, manually enter UPDATE in the “Operation” field, and the value you want to change the bid to. Your sheet might resemble the following: 

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|	|500371812775953	|	|	|	|	|	|	|	|Campaign name 1	|Enabled	|20241202	|	|AUTO	|	|	|1000	|Daily	|	|	|	|	|
|RAS	|Ad Group	|	|500371812775953	|410868284805583	|	|	|	|	|224958227503444	|	|Ad group name 1	|Enabled	|	|	|	|	|	|	|	|	|1.5	|	|	|
|RAS	|Product Ad	|	|500371812775953	|376584553712841	|	|308096492277259	|	|	|224958227503444	|508320885088718	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Target	|Update	|298342158186968	|466965360570401	|301890742579947	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.5	|
|RAS	|Target	|	|298342158186968	|466965360570401	|337887013349256	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.09	|

#### Example 3. Update campaign budget

Required field: Product, Entity, **Operation**, Campaign ID, State, **Budget** amount

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|Update	|500371812775953	|	|	|	|	|	|	|	|Campaign name 1	|Enabled	|20241202	|	|AUTO	|	|	|2500	|Daily	|	|	|	|	|
|RAS	|Ad Group	|	|500371812775953	|410868284805583	|	|	|	|	|224958227503444	|	|Ad group name 1	|Enabled	|	|	|	|	|	|	|	|	|1.5	|	|	|
|RAS	|Product Ad	|	|500371812775953	|376584553712841	|	|308096492277259	|	|	|224958227503444	|508320885088718	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Target	|	|298342158186968	|466965360570401	|301890742579947	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.5	|
|RAS	|Target	|	|298342158186968	|466965360570401	|337887013349256	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.09	|

### Pause campaigns and other entities 

Pausing a campaign or other entity requires two inputs, assuming the downloaded file already contains IDs. 

To pause, you’ll set the “Operation” to **update**, and change the “State” to **paused**. All other fields can be left as-is:

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|**Update**	|500371812775953	|	|	|	|	|	|	|	|Campaign name 1	|Paused	|20241202	|	|AUTO	|	|	|1000	|Daily	|	|	|	|	|
|RAS	|Ad Group	|	|500371812775953	|410868284805583	|	|	|	|	|224958227503444	|	|Ad group name 1	|Enabled	|	|	|	|	|	|	|	|	|1.5	|	|	|
|RAS	|Product Ad	|	|500371812775953	|376584553712841	|	|308096492277259	|	|	|224958227503444	|508320885088718	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Target	|	|298342158186968	|466965360570401	|301890742579947	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.5	|

To make the campaign active again later, perform these same steps, but set the “State” to **enabled**. 

### Archive campaigns and other entities

>[TIP] Archiving is an action that cannot be undone. If you want to enable the campaign again in the future, you should pause it instead (see the next section for instructions)

Archiving a campaign or other entity requires just one input, assuming the downloaded file already contains IDs and other values. 

To archive, you’ll set the “Operation” field to **Archive**. As long as the entity ID is in the sheet, this is all you need to do—you can even leave the “State” populated because it will be overridden when you upload the file:

|Product	|Entity	|Operation	|Campaign ID	|Ad Group ID	|Target ID	|Product Ad ID	|Portfolio ID	|Retailer	|Retailer ID	|Retailer Offer ID	|Name	|State	|Start Date	|End Date	|Targeting Type	|Target Type	|Target Level	|Budget	|Budget Type	|SKU	|Ad Group Default Bid	|Negative	|Bid	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|RAS	|Campaign	|**Archive**	|500371812775953	|	|	|	|	|	|	|	|Campaign name 1	|Enabled	|20241202	|	|AUTO	|	|	|1000	|Daily	|	|	|	|	|
|RAS	|Ad Group	|	|500371812775953	|410868284805583	|	|	|	|	|224958227503444	|	|Ad group name 1	|Enabled	|	|	|	|	|	|	|	|	|1.5	|	|	|
|RAS	|Product Ad	|	|500371812775953	|376584553712841	|	|308096492277259	|	|	|224958227503444	|508320885088718	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|
|RAS	|Target	|	|298342158186968	|466965360570401	|301890742579947	|	|	|	|	|	|	|Enabled	|	|	|	|Keyword	|Ad Group	|	|	|	|	|FALSE	|3.5	|


