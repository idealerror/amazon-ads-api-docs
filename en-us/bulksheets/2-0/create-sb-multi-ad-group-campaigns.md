---
title: How to create Sponsored Brands multi-ad group campaigns with bulksheets
description: A guide explaining how to create new Sponsored Brands campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands V4
    - Sponsored Brands multi-ad groups
    - Campaign management
keywords:
    - bulksheets  
    - create campaigns
---

# How to create Sponsored Brands multi-ad group campaigns in bulksheets

[← Previous: Overview - Sponsored Brands multi-ad group campaigns in bulksheets](bulksheets/2-0/sb-multi-ad-groups-overview)

### Create a Sponsored Brands multi-ad group campaign

| [IMAGE] |   |
| ---- | ---- |
|The following steps will guide you through creating a new Sponsored Brands multi-ad group campaign. Allowable entities are shown in the diagram on the right, with required entities shown in solid pink **→**  <br><br>The entities outlined in pink represent choices you’ll need to make depending on your campaign strategy: For example, you’ll need to choose an **ad format** from the four options shown, as well as a **targeting strategy** (either keyword or product targeting).  |![Multi-ad group entity diagram](/_images/bulksheets/2-0-images/sb-multi-ad-group-entities.png "Multi-ad group entity diagram")|

Learn more about [entities and targeting types, and their relationships to Sponsored Brands multi-ad group campaigns](bulksheets/2-0/sb-multi-ad-group-entities). 

Each step section below represents one row in your bulksheets file. The guidelines describe what you should enter into each field. Note that the read-only, informational-only, and metrics columns are not included in the guidelines below, and you can ignore these fields completely when you create new campaigns.  Also note that these steps are for a campaign that includes a bidding adjustment by placement, with a **Product collection** ad format. *See note below* 

*Note about ad formats*: As you’ll see in the steps below, you will have an entity row to define your desired **Ad format**. Similar to what you would see in the advertising console, you will define a “Landing page type“ in this row, which is related to the **Ad format**. For instance, a **Store spotlight** will drive shoppers to your ”Store on Amazon“ landing page whereas a **Product collection** can send shoppers either to a ”Store on Amazon“ landing page, a ”New landing page“ that we’ll create based on the ASINs you provide, or a ”Custom URL“ landing page of your choice.

For additional information about how to create different ad formats, see [Examples of Sponsored Brands multi-ad group campaigns in bulksheets](bulksheets/2-0/examples-sb-multi-ad-group-campaigns)

#### Step 1. Define the campaign entity

Required fields include: Product, Entity, Operation, Campaign ID, Campaign name, Budget type, Budget, and Bid optimization

Use the guidelines below to define the top-level (parent) campaign entity in the first row of your bulksheets file:

|Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Campaign	|
|Operation	|Create	|
|Campaign ID	|For new campaigns, you should type a text-based name to identify your campaign. You will also enter this exact Campaign ID for each entity you create underneath this parent campaign. <br><br> **NOTE:** When you upload your bulksheets file, the Campaign ID will become the actual unique identifier associated with the campaign and all of its "child" entities. So if you want to update this campaign later, you would enter the actual Campaign ID into this field.	|
|Portfolio ID	|If you want to put the new campaign into a portfolio, enter the portfolio ID. Otherwise, leave blank.	|
|Ad Group ID	|[Leave blank when creating campaign entity]	|
|Ad ID	|[Leave blank when creating campaign entity]	|
|Keyword ID	|[Leave blank when creating campaign entity]	|
|Product Targeting ID	|[Leave blank when creating campaign entity]	|
|Campaign name	|Enter the same campaign name that you entered in the "Campaign ID" column	|
|Ad group name	|[Leave blank when creating campaign entity]	|
|Ad name	|[Leave blank when creating campaign entity]	|
|Start date	|You can leave this field blank, and the start date will default to today's date. If you want to enter a date, the format must be yyyyMMdd. For example, the date December 1, 2024 would be  20241201. **NOTE:** You can start on today's date or a future date, but the date cannot  be in the past. So if you manually set the date for today, be sure you create and  upload the file on today's date.	|
|End date	|You can leave this field blank if you don't want to set an end date. Otherwise, the format must be yyyyMMdd.	|
|State	|Enter **enabled** if you want the campaign to go live. You can also enter **paused** for now, and then change the "State" to **enabled** when you want it to start. 	|
|Brand Entity ID	|**For sellers only, vendors should leave this field blank.** <br> This value is located in the **Brand Assets Tab** when you download campaign data from the **Bulk operations** page. <br> Learn more about [brand assets data and bulksheets](bulksheets/2-0/understand-bulksheets-data).	|
|Budget type	|Enter <b>Lifetime</b>  or <b>Daily</b>	|
|Budget	|Enter the max value you want to spend for your budget. Do not include symbols such as dollar signs or commas. For example, a budget of $100 would be written  as 100 <br> **NOTE:** Be sure your [budget does not exceed the limit for your region](https://advertising.amazon.com/API/docs/en-us/bulksheets/sb/sb-general-info/sb-campaign-budget)	|
|Bid optimization	|Enter **true** if you want Amazon to automatically adjust your budget. Enter **false** if you want to define a custom bidding adjustment by placement. If you enter **false**, you should create a new entity row underneath this campaign for a **Bidding adjustment by placement** entity. <br>For this example, we'll enter **false** so we can enter a custom bid adjusement in the next row. <br><br>The remaining fields can be left blank when creating the "Campaign" entity.	|

Putting it all together, the “campaign” entity row might look like this:


|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|	|	|	|New Campaign	|	|	|	|	|Enabled	|	|Daily	|100	|FALSE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

#### (Optional) Step 1a. Define the bidding adjustment by placement entity

Since we entered “false” for the **Bid optimization** value, we need to define a custom “bidding adjustment” entity. Required fields include: Product, Entity, Operation, Campaign ID, Placement, and a numeric value for the Percentage.

Use the guidelines below to define this entity row. 

|Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Bidding adjustment by placement	|
|Operation	|Create	|
|Campaign ID	|Enter the exact same campaign ID that you entered in the "Campaign" entity row	|
|Portfolio ID	|[Leave blank for bidding adjustment entity]	|
|Ad Group ID	|[Leave blank for bidding adjustment entity]	|
|Ad ID	|[Leave blank for bidding adjustment entity]	|
|Keyword ID	|[Leave blank for bidding adjustment entity]	|
|Product Targeting ID	|[Leave blank for bidding adjustment entity]	|
|Campaign name	|[Leave blank for bidding adjustment entity]	|
|Ad group name	|[Leave blank for bidding adjustment entity]	|
|Ad name	|[Leave blank for bidding adjustment entity]	|
|Start date	|[Leave blank for bidding adjustment entity]	|
|End date	|[Leave blank for bidding adjustment entity]	|
|State	|[Leave blank for bidding adjustment entity]	|
|Brand Entity ID	|[Leave blank for bidding adjustment entity]	|
|Budget type	|[Leave blank for bidding adjustment entity]	|
|Budget	|[Leave blank for bidding adjustment entity]	|
|Bid optimization	|[Leave blank for bidding adjustment entity]	|
|Product location	|[Leave blank for bidding adjustment entity]	|
|Bid	|[Leave blank for bidding adjustment entity]	|
|Placement	|Enter **Home**, **Detail page**, or **Other**. Note that after you create this campaigh, you will see three "Bidding adjustment" rows in the downloaded file--one for each placement type. The placements that you did not explicitly set a value for will have a value of 0. To update these entities after creating the campaign, set the "Operation" field to **Update**, and change the "Percentage" value. 	|
|Percentage	|Enter a positive or negative value to set a custom bid adjustment. For example, to set a 40 percent multiplier, enter **40**.	|

After you add the “bidding adjustment by placement” row, your spreadsheet might look like this: 


|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|	|	|	|New Campaign	|	|	|	|	|Enabled	|	|Daily	|100	|FALSE	|	|100	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Bidding adjustment by placement	|Create	|New Campaign	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|Home	|40%	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

#### Step 2. Define the Ad group entity. 

Next, you’ll enter the **Ad group** details in the row underneath the campaign. Required fields include: Product, Entity, Operation, Campaign ID, Ad group ID, Ad group name, and State.

In the next row of your bulksheets file, use the following guidelines to define the “Ad group” entity for this campaign:

|Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Ad group	|
|Operation	|Create	|
|Campaign ID	|Enter the exact same campaign ID that you entered in the "Campaign" entity row. This will ensure that the ad group becomes a "child" of the top-level campaign "parent" entity.	|
|Portfolio ID	|[Leave blank for Ad group entity]	|
|Ad Group ID	|For new ad groups, you should type a text-based name to identify the ad group. You will also type this exact same name into the “Entity Name” column for this row. **NOTE:** When you upload your bulksheets file, the Ad Group ID will become the actual unique identifier associated with this ad group. You will use this unique ID if you want to update this ad group later. 	|
|Ad ID	|[Leave blank for Ad group entity]	|
|Keyword ID	|[Leave blank for Ad group entity]	|
|Product Targeting ID	|[Leave blank for Ad group entity]	|
|Campaign name	|[Leave blank for Ad group entity]	|
|Ad group name	|Enter the same name that you entered in the "Ad Group ID" column	|
|Ad name	|[Leave blank for Ad group entity]	|
|Start date	|[Leave blank for Ad group entity]	|
|End date	|[Leave blank for Ad group entity]	|
|State	|Enter *enabled* 

The remaining fields can be left blank for the "Ad group" entity row. 	|

Putting it all together, your spreadsheet rows might now look like this:

|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|	|	|	|New Campaign	|	|	|	|	|Enabled	|	|Daily	|100	|FALSE	|	|100	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Bidding adjustment by placement	|Create	|New Campaign	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|Home	|40%	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|New Campaign	|	|New Ad group	|	|	|	|	|New Ad group	|	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

You could add more ad groups to this campaign by repeating the previous step. 

#### Step 3. Define the product collection ad entity. 

In the next row of your bulksheets file, use the following guidelines to define the ad format that you want for this campaign and ad group. Options include **Product collection ad**, **Store spotlight ad**, **Video ad**, or **Brand video ad**. For this example, we’ll create a **Product collection** **ad**. Required fields include: Product, Entity, Operation, Campaign ID, Ad group ID, Ad name, State, Landing page URL, Brand name. Brand logo asset ID, Creative headline, Creative ASINs, and Custom image.

>[NOTE]Effective January 31, 2024, for product collection ads, you must specify a custom image. Otherwise, you will get a validation error for the "Product ad" entity. Note that the campaign may still get created, but your ads will not serve or get impressions without custom images.

Define the custom images using the format shown in the table below.

|Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Product collection ad	|
|Operation	|Create	|
|Campaign ID	|Enter the exact same Campaign ID that you entered in the "Campaign" entity row. This will ensure that the ad group becomes a "child" of the top-level campaign "parent" entity.	|
|Portfolio ID	|[Leave blank for Product collection entity]	|
|Ad Group ID	|Enter the exact same Ad group ID that you entered in the "Ad group" entity row. 	|
|Ad ID	|[Leave blank for Product collection ad entity]	|
|Keyword ID	|[Leave blank for Product collection ad entity]	|
|Product Targeting ID	|[Leave blank for Product collection ad entity]	|
|Campaign name	|[Leave blank for Product collection ad entity]	|
|Ad group name	|[Leave blank for Product collection ad entity]	|
|Ad name	|Enter a unique name for the ad	|
|Start date	|[Leave blank for Product collection ad entity]	|
|End date	|[Leave blank for Product collection ad entity]	|
|State	|Enter *enabled*|
|Brand Entity ID	|If you are a seller, you can find this value in the "Brand assets data" tab in a downloaded bulksheets file. If you are a vendor, leave this field blank. 	|
|Budget type	|[Leave blank for Product collection ad entity]	|
|Budget	|[Leave blank for Product collection ad entity]	|
|Bid optimization	|[Leave blank for Product collection ad entity]	|
|Product location	|[Leave blank for Product collection ad entity]	|
|Bid	|[Leave blank for Product collection ad entity]	|
|Placement	|[Leave blank for Product collection ad entity]	|
|Percentage	|[Leave blank for Product collection ad entity]	|
|Keyword text	|[Leave blank for Product collection ad entity]	|
|Match type	|[Leave blank for Product collection ad entity]	|
|Native language keyword	|[Leave blank for Product collection ad entity]	|
|Native language locale	|[Leave blank for Product collection ad entity]	|
|Product targeting expression	|[Leave blank for Product collection ad entity]	|
|Landing page URL	|The landing page is where shoppers are directed after they interact with your ad. You can leave this field blank, and Amazon will create a new landing page—you can get the URL from a downloaded bulksheets file. If you want to use an existing Store on Amazon or another website, enter the URL in this field. 	|
|Landing page ASINs	|Leave blank. You'll list your advertised ASINs in the "Creative ASINs" field.	|
|Landing page type	|Enter **Product list** to direct shoppers to a new landing page that features the three ASINs you want to advertise. Enter **Store** for a Store on Amazon landing page--and also include the URL in the "Landing page URL" field above**.** You can also leave this field blank, and it will default to a **Product list** page. 	|
|Brand name	|Enter your brand name or the name of the brand you want to manage	|
|Consent to translate	|[Leave blank for Product collection entity]	|
|Brand logo asset ID	|Enter the ID for your brand logo. You can locate this in a downloaded bulksheets file if you include "Brand assets data" from the checklist before you download the file. The ID will look something like this: amzn1.assetlibrary.asset1.38f47f3	|
|Brand logo crop	|Leave this field blank if your image doesn't need to be resized.   If your logo needs to be resized, you can adjust the image in this field. Enter the following string, including the desired pixel sizes: {"left":"0","top":"0","width": 0","height":"0"} <br><br>Note that 400 x 400 is the minimum px size. 	|
|Custom images	|You must define custom images using the following format: [{"assetId":"amzn1.assetlibrary.asset1.your_assetID_1","crop":{"left":"0","top":"354","width":"4950","height":"2591"}},{"assetId":"amzn1.assetlibrary.asset1.your_assetID_2","crop":{"left":"0","top":"355","width":"4950","height":"2591"}},{"assetId":"amzn1.assetlibrary.asset1.your_assetID_3","crop":{"left":"0","top":"354","width":"4950","height":"2591"}}] <br><br> In this format, the “assetID” value can be found in the “Brand assets data” tab of a dowloaded bulksheets file, or in your creative assets library in the advertising console. This ID refers to the Asset IDs that you want to use for the custom image. <br><br> TIPS: In the “Brand assets ID” tab, filter Column A (Brand entity ID) to show only custom images. Look in Column D to see the asset names if you need to locate the image. Then, use the corresponding Asset ID found in Column B in the formula provided above. <br><br> Note that if you don't specify crop values, the entire image will be used. In the advertising console, this is done in the "Creative" section where you add your logo, creative headline, etc. 	|
|Creative headline	|This headline highlights the unique benefits of the products in your ad. Use a conversational tone that matches your brand. The maximum is 50 characters. 	|
|Creative ASINs	|Enter three ASINs, separated by a comma, that you want to advertise 	|
|Video asset IDs	|[Leave blank for Product collection ad entity]	|
|Sub-pages	|[Leave blank for Product collection entity. Sub-pages are only used for Store spotlight ad formats]	|

After adding the **Product collection ad** row, your spreadsheet might look like this:

|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|	|	|	|New Campaign	|	|	|	|	|Enabled	|	|Daily	|100	|FALSE	|	|100	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Bidding adjustment by placement	|Create	|New Campaign	|	|	|	|	|	|	|	|	|	|	|Enabled	|	|	|	|	|	|	|	|30	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|New Campaign	|	|New Ad group	|	|	|	|	|New Ad group	|	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|Create	|New Campaign	|	|New Ad group	|	|	|	|	|	|My Ad name	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/23F9CB2E	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3	|{"left":"0","top":"0","width":"450","height":"450"}	|	[{"assetId":"amzn1.assetlibrary.asset1.your_assetID_1","crop":{"left":"0","top":"354","width":"4950","height":"2591"}},{"assetId":"amzn1.assetlibrary.asset1.your_assetID_2","crop":{"left":"0","top":"355","width":"4950","height":"2591"}},{"assetId":"amzn1.assetlibrary.asset1.your_assetID_3","crop":{"left":"0","top":"354","width":"4950","height":"2591"}}]|My Creative Headline	|B000132WC0, B0CJ9F17HX, 1549232401	|	|	|

#### Step 4. Define the keyword entity

For this campaign, we’re using **keyword** targeting. If you want to use **product targeting** instead, you can find an example in [Examples of Sponsored Brands multi-ad group campaigns](_bulksheets/2-0/examples-sb-multi-ad-group-campaigns)_

For keyword targeting, required fields include: Product, Entity, Operation, Campaign ID, Ad group ID, State, Bid, Keyword text, and Match type

|Column header	|Guidelines on what to enter	|
|---	|---	|
|Product	|Sponsored Brands	|
|Entity	|Keyword	|
|Operation	|Create	|
|Campaign ID	|Enter the exact same Campaign ID that you entered in the "Campaign" entity row	|
|Portfolio ID	|[Leave blank for keyword targeting]	|
|Ad Group ID	|Use the exact same Ad group ID that you entered in the "Ad group" entity row	|
|Ad ID	|[Leave blank for keyword targeting]	|
|Keyword ID	|Leave blank when creating a new keyword entity. To update the keyword later, you'll use the system-generated Keyword ID that you can find in a downloaded bulksheets file. 	|
|Product Targeting ID	|[Leave blank for keyword entity]	|
|Campaign name	|[Leave blank for keyword entity]	|
|Ad group name	|[Leave blank for keyword entity]	|
|Ad name	|[Leave blank for keyword entity]	|
|Start date	|[Leave blank for keyword entity]	|
|End date	|[Leave blank for keyword entity]	|
|State	|To create the keyword, enter **enabled**. For existing keywords that you want to stop running, you could enter **paused** here and include the keyword ID.	|
|Brand Entity ID	|[Leave blank for keyword entity]	|
|Budget type	|[Leave blank for keyword entity]	|
|Budget	|[Leave blank for keyword entity]	|
|Bid optimization	|[Leave blank for keyword entity]	|
|Product location	|[Leave blank for keyword entity]	|
|Bid	|Enter the CPC amount you are willing to pay when a shopper clicks your ad. Do not use money symbols or commas. For example, to bid 75 cents, enter **0.75**. To bid one dollar, enter **1**.	|
|Placement	|[Leave blank for keyword entity]	|
|Percentage	|[Leave blank for keyword entity]	|
|Keyword text	|Enter one keyword or phrase. To add more keywords, create a new row for each keyword entity.	|
|Match type	|Options are: **broad**, **phrase**, **exact**	|

After you add the keyword targeting entity data, your bulksheets file might now look like this:


|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|	|	|	|New Campaign	|	|	|	|	|Enabled	|	|Daily	|100	|FALSE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Bidding adjustment by placement	|Create	|New Campaign	|	|	|	|	|	|	|	|	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|New Campaign	|	|New Ad group	|	|	|	|	|New Ad group	|	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|Create	|New Campaign	|	|New Ad group	|	|	|	|	|	|My ad name	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/23F9CB2E	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3	|{"left":"0","top":"0","width":"450","height":"450"}	|	|My Creative Headline	|B000132WC0, B0CJ9F17HX, 1549232401	|	|	|
|Sponsored Brands	|Keyword	|Create	|New Campaign	|	|New Ad group	|	|	|	|	|	|	|	|	|Enabled	|	|	|	|	|	|1	|	|	|My Keyword	|phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

#### Upload your bulksheets file to create the new Sponsored Brands multi-ad group campaign

At this point, all mandatory entities are included in this Sponsored Brands multi-ad group campaign—but you could add more ad groups or targets by following the same steps explained above. 

| [IMAGE] |   |
| ---- | ---- |
|If you’re ready to create the campaign, you should save the file and then navigate back to the “Bulk operations” page.   <br><br>Under “3. Upload your file to update your campaigns,” click **Choose file** to upload the sheet you just created.  |![Upload bulksheets file](/_images/bulksheets/2-0-images/sp-3-upload-your-file.png "Upload bulksheets file")|

#### Optional: Add more entities to the campaign

If you want to add more entities, such as additional ad groups or keywords, you can continue adding each new entity into a new row using the processes described above. Be sure to always include **Sponsored Brands** in the “Product” column for each row, along with **Create** as the “Operation” for new campaigns, along with the parent IDs for child entities. 

See more [examples of creating other entities and campaign types for Sponsored Brands multi-ad group campaigns](bulksheets/2-0/examples-sb-multi-ad-group-campaigns)
