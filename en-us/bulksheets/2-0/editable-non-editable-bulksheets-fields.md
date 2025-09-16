---
title: Understand which fields can be updated in bulksheets 
description: A guide explaining which fields you can edit or update in bulksheets.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
keywords:
    - update campaigns
    - bulksheets  
---

# Understand which fields can be updated

When you are updating campaigns with bulksheets, it’s important to understand which fields can and cannot be updated so you can avoid wasting time or changing a campaign element unintentionally. If you change any non-editable values, you will see error messages when you upload the bulksheets file. For fields that can’t be edited (for example, keyword text or ASIN/SKU), a typical workaround is to archive its associated entity, then create a new one with the updated information. 

The tables below provide additional guidance and tips. 

*Jump to* [Sponsored Brands fields](bulksheets/2-0/editable-non-editable-bulksheets-fields#sponsored-brands-fields) | [Sponsored Display fields](bulksheets/2-0/editable-non-editable-bulksheets-fields#sponsored-display-fields)

## Sponsored Products fields

|[grid]-	|**Can update?**	|**Guidelines and tips** 	|
|---	|---	|---	|
|**Product**	|N/A	|This foundational field defines the campaign type	|
|**Entity**	|N/A	|This foundational field defines the entity you want to manage	|
|**Operation**	|N/A	|This foundational field defines the action you want to happen when you upload the file (create, update, or archive). When this field is blank, the row will be ignored in the upload process.	|
|**Campaign Id**	|No	|None of the ID fields can be changed, but you will use them when updating or archiving the associated entity. For example, to update a campaign name, you will enter the Campaign ID in this field and put the new name in the "Campaign Name" field. This same general process is used when updating any entity.	|
|**Ad Group Id**	|No	|Include this ID to update or archive the **ad group** entity. 	|
|**Portfolio Id**	|No	|You can create a new campaign within an existing portfolio if you include the Portfolio ID in the same row where you are creating the campaign.	|
|**Ad Id**	|No	|Include this ID to update or archive the **product ad** entity. 	|
|**Keyword Id**	|No	|Include this ID to update or archive the **keyword** entity.  	|
|**Product Targeting Id**	|No	|	Include this ID to update or archive a SKU or ASIN using the **product targeting** entity. |
|**Campaign Name**	|Yes	|Enter the new name in this field, and set "Operation" to **update**. Be sure to also include the Campaign ID.  	|
|**Ad Group Name**	|Yes	|Enter the new name in this field, and set "Operation" to **update**. Be sure to also include the Ad Group and Campaign IDs.	|
|**Start Date**	|No	|When updating a campaign, you cannot change the start date. Either leave the value unchanged, or delete the value and leave the start date field blank. Leaving this field blank will not affect the start date--it will be ignored in the upload process.	|
|**End Date**	|Yes	|When updating a campaign, you can leave this value unchanged to keep the current end date, enter a date if you want to set a new end date, or leave blank if you want the campaign to run indefinitely. ****Note that leaving this field blank will override and remove the current end date****	|
|**Targeting Type**	|No	|You cannot update an auto campaign to be manual, and vice versa. 	|
|**State**	|N/A	|This field should almost always contain a value, with these exceptions: If the "Entity" is **bidding adjustment**, leave this field blank. And if the "Operation" is set to **archive**, leave this field blank. Note that you'll use this field to pause entities (such as keywords) by setting "State" to **paused** and "Operation" to **update**.	|
|**Daily Budget**	|Yes	|Update this field by setting the "Entity" to **campaign** and the "Operation" to **update**	|
|**SKU**	|No	|You can't update a SKU or ASIN, but you can pause or archive the associated **product ad entity** and then create a new one with the SKU or ASIN you want to add.	|
|**ASIN**	|No	|You can't update a SKU or ASIN, but you can pause or archive the associated **product ad entity** and then create a new one with the SKU or ASIN you want to add.	|
|**Ad Group Default Bid**	|Yes	|Enter a new value and update the associated ad group entity 	|
|**Bid**	|Yes	|Enter a new value and update the associated keyword or product targeting entity 	|
|**Keyword Text**	|No	|You cannot update keywords by changing the keyword text. Instead, you should [pause or archive the keywords that you want to stop running](bulksheets/2-0/update-sp-campaigns#update-the-keyword-entity).	|
|**Match Type**	|No	|	|
|**Bidding Strategy**	|Yes	|This field is set at the campaign level. Update using the associated Campaign ID	|
|**Placement**	|No	|	|
|**Percentage**	|Yes	|Enter a new value and update the associated bidding adjustment entity 	|
|**Product Targeting Expression**	|No	|Targeting can only be archived or paused.	|

## Sponsored Brands fields

|[grid]-	|**Can update?**	|**Guidelines and tips** 	|
|---	|---	|---	|
|**Product**	|N/A	|This foundational field defines the campaign type	|
|**Entity**	|N/A	|This foundational field defines the entity you want to manage	|
|**Operation**	|N/A	|This foundational field defines the action you want to happen when you upload the file (create, update, or archive). When this field is blank, the row will be ignored in the upload process.	|
|**Campaign Id**	|No	|None of the ID fields can be changed, but you will use them when updating or archiving the associated entity. For example, to update a campaign name, you will enter the Campaign ID in this field and put the new name in the "Campaign Name" field. This same general process is used when updating any entity. 	|
|**Draft Campaign Id**	|No	|	|
|**Portfolio Id**	|No	|You can create a new campaign within an existing portfolio if you include the Portfolio ID in the same row where you are creating the campaign.	|
|**Ad Group Id**	|No	|Include this ID to update or archive the **ad group** entity 	|
|**Keyword Id**	|No	|Include this ID to update or archive the **keyword** entity.	|
|**Product Targeting Id**	|No	|When you create a **product targeting expression** using ASINs, unique IDs will be created and can be found in this "Product Targeting ID" field when you download a bulksheets file. You will use this ID if you want to pause or archive the product targeting entity. 	|
|**Campaign Name**	|Yes	|Enter the new name in this field, and set "Operation" to **update**. Be sure to also include the Campaign ID.  	|
|**Start Date**	|Yes	|When updating a campaign, you cannot change the start date. Either leave the value unchanged, or delete the value and leave the start date field blank. Leaving this field blank will not affect the start date--it will be ignored in the upload process.	|
|**End Date**	|No	|When updating a campaign, you can leave this value unchanged to keep the current end date, enter a date if you want to set a new end date, or leave blank if you want the campaign to run indefinitely. ****Note that leaving this field blank will override and remove the current end date****	|
|**State**	|Yes	|This field should always contain a value, unless the "Operation" is set to ****archive****. In that case, the "State" field can be left blank. Note that you'll use this field to pause entities (such as keywords) by setting "State" to **paused** and "Operation" to **update**.	|
|**Budget Type**	|Yes	|To change the budget type, enter **daily** or **lifetime** and update the campaign entity.	|
|**Budget**	|Yes	|Enter a new budget amount, and update the **campaign** entity.	|
|**Bid Optimization**	|Yes	|If you enter ****manual****, you should also enter a value in the "Bid Multiplier" field. If you enter ****auto****, leave the "Bid Multiplier" field blank. Bid optimization and multiplier are both set at the campaign entity level. 	|
|**Bid Multiplier**	|Yes	|Enter a value if you are also updating the "Bid Optimization" field to be **manual**, and update the campaign entity. 	|
|**Bid**	|Yes	|Enter a new value and update the associated keyword or product targeting entity 	|
|**Keyword Text**	|No	|Keyword text cannot be updated. Instead, you can archive or pause keywords that you no longer want to be active 	|
|**Match Type**	|No	|	|
|**Product Targeting Expression**	|No	|Targeting can only be archived or paused.	|
|**Ad Format**	|No	|	|
|**Landing Page URL**	|No	|None of the creative assets can be changed after they're created. Instead, you can **archive** the associated entity and create new ones. 	|
|**Landing Page Asins**	|No	|	|
|**Brand Entity Id**	|No	|	|
|**Brand Name**	|No	|	|
|**Brand Logo Asset Id**	|No	|	|
|**Brand Logo URL**	|No	|	|
|**Creative Headline**	|No	|If you want to change a creative headline, you must archive the associated campaign and create a new one with the new headline.	|
|**Creative ASINs**	|No	|	|
|**Video Media Ids**	|No	|	|
|**Creative Type**	|No	|	|

## Sponsored Display fields

|[grid]-	|**Can update?**	|**Guidelines and tips** 	|
|---	|---	|---	|
|**Product**	|N/A	|This foundational field defines the campaign type	|
|**Entity**	|N/A	|This foundational field defines the entity you want to manage	|
|**Operation**	|N/A	|This foundational field defines the action you want to happen when you upload the file (create, update, or archive). When this field is blank, the row will be ignored in the upload process.	|
|**Campaign Id**	|No	|None of the ID fields can be changed as they are defined by the Amazon ads backend. 	|
|**Ad Group Id**	|No	|Include this ID to update or archive the **ad group** entity. 	|
|**Ad Id**	|No	|Include this ID to update or archive the **product ad** entity.	|
|Targeting Id	|No	|Include this ID to update or archive the **targeting** entity.	|
|**Campaign Name**	|Yes	|Enter the new name in this field, and set "Operation" to **update*. Be sure to also include the Campaign ID.  	|
|**Ad Group Name**	|Yes	|Enter the new name in this field, and set "Operation" to **update**. Be sure to also include the Ad Group and Campaign IDs.	|
|**Start Date**	|No	|When updating a campaign, you cannot change the start date. Either leave the value unchanged, or delete the value and leave the start date field blank. Leaving this field blank will not affect the start date--it will be ignored in the upload process.	|
|**End Date**	|Yes	|When updating a campaign, you can leave this value unchanged to keep the current end date, enter a date if you want to set a new end date, or leave blank if you want the campaign to run indefinitely. ****Note that leaving this field blank will override and remove the current end date****	|
|**State**	|N/A	|This field should always contain a value, unless the "Operation" is set to ****archive****. In that case, the "State" field can be left blank. Note that you'll use this field to pause entities by setting "State" to ****paused**** and "Operation" to ****update****.	|
|**Tactic**	|No	|	|
|**Budget Type**	|N/A	|For now, the only supported budget type for Sponsored Display in bulksheets is ****daily****	|
|**Budget**	|Yes	|Enter a new value and update the campaign entity	|
|**SKU**	|No	|You can't update a SKU or ASIN, but you can pause or archive the associated **product ad entity** and then create a new one with the SKU or ASIN you want to add.	|
|**ASIN**	|No	|You can't update a SKU or ASIN, but you can pause or archive the associated **product ad entity** and then create a new one with the SKU or ASIN you want to add.	|
|**Ad Group Default Bid**	|Yes	|Enter a new amount and update the **ad group** entity 	|
|**Bid**	|Yes	|Enter a new amount and update the **targeting** entity 	|
|**Bid Optimization**	|No	|	|
|**Cost Type**	|No	|	|
|**Targeting Expression**	|No	|	|
