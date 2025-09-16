---
title: Sponsored ads reporting FAQ (version 2)
description: Get answers to frequently asked questions about version 2 reporting for sponsored ads campaigns using the Amazon Ads API. 
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Reporting frequently asked questions

Get answers to commonly asked questions about reporting.

## General

### What's the difference between an export and a report?

Exports contain metadata about an account's campaign structure at the time the export request was made. Reports contain campaign performance data, and may also contain metadata. For best performance we recommend requesting all metadata via exports, then joining metadata and performance data using an identifier or name. 

- Learn more about [Reports](guides/reporting/overview).
- Learn more about [Exports](guides/exports/overview). 

## Timing & reporting periods

### How long does it take for sponsored ads reporting data to become available?

Initial impression and click data is available using the API within 12 hours. Note that small changes to impression and click data may happen up to three days after the initial click date due to the [traffic validation process](https://advertising.amazon.com/help#GBYPYH79NGJJ5JPS). We recommend retrieving click and impression data again after three days to ensure you have the latest data. 

Initial conversion data is available within 24 hours, but may change as part of the restatement process. Restatements of conversion data occur 1, 7, and 28 days after the conversion event. Because conversions are reported on the date of the ad interaction, restatement includes all dates in your attribution window. For example, if your attribution window is 14 days, restatement can update attribution up to 42 days (14 plus 28) after the report date.

### How do I run a sponsored ads report for a specific date range?

API reports contain performance data for a single day only. If you need a data from a specific date range, request a report for each day in the desired time range then aggregate the data. 

You can also retrieve reports for specific date ranges using the advertising console. The [version 3 reporting API](guides/reporting/v3/overview) also supports reports with a specific date range. 

### What is the available report history for the version 2 reporting API?

Sponsored ads reports are available for 60 days using the API. For example, if today is January 1, 2022, the earliest possible date you can request data for is November 2, 2021. 

### What if I need sponsored ads data older than 60 days?

Some [advertising console reports](https://advertising.amazon.com/help#GBYSPTSLR337JMLH) have a longer reporting period. To join console and API reporting data, you can use the campaign or ad group name as an index.

You can also try using the [version 3 reporting API](guides/reporting/v3/overview). Some report types support longer reporting periods in the new version. 

### What time zone is used for my reports?

The time zone for a report is the time zone associated with the advertising account used to request the report, as determined by the profile ID provided in the request headers. To check the time zone for a given profile ID, use the [GET /v2/profiles/{profileId}](reference/2/profiles#tag/Profiles/operation/getProfileById) endpoint.

## Troubleshooting

### Why are my reports being rate limited?

You may experience report limiting during busy times of day. To avoid excessive rate limiting, spread report requests throughout the day rather than requesting all at once.

For more best practices, see [Rate limiting](reference/concepts/limits).

### Why is my report taking so long to generate?

Reports that request many metrics result in larger data files that may take longer to generate. To avoid large reports, specify necessary metrics only in your report request. Consider report requests that don't specify metadata (for example, `campaignName`, `campaignId`, `adGroupName`, `adGroupId`) in the report request body itself. Instead, retrieve metadata fields using an [export](guides/exports/overview) and then merge the retrieved metadata into your report.  

### Why is my report empty?

If there are no clicks or views associated to campaigns on the requested day, your report may contain no data or an empty JSON array (`[]`).   

### Why aren't my Sponsored Display reports returning data?

Make sure you understand the targeting tactic used by your campaigns. If your campaigns use both contextual targeting and audience targeting, you need to request two versions of each Sponsored Display report, one using `"tactic": "T00020"` and the other using `"tactic": "T00030"`. You can then aggregate the data for full daily performance. 

The type of bid optimization may also have an impact on your reporting data. Sponsored Display campaigns have two options for bid optimization: cost per click (CPC) and cost per thousand viewable impressions (vCPM). 

If you have CPC campaigns, you can use the standard metrics for Sponsored Display reports: [impressions](guides/reporting/v2/metrics#impressions), [attributedConversions14d](guides/reporting/v2/metrics#impressions), [attributedDetailPageView14d](guides/reporting/v2/metrics#impressions), [attributedSales14d](guides/reporting/v2/metrics#impressions), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#impressions).

If you have vCPM campaigns, you need to use the vCPM specific metrics:  [viewAttributedConversions14d](guides/reporting/v2/metrics#impressions), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#impressions), [viewAttributedSales14d](guides/reporting/v2/metrics#impressions), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#impressions), [viewImpressions](guides/reporting/v2/metrics#impressions).

If you have CPC campaigns only, vCPM specific metrics return the values associated with clicks only (meaning they will be the same as the standard metrics). To get full performance data for both cost types, include all metrics in your report request and then aggregate the data. 

### Why aren't my Sponsored Brands reports returning data?

If you are running only Sponsored Brands Video campaigns, make sure you set `creativeType` to `video` in the request body. If `creativeType` is not set, the report returns data for non-video campaigns only. If you are running both video and non-video Sponsored Brands campaigns, make sure to create two report requests: one for video and one for non-video.

### Can I filter a Sponsored Brands report?

Sponsored Brands reports do not currently support filtering using the `stateFilter` request body parameter available for Sponsored Display and Sponsored Products reports. By default, any campaign entity with performance data for the requested day will appear in your report. 

However, if certain metrics are included in a report request, the report will contain *all* historical campaign entities, regardless of whether performance data is available for a particular entity or whether the entity is active. This can result in a very large report that contains many rows with no performance data.

To avoid large reports, do **not** include the following metrics in Sponsored Brands report requests:

* campaignName
* campaignStatus
* campaignBudget
* campaignBudgetType
* adGroupName
* keywordBid
* keywordStatus
* keywordText
* matchType

We recommend retrieving this information using an [export](guides/exports/overview), then joining the export and report using the relevant entity ID (e.g., `campaignId`, `keywordId`). 

### Why am I seeing Sponsored Display campaigns in my Sponsored Products report?

This is a known issue in version 2 and may impact a small number of advertisers. This issue has been resolved in the [version 3 reporting API](guides/reporting/v3/overview).

### Why does the search term report only have a subset of the items included in the advertising console report?

The Sponsored Brands and Sponsored Products search term reports in the advertising console contains all search terms that had an impression, regardless of if that term resulted in a click. The API search term report only contains impressions that also had at least one click. Therefore you will see a difference in the number of entries between the console and API reports.

## Postman

### I am trying to [download a report](guides/reporting/v2/sponsored-ads-reports#downloading-reports) using Postman. Why am I not getting any data?

When you send your request in Postman, go to the arrow next to the **Send** button, and click **Send and Download**. 