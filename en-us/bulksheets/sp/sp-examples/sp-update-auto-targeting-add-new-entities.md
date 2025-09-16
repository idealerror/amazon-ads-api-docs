---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Updating a Sponsored Products Automatic Targeting campaign to add new entities

This is similar to the example [Updating a Sponsored Products Manual Targeting campaign to add new entities](bulksheets/sp/sp-examples/sp-update-manual-targeting-add-new-entities). See the [entity diagram](bulksheets/sp/sp-entities/sp-entity-hierarchy) to know the entities that can be added.


__Key Points to Note__

* For an existing Sponsored Products Automatic Targeting campaign, new campaign level negative keywords and ad groups can be added. In addition, new ads can be added under the ad groups. 
* For adding new entities, leave the ‘Record ID’ column empty, but fill the ‘Campaign ID’ column with the existing campaign ID as this is an update operation for an existing campaign. For example, adding a new ad group with a new ad and a new negative keyword for an existing campaign would look like the example below. In this example, campaign with ID ‘8374892837’ already exists. Since we want to add the new entities to that campaign, we fill the ‘Campaign ID’ cell for those entity with ‘8374892837’ indicating that we want to add these new entities to this existing campaign.
* Only new entities that are being added need to be included in the bulksheet and not the entire hierarchy of entities. In the example below, the Record Type campaign is not included. However, doing so will not result in an error.
* To reduce the processing time, delete rows that do not contain updates. Do not forget to save changes before uploading the file. Deleted rows will not be changed upon upload.


| Record ID | Record Type | Campaign ID | Campaign                | Campaign Daily Budget | Portfolio ID | Campaign Start Date | Campaign End Date | Campaign Targeting Type | Ad Group     | Max Bid | Keyword or Product Targeting  | Product Targeting ID | Match Type              | SKU     | Campaign Status | Ad Group Status | Status  | Bidding strategy | Placement Type | Increase bids by placement |
|-----------|-------------|-------------|-------------------------|-----------------------|--------------|---------------------|-------------------|-------------------------|--------------|---------|-------------------------------|----------------------|-------------------------|---------|-----------------|-----------------|---------|------------------|----------------|----------------------------|
|  <br/>         | Keyword     | 8374892837  | Sample SP Auto Campaign |                       |              |                     |                   |                         |              |         | New Campaign Negative Keyword |                      | Campaign Negative Exact |         |                 |                 | enabled |                  |                |                            |
|           | Ad Group    | 8374892837  | Sample SP Auto Campaign |                       |              |                     |                   |                         | New Ad Group | 0.95    |                               |                      |                         |         |                 | enabled         |         |                  |                |                            |
|           | Ad          | 8374892837  | Sample SP Auto Campaign |                       |              |                     |                   |                         | New Ad Group |         |                               |                      |                         | New SKU |                 |                 | enabled |                  |                |                            |