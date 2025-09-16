---
title: Bulksheets error reports
description: Understand bulksheets error reports and error messages
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
    - Bulksheets
    - Bulk operations
keywords:
    - bulksheets
    - troubleshooting
    - error messages
    - error reports
---
# Bulksheets error messages

This article explains the most common bulksheets errors, and provides guidance on how to interpret and resolve these errors. The content is divided into sections based on the error category. 

*Jump to* [ID-related](#id-related-errors) | [Invalid values](#invalid-values) | [Missing values](#missing-values) | [Duplicates](#duplicate-values) | [Limits](#limits) | [Other common error messages](#other-errors)

When you upload a bulksheets file on the [bulk operations home page](https://advertising.amazon.com/bulksheet/HomePage), you will see a notification in the UI if any errors occurred during the upload process. In the “Bulk spreadsheet history” section of the page, the status will read “Finished with errors” if any are present, and you can then **download an error report** to troubleshoot:

![Bulksheets error notification](/_images/bulksheets/2-0-images/error-report-download.png)

The report will contain two tabs\*: one labeled “Upload Processing Summary,” and the other labeled with the sponsored ads campaign type (note that if you were updating more than one campaign type, your report would contain additional tabs for each campaign type). The “Upload Process Summary” tab will show the row numbers where the errors occurred, along with an error message to help you troubleshoot. 

Here is a sample excerpt from this tab:  

|Original record #	|Error type	|Error code	|Error message	|
|---	|---	|---	|---	|
|2	|Warning	|Not Processed	|This row was not processed due to errors from other rows (*See note below*)	|
|3	|Input Error	|Missing Parent ID	|Invalid value for column: "Campaign ID", which can't be found  in any parent entity row	|
|5	|Input Error	|Duplicate Text	|Duplicate "Keyword Text", which is the same as row 4	|

>[NOTE] It is common to see the warning message “This row was not processed due to errors from other rows.” This occurs when another error prevents a row from being processed. For example, if you try to create a new campaign with an ad group and keyword, but you have an error in the campaign row, then the ad group and keyword rows would likely receive this error. This is because those child entities can't be created if the parent entity isn't created. There is generally nothing to do in the row where this error happened. Instead, review the other errors that likely prevented the sheet from being processed. 

Generally, columns A and D are the most helpful because they indicate where the error occurred and provide more information about what happened: The **Original record #** is the row number where that particular error occurred, and the **Error message** will give you more details. Note: This exact same information is also available in the campaign tab, if you scroll all the way over to the far right of the sheet. The error messages will appear in the rows where the errors occurred.

>[TIP] You can make changes directly in the campaign tab of the error report, save the file, and upload it to fix the errors. You do not need to download another file or use the one you had previously uploaded.  

## ID-related errors

A common cause of errors in bulksheets is related to entity IDs—in particular, the Campaign IDs and Ad group IDs. When you create a new campaign from scratch, you must include temporary, text-based IDs for these entities. When you upload the campaign, the temporary IDs will become numeric, system-generated ones that you can find in a downloaded bulksheets file. To update a campaign after it is created, you will use the system-generated IDs. 

>[TIP] You can learn more in this [video tutorial for creating campaigns](bulksheets/2-0/bulksheets-videos#video-highlights-3). Skip to the three-minute mark to see entity ID-related tips and how these relate to campaign structure

|Error message	|Notes	|
|---	|---	|
|Invalid value for column: "Campaign ID", which can't be found in any parent entity row	|This error typically means you've input a Campaign ID value in a child entity row that does not match a parent Campaign ID. A common cause is if you make a typo in one of the child entity rows. For example, let's say you are creating a new campaign with the temporary ID **Spring toys**. You would need to input that exact same temporary ID in all child entity rows. If the ID in a child entity row does not match exactly, you'll get this error. 	|
|Invalid value for column: "Ad Group ID", which can't be found in any parent entity row	|Similar to Campaign ID, you must enter a temporary, text-based value for the Ad Group ID when you create a new one. This same Ad Group ID must be included in all child entity rows of the ad group. You will get an error if these IDs do not match exactly. 	|
|This Create operation requires you to specify a temporary "Campaign ID" (any non-numeric text)	|This error typically means you have entered a numeric value in the Campaign ID field when creating a new campaign. To create a new campaign, specify a text-based temporary ID.	|
|This Create operation requires you to specify a temporary "Ad Group ID" (any non-numeric text)	|This error typically means you have entered a numeric value in the Ad group ID field when creating a new ad group. To create a new ad group, specify a text-based temporary ID.	|
|This Update operation requires you to specify an actual "Campaign ID", rather than a temporary ID	|This error implies you have entered an alphabetic value for the Campaign ID in a campaign that already exists. Existing campaign IDs will be numeric, with no letters. Check the ID in a downloaded bulksheets file, and re-enter the correct ID. Be sure you are using the bulksheets download ID--**not** the ID found in the advertising console.	|
|This Update operation requires you to specify an actual "Ad Group ID", rather than a temporary ID	|Similar to the note above, be sure to enter the actual ID for ad groups that already exist, and use the ID found in bulksheets--not the ID from the advertising console. 	|
|Parent entity not found	|This error relates to ad group child entities--e.g. keywords, product targets, product ads. It typically occurs when you try to create new child entities, but you use a parent ID that our system doesn't recognize. Learn more about parent-child relationships in the [campaign entity diagrams](bulksheets/2-0/sp-entities)	|
|Could not find keyword with id: OBJECT_ID	|For all of the "OBJECT_ID" messages in the rows below, this error occurs when you enter an entity ID value that doesn't exist in our system. Check the ID in a downloaded bulksheets file, and re-enter the correct ID. Be sure you are using the bulksheets download ID--**not** the ID found in the advertising console.	|
|Could not find targeting clause with id: OBJECT_ID	|See note above	|
|Could not find campaign with id: OBJECT_ID	|See note above	|
|Could not find ad group with id: OBJECT_ID	|See note above	|
|Could not find product ad with id: OBJECT_ID	|See note above	|
|Could not find negative keyword with id: OBJECT_ID	|See note above	|

## Invalid values

For invalid value errors, the solution is typically simple: You should fix the value, save, and re-upload your file. Most values are not case sensitive, but must be spelled correctly. 

|Error message	|Notes	|
|---	|---	|
|Keyword is invalid	|This error typically means you included invalid characters in the keyword field. Review the [keyword character constraints guide](reference/concepts/limits#keyword-character-constraints) for more information	|
|Invalid value: "VALUE" for column: "Bid"	|Bid values should be numeric, and the [bid limits vary by marketplace](reference/concepts/limits#bid-constraints-by-marketplace)	|
|Invalid value: "VALUE" for column: "Match Type"	|"Match type" is relevant for keyword targeting. For Sponsored Products manual targeting and Sponsored Brands campaigns, options are  **broad**, **phrase**, or **exact**. For negative keywords, options are **negative exact** or **negative phrase**. These values are not case sensitive but must be spelled correctly.	|
|Invalid value: "VALUE" for column: "State"	|The "State" field is the status of the entity in that row. Options are **enabled**, **paused**, or **archived**. 	|
|Invalid value: "VALUE" for column: "Bidding Strategy"	|Options include **Dynamic bids - down only**, **Dynamic bids - up and down**, or **Fixed bid**. These values are not case sensitive but must be spelled correctly. <br/>Note that the "Bidding strategy" is defined in the "Campaign" entity row. 	|
|Invalid product: "PRODUCT_INPUT"	|In bulksheets, the "Product" field is the campaign type. The value must be either Sponsored Products, Sponsored Brands, or Sponsored Display, or Portfolios. The field is not case-sensitive, but you will get an error if you misspell or make a typo. 	|
|Invalid entity: "ENTITY_INPUT"	|The "Entity" field is the campaign component. Common examples of entities include Campaign, Ad group, Keyword, Product ad, Negative product targeting, etc. 	|
|Invalid operation: "OPERATION_INPUT"	|The "Operation" field is the action you want to take in this row. Values must be either Create, Update, or Archive. Note that if you want to pause an entity, you should set this field to "Update," and change the "State" field to **paused** for the entity row you want to pause. 	|
|Campaign name has invalid characters	|Be sure to use only valid characters in campaign names. [Read more about which characters are valid](reference/concepts/limits#entity-name-character-constraints) 	|
|Ad group name has invalid characters	|Be sure to use only valid characters in ad group names. [Read more about which characters are valid](reference/concepts/limits#entity-name-character-constraints) 	|
|Invalid value: INVALID_DATE_INPUT specified for field: Start Date, the correct date format is yyyyMMdd	|The date must be entered as year-month-day in the format yyyyMMdd. So July 12, 2024 would be entered as: 20240712	|

## Missing values

A “missing value” error can occur for various entities. To fix these errors, the typical solution is to simply add the missing value in the appropriate field. Remember to check the “Original record #” value in Column A of the error report (not shown below) to see which row the error occurred in.  

|Error message	|Notes	|
|---	|---	|
|Missing value for column:  "State"	|All entity rows require a "State" value. The only exception is the **Bidding adjustment** row, where this field can be left blank. 	|
|Missing value for column: "Campaign ID"	|The Campaign ID is required for every row in your spreadsheet. For the campaign and all child entities (e.g. ad group, keyword, product ad), the IDs must match exactly in order to place those child entities into the parent campaign. 	|
|Missing value for column: "Ad Group ID"	|The Ad group ID is required for every row in your spreadsheet, other than the "Campaign" row. For the ad group and all child entities (e.g. keyword, product ad), the IDs must match exactly in order to place those entities into the parent ad group. 	|
|Missing value for column: "Keyword ID"	|To update any entity, you must enter the system-generated ID found in a bulksheets download. So for example, to update a keyword bid, you would need to include the Keyword ID. 	|
|Missing value for columns: Campaign ID, Ad Group ID	|If multiple values are missing, you will see an error message that mentions all missing values. 	|
|Missing value for column: "Match Type"	|To create a Sponsored Products manual targeting campaign, or a Sponsored Brands keyword campaign, you must include the keyword match type. Options for **keywords** include **broad**, **phrase**, or **exact**. Options for **negative keywords** include **negative exact** or **negative phrase**.	|
|Missing value for column: "Ad Group Default Bid"	|For Sponsored Products campaigns, to create a new ad group, you need to specify a default bid in this row. This default bid amount will apply to all child entities of this ad group if you don't define bids for the child entities explicitly. For instance, if a keyword child entity doesn't include a defined bid, this default amount would apply.	|
|Missing value for column: “Ad group name”	|All ad groups--as well as campaigns--must have unique names. If you leave the "Name" field blank, you will get an error. 	|
|Product ad did not specify sku for create operation.	|To create a product ad, you must specify the SKU for the product you want to advertise. If you are a vendor, enter the ASIN.	|

## Duplicate values

Many values cannot be duplicated, particularly within the same parent campaign. For example, you cannot have two ad groups with the exact same name within the same parent campaign. Entity IDs are also unique and cannot be duplicated within the same advertiser account. For example, in any given bulk spreadsheet, every Campaign ID and name should be unique. Note that in the error messages below, the “ROW_NUMBER” would contain a value to show which row the error is in. 

|Error message	|Notes	|
|---	|---	|
|Duplicate Id for column: "Ad Group ID", which is the same as row ROW_NUMBER	|This means you've used the exact same Ad group ID multiple times. To fix, you can [create a new ad group within the existing campaign](bulksheets/2-0/update-sp-campaigns#add-a-new-ad-group-to-an-existing-sponsored-products-campaign). Or, you can create a new campaign and new ad group. 	|
|Duplicate Id for column: "Campaign ID", which is the same as row ROW_NUMBER	|This means you've used the exact same Campaign ID multiple times. If you are trying to create a new campaign, use a temporary text-based ID to create the entity. 	|
|Duplicate Id for column: "Keyword ID", which is the same as row ROW_NUMBER	|The next two examples all indicate that you've input a duplicate ID value. When creating new keywords, product targets, and product ads, you should leave the ID field blank. After you create the entities, the IDs will be available in a downloaded bulksheet if you want to update these entities later. <br>For example, to update a keyword bid, you would need the Keyword ID to be included in the sheeet. 	|
|Duplicate Id for column: "Product Targeting ID", which is the same as row ROW_NUMBER	|See note above	|
|Duplicate Id for column: "Ad ID", which is the same as row ROW_NUMBER	|See note above	|
|Duplicate "Keyword Text", which is the same as row ROW_NUMBER	|In any given ad group, the combination of keyword text and match type must be unique. 	|
|Campaign with name=CAMPAIGN_NAME already exists!	|Within a single advertiser account, campaign names must be unique. 	|
|AdGroup with name=AD_GROUP_NAME already exists!	|Within a single campaign, Ad group names must be unique. For this error and the campaign-related one above, the message typicaly provides the duplicate name so you can identify the issue quickly. 	|
|TargetingClause with campaignId=OBJECT_ID, adGroupId=OBJECT_ID, expressionType=MANUAL, expression=[(TARGETING_EXPRESSION)] already exists!	|This error is often caused when you try to copy or clone a target from one campaign to another--or from one ad group to another. If you want to copy a keyword or product target into a new campaign or ad group, be sure to replace the IDs with temporary ones. Or, if you want to copy the targets into another existing campaign, change the IDs to match the new campaign. 	|
|NegativeTargetingClause with campaignId=OBJECT_ID, adGroupId=OBJECT_ID, expression=[(TARGETING_EXPRESSION)] already exists!	|See note above	|
|ProductAd with campaignId=OBJECT_ID, adGroupId=OBJECT_ID, asin=ASIN, sku=SKU already exists!	|See note above	|

## Limits

All sponsored ads campaign types have minimum and/or maximum limitations on various elements. Below are some examples of limits-related errors you might see, along with links to documentation to help troubleshoot:

|Error message	|Notes	|
|---	|---	|
|Number of non archived campaigns exceeds the limit per advertiser	|[Review this guide for campaign limits](https://advertising.amazon.com/help/G86H2227323T8T4Q). Note that these limits apply to all sponsored ads campaign types, other than keyword limits which are not used in Sponsored Display campaigns. 	|
|Number of non archived ad groups exceeds the limit per advertiser	|See note above	|
|Number of non archived ad groups exceeds the limit per campaign	|See note above	|
|Campaign name is too long	|[Review this guide for constraints on character limits, bids, and budgets](reference/concepts/limits#entity-name-character-constraints). 	|
|Ad group name is too long	|See note above	|
|Keyword text is too long	|See note above	|
|Bid is lower than the minimum allowed by the marketplace	|See note above	|
|Bid is out of range (must be in [1.0, 1000.0])	|See note above	|

## Other errors

Below are other common errors that are outside of the categories grouped above: 

|**Error message**	|Notes	|
|---	|---	|
|Product is ineligible for advertising	|If you try creating a product ad with an ASIN or SKU that is ineligible to serve, you will see this error. Eligibility can be affected by many factors, including not having an image or a title, or if the product is out of stock. You can [check for eligibility in a downloaded bulksheets file](release-notes/archive/bulk-operations#asin-eligibility-is-now-shown-in-bulksheets): Look in the column titled **Eligibility status**. If any are labeled as ineligible, check the column titled **Reason for ineligibility** to see why. 	|
|Archived entity cannot be modified	|This error could happen for a couple of reasons: <br>1. You are trying to un-archive or modify/update an archived entity. Archived entities cannot be changed in any way. To "re-activate" an archived entity, you should copy the relevant data from bulksheets, and use it to create a new version of that entity. Be sure to change the entity IDs as needed when you create the new version. If this is the case, then you will need to take additional action because the sheet was not processed correctly. <br><br>Tip: You can pause entities instead of archiving if you want to re-enable the entities at a later date. To do this, set the "Operation" field to **Update**, and set the "State" to **paused**. To re-enable the paused entity, set the "Operation" to **Update**, and set the "State" to **enabled**. <br><br>2. You are trying to archive an entire campaign, including its child entities, and you included the "Archive" Operation in the child entity rows. To archive an entire campaign, you only need to archive the parent campaign. If you include the "Archive" Operation in child entity rows, you'll see this error in each child row. If this is the case, you don't need to take any additional action because the campaign will have been archived, so the errors in the child rows are inconsequential. 	|
|Only negative keywords and negative product targets are allowed in auto-targeting campaigns	|For Sponsored Products campaigns, you can choose either **auto** or **manual** targeting. If you select **auto** targeting, you cannot add your own keywords when you create the campaign. Only negative keywords or product targets can be added for auto campaigns. <br><br>There is an exception to the product targeting rule: If you want more control over bids in an auto campaign, you can [create four **product targeting** entities as described in this FAQ](bulksheets/2-0/faqs#sponsored-products-faqs). In those four rows, you'll set the "Product targeting expression" fields to the following values--one for each entity: close-match, loose-match, complements, substitutes	|
|Start date cannot be in the past	|The start date can be the same day you upload or create the campaign, or it can be a date in the future. It cannot be in the past. This error is common, and it often occurs accidentally--for example, let's say you start working on a bulksheets file on Monday, and you enter that day's date. If you don't upload the file that day, you will need to change the date to be later. 	|
|merchantSku cannot be changed	|Once you create an ad with your SKU or ASIN, these values cannot be updated. If you want to change the product you're advertising, you should create a new "Product ad" entity with the SKU or ASIN you want. If you no longer want to advertise a current SKU or ASIN, you can pause or archive them. 	|

