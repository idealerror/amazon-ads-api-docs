---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Creating a Sponsored Brands campaign 

__Key Points to Note__

* Each Sponsored Brands campaign is a collection of Keywords and Product Targeting. To create an entity at any level, creating its parent entity correctly is a prerequisite. Lack of this knowledge is a source of common confusion, often resulting in errors when trying to create and manage campaigns via Bulksheets. Hence, it is important to understand the relationship between different entity types for an ad product before creating them using Bulksheets. See the [entity diagram](bulksheets/sb/sb-entities/sb-entity-hierarchy) for details.
* Note that ‘Record ID’ and ‘Campaign ID’ columns are left empty, because the intent here is to create a new campaign with related entities. Since the entities don’t exist yet, there are no corresponding ‘Record ID’s’.
* Once your campaign is uploaded, it goes through a review process. If you download a sheet while the campaign is still in review, you see a ‘servingStatus’ column set to ‘pendingReview’. No changes can be made to this campaign until the review completes. If the campaign is rejected by the review process, then it will automatically become a draft.
* Refer to the corresponding entity details under 'Entities' to find the allowed values for each cell. Common sources of error include the following:
  * Non-valid landing page and creative information. See [campaign entity details](bulksheets/sb/sb-entities/sb-entity-campaign).
  * Bid multiplier range is -99.00% to +99.00%. Note that adding the “-” or “+” and “%” is mandatory. If automated bidding is disabled, then you have to specify the bid multiplier in range of -99.00% to +99.00%.
  * Status column for creating keywords must be left blank. It should be provided only when updating a keyword. When left blank, the default status is set to "Enabled".
  * Specify Ad Format as "Video" to create a Sponsored Brands Video campaign (Only available in US, UK, and DE). Leave it blank or specify "Product Collection" for default Sponsored Brands campaigns.

<br/>
<br/>
You can copy-paste the example below in a spreadsheet and edit it to your needs in order to get started quickly. Please make sure the field formats are still valid (e.g. the date format should be mm/dd/yyyy). The example has the following campaign structure:

* Sample SB Campaign
  * Sample Keyword
  * Sample Negative Keyword


| Record ID | Record Type | Campaign ID | Campaign              | Campaign Type    | Ad Format   | Budget | Budget Type | Portfolio ID | Campaign Start Date | Campaign End Date | Landing Page Url                                                        | Landing Page ASINs | Brand Name                 | Brand Entity ID | Brand Logo Asset ID  | Headline                 | Creative ASINs                   | Media ID      | Automated Bidding | Bid Multiplier | Max Bid | Keyword                 | Match Type     | Campaign Status | Status  |
|-----------|-------------|-------------|-----------------------|------------------|-------------|--------|-------------|--------------|---------------------|-------------------|-------------------------------------------------------------------------|--------------------|----------------------------|-----------------|----------------------|--------------------------|----------------------------------|---------------|-------------------|----------------|---------|-------------------------|----------------|-----------------|---------|
|   <br/>   | Campaign    |             | Sample Campaign       | Sponsored Brands |             | 10     | Daily       |              | 04/23/2021          | 04/30/2021        | https://www.amazon.com/stores/page/02D1F273-90BF-44DD-8246-375256F76198 |                    | Sample Creative Brand Name |                 | AWz-C_2InEZrJWCZIR5a | Sample Creative Headline | B018UQ5AMS,B07PC7MHQ8,B07C1XC3GF |               | enabled           |                |         |                         |                | enabled         |         |
|           | Keyword     |             | Sample Campaign       | Sponsored Brands |             |        |             |              |                     |                   |                                                                         |                    |                            |                 |                      |                          |                                  |               |                   |                | 0.5     | Sample Keyword          | Exact          |                 | enabled |
|           | Keyword     |             | Sample Campaign       | Sponsored Brands |             |        |             |              |                     |                   |                                                                         |                    |                            |                 |                      |                          |                                  |               |                   |                |         | Sample Negative Keyword | Negative Exact |                 | enabled |
|   <br/>   | Campaign    |             | Sample Video Campaign | Sponsored Brands | Video       | 10     | Daily       |              | 04/23/2021          | 04/30/2021        |                                                                         |                    |                            | ENTITY1233121   |                      |                          | B018UQ5AMS                       | ams123.3123.d |                   |                |         |                         |                | enabled         |         |
|           | Keyword     |             | Sample Video Campaign | Sponsored Brands | Video       |        |             |              |                     |                   |                                                                         |                    |                            |                 |                      |                          |                                  |               |                   |                | 0.5     | Sample Keyword          | Exact          |                 | enabled |
|           | Keyword     |             | Sample Video Campaign | Sponsored Brands | Video       |        |             |              |                     |                   |                                                                         |                    |                            |                 |                      |                          |                                  |               |                   |                |         | Sample Negative Keyword | Negative Exact |                 | enabled |