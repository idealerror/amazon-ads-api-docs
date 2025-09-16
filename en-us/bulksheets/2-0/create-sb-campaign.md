---
title: How to create Sponsored Brands campaigns with bulksheets
description: A guide explaining how to create new Sponsored Brands campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands
    - Campaign management
keywords:
    - bulksheets  
    - create campaigns
---

# How to create Sponsored Brands campaigns in bulksheets

[← Get started with bulksheets for Sponsored Brands](bulksheets/2-0/get-started-with-sb-bulksheets)

When you create a Sponsored Brands (SB) campaign, you will need your brand assets information, including some of the IDs found in the “Brand Assets Data” tab in a downloaded bulksheets file. Review this article to [understand bulksheets downloads and brand assets data](bulksheets/2-0/understand-bulksheets-data).

### Create a Sponsored Brands campaign

| [IMAGE] |   |
| ---- | ---- |
| Allowable entities are shown in the diagram on the right, with required entities shown in green. For SB campaigns, most of the brand-related values are defined at the top-level parent (campaign) entity, so you will usually enter more data in the “Campaign” entity row compared with “Keyword” and “Product targeting” entity rows.<br><br>Learn more about [entity and targeting types, and their relationships to Sponsored Brands campaigns](bulksheets/2-0/sb-entities). | ![Sponsored Brands entity diagram](/_images/bulksheets/2-0-images/sb-entities.png "Sponsored Brands entity diagram") |

Each “step” section below represents one row in your bulksheets file. The guidelines describe what you should enter into each field. **NOTE:** Read-only, informational-only, and metrics columns in your Sponsored Brands bulksheets file are not included in the guide below, and **you can ignore these fields completely when you create a new campaign** with bulksheets. 

**TIPS:** 

