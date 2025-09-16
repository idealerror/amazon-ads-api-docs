---
title: How to update Sponsored Brands campaigns with bulksheets
description: A guide explaining how to update existing Sponsored Brands campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands
    - Campaign management
keywords:
    - bulksheets  
    - update campaigns
---
# How to update Sponsored Brands campaigns

← [Overview - update campaigns with bulksheets](bulksheets/2-0/campaign-update-overview)

This article explains how to update Sponsored Brands campaigns using bulksheets, including instructions for updating the following entities:

* Update campaign name
* Update campaign end date* and budget
* Archive a campaign
* Pause or archive keywords
* Archive ASINs for product targeting

If you want a more general explanation of how to use bulksheets or how to create new campaigns, you can review [Get Started with Bulksheets Part I](bulksheets/2-0/get-started-with-bulksheets-part1). 

**Some tips to keep in mind:**

* Before you edit a bulksheets file, you should first [download a file from the “Bulk operations” page](bulksheets/2-0/understand-bulksheets-data). 
* To update campaigns, ad groups, or keywords, you’ll need the entity IDs, which can be found in the “Sponsored Brands” tab of the downloaded file. IDs for creative library assets will be in the "Brand Assets" tab.
* In the file, you can delete any tabs that don’t pertain to the campaign you’re updating, if they weren’t excluded from the download.
* Only certain fields can be updated after a campaign has been created. Learn more about [which fields you can and cannot change, along with guidance and tips](bulksheets/2-0/editable-non-editable-bulksheets-fields).
* When **archiving** an entity, the only ID required is the entity you want to archive. For example, to archive an ad group, you only need the ad group ID—you can leave campaign ID blank.

>[NOTE]The examples below show guidelines for updating various entities. For simplicity, these examples use a blank template and do not contain the additional fields you will typically see when you download a custom spreadsheet with past campaign data. To understand how to start with a downloaded file that already contains data, review the [overview of updating campaigns with bulksheets](bulksheets/2-0/campaign-update-overview).

## Update the campaign entity

For all updates to the campaign entity, the first three fields should always be **Sponsored Brands**, **Campaign**, and **Update**. In addition, you'll need to include the following values: 

#### Update campaign name

1. Enter the campaign ID in the “Campaign ID” column 
2. In the “Campaign Name” column, enter the **new name** you want the campaign changed to. When you upload the file, the campaign’s child entities will inherit the new name, so you do not need to update the campaign name in other entity rows. 

If you only want to update the campaign name, you can leave all other fields blank. If you are only updating a single campaign name, your file would look like this:

|Product    |Entity    |Operation    |Campaign Id    |Draft Campaign Id    |Portfolio Id    |Ad Group Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Start Date    |End Date    |State    |Budget Type    |Budget    |Bid Optimization    |Bid Multiplier    |Bid    |Keyword Text    |Match Type    |Product Targeting Expression    |Ad Format    |Landing Page URL    |Landing Page Asins    |Brand Entity Id    |Brand Name    |Brand Logo Asset Id    |Brand Logo URL    |Creative Headline    |Creative ASINs    |Video Media Ids    |Creative Type    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Brands	|Campaign	|Update	|**3729461**	|	|	|	|	|	|**NEW SB campaign name**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

Let’s say you have a different campaign, and you want to change the end date and the daily budget:

#### Update campaign end date and budget

1. Enter the campaign ID in the “Campaign ID” column
2. In the “End Date” column, enter the date when the campaign should end, using the format yyyyMMdd
3. In the “State” column, enter **enabled**
4. In the “Budget” column, enter the new amount using numbers only

|Product    |Entity    |Operation    |Campaign Id    |Draft Campaign Id    |Portfolio Id    |Ad Group Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Start Date    |End Date    |State    |Budget Type    |Budget    |Bid Optimization    |Bid Multiplier    |Bid    |Keyword Text    |Match Type    |Product Targeting Expression    |Ad Format    |Landing Page URL    |Landing Page Asins    |Brand Entity Id    |Brand Name    |Brand Logo Asset Id    |Brand Logo URL    |Creative Headline    |Creative ASINs    |Video Media Ids    |Creative Type    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Brands	|Campaign	|Update	|**9283347**	|	|	|	|	|	|	|	|**20220331**	|**Enabled**	|	|**150**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

#### Archive a campaign 

>[TIP]When you archive a parent entity, all child entities will also be archived. You should avoid archiving both a parent and its child entity, or you’ll get an error indicating that the child entity failed to archive. The error will not cause any issues because the child will have been archived, but if you only archive the parent, you can avoid seeing this error.

You can update the status of a campaign to be **archived** using the “Operation” field—and note that this same process can be applied to any entity you want to archive (ad group, keyword, and so on). Just be sure to always include the entity ID:

