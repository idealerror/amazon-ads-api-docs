---
title: How to update Sponsored Products campaigns with bulksheets
description: A guide explaining how to update existing Sponsored Products campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - bulksheets  
    - update campaigns
    - change campaign name
    - change ad group name
---

# How to update Sponsored Products campaigns

← [Overview - update campaigns with bulksheets](bulksheets/2-0/campaign-update-overview)

This article explains how to update Sponsored Products campaigns using bulksheets, including instructions for updating the following entities:

* Change campaign name
* Update campaign end date, daily budget, and bidding strategy
* Archive a campaign
* Change an ad group name and update the default bid
* Archive an ad group
* Add a new ad group to an existing campaign
* Pause a keyword
* Archive a keyword
* Add new keywords to an existing campaign

If you want a more general explanation of how to use bulksheets or how to create new campaigns, you can review [Get Started with Bulksheets Part I](bulksheets/2-0/get-started-with-bulksheets-part1). 

**Some tips to keep in mind:**

* Before you edit a bulksheets file, you should first [download a file from the “Bulk operations” page](bulksheets/2-0/understand-bulksheets-data). 
* To update campaigns, ad groups, or keywords, you’ll need the entity IDs, which can be found in the “Sponsored Products” tab of the downloaded file. **Note that bulksheets entity IDs are different than the IDs in the advertising console, so be sure you're getting the IDs from a bulksheets download, and not from the console.** 
* In the file, you can delete any tabs that don’t pertain to the campaign you’re updating, if they weren’t excluded from the download.
* Note that if an existing campaign is in a portfolio, you must include the Porfolio ID when you update the campaign. Otherwise, it will be removed from the portfolio when you upload the file.
* Only certain fields can be updated after a campaign has been created. Learn more about [which fields you can and cannot change, along with guidance and tips](bulksheets/2-0/editable-non-editable-bulksheets-fields).

>[NOTE]The examples below show guidelines for updating various entities. For simplicity, these examples use a blank template and do not contain the additional fields you will typically see when you download a custom spreadsheet with past campaign data. To understand how to start with a downloaded file that already contains data, review the [overview of updating campaigns with bulksheets](bulksheets/2-0/campaign-update-overview).

## Update the campaign entity


For all updates to the campaign entity, the first three fields of your bulksheets file will be the same: Enter **Sponsored Products** in Column A (Product), **Campaign** in Column B (Entity), and **Update** in Column C (Operation). Then, use the guidelines below to complete the bulksheets file based on what you want to update. 

#### Update campaign name

1. Enter the campaign ID in the “Campaign ID” column. This must be the campaign ID found in bulksheets--not the ID found in the advertising console. **Note that if you are using a downloaded bulksheet, this ID will already be populated, and you can leave it unchanged.**
2. In the “Campaign Name” column, enter the **new name** you want the campaign changed to. When you upload the file, the campaign’s child entities will inherit the new name, so you do not need to update the campaign name in other entity rows
3. In the “End Date” column, enter a date if you want to specify when the campaign should end. Otherwise, you can leave this blank and the campaign will run indefinitely. Note that leaving this field blank will override any previously set end dates—so if the campaign currently has a defined end date, leaving this field blank will remove the end date. 
4. In the “State” column, enter **Enabled** if you want the updates to take effect immediately after you upload the file

If you only want to update the campaign name, you can leave all other fields either unchanged or blank. If you are only updating a single campaign name, your file might look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Campaign	|Update	|**1009384**	|	|	|	|	|	|**NEW campaign name**	|	|	|	|	|**Enabled**	|	|	|	|	|	|	|	|	|	|	|	|

Let’s say you have a different campaign, and you want to change the end date, daily budget, and bidding strategy for that campaign. 

#### Update campaign end date, daily budget, and bidding strategy

1. Enter the campaign ID in the “Campaign ID” column
2. In the “End Date” column, enter the date when the campaign should end, using the format yyyyMMdd
3. In the “Daily Budget” column, enter the new amount using numbers only
4. In the “Bidding Strategy” column, enter the new bidding strategy you want to implement: either **Fixed bid**, **Dynamic bids - down only**, or **Dynamic bids - up and down** (we used **Fixed bid** in the example below)

All other fields can be left either unchanged or blank: 

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Campaign	|Update	|**9283347**	|	|	|	|	|	|	|	|	|**20220331**	|	|	|****200****	|	|	|	|	|	|	|**Fixed bid**	|	|	|	|

#### Archive a campaign 

>[TIP]When you archive a parent entity, all child entities will also be archived. You should avoid archiving both a parent and its child entity, or you’ll get an error indicating that the child entity failed to archive. The error will not cause any issues because the child will have been archived with the parent, but if you only archive the parent, you can avoid seeing this error.

You can update the status of a campaign to be **archived** using the “Operation” field—and note that this same process can be applied to any entity you want to archive (ad group, keyword, and so on). You only need to include the ID of the entity you want to archive:

1. In the “Operation” field, enter **archive**
2. Enter the campaign ID in the “Campaign ID” column

All other fields can be left unchanged or blank:


|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Campaign	|**Archive**	|**738572948**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
<br>

## Update the ad group entity

For all updates to the ad group entity, the first two fields of your bulksheets file will be the same: Enter **Sponsored Products** in Column A (Product) and **Ad group** in Column B (Entity). 

#### Change ad group name

