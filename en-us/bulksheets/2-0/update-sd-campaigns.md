---
title: How to update Sponsored Display campaigns with bulksheets
description: A guide explaining how to update existing Sponsored Display campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Display
    - Campaign management
keywords:
    - bulksheets  
    - update campaigns
---

# How to upate Sponsored Display campaigns

← [Overview - update campaigns with bulksheets](bulksheets/2-0/campaign-update-overview)

This article explains how to update Sponsored Display campaigns using bulksheets, including instructions for updating the following entities:

* Update campaign name
* Update campaign end date* and budget
* Archive a campaign
* Update an ad group name and default bid
* Archive an ad group
* Add a new ad group to an existing campaign 

If you want a more general explanation of how to use bulksheets or how to create new campaigns, you can review [Get Started with Bulksheets Part I](bulksheets/2-0/get-started-with-bulksheets-part1). 

**Some tips to keep in mind:**

* Before you edit a bulksheets file, you should first [download a file from the “Bulk operations” page](bulksheets/2-0/understand-bulksheets-data). 
* To update campaigns, ad groups, or keywords, you’ll need the entity IDs, which can be found in the “Sponsored Display” tab of the downloaded file. 
* In the file, you can delete any tabs that don’t pertain to the campaign you’re updating, if they weren’t excluded from the download.
* Only certain fields can be updated after a campaign has been created. Learn more about [which fields you can and cannot change, along with guidance and tips](bulksheets/2-0/editable-non-editable-bulksheets-fields).

>[NOTE]The examples below show guidelines for updating various entities. For simplicity, these examples use a blank template and do not contain the additional fields you will typically see when you download a custom spreadsheet with past campaign data. To understand how to start with a downloaded file that already contains data, review the [overview of updating campaigns with bulksheets](bulksheets/2-0/campaign-update-overview).

## Update the campaign entity

For all updates to the campaign entity, the first three fields of your bulksheets file will be the same: Enter **Sponsored Display** in Column A (Product), **Campaign** in Column B (Entity), and **Update** in Column C (Operation). Then, use the guidelines below to complete the bulksheets file based on what you want to update. 

#### Update campaign name

1. Enter the campaign ID in the “Campaign ID” column 
2. In the “Campaign Name” column, enter the **new name** you want the campaign changed to. When you upload the file, the campaign’s child entities will inherit the new name, so you do not need to update the campaign name in other entity rows. 
3. In the “End Date” column, enter a date if you want to specify when the campaign should end. Otherwise, you can leave this blank and the campaign will run indefinitely. Note that leaving this field blank will override any previously set end dates—so if the campaign currently has a defined end date, leaving this field blank will remove the end date. 
4. In the “State” column, enter **Enabled** if you want the updates to take effect immediately after you upload the file

If you only want to update the campaign name, you can leave all other fields blank. If you are only updating a single campaign name, your file would look like this:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Ad Id    |Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |State    |Tactic    |Budget Type    |Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Bid Optimization    |Cost Type    |Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Display	|Campaign	|Update	|**1009384**	|	|	|	|**NEW campaign name**	|	|	|	|**Enabled**	|	|	|	|	|	|	|	|	|	|	|

Let’s say you have a different campaign, and you want to change the end date, daily budget, and bidding strategy for that campaign. 

#### Update campaign end date and budget

1. Enter the campaign ID in the “Campaign ID” column
2. In the “End Date” column, enter the date when the campaign should end, using the format yyyyMMdd
3. In the “Budget” column, enter the new amount using numbers only
4. In the “State” column, enter **enabled** if you want the updates to take effect immediately after you upload the file

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Ad Id    |Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |State    |Tactic    |Budget Type    |Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Bid Optimization    |Cost Type    |Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Display	|Campaign	|Update	|**9283347**	|	|	|	|	|	|	|**20220331**	|**Enabled**	|	|	|**100**	|	|	|	|	|	|	|	|

#### Archive a campaign 

