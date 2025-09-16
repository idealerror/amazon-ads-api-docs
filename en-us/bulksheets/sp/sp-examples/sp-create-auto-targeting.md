---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Creating a Sponsored Products Automatic Targeting campaign

__Key Points to Note__

* Each Sponsored Products campaign is a collection of nested ad entities including Campaign, Ad Groups, Keywords (campaign level and ad group level), Ads and Product Targeting. To create an entity at any level, creating its parent entity correctly is a prerequisite. Lack of this knowledge is a source of common confusion, often resulting in errors when trying to create and manage campaigns via Bulksheets. Hence, it is important to understand the relationship between different entity types for an ad product before creating them using Bulksheets. See the [Sponsored Products Entity Diagram](bulksheets/sp/sp-entities/sp-entity-hierarchy) for details. 
* Note that ‘Record ID’ and ‘Campaign ID’ columns are left empty, because the intent here is to create a new campaign with related entities. Since the entities don’t exist yet, there are no corresponding ‘Record ID’s’.
* In the [Sponsored Products Entity Diagram](bulksheets/sp/sp-entities/sp-entity-hierarchy), note how keywords in an automatic targeting campaign cannot be created at the ad group level. Only negative keyword can be created at a campaign level. 
* Refer to the corresponding entity details under 'Entities' to know the allowed values for each cell. 
* It is mandatory that you create at least one ad group for your automatic targeting campaign. 
* This is a Seller example; for Vendor, change the SKU column to ASIN. 

 <br/> 
 <br/> 
 
 You can copy-paste the example below in a spreadsheet and make edits to get started quickly. Please make sure that doing so does not alter the field values to an unaccepted format (for example, the date format should be mm/dd/yyyy). This example  has the following campaign structure:

* Sample SP Auto Campaign
  * Sample Campaign Negative Keyword
  * Sample Ad Group
    * Ad (Sample SKU)
  * Adjust Bids By Placement (Top of search (page 1))
  * Adjust Bids By Placement (Product pages)



| Record ID | Record Type           | Campaign ID | Campaign                | Campaign Daily Budget | Portfolio ID | Campaign Start Date | Campaign End Date | Campaign Targeting Type | Ad Group        | Max Bid | Keyword or Product Targeting     | Product Targeting ID | Match Type              | SKU        | Campaign Status | Ad Group Status | Status  | Bidding strategy              | Placement Type         | Increase bids by placement |
|-----------|-----------------------|-------------|-------------------------|-----------------------|--------------|---------------------|-------------------|-------------------------|-----------------|---------|----------------------------------|----------------------|-------------------------|------------|-----------------|-----------------|---------|-------------------------------|------------------------|----------------------------|
|   <br/>        | Campaign              |             | Sample SP Auto Campaign | 100                   |              | 02/05/2021          | 02/12/2021        | Auto                    |                 |         |                                  |                      |                         |            | enabled         |                 |         | Dynamic bidding (up and down) |                        |                            |
|           | Keyword               |             | Sample SP Auto Campaign |                       |              |                     |                   |                         |                 |         | Sample Campaign Negative Keyword |                      | Campaign Negative Exact |            |                 |                 | enabled |                               |                        |                            |
|           | Ad Group              |             | Sample SP Auto Campaign |                       |              |                     |                   |                         | Sample Ad Group | 0.95    |                                  |                      |                         |            |                 | enabled         |         |                               |                        |                            |
|           | Ad                    |             | Sample SP Auto Campaign |                       |              |                     |                   |                         | Sample Ad Group |         |                                  |                      |                         | Sample SKU |                 |                 | enabled |                               |                        |                            |
|           | Campaign By Placement |             | Sample SP Auto Campaign |                       |              |                     |                   |                         |                 |         |                                  |                      |                         |            |                 |                 |         |                               | Top of search (page 1) | 100.00%                    |
|           | Campaign By Placement |             | Sample SP Auto Campaign |                       |              |                     |                   |                         |                 |         |                                  |                      |                         |            |                 |                 |         |                               | Product pages          | 200.00%                    |