1. In the “Operation field, enter **update**
2. Enter the campaign ID and ad group ID in their respective column fields
3. In the “Ad Group Name” column, enter the new name you want the ad group changed to. When you upload the file, the ad group’s child entities will inherit the new name, so you do not need to update the ad group name in other entity rows. 

All other fields can be left either unchanged or blank

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Ad group	|**Update**	|**88910384**	|**3920483**	|	|	|	|	|	|**NEW ad group name**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
<br>

#### Change ad group name and update default bid

If you only want to update the ad group name, the example above could be uploaded to change the name. But let’s say you want to also change the default bid for that same ad group--you could do this in the same row with the new ad group name by also  including a value in the “Ad Group Default Bid” field:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Ad group	|**Update**	|**88910384**	|**3920483**	|	|	|	|	|	|**NEW ad group name**	|	|	|	|	|	|	|	|****15****	|	|	|	|	|	|	|	|

#### Archive an ad group

You can change the status of an ad group to be **archived** using the “Operation” field:

1. In the “Operation” field, enter **archive**
2. In the “Ad Group ID” field, enter the ID

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Ad group	|**Archive**	|	|**6827594**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

#### Add a new ad group to an existing Sponsored Products campaign

You can add a new ad group to an existing Sponsored Products campaign if you include the campaign ID in the row where you are creating the new ad group:

1. In the “Entity” field, enter **ad group**
2. In the “Operation” field, enter **create**
3. In the “Campaign ID” field, enter the ID of the campaign that you want the ad group to be a part of 
4. In the “Ad Group ID” field, enter the name you want to give the ad group
5. In the “Ad Group Name” field, enter the same name you entered into the “Ad Group ID” field
6. In the “State” field, enter **enabled**
7. In the “Ad Group Default Bid” field, enter the amount you want to spend, using numbers only

Your sheet would resemble the example below:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|**Ad group**	|**Create**	|**9283745**	|**6827594**	|	|	|	|	|	|**New ad group name**	|	|	|	|**Enabled**	|	|	|	|**10**	|	|	|	|	|	|	|	|

## Update the keyword entity

You cannot update or change keywords by changing the keyword text. Instead, you should pause or archive the keywords that you want to stop running, and then create new ones in the campaign. 

#### Pause a keyword

You can pause keywords by updating its entity and setting the “State” to **paused**. You can pause as many keywords as you want in a single bulk upload.  

1. In the “Entity” field, enter **keyword**
2. In the “Operation field,” enter **update**
3. Enter the campaign ID, ad group ID, and keyword IDs in their respective fields
4. In the “State” field, enter **paused**

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|**Keyword**	|**Update**	|**9283745**	|**6827594**	|	|	|****38572938****	|	|	|	|	|	|	|**Paused**	|	|	|	|	|	|	|	|	|	|	|	|

#### Archive a keyword

To archive keywords, set the “Operation” field to **archive**. You can archive as many keywords as you want in a single bulk upload. 

1. In the “Entity” field, enter **keyword**
2. In the “Operation field,” enter **archive**
3. In the "Keyword ID" field, enter the keyword ID.

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|**Keyword**	|**Archive**	|	|	|	|	|**7957362**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

#### Add a new keyword to an existing campaign

You can add new keywords to existing campaigns using the campaign and ad group IDs. The example below shows adding a single new keyword, but you can add hundreds, or even thousands, of new keywords in batches. Be sure to use a separate row for each new keyword you want to create.  

1. In the “Entity” field, enter **keyword**
2. In the “Operation field,” enter **create**
3. Enter the campaign ID and ad group IDs in their respective fields
4. In the “State” field, enter **enabled**
5. Enter the keywords in the "Keyword Text" field

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|**Keyword**	|**Create**	|**9283745**	|**6827594**	|	|	|	|	|	|	|	|	|	|**Enabled**	|	|	|	|	|	|	**example keyword**|	|	|	|	|	|


## Update campaigns and ad groups within the same bulksheets file

The examples above show each scenario separately. Here’s an example showing all of the updates in a single file, including

* Update campaign name
* Update campaign end date, daily budget, and bidding strategy
* Archive a campaign
* Update an ad group name and default bid
* Archive an ad group
* Add a new ad group to an existing campaign
* Pause a keyword
* Archive a keyword

Here’s what this sheet would look like if other fields are left blank:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Portfolio Id    |Ad Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |Targeting Type    |State    |Daily Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Keyword Text    |Match Type    |Bidding Strategy    |Placement    |Percentage    |Product Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Products	|Campaign	|Update	|1009384	|	|	|	|	|	|NEW campaign name	|	|	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Products	|Campaign	|Update	|9283347	|	|	|	|	|	|	|	|	|20220331	|	|Enabled	|200	|	|	|	|	|	|	|Fixed bid	|	|	|	|
|Sponsored Products	|Campaign	|Archive	|738572948	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Products	|Ad group	|Update	|88910384	|3920483	|	|	|	|	|	|NEW ad group name	|	|	|	|Enabled	|	|	|	|15	|	|	|	|	|	|	|	|
|Sponsored Products	|Ad group	|Archive	|9283745	|6827594	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Products	|Ad group	|Create	|59385793	|New ad group	|	|	|	|	|	|New ad group	|	|	|	|Enabled	|	|	|	|10	|	|	|	|	|	|	|	|
|Sponsored Products	|Keyword	|Update	|9283745	|6827594	|	|	|38572938	|	|	|	|	|	|	|Paused	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Products	|Keyword	|Archive	|9283745	|6827594	|	|	|7957362	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

* * *
