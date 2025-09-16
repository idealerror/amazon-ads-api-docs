---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Updating a Sponsored Brands campaign to edit existing entities

__Key Points to Note__

* For making an update, it is important that the ‘Record ID’ and ‘Campaign ID’ fields are filled out. The fastest way to get the right values for these cells is to download a bulksheet with the campaigns that you wish to update.
* Refer to the corresponding entity details under 'Entities' to know if a field can be updated and the allowed values. 
* Only the entities to be updated need to be included in a bulksheet and not the entire hierarchy of entities, although doing so will not result in errors.
* To reduce the processing time, delete rows that do not contain updates. Do not forget to save your changes before you upload the file. Deleted rows will not be changed when you upload the file.
* Note if the parent entity of the entity that you wish to update is archived, you will receive an error while updating it. 

<br/>
<br/>

The following example demonstrated updates on the above created Sample Campaign. The following updates are being made:

* Sample Campaign
  * Budget updated from $10 to $20
  * Campaign End Date updated from 04/30/2021 to 04/30/2022
* Sample Keyword - Max Bid updated from $0.5 to $0.65
* Sample Negative Keyword - Status updated from “enabled” to “paused”


| Record ID  | Record Type | Campaign ID | Campaign              | Campaign Type           | Ad Format    | Budget | Budget Type | Portfolio ID | Campaign Start Date | Campaign End Date | Landing Page Url                                                        | Landing Page ASINs | Brand Name                 | Brand Entity ID | Brand Logo Asset ID  | Headline                 | Creative ASINs                   | Media ID            | Automated Bidding | Bid Multiplier | Max Bid | Keyword                 | Match Type     | Campaign Status | Status  |
|------------|-------------|-------------|-----------------------|-------------------------|--------------|--------|-------------|--------------|---------------------|-------------------|-------------------------------------------------------------------------|--------------------|----------------------------|-----------------|----------------------|--------------------------|----------------------------------|---------------------|-------------------|----------------|---------|-------------------------|----------------|-----------------|---------|
| 8372987368 | Campaign    | 8372987368  | Sample Campaign       | Sponsored Brands        |              | 20     | Daily       |              | 04/23/2021          | 04/30/2022        | https://www.amazon.com/stores/page/02D1F273-90BF-44DD-8246-375256F76198 |                    | Sample Creative Brand Name |                 | AWz-C_2InEZrJWCZIR5a | Sample Creative Headline | B018UQ5AMS,B07PC7MHQ8,B07C1XC3GF |                     | enabled           |                |         |                         |                | enabled         |         |
| 3984728390 | Keyword     | 8372987368  | Sample Campaign       | Sponsored Brands        |              |        |             |              |                     |                   |                                                                         |                    |                            |                 |                      |                          |                                  |                     |                   |                | 0.65    | Sample Keyword          | Exact          |                 | enabled |
| 2849847204 | Keyword     | 8372987368  | Sample Campaign       | Sponsored Brands        |              |        |             |              |                     |                   |                                                                         |                    |                            |                 |                      |                          |                                  |                     |                   |                |         | Sample Negative Keyword | Negative Exact |                 | paused  |