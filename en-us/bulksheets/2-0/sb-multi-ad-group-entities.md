---
title: Sponsored Brands v4 (multi-ad group) entities 
description: A guide explaining campaign structure and hierarchy to help you understand how Sponsored Brands V4 entities relate to one another. Understanding this structure can make it easier to manage campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands V4
    - Campaign management
keywords:
    - sponsored brands campaign structure
    - sponsored brands campaign hierarchy
    - bulksheets  
---

# Sponsored Brands multi-ad group entity diagram

To manage Sponsored Brands V4 (multi-ad group) campaigns effectively using bulksheets, it’s helpful to understand the different entities and their relationships. An “entity” starts with a top-level **campaign** (the “parent”) along with “child” entities underneath. For Sponsored Brands, child entities include the **ad group(s)** and optional **bidding adjustment by placement**. Then, the targets (either **keyword** or **product targeting**) are child entities of both the ad group and the campaign—this means that you’ll need to include the parent IDs for the campaign and the ad group when you create the targeting rows in bulksheets. Similarly, the four ad formats are child entities of the ad group and campaign, so those parent IDs are also required in the rows where you create the ad. 

With bulksheets, you will start at the top-level parent entity when you start to create a new campaign in your spreadsheet, and work down from the parent to add child entities in spreadsheet rows underneath. The visual diagram below shows these entity types and their relationships for SB campaigns. These entities correspond to the “Entity” field (Column B) that you will see in a bulksheets file.  

**NOTE**: The required entities are colored pink for reference. These are the basic entities that you must have in order to create a successful Sponsored Brands multi-ad group campaign. Thus, based on the diagram below, the mandatory entities you must create include the **campaign**, **ad group**, **ad format**, and **targeting strategy**. The **bidding adjustment by placement** and **negative targets** are optional entities. Each entity will be in its own row in your bulk spreadsheet. 

![Entity diagram for Sponsored Brands multi-ad group campaign](/_images/bulksheets/2-0-images/sb-multi-ad-group-entities.png "Entity diagram for Sponsored Brands multi-ad group campaign")

Learn more about [how to create a Sponsored Brands multi-ad group campaign using bulksheets](bulksheets/2-0/create-sb-multi-ad-group-campaigns). 
