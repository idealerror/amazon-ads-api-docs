---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Updating a Sponsored Products Manual Targeting campaign to add new entities

__Key Points to Note__

* For an existing Sponsored Products campaign, new ad groups can be added along with other nested entities including new keywords, ads and product targeting. 
* As shown in [Creating a Sponsored Products Automatic Targeting campaign](bulksheets/sp/sp-examples/sp-create-auto-targeting), for adding new entities, leave the ‘Record ID’ column empty, but fill the ‘Campaign ID’ column with the existing campaign ID as this is an update operation for an existing campaign. For example, adding a new ad group with a new ad and new keywords for an existing campaign would look like the example below. In this example, campaign with ID ‘8374837483’ already exists. Since we want to add the new entities to this campaign, we fill the ‘Campaign ID’ field for these entities with ‘8374837483’ indicating that we want to add these new entities to this existing campaign.
* Only new entities that are being added need to be included in the bulksheet and not the entire hierarchy of entities. In the example below, the Record Type campaign is not included. However, doing so will not result in an error.
* To reduce the processing time, delete rows that do not contain updates. Do not forget to save changes before uploading the file. Deleted rows will not be changed upon upload.


| Record ID | Record Type | Campaign ID | Campaign                  | Campaign Daily Budget | Portfolio ID | Campaign Start Date | Campaign End Date | Campaign Targeting Type | Ad Group       | Max Bid | Keyword or Product Targeting | Product Targeting ID | Match Type     | SKU          | Campaign Status | Ad Group Status | Status  | Bidding strategy | Placement Type | Increase bids by placement |
|-----------|-------------|-------------|---------------------------|-----------------------|--------------|---------------------|-------------------|-------------------------|----------------|---------|------------------------------|----------------------|----------------|--------------|-----------------|-----------------|---------|------------------|----------------|----------------------------|
|   <br/>        | Ad Group    | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | New Ad Group 1 | 0.95    |                              |                      |                |              |                 | enabled         |         |                  |                |                            |
|           | Ad          | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | New Ad Group 1 |         |                              |                      |                | Sample SKU 1 |                 |                 | enabled |                  |                |                            |
|           | Keyword     | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | New Ad Group 1 | 0.75    | New Keyword                  |                      | Exact          |              |                 |                 | enabled |                  |                |                            |
|           | Keyword     | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | New Ad Group 1 |         | New Negative Keyword         |                      | Negative Exact |              |                 |                 | enabled |                  |                |                            |