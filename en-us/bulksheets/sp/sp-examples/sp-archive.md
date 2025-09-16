---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Archiving a Sponsored Products Campaign

__Key Points to Note__

* Similar to the Update examples, make sure the “Record ID” and “Campaign ID” for all 'Record Types' are present. 
* By changing the status of the corresponding row to “archived”, you can archive that particular row. 
* If you archive a campaign, all items associated with that campaign (i.e. the ad groups, ads, keywords, and product targeting) will automatically get archived. If you archive an ad group, all the items associated with that ad group (i.e. the ads, keywords, and product targeting) will automatically get archived. 
* Refer to the [Sponsored Products Entity Diagram](bulksheets/sp/sp-entities/sp-entity-hierarchy) to understand the hierarchy.
* As shown in the example below, archiving ‘Sample Ad Group 1’ row alone would archive all that is within that Ad Group. The status of all the child entities will be automatically updated. This can be confirmed by initiating a new Bulksheet download.
* Archiving an ad Group will not affect other ad groups.

| Record ID  | Record Type | Campaign ID | Campaign                  | Campaign Daily Budget | Portfolio ID | Campaign Start Date | Campaign End Date | Campaign Targeting Type | Ad Group          | Max Bid | Keyword or Product Targeting | Product Targeting ID | Match Type     | SKU          | Campaign Status | Ad Group Status | Status  | Bidding strategy | Placement Type | Increase bids by placement |
|------------|-------------|-------------|---------------------------|-----------------------|--------------|---------------------|-------------------|-------------------------|-------------------|---------|------------------------------|----------------------|----------------|--------------|-----------------|-----------------|---------|------------------|----------------|----------------------------|
| 9812764938 | Ad Group    | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 | 2.00    |                              |                      |                |              |                 | archived        |         |                  |                |                            |
| 8967329634 | Ad          | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 |         |                              |                      |                | Sample SKU 1 |                 |                 | enabled |                  |                |                            |
| 7619837209 | Keyword     | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 | 0.85    | Sample Keyword               |                      | Exact          |              |                 |                 | enabled |                  |                |                            |
| 7289174629 | Keyword     | 8374837483  | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 |         | Sample Negative Keyword      |                      | Negative Exact |              |                 |                 | paused  |                  |                |                            |
