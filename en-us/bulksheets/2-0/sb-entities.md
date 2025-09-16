---
title: Sponsored Brands entities 
description: A guide explaining campaign structure and hierarchy to help you understand how Sponsored Brands entities relate to one another. Understanding this structure can make it easier to manage campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands
    - Campaign management
keywords:
    - sponsored brands campaign structure
    - sponsored brands campaign hierarchy
    - bulksheets  
---

# Sponsored Brands entities

To manage Sponsored Brands campaigns effectively using bulksheets, it’s helpful to understand the different entities and their relationships. An “entity” starts with a top-level **campaign** (the “parent”) along with “child” entities underneath. For Sponsored Brands, child entities include **keyword** and **negative keyword** if you choose keyword targeting, or **product targeting** and **negative product targeting** if you choose product targeting.

With bulksheets, you will start at the top-level parent entity when you start to create a new campaign in your spreadsheet, and work down from the parent to add child entities in spreadsheet rows underneath. The visual diagram below shows these entity types and their relationships for SB campaigns. These entities correspond to the “Entity” field (Column B) that you will see in a bulksheets file.  

**NOTE**: The mandatory entities are colored green for reference. These are the basic entities that you must have in order to successfully [create a Sponsored Brands campaign](bulksheets/2-0/create-sb-campaign). After selecting a **targeting strategy**, you will further define the strategy by creating a bulksheets entity for **keyword** or **product targeting**, depending on which approach you use. 

![Entity diagram for Sponsored Brands campaign](/_images/bulksheets/2-0-images/sb-entities.png)


Learn more about [how to create a Sponsored Brands campaign using bulksheets](bulksheets/2-0/create-sb-campaign). 