>[TIP]When you archive a parent entity, all child entities will also be archived. You should avoid archiving both a parent and its child entity, or you’ll get an error indicating that the child entity failed to archive. The error will not cause any issues because the child will have been archived, but if you only archive the parent, you can avoid seeing this error.

You can update the status of a campaign to be **archived** using the “Operation” field—and note that this same process can be applied to any entity you want to archive (ad group, keyword, and so on). Just be sure to always include the entity ID:

1. In the “Operation” field, enter **archived**
2. Enter the campaign ID in the “Campaign ID” column

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Ad Id    |Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |State    |Tactic    |Budget Type    |Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Bid Optimization    |Cost Type    |Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Display	|Campaign	|**Archive**	|**738572948**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

## Update the ad group entity

For all updates to the ad group entity, the first three fields of your bulksheets file will be the same: Enter **Sponsored Display** in Column A (Product) and **Ad group** in Column B (Entity).

#### Update ad group name

1. In the “Operation” field, enter **update**
2. Enter the Campaign ID and Ad group ID in their respective column fields
3. In the “Ad Group Name” column, enter the new name you want the ad group changed to. When you upload the file, the ad group’s child entities will inherit the new name, so you do not need to update the ad group name in other entity rows. 
4. In the “State” column, enter **Enabled** if you want the updates to take effect immediately after you upload the file

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Ad Id    |Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |State    |Tactic    |Budget Type    |Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Bid Optimization    |Cost Type    |Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Display	|Ad group	|**Update**	|**88910384**	|**3920483**	|	|	|	|**NEW ad group name**	|	|	|**Enabled**	|	|	|	|	|	|	|	|	|	|	|

#### Update ad group name and default bid

If you only want to update the ad group name, the example above could be uploaded to change the name. But let’s say you want to also change the default bid for that same ad group--you could do this in the same row with the new ad group name by also  including a value in the “Ad Group Default Bid” field:

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Ad Id    |Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |State    |Tactic    |Budget Type    |Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Bid Optimization    |Cost Type    |Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Display	|Ad group	|**Update**	|**88910384**	|**3920483**	|	|	|	|**NEW ad group name**	|	|	|**Enabled**	|	|	|	|	|	|****15****	|	|	|	|	|

#### Archive an ad group

You can change the status of an ad group to be **archived** using the “Operation” field:

1. In the “Operation” field, enter **archived**
2. In the “Ad Group ID” field, enter the ID

All other fields can be left unchanged or blank:

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|M	|N	|O	|P	|Q	|R	|S	|T	|U	|V	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Ad Id	|Targeting Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|State	|Tactic	|Budget Type	|Budget	|SKU	|ASIN	|Ad Group Default Bid	|Bid	|Bid Optimization	|Cost Type	|Targeting Expression	|
|Sponsored Display	|Ad group	|**Archived**	|	|**6827594**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

## Update campaigns and ad groups within the same bulksheets file

The examples above show each scenario separately. Here’s an example showing all of the updates in a single file, including

* Update campaign name
* Update campaign end date and budget
* Archive a campaign
* Update an ad group name and default bid
* Archive an ad group

|Product    |Entity    |Operation    |Campaign Id    |Ad Group Id    |Ad Id    |Targeting Id    |Campaign Name    |Ad Group Name    |Start Date    |End Date    |State    |Tactic    |Budget Type    |Budget    |SKU    |ASIN    |Ad Group Default Bid    |Bid    |Bid Optimization    |Cost Type    |Targeting Expression    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Display	|Campaign	|Update	|1009384	|	|	|	|NEW campaign name	|	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Display	|Campaign	|Update	|9283347	|	|	|	|	|	|	|20220331	|Enabled	|	|	|100	|	|	|	|	|	|	|	|
|Sponsored Display	|Campaign	|Archive	|738572948	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Display	|Ad group	|Update	|88910384	|3920483	|	|	|	|NEW ad group name	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Display	|Ad group	|Archive	|	|6827594	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

* * *
