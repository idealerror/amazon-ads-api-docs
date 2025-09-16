---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Updating a Sponsored Brands campaign to add new entities

__Key Points to Note__

* For an existing Sponsored Brands campaign, new keywords can be added at any time.
* For adding new keywords, leave the ‘Record ID’ column empty, but fill the ‘Campaign ID’ column with the existing campaign ID as this is an update operation for an existing campaign. Adding a new keyword for an existing campaign would look like the example below. In this example, campaign with ID ‘8372987368’ already exists. Since we want to add the new entities to that campaign, we fill the ‘Campaign ID’ cell for those entity with ‘8372987368’ indicating that we want to add these new entities to this existing campaign.
* Only new entities that are being added need to be included in the bulksheet and not the entire hierarchy of entities. In the example below, the Record Type "Campaign" is not included. However, doing so will not result in an error.
* To reduce the processing time, delete rows that do not contain updates. Do not forget to save your changes before you upload the file. Deleted rows will not be changed when you upload the file.
* To add entities to Sponsored Brands Video campaigns specify Ad Format as 'Video'.



| Record ID | Record Type | Campaign ID | Campaign              | Campaign Type          | Ad Format    | Budget | Budget Type | Portfolio ID | Campaign Start Date | Campaign End Date | Landing Page Url | Landing Page ASINs | Brand Name | Brand Entity ID | Brand Logo Asset ID | Headline | Creative ASINs | Media ID            | Automated Bidding | Bid Multiplier | Max Bid | Keyword              | Match Type     | Campaign Status | Status |
|-----------|-------------|-------------|-----------------------|------------------------|--------------|--------|-------------|--------------|---------------------|-------------------|------------------|--------------------|------------|-----------------|---------------------|----------|----------------|---------------------|-------------------|----------------|---------|----------------------|----------------|-----------------|--------|
|  <br/>    | Keyword     | 8372987368  | Sample Campaign       | Sponsored Brands       |              |        |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                     |                   |                | 0.5     | New Keyword          | Exact          |                 | enabled|
|           | Keyword     | 8372987368  | Sample Campaign       | Sponsored Brands       |              |        |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                     |                   |                |         | New Negative Keyword | Negative Exact |                 | enabled|
|  <br/>    | Keyword     | 8372987365  | Sample Video Campaign | Sponsored Brands       | Video        |        |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                     |                   |                | 0.5     | New Keyword          | Exact          |                 | enabled|