1. In the “Operation” field, enter **archived**
2. Enter the campaign ID in the “Campaign ID” column

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Draft Campaign Id    |Portfolio Id    |Ad Group Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Start Date    |End Date    |State    |Budget Type    |Budget    |Bid Optimization    |Bid Multiplier    |Bid    |Keyword Text    |Match Type    |Product Targeting Expression    |Ad Format    |Landing Page URL    |Landing Page Asins    |Brand Entity Id    |Brand Name    |Brand Logo Asset Id    |Brand Logo URL    |Creative Headline    |Creative ASINs    |Video Media Ids    |Creative Type    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Brands	|Campaign	|**Archived**	|**738572948**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

## Update the keyword entity

>[TIP]You cannot update keywords by changing the keyword text. Instead, you should pause or archive the keywords that you want to stop running. 

#### Pause a keyword

You can pause keywords by updating its entity and setting the “State” to **paused**. You can pause as many keywords as you want in a single bulk upload.  

1. In the “Operation field,” enter **update**
2. Enter the campaign ID, ad group ID, and keyword IDs in their respective fields
3. In the “State” field, enter **paused**

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Draft Campaign Id    |Portfolio Id    |Ad Group Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Start Date    |End Date    |State    |Budget Type    |Budget    |Bid Optimization    |Bid Multiplier    |Bid    |Keyword Text    |Match Type    |Product Targeting Expression    |Ad Format    |Landing Page URL    |Landing Page Asins    |Brand Entity Id    |Brand Name    |Brand Logo Asset Id    |Brand Logo URL    |Creative Headline    |Creative ASINs    |Video Media Ids    |Creative Type    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Brands	|Keyword	|**Update**	|**88910384**	|	|	|	|**3920483**	|	|	|	|	|**Paused**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

<br>

#### Archive a keyword

To archive keywords, set the “Operation” field to **Archive**. You can archive as many keywords as you want in a single bulk upload. 
 
1. In the “Operation field,” enter **archive**
2. In the “Keyword ID” field, enter the ID

All other fields can be left unchanged or blank:


|Product    |Entity    |Operation    |Campaign Id    |Draft Campaign Id    |Portfolio Id    |Ad Group Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Start Date    |End Date    |State    |Budget Type    |Budget    |Bid Optimization    |Bid Multiplier    |Bid    |Keyword Text    |Match Type    |Product Targeting Expression    |Ad Format    |Landing Page URL    |Landing Page Asins    |Brand Entity Id    |Brand Name    |Brand Logo Asset Id    |Brand Logo URL    |Creative Headline    |Creative ASINs    |Video Media Ids    |Creative Type    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Brands	|Keyword	|**Archive**	|	|	|	|	|**1003948**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

<br>
## Update product targeting

#### Pause targeting
Similar to keywords, you cannot change or delete the target, but you can pause or archive any product targeting that you no longer want active. 

1. In the “Entity field, enter **product targeting**
2. In the “Operation” field, enter **update**
3. Enter the **campaign ID** and **product targeting ID** in their respective fields
4. In the “State” column, enter **paused** 

All other fields can be left unchanged or blank:

|Product    |Entity    |Operation    |Campaign Id    |Draft Campaign Id    |Portfolio Id    |Ad Group Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Start Date    |End Date    |State    |Budget Type    |Budget    |Bid Optimization    |Bid Multiplier    |Bid    |Keyword Text    |Match Type    |Product Targeting Expression    |Ad Format    |Landing Page URL    |Landing Page Asins    |Brand Entity Id    |Brand Name    |Brand Logo Asset Id    |Brand Logo URL    |Creative Headline    |Creative ASINs    |Video Media Ids    |Creative Type    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Brands	|**Product targeting**	|**Update**	|**88910384**	|	|	|	|	|**67249384**	|	|	|	|**Paused**	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

<br>
## Update campaigns and ad groups within the same bulksheets file

The examples above show each scenario separately. Here’s an example showing all of the updates in a single file, including

* Update campaign name
* Update campaign end date and budget
* Archive a campaign
* Pause a keyword
* Archive a keyword
* Pause product targeting

|Product    |Entity    |Operation    |Campaign Id    |Draft Campaign Id    |Portfolio Id    |Ad Group Id    |Keyword Id    |Product Targeting Id    |Campaign Name    |Start Date    |End Date    |State    |Budget Type    |Budget    |Bid Optimization    |Bid Multiplier    |Bid    |Keyword Text    |Match Type    |Product Targeting Expression    |Ad Format    |Landing Page URL    |Landing Page Asins    |Brand Entity Id    |Brand Name    |Brand Logo Asset Id    |Brand Logo URL    |Creative Headline    |Creative ASINs    |Video Media Ids    |Creative Type    |
|---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |---    |
|Sponsored Brands	|Campaign	|Update	|3729461	|	|	|	|	|	|NEW SB campaign name	|	|	|Enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Campaign	|Update	|9283347	|	|	|	|	|	|	|	|20220331	|Enabled	|	|150	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Campaign	|Archive	|738572948	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Keyword	|Update	|88910384	|	|	|	|3920483	|	|	|	|	|Paused	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Keyword	|Archive	|	|	|	|	|1003948	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product targeting	|Update	|88910384	|	|	|	|	|67249384	|	|	|	|Paused	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

* * *