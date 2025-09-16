---
title: Sponsored Products entities 
description: A guide explaining campaign structure and hierarchy to help you understand how Sponsored Products entities relate to one another. Understanding this structure can make it easier to manage campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - sponsored products campaign structure
    - sponsored products campaign hierarchy
    - bulksheets  
---

# Sponsored Products: Targeting types & entities

To manage Sponsored Products campaigns effectively using bulksheets, it’s helpful to understand the different entities and their relationships. An “entity” starts with a top-level **campaign** (the “parent”) along with “child” entities underneath. For example, an **ad group** and **bid adjustment** are each a child entity of the parent-level **campaign**. Some child entities have sub-entities of their own. For instance, under the **ad group** entity, a **product ad** would be a child of that **ad group**. 

With bulksheets, you will start at the top-level parent entity when you start to create a new campaign in your spreadsheet, and work down from the parent to add child entities in spreadsheet rows underneath. The visual diagrams below show these entity types and their relationships for SP campaigns. These entities correspond to the “Entity” field (Column B) that you will see in a bulksheets file.  

Whenever you [create a new Sponsored Products campaign](bulksheets/2-0/create-sp-campaign), you will choose between two targeting types: automatic (auto) or manual. If you create a manual targeting campaign, you will also choose between keyword or product targeting:

* Auto targeting 
* Manual targeting
    * Keyword targeting
    * Product targeting

**NOTE**: For each targeting type, the mandatory entities are colored green for reference. These are the basic entities that you must have in order to successfully [create a Sponsored Products campaign](bulksheets/2-0/create-sp-campaign). You can also include other entities, such as a bid adjustment or negative keyword, to further customize your campaign.   

![Entity diagram for auto campaign](/_images/bulksheets/2-0-images/sp-auto-entities.png)

<br>

**
<br>

![Entity diagram for manual campaign](/_images/bulksheets/2-0-images/sp-manual-entities.png)
<br><br>
Learn more about [automatic and manual targeting options](https://advertising.amazon.com/help#GMCARW8DKXNM33LF). 
