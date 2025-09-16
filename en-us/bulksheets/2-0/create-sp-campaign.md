---
title: How to create Sponsored Products campaigns with bulksheets
description: A guide explaining how to create new Sponsored Products campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - bulksheets  
    - create campaigns
---

# How to create Sponsored Products campaigns in bulksheets 

[← Get started with bulksheets for Sponsored Products](bulksheets/2-0/get-started-with-sp-bulksheets)

This article shows detailed steps on creating new Sponsored Products campaigns using a blank bulksheets template. If you want information about how to pause, archive, or update campaigns--including how to change campaign or ad group names--see the [overview on updating campaigns with bulksheets](bulksheets/2-0/campaign-update-overview). 

To create a Sponsored Products campaign, first decide which [targeting type](https://advertising.amazon.com/help?entityId=ENTITY315209Y8E0NPP#GMCARW8DKXNM33LF) you want: Either manual targeting or automatic (auto) targeting.  Similar to the advertising console UI, if you select  "manual," you'll need bulksheets entities for either keyword or product targeting. If you select "auto," Amazon will target keywords and products that are similar to the product in your ad, and those targets will inherit the ad group default bid. With auto targeting, you also have the option to set bids by targeting group if you create product targets.  We'll explain how to create bulksheets files for these auto and manual campaigns in the next two sections, starting with auto targeting. [*Jump to manual targeting*](#create-manual-targeting-campaign)

### Create auto targeting campaign

| [IMAGE] |   |
| ---- | ---- |
| Allowable entities (Column B of your bulksheets file) are shown in the hierarchical diagram on the right—the green entities are required fields that you must define for a SP auto targeting campaign. <br><br>Learn more about [SP entity and targeting types, and their relationships to Sponsored Products campaigns](bulksheets/2-0/sp-entities). | ![Sponsored Products entity diagram](/_images/bulksheets/2-0-images/NEWsp-auto-entities.png) |
	


The steps below show which fields you should complete on a row-by-row basis, starting with the top-level **campaign** entity. Each “step” section represents one row in your bulksheets file. The guidelines describe what you should enter into each field of the blank template.

####Step 1. Define the campaign entity

To create the top-level (parent) campaign entity, required fields include Product, Entity, Operation, Campaign ID, Campaign name, Start date, Targeting type, State, and Daily budget. 

Use the guidelines below to define the campaign entity in the first row of your bulksheets file:

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Campaign    |
|Operation    |Create    |
|Campaign ID*    |For new campaigns, you should type a text-based name to identify your campaign. You will enter this exact Campaign ID for each entity you create underneath this parent campaign. NOTE: When you upload your bulksheets file, the Campaign ID will become the actual unique identifier associated with the campaign and all of its "child" entities. So if you want to update this campaign later, you would enter the actual Campaign ID into this field.    |
|Ad Group ID*    |[Leave blank when creating campaign entity]    |
|Portfolio ID*    |If you want to put the new campaign into an existing portfolio, enter the portfolio ID. Otherwise, leave blank. [Learn more about using portfolios with bulksheets](bulksheets/2-0/bulksheets-portfolios)    |
|Ad ID*    |[Leave blank when creating campaign entity]    |
|Keyword ID*    |[Leave blank. This field will be useful when you want to archive or pause keywords]    |
|Product Targeting ID*    |[Leave blank when creating campaign entity]    |
|Campaign Name    |Enter the same campaign name that you entered in the "Campaign ID" column.    |
|Ad Group Name    |[Leave blank when creating campaign entity]    |
|Start Date    |Format must be yyyyMMdd. For example, the date April 1, 2024 would be 20240401. <br> **NOTE:** You can start on today's date or a future date, but the date cannot be in the past. So if you set the date for today, be sure you create and upload the file on today's date. If you leave this field blank, the start date will be set for today's date.   |
|End Date    |You can leave this field blank if you don't want to set an end date. Otherwise, the format must be yyyyMMdd.    |
|Targeting Type*    |Enter **auto**    |
|State    |Options are <b>enabled</b> or <b>paused</b>. Enter  **enabled** to create a new campaign.    |
|Daily Budget    |Do not include symbols such as dollar signs. For example, a daily budget of $100 would be written as **100**. If the figure isn't a whole number, use a decimal point and not a comma. Be sure your [budget is within the allowed limits for your region](concepts/limits#budget-constraints-by-marketplace).    |
|sku*    |[Leave blank when creating campaign entity]    |
|asin*    |[Leave blank when creating campaign entity]    |
|Ad Group Default Bid    |[Leave blank when creating campaign entity]    |
|Bid    |[Leave blank when creating campaign entity]    |
|Keyword Text*    |[Leave blank when creating campaign entity]    |
|Match Type*    |[Leave blank. This field only applies to the **keyword** entity]    |
|Bidding Strategy    |Similar to what you'd see in the UI, you can choose a bidding strategy: Enter **Dynamic bids - down only**, **Dynamic bids - up and down**, or **Fixed bid**.    |
|Placement*    |[Leave blank. To define the placement, create a **bidding adjustment** entity in this campaign]    |
|Percentage    |[Leave blank. The percentage adjustment is defined with the **bidding adjustment** entity]    |
|Product Targeting Expression*    |[Leave blank when creating campaign entity. Only used for manual campaign product targeting entity]    |

Putting it all together, this bulksheets row for creating the campaign entity might look like this—note that some fields, such as Campaign ID, contain sample text for reference:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Spring toys 2024    |    |    |    |    |    |Spring toys 2024    |    |20240401    |    |Auto    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |

<br>
####Step 2. Define the bidding adjustment entity (OPTIONAL)

In the advertising console UI, the optional bidding adjustment is part of the overall campaign bidding strategy that adjusts bids by placement. Bulksheets supports placements for top-of-page (first page), product detail pages, rest-of-search, and Site Amazon Business. 

To create a bidding adjustment, required fields include Product, Entity, Operation, Campaign ID, Bidding strategy, Placement, and Percentage. 

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Bidding adjustment    |
|Operation    |Create    |
|Campaign ID*    |Enter the exact same campaign ID that you entered in the campaign entity row.     |
|Ad Group ID*    |[Leave blank when creating a bidding adjustment]    |
|Portfolio ID*    |[Leave blank when creating a bidding adjustment]    |
|Ad ID*    |[Leave blank when creating a bidding adjustment]    |
|Keyword ID*    |[Leave blank when creating a bidding adjustment]    |
|Product Targeting ID*    |[Leave blank when creating a bidding adjustment]    |
|Campaign Name    |[Leave blank when creating a bidding adjustment]    |
|Ad Group Name    |[Leave blank when creating a bidding adjustment]    |
|Start Date*    |[Leave blank when creating a bidding adjustment]    |
|End Date    |[Leave blank when creating a bidding adjustment]    |
|Targeting Type*    |[Leave blank when creating a bidding adjustment]    |
|State    |[Leave blank--the bidding adjustment entity is the only one that does not need a value in this field]    |
|Daily Budget    |[Leave blank when creating a bidding adjustment]    |
|sku*    |[Leave blank when creating a bidding adjustment]    |
|asin*    |[Leave blank when creating a bidding adjustment]    |
|Ad Group Default Bid    |[Leave blank when creating a bidding adjustment]    |
|Bid    |[Leave blank when creating a bidding adjustment]    |
|Keyword Text*    |[Leave blank when creating a bidding adjustment]    |
|Match Type*    |[Leave blank when creating a bidding adjustment]    |
|Bidding Strategy    |Leave blank--the bidding strategy is defined in the <b>campaign</b> entity row.     |
|Placement*    |Enter either **placement top** or **placement product page** or **placement rest of search<sup>(BETA)</sup>**. These values are **not** case sensitive.    |
|Percentage    |Enter a figure using digits only, up to a maximum of 900. For example, a 35% increase should be entered as **35**. This will define the percentage increase applied to your base bid, up to 900% max. [Learn more about adjusting bids by placement](https://advertising.amazon.com/help#GYYZVM7LGSRYGWV5)    |
|Product Targeting Expression*    |[Leave blank when creating a bidding adjustment]    |

After adding the bidding adjustment, your template would look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Spring toys 2024    |    |    |    |    |    |Spring toys 2024    |    |20240401    |    |Auto    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |
|Sponsored Products    |Bidding adjustment    |Create    |Spring toys 2024    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |placement top    |35    |    |



####Step 3. Define the ad group entity

To create the ad group, required fields include Product, Entity, Operation, Campaign ID, Ad group ID, Ad group name, State, and Ad group default bid. 

In the next row of your bulksheets template, use the following guidelines to define the “Ad group” entity for this campaign:

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Ad group    |
|Operation    |Create    |
|Campaign ID*   |Enter the exact same text-based campaign ID that you entered in the "Campaign" entity row. This will ensure that the ad group becomes a "child" of the top-level campaign "parent" entity.    |
|Ad Group ID*    |For new ad groups, you should type a text-based name to identify the ad group. You will also type this exact same name into the “Entity Name” column for this row. <br> **NOTE:** When you upload your bulksheets file, the Ad Group ID will become the actual unique identifier associated with this ad group.    |
|Portfolio ID*    |[Leave blank when creating ad group entity]    |
|Ad ID*    |[Leave blank when creating ad group entity]    |
|Keyword ID*    |[Leave blank when creating ad group entity]    |
|Product Targeting ID*    |[Leave blank when creating ad group entity]    |
|Campaign Name    |[Leave blank when creating ad group entity]    |
|Ad Group Name    |Enter the same name that you entered in the "Ad Group ID" column    |
|Start Date*    |[Leave blank. The start date is set at the campaign level and applies to all child entities]    |
|End Date    |[Leave blank when creating ad group entity]    |
|Targeting Type*    |[Leave blank when creating ad group entity]    |
|State    |Enter **enabled** to create the ad group.    |
|Daily Budget    |[Leave blank when creating ad group entity]    |
|sku*    |[Leave blank when creating ad group entity]    |
|asin*    |[Leave blank when creating ad group entity]    |
|Ad Group Default Bid    |Enter an exact figure with no money symbols, and use a decimal point (not a comma).  For example, if you want to set this to 75 cents, enter **0.75** <br> **NOTE:** This default bid amount will apply to all child entities of this ad group if you don't define bids for the child entities explicitly. For instance, if a keyword child entity doesn't include a defined bid, this default amount would apply.    |
|Bid    |[Leave blank. This field is for keyword bidding]    |
|Keyword Text*    |[Leave blank. This field only applies to the *keyword* entity]    |
|Match Type*    |[Leave blank when creating ad group entity]    |
|Bidding Strategy    |[Leave blank when creating ad group entity]    |
|Placement*    |[Leave blank when creating ad group entity]    |
|Percentage    |[Leave blank when creating ad group entity]    |
|Product Targeting Expression*    |[Leave blank when creating ad group entity. Only used for manual campaign **product targeting** entity]    |

After you add the “Ad group” entity into the bulksheets template, it might now look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Spring toys 2024    |    |    |    |    |    |Spring toys 2024    |    |20240401    |    |Auto    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |
|Sponsored Products    |bidding adjustment    |Create    |Spring toys 2024    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |Fixed bid    |placement top    |35    |    |
|Sponsored Products    |Ad group    |Create    |Spring toys 2024    |Outdoors    |    |    |    |    |    |Outdoors    |    |    |    |Enabled    |    |    |    |0.75    |    |    |    |    |    |    |    |

<br>

####Step 4. Define the product ad

Next, you’ll define the “Product ad” entity in the row underneath your “Ad group” entity. Required fields include Product, Entity, Operation, Campaign ID, Ad group ID, State, and sku or asin. 

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Product ad    |
|Operation    |Create    |
|Campaign ID*    |Enter the exact same campaign name that you entered in the "Campaign" entity row    |
|Ad Group ID*    |Enter the exact same ad group name that you entered in the "Ad group ID" row. (You need Ad Group ID for this entity because the Product ad is a child of the Ad group)    |
|Portfolio ID*    |[Leave blank when creating product ad entity]    |
|Ad ID*    |[Leave blank. This field will only be used for updating or archiving an existing product ad]    |
|Keyword ID*    |[Leave blank when creating product ad entity]    |
|Product Targeting ID    |[Leave blank when creating product ad entity]    |
|Campaign Name    |[Leave blank when creating product ad entity]    |
|Ad Group Name    |[Leave blank when creating product ad entity]    |
|Start Date*    |[Leave blank when creating product ad entity]    |
|End Date    |[Leave blank when creating product ad entity]    |
|Targeting Type*    |[Leave blank when creating product ad entity]    |
|State    |Enter **enabled**    |
|Daily Budget    |[Leave blank when creating product ad entity]    |
|sku*    |For sellers, you should enter the product sku. For vendors, leave the sku field blank    |
|asin*    |For vendors, you should enter the product asin. For sellers, leave the asin field blank. <br><br> <b>NOTE</b>: You can see asin eligibility statuses, along with reasons for ineligibility, when you download a Sponsored Products bulksheets file. This information is available in two informational-only columns: <b>Eligibility Status</b> and <b>Reason for Ineligibility</b>.    |
|Ad Group Default Bid    |[Leave blank when creating product ad entity]    |
|Bid    |[Leave blank when creating product ad entity]    |
|Keyword Text*    |[Leave blank when creating product ad entity]    |
|Match Type*    |[Leave blank when creating product ad entity]    |
|Bidding Strategy    |[Leave blank when creating product ad entity]    |
|Placement*    |[Leave blank when creating product ad entity]    |
|Percentage    |[Leave blank when creating product ad entity]    |
|Product Targeting Expression*    |[Leave blank when creating product ad entity]    |

After you add the product ad entity data, your bulksheets file might now look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Spring toys 2024    |    |    |    |    |    |Spring toys 2024    |    |20240401    |    |Auto    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |
|Sponsored Products    |bidding adjustment    |Create    |Spring toys 2024    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |Fixed bid    |placement top    |35    |    |
|Sponsored Products    |Ad group    |Create    |Spring toys 2024    |Outdoors    |    |    |    |    |    |Outdoors    |    |    |    |Enabled    |    |    |    |0.75    |    |    |    |    |    |    |    |
|Sponsored Products    |Product ad    |Create    |Spring toys 2024    |Outdoors    |    |    |    |    |    |    |    |    |    |Enabled    |    |BR549    |    |    |    |    |    |    |    |    |    |

>[NOTE] You can create four **product targeting** entities in auto-targeting campaigns if you want to define the bid amounts at the targeting level. Otherwise, Amazon will automatically create these rows, and the bid amount will match the ad group default bid. The four product targets will each have one of these four **targeting expression** values: close-match, loose-match, substitutes, complements. If you do include these four rows, you can customize the bid for each product target you want to bid on—and you can set the “state” to **paused** for any targeting expressions you do not want to bid on. <br> <br>The optional step below shows how to create these four product targeting entities. 

#### (Optional) Step 5. Define the product targeting. 
Note that you will **repeat this step four times to create each of the four product targeting entities**. If you do not create all four entities, Amazon will automatically create them, and the ad group default bid will apply. Fields with an asterisk ( * ) cannot be edited or updated fter the campaign is created.

|[grid]Column header |Guidelines on what to enter |
|--- |--- |
|Product |Sponsored Products |
|Entity |Product targeting |
|Operation |Create |
|Campaign Id* |Enter the exact same campaign name that you entered in the "Campaign" entity row |
|Ad Group Id* |Enter the exact same ad group name that you entered in the "Ad group ID" row. (You need Ad Group ID for this entity because Product targeting is a child of the Ad group) |
|Portfolio Id |[Leave blank when creating product targeting entity] |
|Ad ID |[Leave blank when creating product targeting entity] |
|Keyword ID |[Leave blank when creating product targeting entity] |
|Product Targeting ID |[Leave blank when creating product targeting entity] |
|Campaign Name |[Leave blank when creating product targeting entity] |
|Ad Group Name |[Leave blank when creating product targeting entity] |
|Start Date* |[Leave blank when creating product targeting entity] |
|End Date |[Leave blank when creating product targeting entity] |
|Targeting Type* |[Leave blank when creating product targeting entity] |
|State |Enter enabled to create and bid on the product target. Enter paused if you don't want to bid on the product target. |
|Daily Budget |[Leave blank when creating product targeting entity] |
|SKU* |[Leave blank when creating product targeting entity] |
|ASIN* |[Leave blank when creating product targeting entity] |
|Ad Group Default Bid |[Leave blank when creating product targeting entity] |
|Bid |Enter a value if you want to bid on the product targeting entity. Otherwise, the ad group default bid will apply. If you are setting the "State" to be <b>paused</b>, you can leave this field blank. |
|Keyword Text* |[Leave blank when creating product targeting entity] |
|Match Type* |[Leave blank when creating product targeting entity] |
|Bidding Strategy |[Leave blank when creating product targeting entity] |
|Placement* |[Leave blank when creating product targeting entity] |
|Percentage |[Leave blank when creating product targeting entity] |
|Product Targeting Expression* |Enter a value—you'll need one row for each: close-match, loose-match, substitutes, complements |

After adding the four product targeting rows, your file might look like this—note that we opted to pause two of these entities because we don’t want to bid on those targets:

|Product |Entity |Operation |Campaign Id |Ad Group Id |Portfolio Id |Ad Id |Keyword Id |Product Targeting Id |Campaign Name |Ad Group Name |Start Date |End Date |Targeting Type |State |Daily Budget |SKU |ASIN |Ad Group Default Bid |Bid |Keyword Text |Match Type |Bidding Strategy |Placement |Percentage |Product Targeting Expression |
|--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |
|Sponsored Products |Campaign |Create |Spring toys 2024 | | | | | |Spring toys 2024 | |20240401 | |Auto |Enabled |100 | | | | | | |Fixed bid | | | |
|Sponsored Products |bidding adjustment |Create |Spring toys 2024 | | | | | | | | | | | | | | | | | | |Fixed bid |placement top |35 | |
|Sponsored Products |Ad group |Create |Spring toys 2024 |Outdoors | | | | | |Outdoors | | | |Enabled | | | |0.75 | | | | | | | |
|Sponsored Products |Product ad |Create |Spring toys 2024 |Outdoors | | | | | | | | | |Enabled | |BR549 | | | | | | | | | |
|Sponsored Products |**Product targeting** |Create |Spring toys 2024 |Outdoors | | | | | | | | | |**Enabled** | | | | |0.5 | | | | | |close-match |
|Sponsored Products |**Product targeting** |Create |Spring toys 2024 |Outdoors | | | | | | | | | |**Enabled** | | | | |1 | | | | | |loose-match |
|Sponsored Products |**Product targeting** |Create |Spring toys 2024 |Outdoors | | | | | | | | | |**Paused** | | | | | | | | | | |substitutes |
|Sponsored Products |**Product targeting** |Create |Spring toys 2024 |Outdoors | | | | | | | | | |**Paused** | | | | | | | | | | |complements |


**Next, you can either [upload the file](#upload-your-bulksheets-file-to-create-the-new-campaign) if you’re ready to create the campaign, or [add more entities](#optional\\:-add-more-entities-to-the-campaign).**



### Create manual targeting campaign

| [IMAGE] |   |
| ---- | ---- |
| The following steps will guide you through creating a new Sponsored Products campaign using manual targeting. <br> <br>Relevant **entities** (Column B) are shown in the hierarchical diagram on the right—the green entities are mandatory fields for manual campaigns. Green outlines are entities required depending on targeting strategy. | ![SP entity diagram for manual campaign](/_images/bulksheets/2-0-images/sp-manual-entities.png) |

>[TIP: Tips] <br>- Select **keyword targeting** when you know the search terms customers use to find products like yours. Select **product targeting** to help shoppers find your product when browsing similar product detail pages and categories. <br> - Headers marked with an asterisk (*) are fields that cannot be updated after you create the campaign. For many of these fields, you can archive them instead and then create a new entity. [Learn more about updating campaigns](bulksheets/2-0/campaign-update-overview)

<br>Each “step” section below represents one row in your bulksheets file. The guidelines describe what you should enter into each field.

####Step 1. Define the campaign entity 

To create the top-level (parent) campaign entity, required fields include Product, Entity, Operation, Campaign ID, Campaign name, Start date, Targeting type, State, and Daily budget. 

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Campaign    |
|Operation    |Create    |
|Campaign ID*    |For new campaigns, you should type a text-based name to identify your campaign. You will enter this exact Campaign ID for each entity you create underneath this parent campaign. NOTE: When you upload your bulksheets file, the Campaign ID will become the actual unique identifier associated with the campaign and all of its "child" entities. So if you want to update this campaign later, you would enter the actual Campaign ID into this field.    |
|Ad Group ID*    |[Leave blank when creating campaign entity]    |
|Portfolio Id    |If you want to put the new campaign into a portfolio, enter the portfolio ID. Otherwise, leave blank.    |
|Ad ID*    |[Leave blank when creating campaign entity]    |
|Keyword ID*    |[Leave blank. This field will be useful when you want to archive or pause keywords]    |
|Product Targeting ID*    |[Leave blank when creating campaign entity]    |
|Campaign Name    |Enter the same campaign name that you entered in the "Campaign ID" column.    |
|Ad Group Name    |[Leave blank when creating campaign entity]    |
|Start Date*    |Format must be yyyyMMdd. For example, the date April 1, 2024 would be 20240401. NOTE: You can start on today's date or a future date, but the date cannot be in the past. So if you set the date for today, be sure you create and upload the file on today's date.    |
|End Date    |You can leave this field blank if you don't want to set an end date. Otherwise, the format must be yyyyMMdd.    |
|Targeting Type*    |Enter **manual**    |
|State    |Enter **enabled**    |
|Daily Budget    |Do not include symbols such as dollar signs. For example, a daily budget of $100 would be written as 100. If the figure isn't a whole number, use a decimal point and not a comma. Be sure your [budget is within the allowed limits for your region](concepts/limits#budget-constraints-by-marketplace).    |
|sku*    |[Leave blank when creating campaign entity]    |
|asin*    |[Leave blank when creating campaign entity]    |
|Ad Group Default Bid    |[Leave blank when creating campaign entity]    |
|Bid    |[Leave blank when creating campaign entity]    |
|Keyword Text*    |[Leave blank when creating campaign entity]    |
|Match Type*    |[Leave blank. This field only applies to the **keyword** entity]    |
|Bidding Strategy    |Similar to what you'd see in the UI, you can choose a bidding strategy: "Dynamic bids - down only", "Dynamic bids - up and down", or "Fixed bid".    |
|Placement*    |[Leave blank. To define the placement, create a **bidding adjustment** entity in this campaign]    |
|Percentage    |[Leave blank. The percentage adjustment is defined with the **bidding adjustment** entity]    |
|Product Targeting Expression*    |[Leave blank when creating campaign entity. Only used for manual campaign product targeting entity]    |

After this step, your bulksheets file would look like this—note that “targeting type” is now set to **manual**:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Manual SP Shoes    |    |    |    |    |    |Manual SP Shoes    |    |20240401    |    |Manual    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |

<br>
####Step 2. Define the ad group entity

To create the ad group, required fields include Product, Entity, Operation, Campaign ID, Ad group ID, Ad group name, State, and Ad group default bid. 

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Ad group    |
|Operation    |Create    |
|Campaign ID*    |Enter the exact same text-based campaign ID that you entered in the "Campaign" entity row. This will ensure that the ad group becomes a "child" of the top-level campaign "parent" entity.    |
|Ad Group ID*    |For new ad groups, you should type a text-based name to identify the ad group. You will also type this exact same name into the “Entity Name” column for this row. <br> NOTE: When you upload your bulksheets file, the Ad Group ID will become the actual unique identifier associated with this ad group.    |
|Portfolio ID*    |[Leave blank when creating ad group entity]    |
|Ad ID*    |[Leave blank when creating ad group entity]    |
|Keyword ID*    |[Leave blank when creating ad group entity]    |
|Product Targeting ID*    |[Leave blank when creating ad group entity]    |
|Campaign Name    |[Leave blank when creating ad group entity]    |
|Ad Group Name    |Enter the same name that you entered in the "Ad Group ID" column    |
|Start Date*    |[Leave blank. The start date is set at the campaign level and applies to all child entities]    |
|End Date    |[Leave blank when creating ad group entity]    |
|Targeting Type*    |[Leave blank when creating ad group entity]    |
|State    |Enter*enabled* to create the ad group.    |
|Daily Budget    |[Leave blank when creating ad group entity]    |
|sku*    |[Leave blank when creating ad group entity]    |
|asin*    |[Leave blank when creating ad group entity]    |
|Ad Group Default Bid    |Enter an exact figure with no money symbols, and use a decimal point (not a comma).  For example, if you want to set this to 75 cents, enter **0.75** <br>**NOTE:** This default bid amount will apply to all child entities of this ad group if you don't define bids for the child entities explicitly. For instance, if a keyword child entity doesn't include a defined bid, this default amount would apply.    |
|Bid    |[Leave blank when creating ad group entity]    |
|Keyword Text*    |[Leave blank when creating ad group entity]    |
|Match Type*    |[Leave blank when creating ad group entity]    |
|Bidding Strategy    |[Leave blank when creating ad group entity]    |
|Placement*    |[Leave blank when creating ad group entity]    |
|Percentage    |[Leave blank when creating ad group entity]    |
|Product Targeting Expression*    |[Leave blank when creating ad group entity. Only used for manual campaign product targeting entity]    |

After adding the *ad group* entity, your bulksheets file would look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Manual SP Shoes    |    |    |    |    |    |Manual SP Shoes    |    |20240401    |    |Manual    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |
|Sponsored Products    |Ad group    |Create    |Manual SP Shoes    |Running    |    |    |    |    |    |Running    |    |    |    |Enabled    |    |    |    |0.75    |    |    |    |    |    |    |    |

<br>
####Step 3. Define the product ad entity

Required fields include Product, Entity, Operation, Campaign ID, Ad group ID, State, and sku or asin. 

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Product ad    |
|Operation    |Create    |
|Campaign ID*    |Enter the exact same campaign name that you entered in the "Campaign" entity row    |
|Ad Group ID*    |Enter the exact same ad group name that you entered in the "Ad group ID" row. (You need Ad Group ID for this entity because the Product ad is a child of the Ad group)    |
|Portfolio ID*    |[Leave blank when creating product ad entity]    |
|Ad ID*    |[Leave blank when creating product ad entity]    |
|Keyword ID*    |[Leave blank when creating product ad entity]    |
|Product Targeting ID    |[Leave blank when creating product ad entity]    |
|Campaign Name    |[Leave blank when creating product ad entity]    |
|Ad Group Name    |[Leave blank when creating product ad entity]    |
|Start Date*    |[Leave blank when creating product ad entity]    |
|End Date    |[Leave blank when creating product ad entity]    |
|Targeting Type*    |[Leave blank when creating product ad entity]    |
|State    |Enter **enabled**    |
|Daily Budget    |[Leave blank when creating product ad entity]    |
|sku*    |For sellers, you should enter the product sku. For vendors, leave the sku field blank    |
|asin*    |For vendors, you should enter the product asin. For sellers, leave the asin field blank    |
|Ad Group Default Bid    |[Leave blank when creating product ad entity]    |
|Bid    |[Leave blank when creating product ad entity]    |
|Keyword Text*    |[Leave blank when creating product ad entity]    |
|Match Type*    |[Leave blank when creating product ad entity]    |
|Bidding Strategy    |[Leave blank when creating product ad entity]    |
|Placement*    |[Leave blank when creating product ad entity]    |
|Percentage    |[Leave blank when creating product ad entity]    |
|Product Targeting Expression*    |[Leave blank when creating product ad entity]    |

At this point, your file would look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Manual SP Shoes    |    |    |    |    |    |Manual SP Shoes    |    |20240401    |    |Manual    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |
|Sponsored Products    |Ad group    |Create    |Manual SP Shoes    |Running    |    |    |    |    |    |Running    |    |    |    |Enabled    |    |    |    |0.75    |    |    |    |    |    |    |    |
|Sponsored Products    |Product ad    |Create    |Manual SP Shoes    |Running    |    |    |    |    |    |    |    |    |    |Enabled    |    |BR549    |    |    |    |    |    |    |    |    |    |

<br>
####Step 4a. Create the keyword targeting entity

Create this entity only if you want a keyword targeting strategy.  For product targeting, [jump to Step 4b](bulksheets/2-0/create-sp-campaign#step-4b-create-the-product-targeting-entity). Required fields include Product, Entity, Operation, Campaign ID, Ad group ID, State, Keyword text, and Match type.

|[grid]Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Keyword    |
|Operation    |Create    |
|Campaign ID*    |Enter the exact same campaign name that you entered in the "Campaign" entity row    |
|Ad Group ID*    |Enter the exact same ad group name that you entered in the "Ad group ID" row. (You need Ad Group ID for this entity because the Product ad is a child of the Ad group)    |
|Portfolio ID*    |[Leave blank when creating keyword entity]    |
|Ad ID*    |[Leave blank when creating keyword entity]    |
|Keyword ID*    |[Leave blank when creating the keyword. This ID will be used for [archiving or pausing existing keywords](http://bulksheets/2-0/campaign-update-overview.md).]    |
|Product Targeting ID*    |[Leave blank when creating keyword entity]    |
|Campaign Name    |[Leave blank when creating keyword entity]    |
|Ad Group Name    |[Leave blank when creating keyword entity]    |
|Start Date*    |[Leave blank when creating keyword entity]    |
|End Date    |[Leave blank when creating keyword entity]    |
|Targeting Type*    |[Leave blank when creating keyword entity]    |
|State    |Enter **enabled** when creating a new keyword. To pause keywords, set the "Operation" to **update**, and enter **paused** in this field. To archive keywords, set the "Operation" to **archive** and leave this field blank.     |
|Daily Budget    |[Leave blank when creating keyword entity]    |
|sku*    |[Leave blank when creating keyword entity]    |
|asin*    |[Leave blank when creating keyword entity]    |
|Ad Group Default Bid    |[Leave blank when creating keyword entity]    |
|Bid    |To set a keyword-level bid, enter the CPC amount you are willing to pay when a shopper clicks your ad. Be sure your [bid is within the limits for your region](concepts/limits#bid-constraints-by-marketplace). Do not use money symbols or commas. For example, to bid 75 cents, enter **0.75**. To bid one dollar, enter **1**. Leave this field blank if you want the keyword to inherit the ad group default bid.     |
|Keyword Text*    |Enter the keyword you want to create. To create multiple keywords, add a new row for each keyword.    |
|Match Type*    |Options are **exact**, **broad**, or **phrase**   |
|Bidding Strategy    |[Leave blank when creating keyword entity]    |
|Placement*    |[Leave blank when creating keyword entity]    |
|Percentage    |[Leave blank when creating keyword entity]    |
|Product Targeting Expression*    |[Leave blank when creating keyword entity]    |

To create additional keywords, add a new row for each keyword. If you add three keywords, your bulksheets file might now look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Manual SP Shoes    |    |    |    |    |    |Manual SP Shoes    |    |20240401    |    |Manual    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |
|Sponsored Products    |Ad group    |Create    |Manual SP Shoes    |Running    |    |    |    |    |    |Running    |    |    |    |Enabled    |    |    |    |0.75    |    |    |    |    |    |    |    |
|Sponsored Products    |Product ad    |Create    |Manual SP Shoes    |Running    |    |    |    |    |    |    |    |    |    |Enabled    |    |BR549    |    |    |    |    |    |    |    |    |    |
|Sponsored Products    |Keyword    |Create    |Manual SP shoes    |Running    |    |    |    |    |    |    |    |    |    |Enabled    |    |    |    |    |1    |shoes    |exact    |    |    |    |    |
|Sponsored Products    |Keyword    |Create    |Manual SP shoes    |Running    |    |    |    |    |    |    |    |    |    |Enabled    |    |    |    |    |1    |running shoes    |phrase    |    |    |    |    |
|Sponsored Products    |Keyword    |Create    |Manual SP shoes    |Running    |    |    |    |    |    |    |    |    |    |Enabled    |    |    |    |    |1    |women's shoes    |broad    |    |    |    |    |

<br>
####Step 4b. Create the product targeting entity

Create this entity only if you want a product targeting strategy. Required fields include Product, Entity, Operation, Campaign ID, Ad group ID, State, Bid, and Product targeting expression.

|Column header    |Guidelines on what to enter    |
|---    |---    |
|Product    |Sponsored Products    |
|Entity    |Product targeting    |
|Operation    |Create    |
|Campaign ID*    |Enter the exact same campaign name that you entered in the "Campaign" entity row    |
|Ad Group ID*    |Enter the exact same ad group name that you entered in the "Ad group ID" row. (You need Ad Group ID for this entity because the Product targeting is a child of the Ad group)    |
|Portfolio ID*    |[Leave blank when creating product targeting entity]    |
|Ad ID*    |[Leave blank when creating product targeting entity]    |
|Keyword ID*    |[Leave blank when creating product targeting entity]    |
|Product Targeting ID*    |[Leave blank when creating product targeting entity]    |
|Campaign Name    |[Leave blank when creating product targeting entity]    |
|Ad Group Name    |[Leave blank when creating product targeting entity]    |
|Start Date*    |[Leave blank when creating product targeting entity]    |
|End Date    |[Leave blank when creating product targeting entity]    |
|Targeting Type*    |[Leave blank when creating product targeting entity]    |
|State    |Enter **enabled**    |
|Daily Budget    |[Leave blank when creating product targeting entity]    |
|sku*    |[Leave blank when creating product targeting entity]    |
|asin*    |[Leave blank when creating product targeting entity]    |
|Ad Group Default Bid    |[Leave blank when creating product targeting entity]    |
|Bid    |Enter the CPC amount you are willing to pay when a shopper clicks your ad. Be sure your bid is [within the limits for your region](concepts/limits#bid-constraints-by-marketplace). Do not use money symbols or commas. For example, to bid 75 cents, enter **0.75**. To bid one dollar, enter **1**    |
|Keyword Text*    |[Leave blank when creating product targeting entity]    |
|Match Type*    |[Leave blank when creating product targeting entity]    |
|Bidding Strategy    |[Leave blank when creating product targeting entity]    |
|Placement*    |[Leave blank when creating product targeting entity]    |
|Percentage    |[Leave blank when creating product targeting entity]    |
|Product Targeting Expression*    |In the UI, this is where you select either "individual products" or "categories" to target. For "individual products," use the asin and enter **asin="A12345"** (using the actual asin ID). For expanded product targeting, enter **asin-expanded="A12345"** (using the actual asin ID).<br><br>For "categories," use the category ID along with other qualifiers such as price limit and star rating. For this field, enter all qualifiers into the same cell of your bulksheets file because they work as a combined unit to define the targeting. <br><br>Example 1. To target by product category, price, and star rating: **category="5524098011" price<49.99 rating>3**<br><br>Example 2. To target by product category and Prime eligibility: **category="5524098011" prime-shipping-eligible="true"**<br><br>**TIP:** To get the numeric values for attributes such as category and age range, you can use past campaign data where you assigned those targeting expressions by exporting the campaign into a bulksheet. Look in the <b>Resolved Targeting Expression (Informational Only) field to see the corresponding category.</b> <br><br>Learn more about targeting expression rules, and [see more sample expressions](https://advertising.amazon.com/API/docs/en-us/bulksheets/sp/sp-general-info/sp-product-attribute-targeting#P).	|

Now, your bulksheets file might look like this if you add the product targeting expression entity details shown above:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |sku    |asin    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products    |Campaign    |Create    |Manual SP Shoes    |    |    |    |    |    |Manual SP Shoes    |    |20240401    |    |Manual    |Enabled    |100    |    |    |    |    |    |    |Fixed bid    |    |    |    |
|Sponsored Products    |Ad group    |Create    |Manual SP Shoes    |Running    |    |    |    |    |    |Running    |    |    |    |Enabled    |    |    |    |0.75    |    |    |    |    |    |    |    |
|Sponsored Products    |Product ad    |Create    |Manual SP Shoes    |Running    |    |    |    |    |    |    |    |    |    |Enabled    |    |BR549    |    |    |    |    |    |    |    |    |    |
|Sponsored Products    |Product targeting    |Create    |Manual SP shoes    |Running    |    |    |    |    |    |    |    |    |    |Enabled    |    |    |    |    |    |    |    |    |    |    |category="20394" price<19.99    |

<br>
### Upload your bulksheets file to create the new campaign

At this point, all mandatory entities are included in this Sponsored Products campaign. You can still add more entities if you want to assign negative keyword or product targets.
If you’re ready to create the campaign, you should save the file and then navigate back to the “Bulk operations” page. 

| [IMAGE] |   |
| ---- | ---- |
| Under “3. Upload your file to update your campaigns,” click **Choose file** to upload the sheet you just created.  | ![Upload your file](/_images/bulksheets/2-0-images/sp-3-upload-your-file.png "Upload your file") |

### Optional: Add more entities to the campaign

If you want to add negative product or keyword entities, enter each one into a new row using the process described above. Always include **Sponsored Products** in the “Product” column for each row, *create* as the “Operation” for new campaigns, the **campaign ID**, and set the “State” to **enabled**. For reference, these are the additional fields you need to complete for the remaining entities:

* **Campaign negative keyword** entity fields: Campaign ID, State, Keyword text, Match type (**negativeExact** or **negativePhrase**)
* **Negative keyword** entity fields: Campaign ID, Ad group ID, State, Keyword text, Match type (**negativeExact** or **negativePhrase**)
* **Negative product targeting** entity fields: Campaign ID, Ad group ID, State, Product targeting expression