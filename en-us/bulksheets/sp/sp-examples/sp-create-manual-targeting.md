---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Creating a Sponsored Products Manual Targeting Campaign

__Key Points to Note__

* Each Sponsored Products campaign is a collection of nested ad entities including Campaign, Ad Groups, Keywords (campaign level and ad group level), Ads and Product Targeting. To create an entity at any level, creating its parent entity correctly is a prerequisite. Lack of this knowledge is a source of common confusion, often resulting in errors when trying to create and manage campaigns via Bulksheets. Hence, it is important to understand the relationship between different entity types for an ad product before creating them using Bulksheets. See the [Sponsored Products Entity Diagram](bulksheets/sp/sp-entities/sp-entity-hierarchy) for details.  
* Note that ‘Record ID’ and ‘Campaign ID’ columns are left empty, because the intent here is to create a new campaign with related entities. Since the entities don’t exist yet, there are no corresponding ‘Record ID’s’. In a manual campaign, you will have to create the keywords or product attribute targeting under an ad group as shown in the example below. See the [Sponsored Products Entity Diagram](bulksheets/sp/sp-entities/sp-entity-hierarchy) for details.  
* Refer to the corresponding entity details under 'Entities' to know the allowed values for each 'Record Type'. 
* An ad group can either have keywords or product attribute targeting but not both. 

 <br/> 
 <br/> 
 
You can copy-paste the example campaign below in a spreadsheet and make edits to get started quickly. This is an example for a Seller; for Vendor, change the SKU column to ASIN. This example campaign has the following structure. 

* Sample SP Manual Campaign
  * Sample Campaign Negative Keyword
  * Sample Ad Group 1
    * Ad (Sample SKU 1)
    * Keyword (Sample Keyword)
    * Negative Keyword (Sample Negative Keyword)
  * Sample Ad Group 2
    * Ad (Sample SKU 2)
    * Product Targeting (Targeting expression: asin="B07G9H7X45")
    * Negative Product Targeting (Negative targeting expression: asin="B07G9H7H87")
  * Adjust Bids By Placement



| Record ID | Record Type           | Campaign ID | Campaign                  | Campaign Daily Budget | Portfolio ID | Campaign Start Date | Campaign End Date | Campaign Targeting Type | Ad Group          | Max Bid | Keyword or Product Targeting     | Product Targeting ID | Match Type                    | SKU          | Campaign Status | Ad Group Status | Status  | Bidding strategy              | Placement Type         | Increase bids by placement |
|-----------|-----------------------|-------------|---------------------------|-----------------------|--------------|---------------------|-------------------|-------------------------|-------------------|---------|----------------------------------|----------------------|-------------------------------|--------------|-----------------|-----------------|---------|-------------------------------|------------------------|----------------------------|
|  <br/>         | Campaign              |             | Sample SP Manual Campaign | 100                   |              | 02/05/2021          | 02/12/2021        | Manual                  |                   |         |                                  |                      |                               |              | enabled         |                 |         | Dynamic bidding (up and down) |                        |                            |
|           | Keyword               |             | Sample SP Manual Campaign |                       |              |                     |                   |                         |                   |         | Sample Campaign Negative Keyword |                      | Campaign Negative Exact       |              |                 |                 | enabled |                               |                        |                            |
|           | Ad Group              |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 | 0.95    |                                  |                      |                               |              |                 | enabled         |         |                               |                        |                            |
|           | Ad                    |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 |         |                                  |                      |                               | Sample SKU 1 |                 |                 | enabled |                               |                        |                            |
|           | Keyword               |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 | 0.75    | Sample Keyword                   |                      | Exact                         |              |                 |                 | enabled |                               |                        |                            |
|           | Keyword               |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 1 |         | Sample Negative Keyword          |                      | Negative Exact                |              |                 |                 | enabled |                               |                        |                            |
|           | Ad Group              |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 2 | 0.95    |                                  |                      |                               |              |                 | enabled         |         |                               |                        |                            |
|           | Ad                    |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 2 |         |                                  |                      |                               | Sample SKU 2 |                 |                 | enabled |                               |                        |                            |
|           | Product Targeting     |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 2 | 0.75    |                                  | asin="B07G9H7X45"    | Targeting Expression          |              |                 |                 | enabled |                               |                        |                            |
|           | Product Targeting     |             | Sample SP Manual Campaign |                       |              |                     |                   |                         | Sample Ad Group 2 |         |                                  | asin="B07G9H7H87"    | Negative Targeting Expression |              |                 |                 | enabled |                               |                        |                            |
|           | Campaign By Placement |             | Sample SP Manual Campaign |                       |              |                     |                   |                         |                   |         |                                  |                      |                               |              |                 |                 |         |                               | Top of search (page 1) | 100%                       |
|           | Campaign By Placement |             | Sample SP Manual Campaign |                       |              |                     |                   |                         |                   |         |                                  |                      |                               |              |                 |                 |         |                               | Product pages          | 100%                       |