* Be sure to review the Sponsored Brands [budget limits](concepts/limits#budget-constraints-by-marketplace) and [bid limits](concepts/limits#bid-constraints-by-marketplace) for your region.
* Headers marked with an asterisk * are fields that <em>cannot be updated</em> after you create the campaign. 
* Sponsored Brands campaigns must be reviewed before they become live. The review process could take up to three days. Learn more about [Sponsored Brands moderation](https://advertising.amazon.com/help#GJB8YEKP7UJYKSR2).<br><br> **IMPORTANT:** Do not edit or alter the bulksheets column headers. This will cause the upload to fail. 

#### Step 1. Define the campaign entity

Use the guidelines below to define the top-level (parent) campaign entity in the first row of your bulksheets file:

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Campaign	|
|Operation	|Create	|
|Campaign Id*	|For new campaigns, you should type a text-based name to identify your campaign. You will also enter this exact Campaign ID for each entity you create underneath this parent campaign. <br> **NOTE:** When you upload your bulksheets file, the Campaign ID will become the actual unique identifier associated with the campaign and all of its "child" entities. So if you want to update this campaign later, you would enter the actual Campaign ID into this field.	|
|Draft Campaign Id*	|[Leave blank]	|
|Portfolio Id	|If you want to put the new campaign into a portfolio, enter the portfolio ID. Otherwise, leave blank.	|
|Campaign Name	|Enter the same campaign name that you entered in the "Campaign ID" column.	|
|Start Date*	|Format must be yyyyMMdd. For example, the date April 1, 2022 would be  20220401. <br> **NOTE:** You can start on today's date or a future date, but the date cannot  be in the past. So if you set the date for today, be sure you create and  upload the file on today's date. If you leave this field blank, the start date will be set for today's date.	|
|End Date	|You can leave this field blank if you don't want to set an end date. Otherwise, the format must be yyyyMMdd.	|
|State	|This is the status you want the campaign to be in after you upload the bulksheets file. To create a new Sponsored Brands campaign, enter **enabled**.	|
|Budget Type	|Enter **lifetime** or **daily**.	|
|Budget	|Do not include symbols such as dollar signs or commas. For example, a daily budget of $100 would be written  as **100**. <br> **NOTE**: Be sure your [budget does not exceed the limit for your region](concepts/limits#budget-constraints-by-marketplace).	|
|Bid Optimization	|Enter **auto** or **manual**. If you enter **manual**, you should enter a value in the "Bid Multiplier" column. <br> <br> In the UI, this is the "Automated bidding" toggle. Enter **auto** if you want to allow Amazon to automatically optimize bids for placements other than top-of-search. Otherwise, enter **manual**.	|
|Bid Multiplier	|Enter a positive or negative percentage value to set a custom bid adjustment. For example, to set a 40 percent multiplier, enter **40%**. <br> Leave this field blank if you entered **auto** for "Bid Optimization." <br> In the UI, this is a checkbox to **Set a custom bid adjustment** when automated bidding is not selected.	|
|Bid	|[Leave blank when creating campaign]	|
|Keyword Text*	|[Leave blank when creating campaign]	|
|Match Type	|[Leave blank when creating campaign]	|
|Product Targeting Expression*	|[Leave blank when creating campaign]	|
|Ad Format*	|Enter **productCollection** or **video**. <br> If you select **productCollection**, you will also need to define the landing page details as well. For **video** ad format, you should leave all of the landing page fields blank. <br> **NOTE**: When you create a new SB campaign in the UI, you have three options for ad format: "Product Collection, "Video," and "Store spotlight."  Bulksheets does not support *creating* a new "Store spotlight" at this time, but you can *update* this format using bulksheets if you've created a store spotlight in the past.	|
|Landing Page URL*	|Enter a URL if you are using **Amazon Store** or **Custom URL** as your landing page type. <br> Leave blank if you are creating a ***new landing page***. In this case, you should instead enter the product asins in the "Landing Page asins" column, and Amazon will generate a new landing page for your product collection. <br> Leave blank if your ad format is **video**. After you create a video campaign, you will see "Landing Page Type" and "Landing Page URL" fields populated if you download a bulksheets file. "Landing Page Type" will be **detailPage**. 
Landing Page asins*	|If you are creating a new landing page, enter between 3 and 100 asins you want to include in your product collection. You can select up to 3 of these to highlight in the "Creative asins" column. <br> If you are not creating a new landing page, or if your ad format is **video**, leave this field blank.	|
|Brand Entity Id*	|***For sellers only--vendors should leave this field blank*** <br> You can find this value in the "Brand Assets Tab" when you download campaign data from the "Bulk operations" page. <br> Learn more about [brand assets data and bulksheets](bulksheets/2-0/understand-bulksheets-data).	|
|Brand Name*	|Enter your brand name.	|
|Brand Logo Asset Id*	|If your ad format is **productCollection**, enter the brand logo asset ID. You can find this ID in the "brand assets data" tab when you download campaign data from the "Bulk operations" page. <br> Leave blank if ad format is **video**.	|
|Brand Logo URL	|Leave blank when you're creating a campaign. After you create a campaign, you will see this URL populated in a downloaded bulksheets file. 	|
|Creative Headline*	|Leave blank if ad format is **video**. <br> If ad format is **productCollection**, you should enter any text value, up to 50 characters. In the UI, this is the headline you enter in the "Creative" section where you upload creative assets.	|
|Creative asins*	|Enter up to three asins for **productCollection** (separated by commas) or one asin if ad format is **video**. These are the products that will be featured on your landing page.	|
|Video Media Ids*	|If your ad format is **video**, enter the "Asset ID" found in the "Brand Assets Tab" of your downloaded bulksheets file. <br> TIP: To locate the video asset ID, you can filter "Asset Type" (Column A) to show video assets. This might make it easier to find the ID in the "Asset ID" column (Column C) |
|Creative Type*	|Enter **video** if your ad format is video. Leave blank if ad format is **productCollection**.	|

Putting it all together, this bulksheets row for a **video** ad format might look like this:


|Product	|Entity	|Operation	|Campaign Id	|Draft Campaign Id	|Portfolio Id	|Campaign Name	|Start Date	|End Date	|State	|Budget Type	|Budget	|Bid Optimization	|Bid Multiplier	|Bid	|Keyword Text	|Match Type	|Product Targeting Expression	|Ad Format	|Landing Page URL	|Landing Page asins	|Brand Entity Id	|Brand Name	|Brand Logo Asset Id	|Brand Logo URL	|Creative Headline	|Creative asins	|Video Media Ids	|Creative Type	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|New Campaign	|20220101	|	|Enabled	|Daily	|100	|Auto	|	|	|	|	|	|video	|	|	|ENTITY1A0PDCYJI5DZG	|My Brand	|	|https://m.media-amazon.com/image.png	|	|B0099HD3Y	|amzn1.assetlibrary.asset1.1031d1a8ae14a71cf04b6e428ec5d276	|video	|


To create the campaign entity using a **productCollection** ad format with an **Amazon Store** landing page might look like this:


|Product	|Entity	|Operation	|Campaign Id	|Draft Campaign Id	|Portfolio Id	|Campaign Name	|Start Date	|End Date	|State	|Budget Type	|Budget	|Bid Optimization	|Bid Multiplier	|Bid	|Keyword Text	|Match Type	|Product Targeting Expression	|Ad Format	|Landing Page URL	|Landing Page asins	|Brand Entity Id	|Brand Name	|Brand Logo Asset Id	|Brand Logo URL	|Creative Headline	|Creative asins	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|New Campaign	|20220101	|	|Enabled	|Daily	|100	|Auto	|	|	|	|	|	|productCollection	|www.mystore.com	|	|ENTITY83028	|My Brand	|BR549003	|	|My Headline	|asin1, asin2, asin3	|

Next, you’ll enter the targeting details in the row underneath the campaign. The entity type will depend on whether you want **keyword** or **product targeting**. 

#### Step 2a. Define the Keyword entity. ***For product targeting, [jump to Step 2b](#step-2b-define-the-product-targeting-entity).* **

In the next row of your bulksheets file, use the following guidelines to define the “Keyword” entity for this campaign:

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Keyword	|
|Operation	|Create	|
|Campaign Id*	|Enter the exact same campaign name that you entered in the "Campaign" entity row. This will ensure that the keyword is associated with the top-level campaign "parent" entity.	|
|Draft Campaign Id*	|[Leave blank]	|
|Portfolio Id	|[Leave blank for keyword entity]	|
|Campaign Name	|[Leave blank for keyword entity]	|
|Start Date*	|[Leave blank for keyword entity]	|
|End Date	|[Leave blank for keyword entity]	|
|State	|To create the keyword, enter **enabled**. <br> For existing keywords that you want to stop running, you could enter **paused** here and include the keyword ID.	|
|Budget Type	|[Leave blank for keyword entity]	|
|Budget	|[Leave blank for keyword entity]	|
|Bid Optimization	|[Leave blank for keyword entity]	|
|Bid Multiplier	|[Leave blank for keyword entity]	|
|Bid	|Enter the CPC amount you are willing to pay when a shopper clicks your ad. Do not use money symbols or commas. For example, to bid 75 cents, enter **0.75**. To bid one dollar, enter **1**.<br> **NOTE**: There is a 2-place decimal limit. If you enter a number with more than 2 decimal places, you will get an error when you upload the file.	|
|Keyword Text*	|Enter one keyword or phrase. To add more keywords, create a new row for each keyword entity.	|
|Match Type	|Options are: **broad**, **phrase**, **exact**	|
|Product Targeting Expression*	|[Leave blank for keyword entity]	|
|Ad Format*	|[Leave blank for keyword entity]	|
|Landing Page URL*	|[Leave blank for keyword entity]	|
|Landing Page asins*	|[Leave blank for keyword entity]	|
|Brand Entity Id*	|[Leave blank for keyword entity]	|
|Brand Name*	|[Leave blank for keyword entity]	|
|Brand Logo Asset Id*	|[Leave blank for keyword entity]	|
|Brand Logo URL	|[Leave blank for keyword entity]	|
|Creative Headline*	|[Leave blank for keyword entity]	|
|Creative asins*	|[Leave blank for keyword entity]	|
|Video Media Ids*	|[Leave blank for keyword entity]	|
|Creative Type*	|[Leave blank for keyword entity]	|
<br>

If you add two keywords to a Sponsored Brands “Product Collection,” your sheet might look like this:

|Product	|Entity	|Operation	|Campaign Id	|Draft Campaign Id	|Portfolio Id	|Campaign Name	|Start Date	|End Date	|State	|Budget Type	|Budget	|Bid Optimization	|Bid Multiplier	|Bid	|Keyword Text	|Match Type	|Product Targeting Expression	|Ad Format	|Landing Page URL|Landing Page asins	|Brand Entity Id	|Brand Name	|Brand Logo Asset Id	|Brand Logo URL	|Creative Headline	|Creative asins	|Video Media Ids
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---
Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|New Campaign	|20220101	|	|Enabled	|Daily	|100	|Auto	|	|	|	|	|	|productCollection	|www.mystore.com	|store	|	|ENTITY83028	|My Brand	|BR549003	|My headline	|asin1, asin2, asin3	|	|
|Sponsored Brands	|Keyword	|Create	|NewCampaign	|	|	|	|	|	|Enabled	|	|	|	|	|0.75	|6th grade math	|exact	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Keyword	|Create	|New Campaign	|	|	|	|	|	|Enabled	|	|	|	|	|1	|math books kids	|phrase	|	|	|	|	|	|	|	|	|	|	|	|	|

#### Step 2b. Define the product targeting entity

|[grid]Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Product targeting	|
|Operation	|Create	|
|Campaign Id*	|Enter the exact same campaign name that you entered in the "Campaign" entity row.	|
|Draft Campaign Id*	|[Leave blank for product targeting]	|
Ad Group Id*	|[Leave blank for product targeting]	|
|Portfolio Id	|[Leave blank for product targeting]	|
|Keyword Id	|[Leave blank for product targeting]	|
|Product Targeting Id	|Leave blank when creating a new campaign. For updating or archiving, you would enter the actual product targeting ID. 	|
|Entity Name	|[Leave blank for product targeting]	|
|Start Date*	|[Leave blank for product targeting]	|
|End Date	|[Leave blank for product targeting]	|
|Ad Format* |[Leave blank for product targeting]	|
|Budget Type	|[Leave blank for product targeting]	|
|Budget	|[Leave blank for product targeting]	|
|Bid Optimization	|[Leave blank for product targeting]	|
|Bid Multiplier	|[Leave blank for product targeting]	|
|State	|To create a product targeting entity, enter **enabled**.  <br>**NOTE:** Options are enabled or  paused.	|
|Bid	|Enter the CPC amount you are willing to pay when a shopper clicks your ad. Do not use money symbols or commas. For example, to bid 75 cents, enter **0.75**. To bid one dollar, enter **1**.	|
|Keyword Text*	|[Leave blank for product targeting]	|
|Match Type*	|[Leave blank for product targeting]	|
|Product Targeting Expression*	|In the UI, this is where you select either "individual products" or "categories" to target. For "individual products," use the asin and enter **asin="A000000000000"** (using the actual **asin** ID--note that in this field, **asin** must be entered all lowercase as shown). For "categories," use the category ID along with other qualifiers such as price limit and star rating. For this field, enter all qualifiers into the same cell of your bulksheets file because they work as a combined unit to define the targeting.  <br> <br> Example 1. To target by product category, price, and star rating: **category="5524098011" price<49.99 rating>3** <br> <br> Example 2. To target by product category and Prime eligibility: **category="5524098011" prime-shipping-eligible="true"** <br> <br> Learn more about targeting expression rules, and [see more sample expressions](https://advertising.amazon.com/API/docs/en-us/bulksheets/sp/sp-general-info/sp-product-attribute-targeting#P). <br> **TIP:** To get the numeric values for attributes such as category and age range, you can use past campaign data where you assigned those targeting expressions by exporting the campaign into a bulksheet.	|
|Landing Page URL*	|[Leave blank for product targeting]	|
|Landing Page asins*	|[Leave blank for product targeting]	|
|Brand Entity Id*	|[Leave blank for product targeting]	|
|Brand Name*	|[Leave blank for product targeting]	|
|Brand Logo Asset Id*	|[Leave blank for product targeting]	|
|Brand Logo URL	|You can leave this blank when you're creating a campaign, and the system will generate a URL that you will find in the "Brand Assets Data" tab of a downloaded bulksheet. But if you have a Brand Logo URL that you want to use, you can include it in this field when creating the campaign. 	|
|Creative Headline*	|[Leave blank for product targeting]	|
|Creative Asins*	|[Leave blank for product targeting]	|
|Video Media Ids*	|[Leave blank for product targeting]	|
|Creative Type*	|[Leave blank for product targeting]	|

After you add the product targeting entity data, your bulksheets file might now look like this:

|Product	|Entity	|Operation	|Campaign Id	|Draft Campaign Id	|Portfolio Id	|Campaign Name	|Start Date	|End Date	|State	|Budget Type	|Budget	|Bid Optimization	|Bid Multiplier	|Bid	|Keyword Text	|Match Type	|Product Targeting Expression	|Ad Format	|Landing Page URL	|Landing Page asins	|Brand Entity Id	|Brand Name	|Brand Logo Asset Id	|Brand Logo URL	|Creative Headline	|Creative asins	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|New Campaign	|20220101	|	|Enabled	|Daily	|100	|Auto	|	|	|	|	|	|productCollection	|www.mystore.com	|	|ENTITY83028	|My Brand	|BR549003	|	|My Headline	|asin1, asin2, asin3	|
Sponsored Brands	|Product targeting	|Create	|New Campaign	|	|	|	|	|	|Enabled	|	|	|	|	|0.75	|	|	|asin="BR5490003"	|	|	|	|	|	|	|	|	|	|

#### Upload your bulksheets file to create the new Sponsored Brands campaign


At this point, all mandatory entities are included in this auto targeting Sponsored Brands campaign. (Or you can still add more entities if you want to add negative targeting)
If you’re ready to create the campaign, you should save the file and then navigate back to the “Bulk operations” page. 

| [IMAGE] |  |
|----	|----	|
| Under “3. Upload your file to update your campaigns,” click **Choose file** to upload the sheet you just created. |![Upload your file](/_images/bulksheets/2-0-images/sp-3-upload-your-file.png "Upload your file")|

#### Optional: Add more entities to the campaign

If you want to add more entities, such as negative keywords, you can continue adding each new entity into a new row using the process described above. Be sure to always include **Sponsored Brands** in the “Product” column for each row, along with **Create** as the “Operation” for new campaigns. For reference, here are the additional fields you need to complete for the remaining entities:

* **Negative keyword** entity fields: Campaign ID, State, Keyword text, Match type (negativeExact or negativePhrase)
* **Negative product targeting** entity fields: Campaign ID, State, Product targeting expression (enter **asin** or **brand** ID to exclude)
