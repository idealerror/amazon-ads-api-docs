---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Updating a Sponsored Brands campaign draft

__Key Points to Note__

For making an update, it is important that the ‘Record ID’ and ‘Campaign ID’ fields are filled out. The fastest way to get the right values for these cells is to download a bulksheet with the campaigns that you wish to update. 
<br/>
<br/>

The example below demonstrates updating the [sample campaign](bulksheets/sb/sb-examples/sb-create-draft). The following updates are being made:

* Sample Campaign
  * Budget updated from $10 to $20
  * Campaign End Date updated from empty to 04/30/2022
* Sample Keyword - Max Bid updated from $0.5 to $0.65

| Record ID | Record Type | Campaign ID | Campaign              | Campaign Type           | Ad Format    | Budget | Budget Type | Portfolio ID | Campaign Start Date | Campaign End Date | Landing Page Url | Landing Page ASINs | Brand Name | Brand Entity ID | Brand Logo Asset ID | Headline | Creative ASINs | Media ID            | Automated Bidding | Bid Multiplier | Max Bid | Keyword        | Match Type | Campaign Status | Status |
|-----------|-------------|-------------|-----------------------|-------------------------|--------------|--------|-------------|--------------|---------------------|-------------------|------------------|--------------------|------------|-----------------|---------------------|----------|----------------|---------------------|-------------------|----------------|---------|----------------|------------|-----------------|--------|
| 8379272947 | Campaign   | 8379272947 | Sample Campaign Draft | Sponsored Brands Draft  |              | 20     | Daily       |              |  04/23/2021         |  04/30/2022       |                  |                    |            | BRANDENTITYID   |                     |          |                |                     |                   |                |         |                |            | draft           |        |
| 2783984738 | Keyword    | 8379272947 | Sample Campaign Draft | Sponsored Brands Draft  |              |        |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                     |                   |                | 0.65    | Sample Keyword | Exact      |                 |   |