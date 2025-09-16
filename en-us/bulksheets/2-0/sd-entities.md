---
title: Sponsored Display entities 
description: A guide explaining campaign structure and hierarchy to help you understand how Sponsored Display entities relate to one another. Understanding this structure can make it easier to manage campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Display
    - Campaign management
keywords:
    - sponsored display campaign structure
    - sponsored display campaign hierarchy
    - bulksheets  
---

# Sponsored Display entities

To manage Sponsored Display campaigns effectively using bulksheets, it’s helpful to understand the different entities and their relationships. An “entity” starts with a top-level **campaign** (the “parent”) along with “child” entities underneath. For example, an SD campaign always contains an **ad group**, which is a child entity of the parent-level **campaign**. Any attributes that are set at the campaign level (such as start dates) will be inherited by the child entities. In some cases, you can set attributes at the child level that will override the campaign-level setting. For example, a bid set at the campaign level will be inherited by the ad group, unless you set an “ad group default bid.” 

With bulksheets, you will start at the top-level parent entity when you start to create a new campaign in your spreadsheet, and work down from the parent to add child entities in spreadsheet rows underneath. The visual diagram below shows these entity types and their relationships for SD campaigns. These entities correspond to the “Entity” field (Column B) that you will see in a bulksheets file.  

Whenever you [create a new Sponsored Display campaign](bulksheets/2-0/create-sd-campaign), you must define the targeting tactic--either audience or contextual targeting.   

**NOTE**: For each targeting type, the mandatory entities are colored green for reference. These are the basic entities that you must have in order to create a successful campaign. 

![Entity diagram for Sponsored Display campaign](/_images/bulksheets/2-0-images/sd-entities.png "Entity diagram for Sponsored Display campaign")
* * 

