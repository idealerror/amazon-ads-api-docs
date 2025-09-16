---
title: Known issues
description: View the statuses for recent known issues related to Amazon Ads advanced tool.  
type: guide
interface: general
tags:
    - Operations
keywords:
    - known issues
    - help
    - troubleshooting
---

# Known issues

>[TIP] To view information on outages, view the [Amazon Ads Status (beta) tool](support/status). 

This page lists known issues related to Amazon Ads advanced tools. Resolutions to known issues will be posted in in the [Release notes](release-notes/index).

## Amazon Ads API

### Sponsored Display targeting responses for /sd/targets will change after targets are deprecated

When a Sponsored Display target has been deprecated, expect a change in the response. 

Instead of resolved expressions for GET `/sd/targets` returning:

```bash
[
    "type": "views",
    "value": [
      { 
        "type": "asinCategorySameAs",    
        "value": "12345678"
      },
      { 
        "type": "lookback",    
        "value": "30"
      },
    ]
]
```

Resolved expressions will return:

```bash
[
      {
        "type": "views",
        "value": "category=\"12345678\" lookback=30"
      }
    ]

```

**Workaround**: None. These targets will not serve any impressions and they should be archived. 

### Incomplete data retention for Sponsored Products reports in version 3

The Sponsored Products [campaigns](guides/reporting/v3/report-types#campaign-reports), [targeting](guides/reporting/v3/report-types#campaign-reports), [search term](guides/reporting/v3/report-types#search-term-reports), [advertised product](guides/reporting/v3/report-types#advertised-product-reports), and [purchased product](guides/reporting/v3/report-types#purchased-product-reports) reports allow users to request reports up to 95 days in the past. The request will succeed and return a report, however only data for the last 65 days is included. 

**Workaround**: None. The engineering team is working to support up to 95 days as stated in the documentation.

## Amazon Marketing Stream

### Amazon DSP [adsp-adgroup-targets](guides/amazon-marketing-stream/datasets/dsp-campaign-management#amazon-dsp-ad-group-targets-beta) dataset truncates some payloads

There are three types of targets (audience, geolocation, and app) that can have documents that are too large to be written through Amazon Marketing Stream via SQS. This is due to the limitation of SQS message size to 256 KB, and does not impact targets in advertising systems in any way. We are working on an alternate solution to resolve the issue. This limitation impacts a small percentage of overall records, and in this version of our API we still send these records, but we will instead denote the fields have been truncated as `PAYLOAD_TRUNCATED`. 

**Workaround**: If you would like to retrieve this information in the interim, please call the [geotargeting API](dsp-ad-group-targeting-geo), [audiences targeting API](dsp-ad-group-targeting-audiences), or the [app targeting API](dsp-ad-group-targeting-apps) to receive the documents in full.

### Unknown records in sp-conversion dataset

There is a known bug in Amazon Marketing Stream that may cause a small number of records from another ad product to show up in [sp-conversion](guides/amazon-marketing-stream/datasets/sp-performance#sponsored-products-conversions-dataset) data without the corresponding records in sp-traffic data. We have confirmed such unwanted data is a very small percentage of the overall conversion records and should not cause any material impact to your analysis. 

**Workaround**: Until we fix this issue, you can ignore these records from your dataset.