---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Updating a Sponsored Products Manual Targeting campaign to edit existing entities

__Key Points to Note__

* For making an update, it is important that ‘Record ID’ and ‘Campaign ID’ are filled. The fastest way to get the right values for these fields is to download a bulksheet with the campaigns that you want to update. 
* Only the entities to be updated need to be included in a bulksheet and not the entire hierarchy of entities, although doing so will not result in an error.
* Refer to the entity details under 'Entities' to find the allowed values for each field for a 'Record Type'. 
* If the parent entity of the entity that you want to update is archived, you will receive an error while updating it. 
* To reduce the processing time, delete rows that do not contain updates. Do not forget to save changes before uploading the file. Deleted rows will not be changed upon upload.

<br/>
<br/>

The following example demonstrates how to update the [sample](bulksheets/sp/sp-examples/sp-create-manual-targeting) Sponsored Products Manual Targeting campaign. The following updates are being made to the campaign:

* Sample SP Manual Campaign
  * Campaign Daily Budget updated from $100 to $200
  * Campaign End Date updated from "02/12/2021" to "02/12/2022"
* Sample Ad Group 1 - Max Bid changes from $0.95 to $2.00
  * Sample Keyword - Max Bid updated from $0.75 to $0.95
  * Sample Negative Keyword - Status updated from “enabled” to “paused”


| Record ID  | Record Type | Campaign ID | Campaign                  | Campaign Daily Budget | Portfolio ID | Campaign Start Date | Campaign End Date | Campaign Targeting Type | Ad Group          | Max Bid | Keyword or Product Targeting     | Product Targeting ID | Match Type              | SKU          | Campaign Status | Ad Group Status | Status  | Bidding strategy              | Placement Type | Increase bids by placement |
|------------|-------------|-------------|---------------------------|-----------------------|--------------|---------------------|-------------------|-------------------------|-------------------|---------|----------------------------------|----------------------|-------------------------|--------------|-----------------|-----------------|---------|-------------------------------|----------------|----------------------------|
| 8374837483 | Campaign    | 8374837483  | Sample SP Manual Campaign | 200                   |              | 02/05/2021          | 02/12/2022        | Manual                  |                   |         |                                  |                      |                         |              | enabled         |                 |         | Dynamic bidding (up and down) |                |                            |
| 8928374098 | Keyword     | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         |                   |         | Sample Campaign Negative Keyword |                      | Campaign Negative Exact |              |                 |                 | enabled |                               |                |                            |
| 9812764938 | Ad Group    | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 | 2       |                                  |                      |                         |              |                 | enabled         |         |                               |                |                            |
| 8967329634 | Ad          | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 |         |                                  |                      |                         | Sample SKU 1 |                 |                 | enabled |                               |                |                            |
| 7619837209 | Keyword     | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 | 0.85    | Sample Keyword                   |                      | Exact                   |              |                 |                 | enabled |                               |                |                            |
| 7289174629 | Keyword     | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 |         | Sample Negative Keyword          |                      | Negative Exact          |              |                 |                 | paused  |                               |                |                            |