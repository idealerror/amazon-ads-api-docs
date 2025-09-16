---
title: How to create Sponsored Display campaigns with bulksheets
description: A guide explaining how to create new Sponsored Display campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Display
    - Campaign management
keywords:
    - bulksheets  
    - create campaigns
---

# How to create Sponsored Display campaigns in bulksheets

[← Get started with bulksheets for Sponsored Display](bulksheets/2-0/get-started-with-sd-bulksheets)

When you create a Sponsored Display (SD) campaign, you will define a targeting tactic: either audience targeting or contextual targeting. Similar to the advertising console UI, you’ll also include your products that you want to advertise using SKU or ASIN, daily budget, and other details. 

### Sponsored Display campaigns

| [IMAGE] |   |
| ---- | ---- |
|Allowable **entities** (Column B of your bulksheets file) are shown in the hierarchical diagram on the right—the green entities are required fields that you must define for a SD campaign. Note that **ad group** is a required child of the parent-level campaign. <br><br>This means you will need to create an **ad group** entity anytime you create a new campaign. Also note that you must define the targeting tactic by selecting either audience or contextual targeting. Negative product targeting is optional.  <br><br>Learn more about [entity and targeting types, and their relationships to Sponsored Display campaigns](bulksheets/2-0/sd-entities).|![Sponsored Display entity diagram](/_images/bulksheets/2-0-images/sd-entities.png "Sponsored Display entity diagram")| 

The steps below show which fields you should complete on a row-by-row basis, starting with the top-level **campaign** entity. 
 

Each “step” section below represents one row in your bulksheets file. The guidelines describe what you should enter into each field. 
**NOTE:** Read-only, informational-only, and metrics columns (impressions, clicks, conversions) are not included in the guide below. 

**TIPS:** 

* Headers marked with an asterisk * are fields that cannot be updated after you create the campaign.  
* Note that "cost type" and "bid optimization" fields are related. In the campaign entity row, you will define the cost type (either CPC or vCPM) depending on the bid optimization strategy that you define in the ad group entity row.<br>
* **IMPORTANT:** Do not edit or alter the bulksheets column headers. This will cause the upload to fail. 

#### Step 1. Define the campaign entity 

Use the guidelines below to define the top-level (parent) campaign entity in the first row of your bulksheets file:

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Display	|
|Entity	|Campaign	|
|Operation	|Create	|
|Campaign Id*	|For new campaigns, you should type a text-based name to identify your campaign. You will enter this exact Campaign ID for each entity you create underneath this parent campaign. <br>**NOTE:** When you upload your bulksheets file, the Campaign ID will become the actual unique identifier associated with the campaign and all of its "child" entities. So if you want to update this campaign later, you would enter the actual Campaign ID into this field.	|
|Ad Group Id*	|[Leave blank for campaign]	|
|Campaign Name	|Enter the same campaign name that you entered in the "Campaign ID" column.	|
|Ad Group Name	|[Leave blank for campaign]	|
|Start Date	|Format must be yyyyMMdd. For example, the date April 1, 2022 would be  20220401.<br>**NOTE:** You can start on today's date or a future date, but the date cannot  be in the past. So if you set the date for today, be sure you create and  upload the file on today's date. If you leave this field blank, the start date will be set for today's date.	|
|State	|Options are **enabled** or **paused**. Enter **enabled** to create a new campaign.	|
|Tactic*	|This is the targeting tactic you want for the campaign: either audience or contextual targeting. For bulksheets, you must enter the tactic ID in this field. <br>For audience targeting, enter **T00030**. <br> For contextual targeting, enter **T00020**.	|
|Budget Type	|Enter **daily**.	|
|Budget	|Enter the amount you're willing to spend on the campaign each day. Do not include symbols such as dollar signs. For example, a daily budget of $100 would be written  as **100**. <br>Be sure your budget is within [the allowed limits for your region](concepts/limits#budget-constraints-by-marketplace).	|
|Cost Type	|Enter either **CPC** or **vCPM**, depending on how you define your bidding strategy in the "Bid Optimization" field when you create the ad group. <br>If you want a bidding strategy of "Optimize for page visits" or "Optimize for conversions," enter **CPC**. <br>If you want a bidding strategy of "Optimize for viewable impressions," enter **vCPM**. In the UI, these settings are in the "Bidding" section. 
Bid Optimization	|Leave this field blank because it is applied at the ad group level, but **see the notes in the "Cost Type" row above** to be sure the cost type and bid optimization fields are aligned.	|
|SKU	|[Leave blank for campaign]	|
|ASIN	|[Leave blank for campaign]	|
|Ad Group Default Bid	|[Leave blank for campaign]	|
|Bid	|[Leave blank for campaign]	|
|Targeting Expression	|[Leave blank for campaign]	|
|Ad Id*	|[Leave blank for campaign]	|
|Targeting Id*	|[Leave blank for campaign]	|

