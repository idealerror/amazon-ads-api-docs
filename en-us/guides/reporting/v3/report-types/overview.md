---
title: Sponsored ads report types (version 3)
description: Learn what report types and metrics are available in version 3 reporting for sponsored ads using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Report types

The Amazon Ads API supports a number of report types based on campaign entity structure. Depending on the ad type, campaign performance can be broken down by various dimensions including campaign, ad group, ad, keyword, target, and ASIN.

### Availability by ad type

>[NOTE] Sponsored Brands reports are currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

Each type of sponsored ad supports different report types and has a separate path defined.

| Report type | Sponsored Products | Sponsored Brands | Sponsored Display | Sponsored Television | Amazon DSP | ALL |
|-------------|--------------------|------------------|------|--------|--------|-----|
| [Ad](guides/reporting/v3/report-types/ad) | | ✓ | | | | |
| [Ad group](guides/reporting/v3/report-types/ad-group) | | ✓ | ✓ | | | |
| [Advertised product](guides/reporting/v3/report-types/advertised-product) | ✓ |  | ✓ | | | | 
| [Audience](guides/reporting/v3/report-types/audience) | | | | | ✓ | | 
| [Audio and video (beta)](guides/reporting/v3/report-types/audio-and-video) | | | | | ✓ | | 
| [Bid adjustments](guides/reporting/v3/report-types/bid-adjustment) | | | | | ✓ | |
| [Campaign](guides/reporting/v3/report-types/campaign) | ✓ | ✓ | ✓ | ✓ | ✓ | | 
| [Conversion Path](guides/reporting/v3/report-types/conversion-path) | | | | | | ✓ | 
| [Geo](guides/reporting/v3/report-types/geo) | | | | | ✓ |  |
| [Gross and invalid traffic](guides/reporting/v3/report-types/gross-and-invalid-traffic) | ✓ | ✓ | ✓ | | | |
| [Inventory](guides/reporting/v3/report-types/inventory) | | | | | ✓ |  |
| [Placement](guides/reporting/v3/report-types/placement) | | ✓ | | | |  |
| [Product](guides/reporting/v3/report-types/product) | | | | | ✓ |  |
| [Purchased product](guides/reporting/v3/report-types/purchased-product) | ✓ | ✓ | ✓ | | | |
| [Reach and frequency](guides/reporting/v3/report-types/reach) | | | | | ✓ | |
| [Search term](guides/reporting/v3/report-types/search-term) | ✓ | ✓ | | | | |
| [Targeting](guides/reporting/v3/report-types/targeting) | ✓ | ✓ | ✓ |✓| | |
| [Tech](guides/reporting/v3/report-types/tech) | | | | | ✓ |  |

