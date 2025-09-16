---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Archiving a Sponsored Brands campaign

__Key Points to Note__

* Sponsored Brands campaigns can be archived only after they are in enabled state. If they are still pending review, the campaign cannot be archived. 
* Similar to update examples, make sure the “Record ID” and “Campaign ID” for all "Record Types" are present. By changing the status of the corresponding row to “**archived**”, you can essentially archive that particular row. 
* If you archive a campaign, all items associated with that campaign (i.e. campaign, keywords and negative keywords) will be automatically archived.
* As shown in the example below, archiving ‘Sample Ad Group 1’ row alone would archive all that is within that Ad Group. The status of all the child entities will automatically update and can be confirmed by initiating a new Bulksheet download.

| Record ID  | Record Type | Campaign ID | Campaign             | Campaign Type    | Ad Format    |Budget | Budget Type | Portfolio ID | Campaign Start Date | Campaign End Date | Landing Page Url | Landing Page ASINs | Brand Name | Brand Entity ID | Brand Logo Asset ID | Headline | Creative ASINs | Media ID        | Automated Bidding | Bid Multiplier | Max Bid | Keyword     | Match Type | Campaign Status | Status   |
|------------|-------------|-------------|----------------------|------------------|--------------|-------|-------------|--------------|---------------------|-------------------|------------------|--------------------|------------|-----------------|---------------------|----------|----------------|-----------------|-------------------|----------------|---------|-------------|------------|-----------------|----------|
| 8523545269 | Keyword     | 8372987368  | Sample Campaign      | Sponsored Brands |              |       |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                 |                   |                | 0.5     | New Keyword | Exact      |                 | archived |
| 8523545269 | Keyword     | 8372987368  | Sample Campaign Video| Sponsored Brands | Video        |       |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                 |                   |                | 0.5     | New Keyword | Exact      |                 | archived |