After this step, your bulksheets file might look like this for a campaign with audience targeting:

|Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|State	|Tactic	|Budget Type	|Budget	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Display	|Campaign	|Create	|SP New Campaign	|	|SP New Campaign	|	|20220401	|	|enabled	|T00030	|daily	|100	|

#### Step 2. Define the ad group entity 

**Note for vendors:** In the advertising console, ad groups aren’t available, so some of the settings might not be as familiar. 

Use the guidelines below to define the ad group entity: 

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Display	|
|Entity	|Ad group	|
|Operation	|Create	|
|Campaign Id*	|Enter the exact same campaign ID that you entered in the "Campaign" entity row.	|
|Ad Group Id*	|When creating a new ad group, enter the Ad Group Name you defined above. When updating an existing ad group, enter the actual ID found in campaign manager.	|
|Campaign Name	|[Leave blank for ad group. Only used for the campaign entity]	|
|Ad Group Name	|For new ad groups, you should type a text-based name to identify the ad group. <br>**NOTE:** When you upload your bulksheets file, the Ad Group ID will become the actual unique identifier associated with this ad group.	|
|Start Date	|[Leave blank for ad group. Only used for the campaign entity]	|
|End Date	|[Leave blank when creating ad group. Only used when creating the campaign entity]	|
|State	|Options are **enabled** or  **paused**. Enter **enabled** to create a new ad group.|
|Tactic*	|[Leave blank for ad group]	|
|Budget Type	|[Leave blank for ad group]	|
|Budget	|[Leave blank for ad group]	|
|Cost Type	|[Leave blank for ad group]	|
|Bid Optimization	|See the notes about "Cost Type" in the campaign entity example above. In the "Bid Optimization" field, enter the bidding strategy you want to apply to this SD campaign. <br><br>Options are **Optimize for page visits**, **Optimize for conversions**, or **Optimize for viewable impressions**. <br><br>Be sure to associate the appropriate cost type depending on which strategy you select.	|
|SKU	|[Leave blank for ad group]	|
|ASIN	|[Leave blank for ad group]	|
|Ad Group Default Bid	|If you want to assign a default CPC bid at the ad group level, enter an amount using numbers and periods only. For example, 20 cents would be entered as **.20** <br> <br>Leave this field blank if you want the campaign default bid to apply to the ad group and its child entities.	|
|Bid	|[Leave blank for ad group]	|
|Targeting Expression	|[Leave blank for ad group]	|
|Ad Id*	|[Leave blank for ad group]	|
|Targeting Id*	|[Leave blank for ad group]	|

After this step, your bulksheets file might look like this:

|Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|State	|Tactic	|Budget Type	|Budget	|Cost Type	|Bid Optimization	|SKU	|ASIN	|Ad Group Default Bid	|Bid	|Targeting Expression	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Display	|Campaign	|Create	|SD New Campaign	|	|SD New Campaign	|	|20220401	|	|enabled	|T00030	|daily	|100	|	|	|	|	|	|	|	|
|Sponsored Display	|Ad group	|Create	|SD New Campaign	|SD New Ad Group	|	|SD New Ad Group	|	|	|enabled	|	|	|	|	|	|	|	|1	|	|	|

