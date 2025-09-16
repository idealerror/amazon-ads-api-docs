---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Creating a Sponsored Brands campaign draft

__Key Points to Note__

* The example below shows how to create a Sponsored Brands campaign draft. 
* Refer to the corresponding entity details under 'Entities' to find the allowed values for each cell. 
* All the fields added in this example for draft campaign are mandatory. Other fields can be optionally added while creating the draft. 
* It is not mandatory to add keywords to your draft, but you can use this example to add them.
* Specify Ad Format as "Video" to create a Sponsored Brands Video draft (Only available in US, UK, and DE). Leave it blank or specify "Product Collection" for default Sponsored Brands drafts.

| Record ID | Record Type | Campaign ID | Campaign              | Campaign Type          | Ad Format    | Budget | Budget Type | Portfolio ID | Campaign Start Date | Campaign End Date | Landing Page Url | Landing Page ASINs | Brand Name | Brand Entity ID | Brand Logo Asset ID | Headline | Creative ASINs | Media ID            | Automated Bidding | Bid Multiplier | Max Bid | Keyword        | Match Type | Campaign Status | Status |
|-----------|-------------|-------------|-----------------------|------------------------|--------------|--------|-------------|--------------|---------------------|-------------------|------------------|--------------------|------------|-----------------|---------------------|----------|----------------|---------------------|-------------------|----------------|---------|----------------|------------|-----------------|--------|
|   <br/>   | Campaign    |             | Sample Draft          | Sponsored Brands Draft |              | 10     | Daily       |              |  04/23/2021         |                   |                  |                    |            | BRANDENTITYID   |                     |          |                |                     |                   |                |         |                |            | draft           |        |
|           | Keyword     |             | Sample Draft          | Sponsored Brands Draft |              |        |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                     |                   |                | 0.5     | Sample Keyword | Exact      |                 |   |
|   <br/>   | Campaign    |             | Sample Video Draft    | Sponsored Brands Draft | Video        | 10     | Daily       |              |  04/23/2021         |                   |                  |                    |            | BRANDENTITYID   |                     |          | B01LA5KACQ     | ams.123.asdb        |                   |                |         |                |            | draft           |        |
|           | Keyword     |             | Sample Video Draft    | Sponsored Brands Draft | Video        |        |             |              |                     |                   |                  |                    |            |                 |                     |          |                |                     |                   |                | 0.5     | Sample Keyword | Exact      |                 |   |
