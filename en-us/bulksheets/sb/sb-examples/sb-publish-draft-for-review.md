---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Submitting a Sponsored Brands campaign draft for review

__Key Points to Note__

* Once your draft has all the necessary Sponsored Brands fields added, it is ready to be submitted for a full campaign review. To trigger this conversion, change the Campaign Status from ‘draft’ to ‘enabled’
* None of the mandatory fields can be left empty at this point. Otherwise, you will receive an error indicating the missing fields
* Once the upload completes, it will initiate a conversion of your draft to an actual campaign. It will now go through the review process mentioned before
* If the campaign is accepted, all child entities of this draft will be automatically converted. The Record Ids could change after submission and thus, it is recommended to download a new bulksheet download before updating these live campaigns.

<br/>
<br/>
The example below demonstrates submitting a draft campaign for a full review.


| Record ID  | Record Type | Campaign ID | Campaign              | Campaign Type          | Ad Format    | Budget | Budget Type | Portfolio ID | Campaign Start Date | Campaign End Date | Landing Page Url | Landing Page ASINs | Brand Name | Brand Entity ID | Brand Logo Asset ID | Headline | Creative ASINs | Media ID            | Automated Bidding | Bid Multiplier | Max Bid | Keyword        | Match Type | Campaign Status | Status |
|------------|-------------|-------------|-----------------------|------------------------|--------------|--------|-------------|--------------|---------------------|-------------------|------------------|--------------------|------------|-----------------|---------------------|----------|----------------|---------------------|-------------------|----------------|---------|----------------|------------|-----------------|--------|
| 8379272947 | Campaign    | 8379272947  | Sample Campaign       | Sponsored Brands Draft      |              | 20     | Daily       |              |  04/23/2021         |  04/30/2022       | https://www.amazon.com/stores/page/02D1F273-90BF-44DD-8246-375256F76198 |                    | Sample Creative Brand Name | BRANDENTITYID   | AWz-C_2InEZrJWCZIR5a | Sample Creative Headline | B018UQ5AMS,B07PC7MHQ8,B07C1XC3GF |                | enabled           |                |         |         |            | enabled         |        |