#### Step 3. Define the product ad entity

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Display	|
|Entity	|Product ad	|
|Operation	|Create	|
|Campaign Id*	|Enter the exact same campaign ID that you entered in the "Campaign" entity row.	|
|Ad Group Id*	|Enter the exact same name that you entered in the "Ad Group ID" field in the Ad Group entity row.	|
|Campaign Name	|[Leave blank for product ad]	|
|Ad Group Name	|[Leave blank for product ad]	|
|Start Date	|[Leave blank for product ad]	|
|End Date	|[Leave blank for product ad]	|
|State	|Options are **enabled** or **paused**. Enter **enabled** to create a new product ad.|
|Tactic*	|[Leave blank for product ad]	|
|Budget Type	|[Leave blank for product ad]	|
|Budget	|[Leave blank for product ad]	|
|Cost Type	|[Leave blank for product ad]	|
|Bid Optimization	|[Leave blank for product ad]	|
|SKU	|If you are a seller, enter the SKU for the product you're advertising. If you are a vendor, leave this blank.	|
|ASIN	|If you are a vendor, enter the ASIN for the product you're advertising. If you are a seller, leave this blank.	|
|Ad Group Default Bid	|[Leave blank for product ad]	|
|Bid	|[Leave blank for product ad]	|
|Targeting Expression	|[Leave blank for product ad]	|
|Ad Id*	|[Leave blank for product ad]	|
|Targeting Id*	|[Leave blank for product ad]	|

At this point, your file would look like this:

|Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|State	|Tactic	|Budget Type	|Budget	|Cost Type	|Bid Optimization	|SKU	|ASIN	|Ad Group Default Bid	|Bid	|Targeting Expression	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Display	|Campaign	|Create	|SD New Campaign	|	|SD New Campaign	|	|20220401	|	|enabled	|T00030	|daily	|100	|	|	|	|	|	|	|	|
|Sponsored Display	|Ad group	|Create	|SD New Campaign	|SD New Ad Group	|	|SD New Ad Group	|	|	|enabled	|	|	|	|	|	|	|	|1	|	|	|
|Sponsored Display	|Product ad	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|BR549	|	|	|	|

#### Step 4a. Create the audience targeting entity

Create this entity *only* if you want audience targeting.  For contextual targeting, [jump to Step 4b](#Step-4b-Create-the-contextual-targeting-entity).

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Display	|
|Entity	|Audience targeting	|
|Operation	|Create	|
|Campaign Id*	|Enter the exact same campaign ID that you entered in the "Campaign" entity row.	|
|Ad Group Id*	|Enter the exact same name that you entered in the "Ad Group ID" field in the Ad Group entity row.	|
|Campaign Name	|[Leave blank for audience targeting. Only used for the campaign entity]	|
|Ad Group Name	|[Leave blank for audience targeting. Only used for the ad group entity]	|
|Start Date	|[Leave blank for audience targeting]	|
|End Date	|[Leave blank for audience targeting]	|
|State	|Options are **enabled** or **paused**. Enter **enabled** to create a new targeting entity.	|
|Tactic*	|[Leave blank for audience targeting]	|
|Budget Type	|[Leave blank for audience targeting]	|
|Budget	|[Leave blank for audience targeting]	|
|Cost Type	|[Leave blank for audience targeting]	|
|Bid Optimization	|[Leave blank for audience targeting]	|
|SKU	|[Leave blank for audience targeting]	|
|ASIN	|[Leave blank for audience targeting]	|
|Ad Group Default Bid	|[Leave blank for audience targeting]	|
|Bid	|Enter the CPC amount you are willing to pay when a shopper clicks your ad. Do not use money symbols or commas. For example, to bid 75 cents, enter **0.75**. To bid one dollar, enter **1**. <br> **NOTE**: There is a 2-place decimal limit. If you enter a number with more than 2 decimal places, we will automatically round to a 2-place decimal figure. For example, if you enter 0.756, we will round to 0.76. If you enter 0.754, we will round to 0.75 <br><br>Be sure your bid is [within the limits for your region](concepts/limits#bid-constraints-by-marketplace). |
| Targeting Expression	|In the console UI, you define targeting expression in the “Products to target” or the “Audiences” section, depending on which targeting strategy you selected. In this bulksheets field, you’ll enter values such as **category=“12345”** to target based on a product category, or **purchases=(related-product lookback=30)** to target based on purchases remarketing. To do this, you'll need the IDs that represent the expression you want. You can get these values by downloading a bulksheet with some past Sponsored Display campaign data. Look in the **Resolved targeting expression (read only)** column for values such as category, for example: category="Kids' Electronics." Then, look in the **Targeting expression** column in that same row for the corresponding ID, for example: category="166164011" Below are some examples of what you might enter in this field, depending on what you want to target: <ul><li>views=(category="2748212011" price=10-50 lookback=30)</li> <li>views=(similar-product lookback=30)</li> <li>purchases=(related-product lookback=30)</li>  <li>purchases=(exact-product lookback=30)</li> <li>audience="360733347960105375"</li></ul>	|
|Ad Id*	|[Leave blank for audience targeting]	|
|Targeting Id*	|Leave blank when creating a new targeting entity. When updating, enter the actual targeting ID.	|

Your bulksheets file might now look like this:

|Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|State	|Tactic	|Budget Type	|Budget	|Cost Type	|Bid Optimization	|SKU	|ASIN	|Ad Group Default Bid	|Bid	|Targeting Expression	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Display	|Campaign	|Create	|SD New Campaign	|	|SD New Campaign	|	|20220401	|	|enabled	|T00030	|daily	|100	|	|	|	|	|	|	|	|
|Sponsored Display	|Ad group	|Create	|SD New Campaign	|SD New Ad Group	|	|SD New Ad Group	|	|	|enabled	|	|	|	|	|	|	|	|1	|	|	|
|Sponsored Display	|Product ad	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|BR549	|	|	|	|
|Sponsored Display	|Audience targeting	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|purchases=(exact-product lookback=30)	|

#### Step 4b. Create the contextual targeting entity

Create this entity *only* if you want a contextual targeting tactic.

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Display	|
|Entity	|Contextual targeting	|
|Operation	|Create	|
|Campaign Id*	|Enter the exact same campaign ID that you entered in the "Campaign" entity row.	|
|Ad Group Id*	|Enter the exact same name that you entered in the "Ad Group ID" field in the Ad Group entity row.	|
|Campaign Name	|[Leave blank for contextual targeting. Only used for the campaign entity]	|
|Ad Group Name	|[Leave blank for contextual targeting. Only used for the ad group entity]	|
|Start Date	|[Leave blank for contextual targeting]	|
|End Date	|[Leave blank for contextual targeting]	|
|State	|Options are **enabled** or  **paused**. Enter **enabled** to create a new targeting entity.	|
|Tactic*	|[Leave blank for contextual targeting]	|
|Budget Type	|[Leave blank for contextual targeting]	|
|Budget	|[Leave blank for contextual targeting]	|
|Cost Type	|	Leave blank for contextual targeting]|
|Bid Optimization	|	Leave blank for contextual targeting]|
|SKU	|[Leave blank for contextual targeting]	|
|ASIN	|[Leave blank for contextual targeting]	|
|Ad Group Default Bid	|[Leave blank for contextual targeting]	|
|Bid	|Enter the CPC amount you are willing to pay when a shopper clicks your ad. Do not use money symbols or commas. For example, to bid 75 cents, enter **0.75**. To bid one dollar, enter **1**. <br>  Be sure your bid is [within the limits for your region](concepts/limits#bid-constraints-by-marketplace). |
|Targeting Expression	|In the advertising console UI, this is where you select either "individual products" or "categories" to target and have the option to target similar products. For "individual products," use the ASIN and enter **asin="A000000000000"** (using the actual ASIN ID). For "categories," use the category ID along with other qualifiers such as price limit and star rating. To target shoppers who viewed the detail pages of products that are similar to your advertised products, enter **similar-product**. For this field, enter all qualifiers into the same cell of your bulksheets file because they work as a combined unit to define the targeting. <br><br>Example 1. To target by product category, price, and star rating: **category="5524098011" price<49.99 rating>3** <br><br>Example 2. To target by product category and Prime eligibility: **category="5524098011" prime-shipping-eligible="true"**<br><br>Example 3. To target shoppers who viewed the detail pages of similar products (this cannot be combined with other qualifiers for contextual targeting): **similar-product**<br><br> **TIP:** To get the numeric values for attributes such as category, you can use past campaign data where you assigned those targeting expressions by exporting the campaign into a bulksheet. The **Resolved Product Targeting Expression** column will show the values as they appear in the UI. You can then map this value to the corresponding ID in the **Product Targeting Expression** column. |
|Ad Id*	|[Leave blank for contextual targeting]	|
|Targeting Id*	|Leave blank when creating a new targeting entity. When updating, enter the actual targeting ID.	|

Now, your bulksheets file might look like this if you add the entity details shown above—note that *instead of adding an additional entity row*, the “contextual targeting” entity has replaced “audience targeting.” You cannot create a campaign using both targeting tactics:

|Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|State	|Tactic	|Budget Type	|Budget	|Cost Type	|Bid Optimization	|SKU	|ASIN	|Ad Group Default Bid	|Bid	|Targeting Expression	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Display	|Campaign	|Create	|SD New Campaign	|	|SD New Campaign	|	|20220401	|	|enabled	|T00030	|daily	|100	|	|	|	|	|	|	|	|
|Sponsored Display	|Ad group	|Create	|SD New Campaign	|SD New Ad Group	|	|SD New Ad Group	|	|	|enabled	|	|	|	|	|	|	|	|1	|	|	|
|Sponsored Display	|Product ad	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|BR549	|	|	|	|
Sponsored Display	|Contextual targeting	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|1	|category="5524098011" price<49.99 rating>3	|

### Upload your bulksheets file to create the new campaign

At this point, all mandatory entities are included in this Sponsored Display campaign. You can still add more entities if you want to assign negative product targeting.

If you’re ready to create the campaign, you should save the file and then navigate back to the “Bulk operations” page. 

| [IMAGE] |   |
| ---- | ---- |
| Under “3. Upload your file to update your campaigns,” click **Choose file** to upload the sheet you just created.  | ![Upload your file](/_images/bulksheets/2-0-images/sp-3-upload-your-file.png "Upload your file") |

### Optional: Add negative targeting entity to the campaign

If you want to add negative targeting, follow the guidelines below:

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Display	|
|Entity	|Negative product targeting	|
|Operation	|Create	|
|Campaign Id*	|Enter the exact same campaign ID that you entered in the "Campaign" entity row.	|
|Ad Group Id*	|Enter the exact same name that you entered in the "Ad Group ID" field in the Ad Group entity row.	|
|Campaign Name	|[Leave blank for negative product targeting]	|
|Ad Group Name	|[Leave blank for negative product targeting]	|
|Start Date	|[Leave blank for negative product targeting]	|
|End Date	|[Leave blank for negative product targeting]	|
|State	|Options are **enabled** or  **paused**. Enter **enabled** to create a new targeting entity.|
|Tactic*	|[Leave blank for negative product targeting]	|
|Budget Type	|[Leave blank for negative product targeting]	|
|Budget	|[Leave blank for negative product targeting]	|
|Cost Type	|[Leave blank for negative product targeting]	|
|Bid Optimization	|[Leave blank for negative product targeting]	|
|SKU	|If you are a seller, enter the SKU for the product you're advertising. If you are a vendor, leave this blank.	|
|ASIN	|If you are a vendor, enter the ASIN for the product you're advertising. If you are a seller, leave this blank.	|
|Ad Group Default Bid	|[Leave blank for negative product targeting]	|
|Bid	|[Leave blank for negative product targeting]	|
Targeting Expression	|Specify either a brand or a product to exclude from targeting, such as **asin="29485928"** or **brand="ABCDE"**.	|
|Ad Id*	|[Leave blank for negative product targeting]	|
|Targeting Id*	|[Leave blank for negative product targeting]	|

After adding negative product targeting, your file might look like this:

|Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|State	|Tactic	|Budget Type	|Budget	|Cost Type	|Bid Optimization	|SKU	|ASIN	|Ad Group Default Bid	|Bid	|Targeting Expression	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Display	|Campaign	|Create	|SD New Campaign	|	|SD New Campaign	|	|20220401	|	|enabled	|T00030	|daily	|100	|	|	|	|	|	|	|	|
|Sponsored Display	|Ad group	|Create	|SD New Campaign	|SD New Ad Group	|	|SD New Ad Group	|	|	|enabled	|	|	|	|	|	|	|	|1	|	|	|
|Sponsored Display	|Product ad	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|BR549	|	|	|	|
|Sponsored Display	|Contextual targeting	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|1	|category="5524098011" price<49.99 rating>3	|
|Sponsored Display	|Negative product targeting	|Create	|SD New Campaign	|SD New Ad Group	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|asin="B09LYCN35V"	|
